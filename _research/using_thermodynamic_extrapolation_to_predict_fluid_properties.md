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

The concept of extrapolating thermodynamic properties measured in classical systems using statistical mechanical principles has been around for some time. This is linked to the fact that derivative properties of a system's free energy are related to fluctuations of observables (such as the number of particles in a simulation) which makes them easy to measure.  Consequently, Taylor series expansions are fairly straightforward to obtain.  Of course, the range over which this expansion is reasonably accurate is limited by the order of the expansion and how accurately you can measure these fluctuations (how long you can afford to run a simulation). However, by combining this idea of extrapolation with biased sampling (forcing a simulation to systematically visit states along a predetermined order parameter path) we can build a "network" of Taylor expansions that work together to create a remarkably accurate predictor of properties (thermodynamic and even structural) of fluid systems over a broad range of conditions. These extrapolations significantly amplify the amount of information that can be extracted from simulations enabling a small set of them to:

* feed data-intensive regression algorithms such as neural networks,
* accelerate the search for, e.g., optimal experimental conditions or protocols that use simulations as a guide,
* reduce the cost of force-field development,
* screen candidate materials for thermodynamic properties faster

This post is an abbreviated summary of concepts discussed in the manuscripts below (text available upon [request](mailto:nathan.mahynski@gmail.com) if you cannot access them); other authors have also expanded upon these ideas to explore of other systems (such as water) as well, so this list is not an exhaustive review of all work in this area.  Please cite appropriately if you find this information helpful.

