ICML, Day 1: Temporal Point Processes (Tutorial)

The first day of ICML consisted of a full day of tutorial sessions, for a full list see [ICML Schedule](https://icml.cc/Conferences/2018/Schedule?type=Tutorial). 
For the morning session, I decided to attend [Learning With Temporal Point Processes](http://learning.mpi-sws.org/tpp-icml18/):

Temporal point processes start from the observation that most time series are noisy observations of complex processes, observed asynchronously and at irregularly sampled intervals. In practice, a commonly used workaround to this is aggragating observations over an interval, but this leads to questions like how long the interval should be, how to handle intervals with 0 observations and so on.

Some applications outside finance are information propagation in social networks or modeling infectious disease propagation.

Definition: A *temporal point process* is a random process whose realization consists of discrete events localized in time.

Rather than modeling time itself as a random variable, which leads to problems when combining events from multiple timelines, the key idea is to use an intensity function $\lambda$, which you can think of as the number of events per unit time. As an intensity function is not a density, it need not integrate to 1, rather the only technical constraint is that it has to be non-negative. It also makes combining multiple timelines straightforward (think for example about all stock trades for stocks in an index from the trades for the individual stocks) 

A few basic building blocks for temporal point processes are:
- The constant rate Poisson process, which is independent of history
- The inhomogeneous Poisson process, with a time-dependent rate, but still independent of its own history
- The survival process, which has some value up to time t_s, and is 0 afterwards - note that this process depends on its own history
- The Hawkes process, a self-exciting process, useful to model clustered or bursty events (Exercise: can you come up with an example from finance?)

For all of the above, well-established techniques exist for sampling (mostly rejection sampling and potentially inverse sampling) and fitting (Maximum likelihood estimation) exist.

One can further generalize this by having mutually exciting processes, but at this point the classical schemes for sampling and inference start to break down.

The second part of the tutorial focussed on a few more complex examples around modeling temporal point processes, and the question of how to uncover cluster assignments from event data where the underlying cluster is not homogeneous. 

For complex, analytically intractable real-world temporal dynamics, the Neural Hawkes process was introduced (see the [original paper by Mei and Eisner](https://papers.nips.cc/paper/7252-the-neural-hawkes-process-a-neurally-self-modulating-multivariate-point-process.pdf)), which is a self-exciting process, but unlike in the classical Hawkes process, the intensity dynamics are modeled with an RNN.

Once a process has been modeled and calibrated, we can use it to make predictions, but also to build a generative model using a GAN architecture and the Wasserstein distance. This was followed by a brief description on how to uncover (Granger) causality from multivariate Hawkes processes.

In the last section of the tutorial the relation to Reinforcement Learning (RL) and Control were discussed, with examples to choosing post times on social networks to optimize feed rankings (i.e. where does your post appear in your followers/friends feed when they next look at it after you posted) and learning an optimal repetition policy for studying vocabulary. While easier settings can be solved with classical stochastic control techniques (basically solving the HJB equation), in situations that either cannot be modeled with stochastic differential equations or where the objective function is intractable, a RL-based policy optimization approach has to be used.
