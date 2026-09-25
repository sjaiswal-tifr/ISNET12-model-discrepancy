# Introduction to model discrepancy

Comparisons between theoretical models and experimental data are at the heart 
of scientific inquiry. Theoretical models guide our understanding of complex 
systems by translating hypotheses into quantitative predictions that can be 
tested experimentally. Traditionally, a close fit between a model's 
predictions and measured data is interpreted as a sign of success, often 
implying that the model parameters capture the underlying physical processes. 
However, this paradigm assumes that the model fully represents the complexity 
of actual systems -- an assumption that is rarely justified in practice. 

All models have inherent limitations beyond their domains of validity. Using 
them beyond these regimes without accounting for theory uncertainties arising 
from imperfections in the models (missing physics or approximations) can lead 
to biased parameter estimates, reducing these parameters to mere 
"fitting variables" rather than meaningful physical quantities. Without 
accounting for these uncertainties, inference can force the approximate model 
to reproduce the data, effectively absorbing model inadequacy into the 
inferred parameters and yielding posteriors that need not reflect their 
physical values. Quantifying these uncertainties is therefore essential for 
reliable parameter estimation [Kennedy and O'Hagan](https://doi.org/10.1111/1467-9868.00294),
[Brynjarsdóttir and O'Hagan](https://doi.org/10.1088/0266-5611/30/11/114007),
[Higdon et al.](https://doi.org/10.1137/S1064827503426693),
[Jaiswal et al.](https://doi.org/10.1016/j.physletb.2025.139946), and
[Jaiswal](https://doi.org/10.1016/j.physletb.2026.140243).


(sec:KOHFramework)=
## The KOH and BOH frameworks

A model discrepancy framework employing Gaussian processes (GP) was introduced in 
Kennedy and O'Hagan ([*KOH*](https://rss.onlinelibrary.wiley.com/doi/abs/10.1111/1467-9868.00294)) 
and variations have been explored in many subsequent studies (Ex. Brynjarsdóttir 
and OʼHagan ([*BOH*](https://iopscience.iop.org/article/10.1088/0266-5611/30/11/114007)). 
In these approaches, uncertainties in observable predictions arising from 
imperfections in the theoretical models are modeled using Gaussian Process. 

However, one persistent challenge is the need to constrain the GP's 
covariance kernel. For example, in BOH, the authors emphasized the importance 
of incorporating knowledge of the theory's validity at specific points in the 
input space (i.e., the domain in which observables are measured) so that both 
the GP and its derivative could be accurately constrained. In practice, 
however, specifying such accurate knowledge about the theory is often difficult. 

Here, we construct the GP covariance kernel based on only 
qualitative prior knowledge of the theory's domain of validity across the 
input space. This type of prior knowledge -- for example, recognizing that 
"*the theory is more reliable in this regime than in that one*" -- is typically 
easier to provide and often available. By leveraging this information, the 
framework prioritizes the accurate extraction of model parameters rather than 
simply optimizing the fit to the observables. Bayesian parameter inference is 
then performed to simultaneously estimate both the model parameters and the GP 
hyperparameters, thereby quantifying uncertainties from both the experimental 
data and the theoretical model.


(sec:statMD)=
## Statistical model

We begin by setting up the model discrepancy framework following the Kennedy and 
O'Hagan framework. Let the $i$-th measurement for an observable $y$ be denoted as

$$
    y(x_i) \sim \zeta(x_i) + \epsilon_i \,,
    \qquad \epsilon_i \sim \mathcal{N}(0,\sigma_i^2) \,.
$$ (eq-stat_true)

Here, the index $i$ corresponds to the point in input space $x$ where the $i$-th 
observation is made (e.g., a specific time in the ball drop experiment described later). 
Independent observation errors $\epsilon_i$ are assumed to follow Gaussian 
distributions with zero mean and standard deviation $\sigma_i$. Here, $\zeta(x_i)$ 
represents the true value of the observable at $x_i$. (We assume that $x$ is made 
unitless by scaling it with a suitable reference scale $x_0$.)

We denote the prediction from a theoretical model for the observable $y$ as 
$\eta(x, \pars)$, where $\pars$ is the vector of true but unknown model parameters. 
The model for $\eta(x,\pars)$ is then defined as:

$$
    \zeta(x) = \eta(x, \pars) + \delta(x) ;
$$ (eq-md)

here $\delta(x)$ quantifies the discrepancy between the theoretical model prediction 
and the true system value at input setting $x$. Combining the above equations, 
we express the observation $y(x_i)$ as

$$
    y(x_i) \sim \eta(x_i, \pars) + \delta(x_i) + \epsilon_i .
$$ (eq-stat_MD)

Thus, each observation is modeled as the sum of the model output (evaluated at 
the true $\pars$), the model discrepancy $\delta$ at $x_i$, and the observational 
error $\epsilon_i$. The statistical model is summarized in {numref}`fig-statistical_model`.


:::{figure} ./images/statistical_model.png
:height: 100px
:name: fig-statistical_model

Statistical model.
:::

If the true parameter values $\pars$ were known *a priori*, then the 
discrepancy between $y(x_i)$ and $\eta(x_i, \pars)$ would quantify how well 
the theoretical model describes the data (up to measurement noise) without 
compensating through parameter shifts. Conversely, if $\delta(x_i)$ were known 
exactly (e.g. from theoretical calculations), then using $\delta(x_i)$ in 
Eq. [](#eq-stat_MD) would, in principle, yield correct estimates of $\pars$. 
In practice, neither $\pars$ nor $\delta(x_i)$ is known, and both must be 
inferred from data simultaneously.