1. ["Predicting low-temperature free energy landscapes with flat-histogram monte carlo methods," N. A. Mahynski, M. A. Blanco, J. R. Errington, V. K. Shen, J. Chem. Phys. <b>146</b>, 074101 (2017).](http://dx.doi.org/10.1063/1.4975331)
2. ["Temperature extrapolation of multicomponent grand canonical free energy landscapes," N. A. Mahynski, J. R. Errington, V. K. Shen, J. Chem. Phys. <b>147</b>, 054105 (2017).](http://dx.doi.org/10.1063/1.4996759)
3. ["Predicting virial coefficients and alchemical transformations by extrapolating mayer-sampling monte carlo simulations," H. W. Hatch, S. Jiao, N. A. Mahynski, M. A. Blanco, V. K. Shen, J. Chem. Phys. <b>147</b>, 231102 (2017).](http://dx.doi.org/10.1063/1.5016165)
4. ["Multivariable extrapolation of grand canonical free energy landscapes," N. A. Mahynski, J. R. Errington, V. K. Shen, J. Chem. Phys., <b>147</b>, 234111 (2017).](http://dx.doi.org/10.1063/1.5006906)
5. ["Predicting structural properties of fluids by thermodynamic extrapolation," N. A. Mahynski, S. Jiao, H. W. Hatch, M. A. Blanco, V. K. Shen, J. Chem. Phys. <b>148</b>, 194105 (2018).](https://doi.org/10.1063/1.5026493)
6. ["Flat-histogram monte carlo as an efficient tool to evaluate adsorption processes involving rigid and deformable molecules," M. Witman, N. A. Mahynski, B. Smit, J. Chem.  Theory Comput. <b>14</b>, 6149–6158 (2018).](https://doi.org/10.1021/acs.jctc.8b00534)
7. ["Flat-histogram extrapolation as a useful tool in the age of big data," N. A. Mahynski, H. W. Hatch, M. Witman, D. A. Sheen, J. R. Errington, V. K. Shen, Molecular Simulation 1–13 (2020).](https://doi.org/10.1080/08927022.2020.1747617)
8. ["Extrapolation and interpolation strategies for efficiently estimating structural observables as a function of temperature and density," J. I. Monroe, H. W. Hatch, N. A. Mahynski, M. S. Shell, V. K. Shen, J. Chem. Phys. <b>153</b>, 144101 (2020).](https://doi.org/10.1063/5.0014282)

Ref. 7 is essentially a review of most of this work, which the interested reader should refer to.

# What is "Thermodynamic Extrapolation"?

## Some Mathematics and Statistical Mechanics

The [partition function](https://en.wikipedia.org/wiki/Partition_function_(statistical_mechanics)) describes the thermodynamic properties of a system as a function of state variables (temperature, $T$, volume, $V$, etc.).  These vary depending the [thermodynamic ensemble](https://en.wikipedia.org/wiki/Ensemble_(mathematical_physics)) you are concerned with.  If we take the system to be classical such that the potential and kinetic energy contributions to the [Hamiltonian](https://en.wikipedia.org/wiki/Hamiltonian_(quantum_mechanics)) can be separated, $H(\vec{r}, \vec{q}) =  U_p(\vec{r}) + U_k(\vec{q})$, then the canonical partition function is as follows:

$Q(\beta,V,\vec{N}) = \overbrace{\left[ \frac{1}{\Pi_{i=1}^k N_i!} \int {\rm exp} \left( -\beta U_p(\vec{r}) \right) {\rm d}\vec{r} \right]}^{Potential~=~Q_p} \overbrace{\left[ \frac{1}{h^{dN_i}} \int {\rm exp} \left( -\beta U_k(\vec{q}) \right) {\rm d}\vec{q} \right]}^{Kinetic~=~Q_k}$

where, $N_i$ refers to the number of particles of species $i$ in the system (simulation), $V$ is the volume ($d$ is the system dimensionality, e.g., $d=3$), $h$ is the [Planck constant](https://en.wikipedia.org/wiki/Planck_constant), and $\beta = 1/(k_{\rm B}T)$ is the inverse of the absolute temperature scaled by the [Boltzmann constant](https://en.wikipedia.org/wiki/Boltzmann_constant). The second integral can be evaluated exactly in terms of the [thermal de Broglie wavelength](https://en.wikipedia.org/wiki/Thermal_de_Broglie_wavelength): $Q_k = \frac{1}{\Pi_{i=1}^k \Lambda_i^{dN_i}}$.  This is typically absorbed into the system's chemical potential in the grand canonical case, which becomes temperature dependent (see Ref. 2 above); for such an open ensemble the partition function can be expressed in terms of the canonical one at each different $\vec{N} = (N_1, N_2, \dots, N_k)$ value.

$\Xi ( \beta, V, \vec{\mu}) = \sum_{N_1} \sum_{N_2} \dots \sum_{N_k} {\rm exp} \left( \beta \mu_k N_k \right) Q(\beta, V, \vec{N})$

The probability of observing a given state, defined by some order parameter, $Y$, is then $\Pi(Y) / \Xi$ where $\Xi = \sum \Pi(Y)$. For many systems, the most interesting order parameter in the grand canonical ensemble is related to the particle number; sometimes it can be broken into a vector of individual particle numbers, or we can use the total, for example. Regardless, it is often easier to work with the logarithm of this probability instead:

${\rm ln} \Pi(\vec{N}) = \beta \sum_{i=1}^k \mu_i N_i + {\rm ln}Q(\beta,V,\vec{N}) - {\rm ln} \Xi ( \beta, V, \vec{\mu})$

This probability distribution is generally referred to as the "macrostate" distribution, where the order parameter defines the "macrostate"; this is because each order parameter value describes a set of microstates. For example, each unique arrangement of particles in a simulation may be considered a microstate, but all microstates with the same total number of particles might be considered a macrostate.  Below, on the left, is an example distribution for a single component system at different temperatures (colors); the dashed lines will be explained later.  At high $T$ there is a single peak in the middle, whereas at low $T$ there are 2, each corresponding to a different phase (one is a vapor, one is a condensed liquid). 

<a href="http://dx.doi.org/10.1063/1.4975331"><img style="float: center" src="lnPI_extrap.jpg" width=450px><a/>
<a href="http://dx.doi.org/10.1063/1.4975331"><img style="float: center" src="isotherm_extrap.png" width=450px></a>

Once the macrostate distribution is known you can compute (equilibrium) thermodynamic (average) properties from it.  These are just weighted sums:

$\langle A \rangle = \frac{\sum A(Y) \times \Pi(Y)}{\Xi}$

In the case of phase separation, you simply break the above sum into regions defining each phase.  Thus, you can compute properties like the average number of particles (composition) or density of each phase.  In the figure above, on the right, is a grand canonical example showing isotherms (density as a function of chemical potential) which can be computed from such a distribution. The "gaps" in isotherms at lower $T$ correspond to the phase envelope.  Other manipulations allow you to compute pressure as well.  Clearly, knowledge of this macrostate distribution equates with knowledge of thermodynamic information of the system.

However, each isotherm above requires the distribution to be known at that temperature; i.e., the 6 curves above require the knowledge of 6 different distribution functions.  In principle, you can perform 6 different simulations to measure these (described below), but this quickly becomes expensive to do many isotherms.  Thermodynamic extrapolation allows you to estimate one distribution from another.  The basic idea is to expand each point in the distribution as a Taylor series in the variable you wish to extrapolate; for many order parameters (extensive properties, such as $N$ or $U$) expanding in their intensive conjugates ($\mu$ or $\beta$) is very easy. In fact, there are well known relationships between derivatives with respect to intensive variables and fluctuations in their extensive conjugates, $f(X,Y) = \langle XY \rangle - \langle X \rangle \langle Y \rangle$, dating back to work by [Robert Zwanzig](https://en.wikipedia.org/wiki/Robert_Zwanzig) in the 1950's. This means that we can obtain simple expressions for these derivatives in terms of extensive properties (like particle number) which are trivial to measure during a simulation, or recover from a log file afterwards.  For example:

$\frac{\partial {\rm ln}\Pi(\vec{N}) }{\partial \beta} = \sum_{i=1}^k \mu_i \left( N_i - \langle N_i \rangle \right) - \left( U - \langle U \rangle \right)$

$\frac{\partial^2 {\rm ln}\Pi(\vec{N}) }{\partial \beta^2} = -\sum_{i=1}^k \mu_i \frac{\partial \langle N_i \rangle}{\partial \beta} - \frac{\partial U}{\partial \beta} + \frac{\partial \langle U \rangle}{\partial \beta}$

where, e.g., $W = U$:

$\frac{\partial \langle W \rangle}{\partial \beta} = \sum_{i=1}^k \mu_i f \left( \langle W \rangle, \langle N_i \rangle \right) - f \left( \langle W \rangle, \langle U \rangle \right)$. 

The references above contain full derivations and expressions for derivatives in a number of ensembles. However, it should be clear that derivatives can be expressed in terms of averages of (powers and products of) extensive properties. These averages can be obtained using the macrostate distribution as described above. Therefore, the knowledge of ${\rm ln}\Pi(\vec{N})$ and $A(\vec{N})$ at one thermodynamic state (e.g., $T$) means we can compute all necessary terms in a Taylor expansion to compute this information at another state:

${\rm ln}\Pi(\vec{N}; \beta) = {\rm ln}\Pi(\vec{N}; \beta^0) + \frac{\partial {\rm ln}\Pi(\vec{N}; \beta)}{\partial \beta} \Delta \beta  + \frac{1}{2!}\frac{\partial^2 {\rm ln}\Pi(\vec{N}; \beta)}{\partial \beta^2} (\Delta \beta)^2 + \dots$

Again, the ensemble, order parameter, and other factors influence the final expression, but this is representative of the procedure. It turns out for many systems the expansions are remarkably accurate even at first or second order!  This is essentially because the macrostate distribution is really made up of **many** Taylor series working together; in the above example, $0 \le N \le 400$, so it is really **400** Taylor series, one at each $N$, making a prediction at a new $\beta$.  These predictions are strung together to make the new distribution.  This "network" means that we can obtain surprisingly accurate predictions over a broad range of conditions using a very small number of simulations (sometimes just 1)! The slope of the line, or parabola depending on the order of the expansion, means that the curvature of the macrostate distribution can change in non-trivial ways, enabling new minima to appear, as in the example of phase separation above. You can imagine an incremental change in $\beta$ like a new frame in a "movie" where each frame is a snapshot of ${\rm ln}\Pi(\vec{N}; \beta)$ at different $\beta$ values.

<img style="float: center" src="extrap_cartoon.png" width=1000px>

## Obtaining the Macrostate Distribution

So how do we obtain the macrostate distribution in the first place? There are a number of different approaches, and the extrapolation methodology described here is agnostic; it does not matter how you obtain it.  The method we have used in the past is based on Wang-Landau Monte Carlo simulations.  In this method, you store a histogram of the number of times a system visits a given state defined by the order parameter chosen (e.g., $\vec{N}$).  Over time, you use the inverse of this to bias the likelihood of visiting those states again.  This pushes the simulation to explore new, "unseen" points along the order parameter (in the example above, different $N$ values). By slowly reducing the amount the histogram is incremented by, you eventually converge to a bias that is the inverse of the macrostate distribution.

<img style="float: center" src="bias.png" width=1000px>

This is know as a "flat histogram" technique because the combination of the true distribution and the bias become "flat" when it has converged.  During the "production" phase of a simulation, properties are measured as a function of the order parameter and used to construct the averages and fluctuations necessary to compute the derivatives.  

We do not always have to extrapolate.  We can also obtain exact expressions that allow us to "reweight" the distribution.  For example, if we know a grand canonical macrostate distribution at one set of chemical potentials, rearranging the initial expressions given for the macrostate distribution we obtain:

${\rm ln}\Pi(\vec{N}; \vec{\mu}, \beta) = {\rm ln}\Pi(\vec{N}; \vec{\mu^0}, \beta) + \beta\sum_{i=1}^k \left( \mu_i - \mu_i^0 \right)N_i + C$,

where $C$ is some constant related to the differences between the partition functions at the two chemical potentials.  In practice, because distributions are normalized during property calculations this is irrelevant and we can assume $C=0$.  Since $\vec{\mu^0}$ are known and we are choosing the new $\vec{\mu}$ it is clear that this amounts to adding a "line" (slope is $\beta \vec{\mu}$) to the current macrostate distribution.  

If we measured the macrostate distribution as a function of all fluctuating extensive properties, we could obtain reweighting expressions that allow us to compute how the distribution changes as a function of the conjugate intensive variables.  For example, in the grand canonical case particle number and energy fluctuate while chemical potential and temperature are fixed, so we would have to measure ${\rm ln}\Pi(N_1, N_2, \dots, N_k, U)$.  Clearly, this is a $k+1$ dimensional histogram which can become unwieldy even for small $k$.  Moreover, numerical deviations from detailed balance come into play when "stitching" together a surface that allows multiple "pathways" between states; for example, in a binary system to go from ($N_1=10, N_2=10$) to ($N_1=11, N_2=11$) we could first add one $N_1$ particle then one $N_2$, or vice versa.  This is beyond the scope of this discussion, but for this and other reasons, it is usually easier to define a more coarse-grained order parameter such as $N_{\rm tot} = N_1 + N_2$ to use instead.  This amounts to moving the "slider" in the figure below to the left; the further left you move, the more coarse-grained the macrostates are. It turns out that such order parameters do not always have convenient reweighting expressions.  However, we can define an extrapolation expression for such cases!

<img style="float: center" src="extrap_vs_reweight.png" width=1000px>

Typically, one can employ **both** reweighting and extrapolation. For the systems discussed here, and in the references above, usually 1 rewighting expression is known (usually in chemical potential) while extrapolation expressions can be obtained that allow exploration of other intensive variables; for example, the chemical potentials of a number of other species, or temperature.

# Examples of Efficient Exploration

## Low Temperature Landscapes

Systems with attractive interactions tend to exhibit (ir)reversible aggregration at sufficiently low temperatures. They simply become "stuck" together for an increasing amount of time as temperature decreases.  However, "time" is relative.  Many systems exhibit rearragements and are [ergodic](https://en.wikipedia.org/wiki/Ergodic_hypothesis) on the order of seconds or less, but typical molecular simulations can only reach (in a reasonable amount of wallclock time) up the order of microseconds or less.  Thus, being able to predict the thermodynamic behavior of systems at "low temperature" instead of directly simulating the system is important to understanding their equilibrium thermodynamic properties.  There are simulation tools, including flat-histogram techniques, that naturally allow you to overcome barriers and perform advanced sampling under these "hard" conditions.  However, it is not always easy to know in advance what the order parameters are that you need to bias, i.e., what the "slow" modes are.

In this example from Ref. 1, a simulation was performed on a simple, single component fluid (a [square well](https://www.nist.gov/mml/csd/chemical-informatics-research-group/square-well-fluid-properties)) at a supercritical temperature (black curve).  We can compare simulations explicitly performed at lower temperatures (solid lines) to those extrapolated from this single supercritical one (dashed lines).  Clearly, there is excellent agreement!  As temperature decreases, phase separation occurs creating 2 peaks separated by a local minima in ${\rm ln}\Pi(N)$.  The primary error occurs in the middle of the curve. This is in log scale, so average properties based on those states leave the prediction of properties (such as density and pressure) essentially unaffected!  The phase diagram generated by multiple explicit simulations vs. the one  resulting from extrapolation of a single supercritical simulation are almost indistinguishable.

<a href="http://dx.doi.org/10.1063/1.4975331"><img style="float: center" src="square_well.png" width=1000px></a>

In fact, properties that depend on the **shape** of ${\rm ln}\Pi(N)$ tend to be the most accurately predicted properties by extrapolation.  For example, the pressure and composition of coexisting phases can be computed from this.  Coexistence itself is determined by finding the chemical potential(s) where the areas under the 2 peaks of ${\rm ln}\Pi(\vec{N})$ are equal.  In the above example, the ${\rm ln}\Pi(N)$ from high $T$ is extrapolated in $\beta$ to lower $T$ then reweighted in chemical potential to find this coexistence condition.  Consequently, the pressure and density are accurately parameterized in terms of each other because they are linked directly to the shape of the macrostate distribution.  However, the chemical potential that this coexistence is found out may not be as reliable.  

As discussed in Ref. 2, the errors that come from approximate extrapolation tend to get "hidden" in the chemical potential for these grand canonical systems because they control the shape of the curve (recall the "line" being added!).  Simply stated, the observable properties computed from the curve tend to be accurate, but the conditions that generate that curve may not be.  So the pressure-density curve, $P$ = f($\rho$), might be accurate, but the $\mu$ each ($P$, $\rho$) occurs at may not be consistent with the fluid's true equation of state.  From a practical perspective, this may be somewhat inconsequential since accurate parameterization of observables is more important in applied settings.

These extrapolations can be similarly applied to anisotropic sytems (see Ref. 1) and systems with internal degrees of freedom (see Ref. 6).

## Confined Systems

So far we have only discussed bulk systems. However, it is also possible to perform such expansions for confined ones as well.  This usually involves assuming the confining material is an additional component which is fixed, into which the fluid adsorbs.  Internal degrees of freedom, adsorbent flexibility, and special sampling moves can influence the methodology, and Ref. 6 contains more information.  In general though, this extrapolation approach can greatly reduce the cost of screening materials for advantageous properties; for example, selective adsorption of one species from a multicomponent mixture.  Below are isotherms of methane asorbing in MOF-950 taken from Ref. 6; open circles are from individual simulations done verify these predictions.  The solid lines are temperature extrapolations of the macrostate distribution (obtained using a slightly different approach than what has been discussed so far) performed at $T=270$ K (purple line). Clearly, the extrapolation is able to predict adsorption isotherms across a range of industrially relevant conditions. Similar performance can easily be achieved for other small molecules of interest.

<a href="https://doi.org/10.1021/acs.jctc.8b00534"><img style="float: center" src="adsorb.png" width=1000px></a>

## Multicomponent Mixtures

For multicomponent mixtures it is often easier to work with $N_{\rm tot}$ as the order parameter.  In this case, a little algebra lets you rearrange the partition function to look like:

$
\Xi(\beta, V, \vec{\mu}) = \sum_{N_{\rm tot}} {\rm exp} \left( \beta \mu_1 N_{\rm tot} \right) \left[ \sum_{N_{2}} {\rm exp} \left( \beta \Delta \mu_2 N_2 \right) \dots \sum_{N_k} {\rm exp} \left( \beta \Delta \mu_k N_{k} \right)  Q \left( \beta, V, \vec{N} \right) \right]
$

$
\Xi(\beta, V, \vec{\mu}) = \sum_{N_{\rm tot}} {\rm exp} \left( \beta \mu_1 N_{\rm tot} \right) \Upsilon(\beta, V, N_{\rm tot}, \Delta \vec{\mu})
$ 

The probability of a macrostate can be expressed as:

${\rm ln}\Pi(N_{\rm tot}) = \beta \mu_1N_{\rm tot} + {\rm ln}\Upsilon - {\rm ln} \Xi$,

where $\Upsilon$ is the isochoric semigrand partition function (a constant) and $\Delta \mu_i = \mu_i - \mu_1$. In practice a simulation is then performed at fixed $\Delta \vec{\mu}$ and $\beta$.  The useful reweighting expression is:

${\rm ln} \Pi(N_{\rm tot}; \mu_1) = {\rm ln} \Pi(N_{\rm tot}; \mu_1^0) + \beta(\mu_1 - \mu_1^0)N_{\rm tot}$.

The expression means that at fixed $\Delta \vec{\mu}$ we can obtain "exact" results for any $\mu_1$; this does not tell us how the macrostate distribution changes as a function of $\beta$ or $\Delta \vec{\mu}$, which is where extrapolation comes in.  A first order derivative in terms of $\beta$ looks like:

$\frac{\partial {\rm ln} \Pi(N_{\rm tot})}{\partial \beta} \sim \mu_1 N_{\rm tot} + \sum_{i=2}^k \Delta \mu_i \langle N_i \rangle - \langle U \rangle.$

More details and higher order terms can be found in Refs. 4 and 7.  In practice, at a given $\beta$ you perform several simulations at different $\Delta \vec{\mu}$; reweighting then lets you move along the blue lines shown below (there are 5 simulations depicted), whereas extrapolation lets you fill in the space in between.  You can also extrapolate in $\beta$ to build such a continuous surface at other temperatures.

<a href="http://dx.doi.org/10.1063/1.5006906"><img style="float: center" src="multi_extrap.png" width=1000px></a>

A simple weighting function can be defined to optimize thermodynamic consistency, that is, to numerically agree with the Gibbs-Duhem equation. Below is an example for an azeotropic system where 5 simulations were performed.  On the left, only the "middle" one (chosen to fall exactly on the azeotrope) was extrapolated; points correspond to simulations performed at the given conditions.  It clearly is reasonable across a range of temperatures, but only fairly close to the azeotrope in terms of mole fraction, $x_1$.  However, accurate Pxy phase diagrams can be obtained by combining extrapolations of 5 different simulations performed over a range of different conditions across the mole fraction space. 

<a href="http://dx.doi.org/10.1063/1.5006906"><img style="float: center" src="pxy.png" width=1000px></a>

# Optimal Combination

An innovation due to Monroe (see Ref. 8) is to combine simulations using an interpolating polynomial instead, which is related to the optimal combination of cumulant expansions.  <a href="https://doi.org/10.1063/5.0014282"><img style="float: right" src="opt_comb.png" width=400px></a> In the context of estimating free energy differences, this is actually identical to thermodynamic integration under certain assumptions.  Given two macrostate distributions, one can exactly fit a polynomial up to order up to order 2$k$+1 if we expand each observable up to order $k$. Note that the $k^{th}$ derivative of a macrostate distribution involves $k-1$ derivatives of observables, e.g., the first derivative of ${\rm ln}\Pi(N)$ involves $\langle N \rangle$ while the second derivative involves $\frac{\partial \langle N \rangle}{\partial \beta}$ and so on. 
The coefficients in this polynomial, $\vec{c} = (c_0, c_1, \dots, c_{2k+1})$, are found by solving the system of linear equations given at the right.

# Extrapolating Structural Observables

So far we have described how the macrostate distribution can be extrapolated, enabling the computation of thermodynamic observables like pressure, density and mole faction.  However, the technique is really more general than that. At its core, like histogram reweighting, thermodynamic extrapolation is just a way to predict the probability that a macrostate of the system occurs at equilibrium. If we know some property of that macrostate, and each is well sampled, then the system's average property is just the weighted sum of those macrostates where the weight is the (normalized) macrostate probability.

In fact, structural properties of these systems are also average properties. Consider that the radial distribution function, $g(r)$, is really just a histogram of how often certain pairwise distances, $r$, are observed in a system at a given temperature:

$
g(r; \beta) = \frac{h(r; \beta) / (N_c N) }{\left[ \left( \frac{N-1}{V} \right) V_{\rm bin}(r) \right] },
$

where $N_c$ is the number of configurations measured, $N$ is the number of particles in the system, and $V_{\rm bin}(r)$ is the volume of the shell centered ar $r$.  The denominator is fixed for a canonical system, while the numerator can be expanded in a Taylor series:

$
g(r; \beta) = \frac{h(r; \beta^0) + \sum_{k=1}^M \frac{(\Delta \beta)^k}{k!} \frac{\partial^k h(r; \beta)}{\partial \beta^k } }{(N_c N) \left[ \left( \frac{N-1}{V} \right) V_{\rm bin}(r) \right] }.
$

Similarly, this can expanded up to some order, $M$, in other intensive variables to work in other ensembles, such as the grand canonical ensemble.  These partial derivatives can also be expressed in terms of products of fluctating extensive variables and the observable, in this case, the value at a given histogram bin. You can couple this with extrapolation of thermodynamic properties as previously described to compute the thermodynamic properties along a binodal curve, for example (see below).

<a href="https://doi.org/10.1080/08927022.2020.1747617"><img style="float: center" src="rdf_extrap.png" width=1000px></a>

More rigorous details are discussed in Ref. 5. It is also possible to extrapolate observables like a polymer's [radius of gyration](https://en.wikipedia.org/wiki/Radius_of_gyration) or the cluster size distribution of a self-assembling system; this allows you to estimate features like the critical micelle concentration (CMC) of amphiphilic systems at low temperatures quite accurately.

# Further Extensions

Even further innovation is due to Hatch et al. (Ref. 3) who extended the concept of thermodynamic extrapolation to importance-sampling methods such as Mayer-sampling Monte Carlo simulations.  These simulations are often used to compute virial coefficients.  This also enables one to perform alchemical transformations by extrapolating model parameter values, such as the point charge in SPC/E water. Thus, extrapolation can help in optimizing model parameters to create force fields by fitting to some experimentally oberved properties.  Furthermore, the plethora of data that results from these extrapolations enables the use of data-intensive regressors, like neural networks, which would otherwise require more data to fit than can be generated with a reasonable amount of computational expense and time.

<a href="https://doi.org/10.1080/08927022.2020.1747617"><img style="float: center" src="nn.png" width=1000px></a>

# Conclusions

Thermodynamic extrapolation is an approximate technique and has very old roots.  However, its simplicity belies its power.  Here, a "network" of Taylor series work in concert to provide a remarkably accurate property predictor. Once a macrostate distribution is known at one condition it is possible to estimate it over a broad range of conditions.  The range over which this extrapolation is accurate has proven to be surprisingly large for many systems.  This enables the calculation of thermodynamic and structural properties with reduced computational effort.  The necessary average quantities, like particle number, can even be reconstructed using log files from older simulations, giving them new life and enabling consistency checks to be performed. Thermodynamic extrapolation has also proven useful in facilitating other types of advanced sampling simulations, in providing sufficient information to enable data-intensive analysis and regression, and can be used to perform high-throughput screening.



