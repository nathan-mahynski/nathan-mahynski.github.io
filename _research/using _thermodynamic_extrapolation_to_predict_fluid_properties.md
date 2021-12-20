---
title: "Using Thermodynamic Extrapolation to Accurately Predict Fluid Properties"
excerpt: "Efficient use of resources to feed the Data Monster."
header:
  image: 
  teaser: /assets/img/isotherm_extrap.png
gallery:
  - url: https://doi.org/10.1063/1.5026493
    image_path: /assets/img/predict_extrap_1.png
    alt: "Predicting structural properties of fluids by thermodynamic extrapolation"
  - url: https://doi.org/10.1021/acs.jctc.8b00534
    image_path: /assets/img/predict_extrap_2.png
    alt: "Flat-Histogram Monte Carlo as an Efficient Tool To Evaluate Adsorption Processes Involving Rigid and Deformable Molecules"
  - url: https://doi.org/10.1080/08927022.2020.1747617
    image_path: /assets/img/predict_extrap_3.png
    alt: "Flat-histogram extrapolation as a useful tool in the age of big data"
classes:
  - wide
tags:
  - thermodynamics
  - extrapolation
  - fluids
  - machine learning
  - property prediction
---

{% include toc icon="gears" title="Table of Contents" %}

{% include gallery caption="" %}

# tl;dr

The concept of extrapolating thermodynamic properties measured in classical systems using statistical mechanical principles has been around for some time. This is linked to the fact that derivative properties of a system's free energy are related to fluctuations of observables (such as the number of particles in a simulation) which makes them easy to measure.  Consequently, Taylor series expansions are fairly straightforward to obtain.  Of course, the range over which this expansion is reasonably accuate is limited by the order of the expansion and how accurately you can measure these fluctuations (how long you can afford to run a simulation). However, by combining this idea of extrapolation with biased sampling (forcing a simulation to systematically visit states along a predetermined order parameter path) we can build a "network" of Taylor expansions that work together to create a remarkably accurate predictor of properties (thermodynamic and even structural) of fluid systems over a broad range of conditions.
These extrapolations significantly amplify the amount of information that can be extracted from simulations enabling a small set of them to:

* feed data-intensive regression algorithms such as neural networks,
* accelerate the search for, e.g., optimal experimental conditions or protocols that use simulations as a guide,
* reduce the cost of force-field development,
* screen candidate materials for thermodynamic properties faster

This post is an abbreviated summary of concepts discussed in the manuscripts below (text available upon [request](mailto:nathan.mahynski@gmail.com) if you cannot access them); other authors have also expanded upon these ideas to explore of other systems (such as water) as well.  Please cite appropriately if you find this information helpful.

