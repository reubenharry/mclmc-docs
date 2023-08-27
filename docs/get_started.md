## Installation

`git clone https://github.com/ammunk/pyprob.git`

!!! Note

    This is a fork of the original PyProb repo, with more features, including W&B training integration, normalizing flows, normalizers, attention, and surrogates.

## API Overview

The meat of `pyprob` is an implementation of amortized inference via "inference compilation"; an inference network is trained to produce proposal distributions $q(x|y)$ that accurately estimate posterior distributions $p(x|y)$. In contrast to variational inference, the network is trained to produce an accurate proposal for all observations $y$, rather than for a specific $y$. These proposal distributions can then be used for importance sampling to produce accurate posterior distributions. The most basic form of a call for `pyprob` to learn an inference network is:

```python
model.learn_inference_network(
    num_traces=int(1e6),
    observe_embeddings={'observations': {'dim':32, 'depth':3}},
    )
```

where `num_traces` is the number of program runs used to produce samples to train the inference network. `observe_embeddings` gives the strategy by which the observations, `y`, are embedded to the input of the inference network that produces `q(x|y)`. In principle, the observations could be fed in as raw inputs, but it is often useful to use a network to embed the observations. In the case of observed image data one might choose to embed the observations using a convolutional network. With `{'observations': {'dim':32, 'depth':3}}` it is assumed that the observations are named `observations` within the model (see the example notebooks) and a standard feed-forward network with 3 hidden layers, each with 32 nodes, is used for the embedding. A different example would be `{'observations': {'embedding':ObserveEmbedding.FEEDFORWARD, 'reshape': [10, 10], 'dim': 32, 'depth': 2}}`, where the embedding network has been stated explicitly. Options are: 

- `ObserveEmbedding.FEEDFORWARD`
- `ObserveEmbedding.CNN2D5C`
- `ObserveEmbedding.CNN3D5C`
- `ObserveEmbedding.CNN2DSTRIDED`

A more thorough call to learn an inference network looks something like this:

```python
model.learn_inference_network(
    num_traces=int(1e6),
    observe_embeddings={'observations': {'dim':32, 'depth':3}},
    inference_network=pyprob.InferenceNetwork.LSTM,
    lstm_dim=512,
    lstm_depth=1,
    learning_rate_init=1e-4,
    optimizer_type=pyprob.Optimizer.ADAM,
    batch_size=100,
    proposal_mixture_components=10,
    log_file_name='training.log',
    num_workers=64,
)
```


When training an inference network there are two options for the `InferenceNetwork`: 
- `InferenceNetwork.FEEDFORWARD`
- `InferenceNetwork.LSTM`

The feed-forward network is able to learn marginal proposal distributions for latent variables individually, but not the covarainces between latent variables. The Long-Short Term Memory (LSTM) network, which has a memory, should be used if parameter covariances are important in your problem (they usually are). The `lstm_dim` and `lstm_depth` parameters control the dimension and depth of the LSTM unit.

The learning rate and optimizer are set with `learning_rate_init` and `optimizer`. `pyprob.Optimizer.ADAM` is a good default choice, but one can also use (stochastic) gradient descent `pyprob.Optimizer.SGD`.

Traces are batched in training in batches of size `batch_size`. Normally larger batches lead to better gradient estimates. The stop-condition for the network can be specified via `num_traces`, `num_iterations` or `train_time`. `num_iterations` gives the number of training steps (each of which contains `batch_size` traces). `train_time` gives the maximum allowed wall-clock time. If all three options are present then training will finish when the first criterion is met.

The proposal distributions, $q(x|y)$, are parameterized as sums of standard distributions that cover the same support as the prior $p(x)$ (e.g., if the prior is normal then the proposal could be a sum of normal distributions). `proposal_mixture_components` gives the number of components in the sum. 

Finally `log_file_name` gives the location for the training log file. Setting `num_workers` allows the data-generation process (from the model/simulation) to be parallelized over many CPUs.

Other arguments to `model.learn_inference_network` of interest are:
- `wandb_run`: Instance of `wandb.init` for logging to Weights & Biases.
- `sample_embedding_dim`: Internal dimension of the sample-embedding network.
- `address_embedding_dim`: Internal dimension of the address-embedding network.
- `save_file_name_prefix`: File-name prefix for saved networks.
- `save_every_sec`: Saves the inference network every this number of seconds.

Finally, the argument `variable_embeddings` can be used to specify how the output of the LSTM layer is converted into parameters for the proposal distributions. An example dictionary would be:

```python
variable_embeddings[parameter_name] = {
    'uniform-proposal': 'beta-mixture',
    'normal-proposal': 'normal-mixture',
    'num_layers': 3, 
    'hidden_dim': 32,
    'num_layers_flow': 5, 
   } 
```

Options for the `uniform-proposal` are: `beta-mixture`; `truncated-normal-mixture`; `flow`. Options for the `normal-proposal` are: `normal-mixture`; `flow`. Here `flow` is a normalizing flow, which has `num_layers_flow` layers if used.

### Normalizers

In working with `pyprob`, we have found that it is often useful to first train a normalization network, before training the LSTM. The inference network outputs parameters of proposal distributions, and it is useful for these to be in a space where the numbers are $\mathcal{O}(1)$. There are two methods to do this, which can be stacked. The first method, `basic`, uses model samples to calculate the mean and variance of each $x$ under the prior and then uses these numbers to rescale according to a "$Z$ score" such that the mean is $0$ and variance is $1$. If the posterior is within the prior this makes sense.

We have found that learning can still be difficult in cases where the posterior is small volume relative to the (rescaled) prior. In this case, a feed-forward network can be trained to learn the marginal posterior $p(x|y)$, and the space can then be normalized (conditionally on $y$) according to this to make learning easier. A call to learn an inference network with normalizers would look like
```
model.learn_inference_network(
    num_iterations=100,
    num_iterations_normalizer=100,
    observe_embeddings={'y': {'dim':32, 'depth':3}},
    variable_normalizers={'x': ['basic', 'learn']},
)
```
`['basic', 'learn']` ensures that both schemes are used for variable `x` (which should be named in the model, see the notebooks for examples). For particularly thin posteriors, the normalization networks can be stacked, e.g., `['basic', 'learn', 'learn', 'learn']`, which would learn 3 sequential normalizers.


!!! Note

    This page was adapted from Andreas Munk and Alex Mead's readme.