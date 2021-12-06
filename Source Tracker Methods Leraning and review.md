# Source Tracker Methods Leraning and review


The theory part：

$$P( z_{i} = v \mid  z^{ \neg i}, x)  \propto P(x_{i} \mid v)  \times P(v \mid x^{ \neg i}) = \begin{cases}
\frac{m_{x_{i}v} + \alpha_{1}}{m_{v} + \tau \alpha_{1}} \times \frac{n_{v}^{ \neg i} + \beta}{n - 1 + \beta V} & v < V
\\
\\
\frac{m_{x_{i}v} + n \alpha_{2}}{m_{v} + \tau n \alpha_{2}} \times \frac{n_{v}^{ \neg i} + \beta}{n - 1 + \beta V} & v = V
\end{cases}$$

Where, x means a sink includes a set of n, representing random sample sequences mapped to a taxa, in which each n is noted to a probable source environment (v∈1…V), including an unknown source; 

The Gibb's sampler works in four basic steps: 
1. Randomly assign the sequences in a sink, where the mixing proportions are unknown, to source environments. These random assignments represent which source a given sink sequence came from.
2. Select one of the sequences from step 1, calculate the *actual* probabilities of that sequence having come from any of the source environments, and update the assigned source environment of the sequence based on a random draw with the calculated probabilities. Repeat many times. 
3. At intervals in the repeats of step 2, take the source environment assingments of all the sequences in a sink and record them. 
4. After doing step 3 a certain number of times (i.e. recording the full assignments of source environments for each sink sequence), terminate the iteration and move to the next sink.  


Steps to create source sample data and sink sequences from your environmental sample:
1.	Design a set of sample site, representing your desired sinks, which are used to assignment to the specific source environment.
2.	Choose several source samples, for example, here I want to set my sources as wastewater influent, gut, soil, specific freshwater as river water or water in reservoir, 


### Usage of rarefy: Rarefaction Species Richness
Rarefied species richness for community ecologists.

**Usage**:
```r
rarefy(x, sample, se = FALSE, MARGIN = 1)
rrarefy(x, sample)
drarefy(x, sample)
rarecurve(x, step = 1, sample, xlab = "Sample Size", ylab = "Species", label = TRUE, col, lty, ...)
rareslope(x, sample)
```
x--Community data, a matrix-like object or a vector. 

sample--Subsample size for rarefying community, either a single value or a vector.

### Burn-In Theory
**What is Burn-In?**
Burn-in is a colloquial term that describes the practice of throwing away some iterations at the beginning of an MCMC run.The burn-in notion says you start somewhere, say at x, then you run the Markov chain for n steps, from which you throw away all the data (no output). This is the burn-in period. After the burn-in you run normally, using each iterate in your MCMC calculations.

The name burn-in comes from electronics (see the entry in the Jargon File). Many electronics components fail quickly. Those that don't are a more reliable subset. So a burn-in is done at the factory to eliminate the worst ones.

Anyone who has ever done any Markov chain simulation has noticed that some starting points are better than others. Even the simplest and best behaved Markov chains exhibit this phenomenon.

Consider an AR(1) time series, having an update defined by

$$X_{n+1}=rX_n+e_n$$

where the en are independent, identically distributed mean-zero normal random variables. The pictures below show an AR(1) sampler with r = .98. First a run started at x = 10.

![x=10](http://users.stat.umn.edu/~geyer/mcmc/ten.gif)

Then a run started at x = 0.

![x=0](http://users.stat.umn.edu/~geyer/mcmc/zero.gif)

As any fool can plainly see, the second run is a lot better than the first. One way to say what is wrong with the first is that there is an "initial transient" that is unrepresentative of the equilibrium distribution. The obvious cure is to toss the initial 200 iterations, or in other words to use a burn-in period of n = 200.

But strictly speaking, the description of the problem using the initial transient notion is mathematical nonsense. The point x = 10 is a possible state which the sampler must eventually visit if run long enough. How long? Under the equilibrium distribution P(X > 10) is less than $10^{−23}$. So the problem is not that we shouldn't see x = 10 at all, the problem is that we shouldn't see it in a run this short. So what we conclude from examples like this is that we do not want to start way out in the tail of the equilibrium distribution.

But that is not the same as saying that burn-in is a useful or even interesting idea. The second run, started at zero, does not have the same problem. There is no obvious need for burn-in.

**Good Alternatives to Burn-In.**
So what should we do instead of burn-in? One rule that is unarguable is

*Any point you don't mind having in a sample is a good starting point.*

In a typical application, one has no mathematical analysis of the Markov chain that tells where the good starting points are (nor how much burn-in is required to get to a good starting point). All decisions about starting points are based on the output of some preliminary runs that appear to have converged to stationarity. Any point of the parts of these preliminary runs one believes (hopes?) to be representative of the equilibrium distribution is as good a starting point as any other.
So one rule I often follow is to start the next run where the last run ended. This is the rule most authorities recommend for random number generator seeds. Attempts by the user to start with random seeds can destroy the randomness properties of a generator. Only when the generator is used as one continuous stream does it have whatever desirable properties are claimed for it. One could do worse than to apply the same principle to MCMC.

Another possible rule is to start at a point, like the mode, known to have reasonably high probability. If no such point is known, this rule is useless, but it could be used more often than it actually is.

I make no claim that either of these rules is the one and only right way to do MCMC. I only claim that burn-in is no better and often worse.