1. ["Extrapolation and interpolation strategies for efficiently estimating structural observables as a function of temperature and density," J. I. Monroe, H. W. Hatch, N. A. Mahynski, M. S. Shell, V. K. Shen, J. Chem. Phys. <b>153</b>, 144101 (2020).](https://doi.org/10.1063/5.0014282)
2. ["Flat-histogram extrapolation as a useful tool in the age of big data," N. A. Mahynski, H. W. Hatch, M. Witman, D. A. Sheen, J. R. Errington, V. K. Shen, Molecular Simulation 1–13 (2020).](https://doi.org/10.1080/08927022.2020.1747617)
3. ["Flat-histogram monte carlo as an efficient tool to evaluate adsorption processes involving rigid and deformable molecules," M. Witman, N. A. Mahynski, B. Smit, J. Chem.  Theory Comput. <b>14</b>, 6149–6158 (2018).](https://doi.org/10.1021/acs.jctc.8b00534)
4. ["Predicting structural properties of fluids by thermodynamic extrapolation," N. A. Mahynski, S. Jiao, H. W. Hatch, M. A. Blanco, V. K. Shen, J. Chem. Phys. <b>148</b>, 194105 (2018).](https://doi.org/10.1063/1.5026493)
5. ["Multivariable extrapolation of grand canonical free energy landscapes," N. A. Mahynski, J. R. Errington, V. K. Shen, J. Chem. Phys., <b>147</b>, 234111 (2017).](http://dx.doi.org/10.1063/1.5006906)
6. ["Predicting virial coefficients and alchemical transformations by extrapolating mayer-sampling monte carlo simulations," H. W. Hatch, S. Jiao, N. A. Mahynski, M. A. Blanco, V. K. Shen, J. Chem. Phys. <b>147</b>, 231102 (2017).](http://dx.doi.org/10.1063/1.5016165)
7. ["Temperature extrapolation of multicomponent grand canonical free energy landscapes," N. A. Mahynski, J. R. Errington, V. K. Shen, J. Chem. Phys. <b>147</b>, 054105 (2017).](http://dx.doi.org/10.1063/1.4996759)
8. ["Predicting low-temperature free energy landscapes with flat-histogram monte carlo methods," N. A. Mahynski, M. A. Blanco, J. R. Errington, V. K. Shen, J. Chem. Phys. <b>146</b>, 074101 (2017).](http://dx.doi.org/10.1063/1.4975331)

# What is "Thermodynamic Extrapolation"?

## Some Mathematics and Statistical Mechanics

The [partition function](https://en.wikipedia.org/wiki/Partition_function_(statistical_mechanics)) describes the thermodynamic properties of a system as a function of state variables (temperature, $T$, volume, $V$, etc.).  These vary depending the [thermodynamic ensemble](https://en.wikipedia.org/wiki/Ensemble_(mathematical_physics)) you are concerned with.  If we take the system to be classical such that the potential and kinetic energy contributions to the [Hamiltonian](https://en.wikipedia.org/wiki/Hamiltonian_(quantum_mechanics)) can be separated, $H(\vec{r}, \vec{q}) =  U_p(\vec{r}) + U_k(\vec{q})$, then the canonical partition function is as follows:

$Q(\beta,V,\vec{N}) = \overbrace{\left[ \frac{1}{\Pi_{i=1}^k N_i!} \int {\rm exp} \left( -\beta U_p(\vec{r}) \right) {\rm d}\vec{r} \right]}^{Potential~=~Q_p} \overbrace{\left[ \frac{1}{h^{dN_i}} \int {\rm exp} \left( -\beta U_k(\vec{q}) \right) {\rm d}\vec{q} \right]}^{Kinetic~=~Q_k}$

where, $N_i$ refers to the number of particles of species $i$ in the system (simulation), $V$ is the volume ($d$ is the system dimensionality, e.g., $d=3$), $h$ is the [Planck constant](https://en.wikipedia.org/wiki/Planck_constant), and $\beta = 1/(k_{\rm B}T)$ is the inverse of the absolute temperature scaled by the [Boltzmann constant](https://en.wikipedia.org/wiki/Boltzmann_constant). The second integral can be evaluated exactly in terms of the [thermal de Broglie wavelength](https://en.wikipedia.org/wiki/Thermal_de_Broglie_wavelength): $Q_k = \frac{1}{\Pi_{i=1}^k \Lambda_i^{dN_i}}$.  This is typically absorbed into the system's chemical potential in the grand canonical case, which becomes temperature dependent (see Ref. 7 above); for such an open ensemble the partition function can be expressed in terms of the canonical one at each different $\vec{N} = (N_1, N_2, \dots, N_k)$ value.

$\Xi ( \beta, V, \vec{\mu}) = \sum_{N_1} \sum_{N_2} \dots \sum_{N_k} {\rm exp} \left( \beta \mu_k N_k \right) Q(\beta, V, \vec{N})$

The probability of observing a given state, defined by some order parameter, $Y$, is then $\Pi(Y) / \Xi$ where $\Xi = \sum \Pi(Y)$. For many systems, the most interesting order parameter in the grand canonical ensemble is related to the particle number; sometimes it can be broken into a vector of individual particle numbers, or we can use the total, for example. Regardless, it is often easier to work with the logarithm of this probability instead:

${\rm ln} \Pi(\vec{N}) = \beta \sum_{i=1}^k \mu_i N_i + {\rm ln}Q(\beta,V,\vec{N}) - {\rm ln} \Xi ( \beta, V, \vec{\mu})$

This probability distribution is generally referred to as the "macrostate" distribution, where the order parameter defines the "macrostate"; this is because each order parameter value describes a set of microstates. For example, each unique arrangement of particles in a simulation may be considered a microstate, but all microstates with the same total number of particles might be considered a macrostate.  Below, on the left, is an example distribution for a single component system at different temperatures (colors); the dashed lines will be explained later.  At high $T$ there is a single peak in the middle, whereas at low $T$ there are 2, each corresponding to a different phase (one is a vapor, one is a condensed liquid). 

<img style="float: center" src="lnPI_extrap.jpg" width=450px>
<img style="float: center" src="isotherm_extrap.png" width=450px> 

Once the macrostate distribution is known you can compute (equilibrium) thermodynamic (average) properties from it.  These are just weighted sums:

$\langle A \rangle = \frac{\sum A(Y) \times \Pi(Y)}{\Xi}$

In the case of phase separation, you simply break the above sum into regions defining each phase.  Thus, you can compute properties like the average number of particles (composition) or density of each phase.  In the figure above, on the right, is a grand canonical example showing isotherms (density as a function of chemical potential) which can be computed from such a distribution. The "gap" in isotherms at lower $T$ correspond to the phase envelope.  Other manipulations allow you to compute pressure as well.  Clearly, knowledge of this macrostate distribution equates with knowledge of thermodynamic information of the system.

However, each isotherm above requires the distribution to be known at that temperature; i.e., the 6 curves above require the knowledge of 6 different distribution functions.  In principle, you can perform 6 different simulations to measure these (described below), but this quickly becomes expensive to do many isotherms.  Thermodynamic extrapolation allows you to estimate one distribution from another.  The basic idea is to expand each value in the distribution as a Taylor series in the variable you wish to extrapolate; for many order parameters (extensive properties, such as $N$ or $U$) expanding in their intensive conjugates ($\mu$ or $\beta$) is very easy. In fact, there are well known relationships between derivatives with respect to intensive variables and fluctuations in their extensive conjugates, $f(X,Y) = \langle XY \rangle - \langle X \rangle \langle Y \rangle$, dating back to work by [Robert Zwanzig](https://en.wikipedia.org/wiki/Robert_Zwanzig) in the 1950's. This means that we can obtain simple expressions for these derivatives in terms of extensive properties (like particle number) which are trivial to measure during a simulation, or recover from a log file afterwards.  For example:

$\frac{\partial {\rm ln}\Pi(\vec{N}) }{\partial \beta} = \sum_{i=1}^k \mu_i \left( N_i - \langle N_i \rangle \right) - \left( U - \langle U \rangle \right)$

$\frac{\partial^2 {\rm ln}\Pi(\vec{N}) }{\partial \beta^2} = -\sum_{i=1}^k \mu_i \frac{\partial \langle N_i \rangle}{\partial \beta} - \frac{\partial U}{\partial \beta} + \frac{\partial \langle U \rangle}{\partial \beta}$

where, e.g., $W = U$:

$\frac{\partial \langle W \rangle}{\partial \beta} = \sum_{i=1}^k \mu_i f \left( \langle W \rangle, \langle N_i \rangle \right) - f \left( \langle W \rangle, \langle U \rangle \right)$. 

The references above contain full derivations and expressions for derivatives in a number of ensembles. However, it should be clear that derivatives can be expressed in terms of averages of (powers and products of) extensive properties. These averages can be obtained using the macrostate distribution as described above. Therefore, the knowledge of ${\rm ln}\Pi(\vec{N})$ and $A(\vec{N})$ at one thermodynamic state (e.g., $T$) means we can compute all necessary terms in a Taylor expansion to compute this information at another state:

${\rm ln}\Pi(\vec{N}; \beta) = {\rm ln}\Pi(\vec{N}; \beta^0) + \frac{\partial {\rm ln}\Pi(\vec{N}; \beta)}{\partial \beta} \Delta \beta  + \frac{1}{2!}\frac{\partial^2 {\rm ln}\Pi(\vec{N}; \beta)}{\partial \beta^2} (\Delta \beta)^2 + \dots$

Again, the ensemble, order parameter, and other factors influence the final expression, but this is representative of the procedure. It turns out for many systems the expansions are remarkably accurate even at first or second order!  This is essentially because the macrostate distribution is really made up of **many** Taylor series working together; in the above example, $0 \le N \le 400$, so it is really **400** Taylor series, one at each $N$, making a prediction at a new $\beta$.  These predictions are strung together to make the new distribution.  This "network" means that we can obtain surprisingly accurate predictions over a broad range of conditions (e.g., temperatures) using a very small number of simulations (sometimes just 1)! The slope of the line, or parabola depending on the order of the expansion, means that the curvature of the macrostate distribution can change in non-trivial ways, enabling new minima to appear, as in the example of phase separation above. You can imagine an incremental change in $\beta$ like a new frame in a "movie" where each frame is a snapshot of ${\rm ln}\Pi(\vec{N}; \beta)$ at different $\beta$ values.

<img style="float: center" src="extrap_cartoon.png" width=1000px>

## Performing Simulations

So how do we obtain the macrostate distribution in the first place? There are different approaches, but the one we have used in the past is based on Wang-Landau Monte Carlo simulations.  In this method, you store a histogram of the number of times a system visits a given state defined by the order parameter chosen (e.g., $\vec{N}$).  Over time, you use the inverse of this to bias the likelihood of visitig those states again.  This pushes the simulation to explore new, "unseen" points along the order parameter (in the example above, different $N$ values). By slowly reducing the amount the histogram is incremented by, you eventually converge to a bias that is the inverse of the macrostate distribution.

<img style="float: center" src="bias.png" width=1000px>

This is know as a "flat histogram" technique because the combination of the true distribution and the bias become "flat" when it has converged.  During the "production" phase of a simulation, properties are measured as a function of the order parameter and used to construct the averages and fluctuations necessary to compute the derivatives.  

We do not always have to extrapolate.  We can also obtain exact expressions that allow us to "reweight" the distribution.  For example, if we know a grand canonical macrostate distribution at one set of chemical potentials, rearranging the initial expressions given for the macrostate distribution we obtain:

${\rm ln}\Pi(\vec{N}; \vec{\mu}, \beta) = {\rm ln}\Pi(\vec{N}; \vec{\mu^0}, \beta) + \beta\sum_{i=1}^k \left( \mu_i - \mu_i^0 \right)N_i + C$,

where $C$ is some constant related to the differences between the partition functions at the two chemical potentials.  In practice, because distributions are normalized during property calculations this is irrelevant and we can assume $C=0$.  Since $\vec{\mu^0}$ are known and we are choosing the new $\vec{\mu}$ it is clear that this amounts to adding a "line" (slope is $\beta \vec{\mu}$) to the current macrostate distribution.  

If we measured the macrostate distribution as a function of all fluctuating extensive properties, we could obtain reweighting expressions that allow us to compute how the distribution changes as a function of the conjugate intensive variables.  For example, in the grand canonical case particle number and energy fluctuate while chemical optential and temperature are fixed, so we would have to measure ${\rm ln}\Pi(N_1, N_2, \dots, N_k, U)$.  Clearly, this is a $k+1$ dimensional histogram which can become unwieldy even for small $k$.  Moreover, numerical deviations from detailed balance come into play when "stitching" together a surface that allows multiple "pathways" between states; for example, in a binary system to go from ($N_1=10, N_2=10$) to ($N_1=11, N_2=11$) we could first add one $N_1$ particle then one $N_2$, or vice versa.  This is beyond the scope of this discussion, but for this and other reasons, it is usually easier to define a more coarse-grained order parameter such as $N_{\rm tot} = N_1 + N_2$ to use instead.  This amounts to moving the "slider" in the figure below to the left; the further left you move, the more coarse-grained the macrostates are. It turns out that such order parameters do not always have convenient reweighting expressions.  However, we can define an extrapolation expression for such cases!

<img style="float: center" src="extrap_vs_reweight.png" width=1000px>

<!--
Properties like phase coexistence (the binodal curve) are computed by finding the chemical potential(s) where the areas under the peaks of ${\rm ln}\Pi(\vec{N})$ are equal.  Moreover, average extensive properties are strongly influenced only by macrostates near their most likely state, since probabilities of states decay quickly (recall the above expressions are in log scale) as you move away from it.  We have found that as a result, properties that depend on the **shape** of the macrostate distribution, such as pressure and mole fraction, can be parameterized in terms of each other very accurately.  To see why imagin

 A remarkably and convenient consequence of this is that

Errors hide in mu when properties are defined by the SHAPE of the distribution.-->

# Examples of Efficient Exploration

## Low Temperature Landscapes

T-extrap
mu-extrap
t-mu extrap

even works for anisoptric particles (marco)

## Multiomponent Mixtures

Multicomponent

transition states are hard to sample

## Confined Systems

Film wetting
Stuff with Matt

# Optimal Combination

Jacob's Work

# Extrapolating Structural Observables

Sally's work

Mention Ward's work so far as first order

Jeppe Dyre
Iso-structural stuff

Other people have reached similar conclusions and used similar results to explore water behavior.

# Extrapolating Other Types of Simulations

Hatch - mayer sampling MC, and also parameters in U(r)

# Conclusions





