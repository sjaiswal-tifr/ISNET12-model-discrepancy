# Discrepancy function and priors 

We now discuss the discrepancy term $\delta(x)$ and priors for the model 
parameters and discrepancy function.


(sec:DF)=
## Discrepancy function

The discrepancy function $\delta(x)$ represents the difference between the 
prediction of an imperfect theoretical model and the true response of the system 
at an input setting $x$. In general, this discrepancy is not known quantitatively. 
However, we may have qualitative prior knowledge about its expected magnitude, 
smoothness, or characteristic variation with $x$. Our goal is to encode this 
information statistically.

This is analogous to assigning a prior distribution to an unknown model parameter. 
The important difference is that $\delta(x)$ is not a single unknown quantity, 
but an unknown **function**. We therefore require a probability distribution over 
functions. A Gaussian process (GP) provides a natural framework for constructing 
such a functional prior.

Following [*KOH*](https://rss.onlinelibrary.wiley.com/doi/abs/10.1111/1467-9868.00294), 
we represent $\delta(x)$ as a zero-mean Gaussian process:

$$
    \delta(\cdot \mid\boldsymbol{\phi}) \sim {\rm GP} 
    \left(\boldsymbol{0}, K(\cdot,\cdot \mid\boldsymbol{\phi}) \right) \,,
$$

where $K(\cdot,\cdot \mid\boldsymbol{\phi})$ is the covariance kernel 
and $\boldsymbol{\phi}$ denotes its hyperparameters. The choice of kernel and 
its hyperparameters encodes our prior assumptions about the possible size, 
smoothness, and correlation structure of the model discrepancy.


## Priors

The role of prior information in modelling the discrepancy is analogous to its
role in ordinary Bayesian parameter estimation. For model parameters $\pars$,
we specify a prior distributions $\mathrm{pr}(\pars)$ that reflects the range of values
that we consider physically plausible before seeing the data. For the
discrepancy $\delta(x)$, however, the unknown object is an entire function
rather than a single number. The corresponding prior must therefore describe
which **functions** we consider plausible.

In a GP, this information is encoded primarily through the covariance kernel $K$.
The kernel determines how the discrepancy at different input points is related,
and therefore controls properties such as the typical magnitude, smoothness,
correlation length, and characteristic variation of functions drawn from the prior.
Thus, choosing a kernel is the functional analogue of choosing a prior for an
ordinary model parameter: it provides a way to incorporate our prior physical
knowledge about the model uncertainty.

As a simple example, suppose that we have prior knowledge that the theoretical
model provides a good description of the system at small $x$, but is expected 
to deviate increasingly from the true system response as $x$ grows. We would 
then want the functional prior for $\delta(x)$ to favor functions whose typical 
magnitude is small at small $x$ and larger at large $x$.


## Example kernel

A kernel that can generate such functional prior has the form

$$
K(x_i, x_j) = \bar{c}^2 (x_i x_j)^r
\exp\left(- \frac{\lVert x_i-x_j\rVert^2}{2 \ell^2} \right)
$$ (eq:Balldrop_Kernel)

The quantities $\boldsymbol{\phi}=(\bar c,\ell,r)$ are the **hyperparameters** 
of the GP. In the same way that the value of an ordinary model parameter 
determines a particular model prediction, the values of the GP hyperparameters 
determine the character of the functions favored by the discrepancy prior, as 
illustrated in {numref}`fig-GP_nonstationary_comparison`.


:::{figure} ./images/GP_nonstationary_comparison.png
:height: 500px
:name: fig-GP_nonstationary_comparison

Five draws from each of four GPs of the form in Eq. {eq}`eq:Balldrop_Kernel` 
with hyperparameters $\ell$, $\bar c$, and $r$ as listed. Note that with $r>0$, 
the variance grows with $x$.
:::


The hyperparameter $\bar{c}$, which is fixed at $\bar{c}=1$ in the figure, 
controls the overall scale of the discrepancy. Larger values of $\bar{c}$ allow 
larger departures of $\delta(x)$ from zero. The parameter $\ell$ is the correlation 
length between any two points; as $|x -x'|$ becomes greater than $\ell$, the 
points become increasingly uncorrelated. Comparing $\ell=1$ to $\ell=3$ shows 
that the latter curves have a longer "wavelength". Thus, a larger $\ell$ favors 
more slowly varying, smoother discrepancy functions, whereas a smaller $\ell$ 
allows the discrepancy to vary more rapidly with $x$.

Finally, the factor $(x_i x_j)^r$ allows the typical size of the discrepancy to change 
with $x$. In particular, the prior variance at a point $x$ is $K(x,x)=\bar{c}^2 x^{2r}$. 
Therefore, for $r>0$, the prior variance of the discrepancy increases with $x$. 
This directly encodes our qualitative prior knowledge that the model is more 
reliable at small $x$ and becomes less reliable as $x$ increases. Notice that 
this does **not** require the discrepancy itself to increase monotonically with 
$x$; rather, it allows increasingly larger deviations from zero as $x$ grows. 


## Bayesian inference 

Bayesian inference then jointly constrains the physical model parameters 
$\pars$ and the discrepancy hyperparameters $\boldsymbol{\phi}$ from the observed 
data. In doing so, it also updates the prior GP for $\delta(x)$ into a posterior 
distribution over discrepancy functions, thereby learning both the physical parameters 
and how the theoretical model is likely to deviate from the true system response.



