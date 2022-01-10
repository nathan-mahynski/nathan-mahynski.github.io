---
title: "Structure Directing Agents"
excerpt: "Programming the Machine and not the Part."
header:
  image: 
  teaser: assets/images/unsplash-gallery-image-1-th.jpg
gallery:
  - url: https://doi.org/10.1038/ncomms5472
    image_path: research/structure_directing_agents/polymer_sda.png
    alt: "Polymer SDA"
  - url: /assets/images/unsplash-gallery-image-1.jpg
    image_path: research/structure_directing_agents/sda_ring.png
    alt: "SDA Rings"
classes:
  - wide
tags:
  - symmetry
  - structure directing agents
  - colloids
  - crystals
  - orbifolds
---

{% include toc icon="gears" title="Table of Contents" %}

{% include gallery caption="" %}

# tl;dr

Structure directing agents (SDA) are typically small molecules added during the synthesis of materials which control their growth; for example, by templating channels and controlling aspect ratios in zeolites. However, SDAs can more broadly be envisioned as any additive which tunes, or even fully programs, the assembly of some structure.  Under this definition, [chaperones](https://en.wikipedia.org/wiki/Chaperone_(protein)) could be considered SDAs for protein folding. 

In this body of work we reconsider the role of additives in colloidal mixtures which serve such a role in tuning their (self-)assembly (text available upon [request](mailto:nathan.mahynski@gmail.com) if you cannot access them):

1. ["Stabilizing colloidal crystals by leveraging void distributions," N. A. Mahynski, A. Z. Panagiotopoulos, D. Meng, S. K. Kumar, Nature Commun. <b>5</b>, 4472 (2014).](https://doi.org/10.1038/ncomms5472)
2. ["Relative stability of the fcc and hcp polymorphs with interacting polymers," N. A. Mahynski, S. K.Kumar, A. Z. Panagiotopoulos, Soft Matter <b>11</b>, 280–289 (2015).](https://doi.org/10.1039/c4sm02191f)
3. ["Tuning polymer architecture to manipulate the relative stability of different colloid crystal morphologies," N. A. Mahynski, S. K. Kumar, A. Z. Panagiotopoulos, Soft Matter <b>11</b>, 5146–5153 (2015).](https://doi.org/10.1039/c5sm00631g)
4. ["Bottom-up with a twist:  a new approach for colloidal crystal assembly," N. A. Mahynski, L. Rovigatti, C. N. Likos, A. Z. Panagiotopoulos, ACS Nano <b>10</b>, 5459–5467 (2016).](https://doi.org/10.1021/acsnano.6b01854)
5. ["Entropic  control  over  nanoscale  colloidal  crystals," N. A. Mahynski, Mol. Phys. <b>114</b>,  2586–2596 (2016).](https://doi.org/10.1080/00268976.2016.1203467)
6. ["Reentrant equilibrium disordering in nanoparticle–polymer mixtures," D. Meng, S. K. Kumar, G. S. Grest, N. A. Mahynski, A. Z. Panagiotopoulos, npj Computational Materials <b>3</b>, 3 (2017).](https://doi.org/10.1038/s41524-016-0005-8)
7. ["Void-based assembly of colloidal crystals," N. A. Mahynski, L. Rovigatti, C. N. Likos, A. Z. Pana-giotopoulos, G.I.T. Laboratory Journal Europe <b>21</b>, 32–34 (2017).](http://bit.ly/GLJ-Mahynski)
8. ["Symmetry-derived structure directing agents for two-dimensional crystals of arbitrary colloids," N. A. Mahynski, V. K. Shen, Soft Matter <b>17</b>, 7853-7866 (2021).](https://dx.doi.org/10.1039/D1SM00875G)

Refs. 5 and 7 are essentially reviews of most of this work, which the interested reader should refer to.

[Colloidal materials](https://en.wikipedia.org/wiki/Colloid) generally derive their utility from how their constituents are arranged, rather than from the chemistry of what they are made of, so SDAs are particular useful for engineering these systems.  Generally, colloidal systems rely on top-down or bottom-up assembly strategies.  Top-down approaches, such as lithography, are often limited "below" by resolution limits and/or "above" by scalability concerns.  Bottom-up strategies, like tuning the surface chemistry or shape of the colloid, require that the colloid itself be mutable.  But what if we wish to make a material out of proteins, antibodies, or quantum dots?  The chemistry of these building blocks can impart (desired) functionality in the final material, but not necessarily if the modifications required to achieve a desired self-assembly into the desired target are too severe.  Moreover, practical issues involving synthesis also limit experimental realization.

SDAs represent a third assembly paradigm.  The SDA is essentially an assembly machine that we program instead of the parts themselves. 

# Polymers As Structure Directing Agents

## The Depletion Force

The ["depletion force"](https://en.wikipedia.org/wiki/Depletion_force) is an effective attraction that arises in colloidal solutions when a (generally smaller) "depletant" is excluded between two approaching surfaces.  For example, if 2 hard spheres are diffusing freely in a solvent and a small polymer is added, then when the 2 spheres approach each other they begin to exclude the polymer from existing between them.  This creates an osmotic "vacuum" which pulls the surfaces together.  More generally, this is regarded as an entropic effect where the polymer gains translational entropy by the colloids aggregating.  This is because when the colloids approach, the volume around their surface that excludes the polymer becomes shared, thus "releasing" some volume into the surrounding medium for the polymer to exist in. 

The first observation of depletion is generally attributed to reports on the "creaming" of rubber latex particles upon the addition of polymers. Depletion plays a role in organizing materials in crowded environments, such as biological cells, and can be used to rationally design interactions between colloidal particles of different shapes. Askaura and Oosawa were the first to produce an explanation of the phenomenon in the 1950s and provided a standard expression for effective interaction between colloids following the first explanation:

$W_{\rm dep}(h) = -PV_{\rm ov}(h)$, 

where $P$ is the osmotic pressure of the polymer depletant and $V_{\rm ov}$ is the overlap (lense) volume which can be computed from [intersecting spheres](https://mathworld.wolfram.com/Sphere-SphereIntersection.html).  <img style="float: right" src="depletion.png" width=400px> The range of depletion is given by $\delta$.  For dilute polymer solutions this is typically its [radius of gyration](https://en.wikipedia.org/wiki/Radius_of_gyration), but depending on the system there can be more complicated expressions. This expression is valid when the surface-surface distance, $h$, is such that $0 \le h \le 2\delta$; formally, $W_{\rm dep}(h) = \infty$ if $h < 0$, and $W_{\rm dep}(h) = 0$ if $h > 2\delta$. Typically, $P$, is approximated assuming the polymer solution is ideal so that $P = n_b k_{\rm B}T$, where $T$ is the temperature, $k_{\rm B}$ is Boltzmann's constant, and $n_b$ is the number density of polymers.  Thus, we have:

$\frac{U_{\rm AO}(h)}{k_{\rm B}T} = -n_b \frac{\pi}{6} (2\delta-h)^2(3R+2\delta+h/2)$,

where the (spherical) colloid has radius $R$.  By increasing the polymer density can induce fluid phase separation, and even crystallization of the colloids.

Some statisical mechanics help explain the entropy interpretation of depletion. Consider a system with volume, $V$, of $N$ colloids in equilibrium with a reservoir of ideal polymer chains with [fugacity](https://en.wikipedia.org/wiki/Fugacity), $z ={\rm exp}(\mu_{\rm poly})$. For a given microstate of the system in which the colloid positions are defined by $\vec{r}^N$, the partition function of a single polymer chain is given by the total number of accessible conformations, $\Omega(\vec{r}^N; V)$. Since the polymers are ideal (non-interacting) the canonical partition function for $M$ polymers is

$Q = \frac{\left[\Omega(\vec{r}^N; V)\right]^M}{M!}$.

We can write the semigrand partition function for the entire system as:

$\Xi(N, z, V) = \int_V^{} {\rm exp} \left( -U_{\rm HS}(\vec{r}^N) \right) \times \sum_{M=0}^{\infty} {\rm exp} \left( M \mu_{\rm poly} \right) Q d{\rm \vec{r}}^N$.

Recalling the [Maclaurin series expansion](https://en.wikipedia.org/wiki/Taylor_series) of the exponential function, we may write down:

$\Xi(N, z, V) = \int_V^{} {\rm exp} \left( -U_{\rm HS}(\vec{r}^N) + z\Omega(\vec{r}^N; V) \right) ~d{\rm \vec{r}}^N $.

This means $\Xi(N, z, V)$ can also be interpreted as the canonical partition function for
a one-component system of colloids where the polymer fugacity sets the effective attraction strength:

$ U_{\rm eff}(\vec{r}^N) = U_{\rm HS}(\vec{r}^N) - z\Omega(\vec{r}^N; V)$.

This is a general expression which clearly articulates the connection to a polymer's accessible configurations, $\Omega(\vec{r}^N; V)$, as a function of how the colloids are arranged.  Essentially, all of the polymer information is integrated out.

## The Difference Between FCC and HCP

At high polymer concentration the depletion force drives colloids to aggregate into the tightest packing possible because it releases the most net volume into the bulk for the polymer.  For isotropic spheres of equal size, the highest packing as conjectured by [Kepler](https://en.wikipedia.org/wiki/Kepler_conjecture) and later proven by [Hales](https://en.wikipedia.org/wiki/Thomas_Callister_Hales) is about 74%.  <a href="https://doi.org/10.1038/ncomms5472"><img style="float: right" src="layers.png" width=300px></a>However, this packing may be obtained in two different crystals: the face-centered cubic (FCC) or the hexagonal close-packed (HCP) structure.  Both begin the same way.  Start with a hexagonal packing of the plane then stack a copy on top of that layer so that spheres in the top layer "nest" in cavities formed by those in the lower layer. This will cover half of the "holes" you can see looking from the top down.  The FCC is formed by placing a third layer to cover up the other half which creates 3 uniquely positioned layers ("ABC"); this is repeated to grow the crystal (ABCABCABC...).  If the other half of the holes are not covered we obtain an "AB" pattern instead; repeating this (ABABAB...) leads to the HCP structure.  Of course, random choices for these layers can also be made which result in close-packing (ABCBABCB...), however, they are non-crystalline.

If a polymer drives the crystallization of a system of equally sized colloids, then it would seem that the above packings would be equally suitable.  However, due to slight differences in the vibrational entropy of the colloids, the FCC was found to be marginally more stable than the FCC, though its magnitude was [debated](http://www.nature.com/nature/journal/v388/n6639/abs/388235b0.html).  In the end, the free energy difference was found to be roughly 0.001 $k_{\rm B}T$ per sphere lower in the FCC than the HCP.  Experiments usually produce a mixture of different packings, which led to the conclusion that in principle, the FCC is always more stable than the HCP, but due to kinetic limitations and possibly gravity, the systems tend to form a random close-packed set of layers.

Importantly, this train of thought is premised on understanding the system as an effectively one-component system of attractive colloids, rather than as a two component system of mutually repulsive colloids and polymers. This follows the AO model of depletion; however, as it turns out, assumptions in the AO model can lead to a breakdown of this conclusion!

## The Details of the Polymer Matters

In fact, when the polymer is sufficiently large, the HCP can be stabilized over the FCC. The reason for this has to do with the void structure of the two competing crystals. <a href="https://doi.org/10.1039/c4sm02191f"><img style="float: right" src="fcc_hcp.png" width=400px></a> The interstices of both polymorphs can be described as forming either tetrahedral voids (TV) or octahedral voids (OV); these shapes can be seen by connecting all neighboring colloids into a platonic solid around the center of each void.  The FCC and HCP have an identical number of voids per colloid in each crystal (in a ratio of 2:1, TV:OV), however, because of the different stacking patterns the voids also adopt a different pattern.  

In the FCC, each OV is surrounded by TVs and vice versa.  In the HCP, the OVs and TVs are arranged in "columns" where OVs stack on top of other OVs, but are surrounded by similar "stacks" of TVs.  When depletion-driven (first order) phase separation occurs in a colloid-polymer mixture, the two components asymmetrically distribute themselves. In crystallization, the solid phase contains mostly colloids with a few polymers, and the other colloidal "gas" phase is the reverse of this.  Previously, the critical assumption was that the small amount of polymer in the crystal does not matter; since the AO model turns the system into an effectively one-component system anyway it is easy to see why this accepted as reasonable.

<a href="https://doi.org/10.1080/00268976.2016.1203467"><img style="float: right" src="relative.png" width=1000px></a>

However, we found that the small amount of polymer inside the crystal does make a significant difference.  This is because it samples the void space and its free energy is directly related the structure of the voids.  The OVs are much larger and have roughly 6 times the available volume as TVs.  So when a polymer partitions into the colloidal crystal, it entropically will prefer to exist in an OV.  When the polymer is small, it can find OVs easily and they do not "see" the neighborhood around the OV very much.  As a linear polymer gets longer, however, it eventually cannot fit into a single void and must spread out to a neighboring void.  Since the HCP has "stacks" of OVs it can allow these polymers to spread out in these OV "channels" with much more accessible volume than in the FCC crystal; in the FCC, each OV is surrounded by TVs, so a polymer must start to exist in one OV and one TV.  This essentially compresses the polymer, raising its free energy enough to overcome the already small free energy difference between the polymorphs.  Thus, there is a length dependent relative stability of the two crystals.  For very short chains, the FCC is still the most stable, per previous arguments.  As the chain grows longer, the HCP can be stabilized instead.  Increasing the length can lead to oscillations and even potentially destabilize close-packing altogether (see Ref. 6)!

This idea can be generalized. A linear polymer of the right length can be made to stabilize the HCP because a linear chain is geometrically more compatible with linear void channels than a "spherically" symmetric distribution of OVs and TVs found in FCC. In fact, a more "spherically" symmetric polymer can do the opposite.  <a href="https://doi.org/10.1039/c5sm00631g"><img style="float: right" src="star.png" width=500px></a> In Refs. 3 and 4 we explored the use of star polymers to stabilize the FCC over the HCP, which works for these qualitative reasons.  This even works for lower density crystals (diamond-like "tetrastacks") which have hexagonal and cubic polymorphs!  We have also explored the case when the polymer begins to adsorb on the polymer in Ref. 2 - in this case, the smaller TVs tend to provide higher energy environments than the OVs (more surface area to volume in the void which is favorable in this case).  Remarkably, this means the TV void "stacks" now stabilize the HCP over the FCC, once again.

Overall, we conclude that the polymer's topology and energetic interactions can actually be used to rationally tune the relative stability of competing solid polymorphs. No programming of the colloids has been performed.  However, the results discussed so far apply most to the case where we have just 2 main competing structures, and when their voids have a characteristic difference that can be easily exploited by a complementary polymer (lock-and-key logic). The open question is: how can this sort of SDA-driven assembly paradigm be generalized?

# Generalizing the Structure of an SDA

The definition of an SDA envisioned at the outset is essentially that of a "molecular engineer" who knows (1) what building blocks they will receive and (2) has all the information they need to assemble them in a certain way.  There should be no information in, or dependence on, the building block.  One way to envision a generalized object that can perform this operation is as a "box" that can envelope some cargo, and then rely on the "programming" that is a function of the box's exterior to organize that cargo.

It turns out there is a remarkable connection between this very idea and crystallographic symmetry! Crystals are composed of fundamental domains, or asymmetric units, which are the smallest "piece" of the crystal that is duplicated in certain ways to create the pattern.  In two Euclidean dimensions (2D), each symmetry (aka ["wallpaper"](https://en.wikipedia.org/wiki/Wallpaper_group)) group can be described by a single, unique [orbifold](/notes/orbifolds). This surface may be "cut" open to find all possible isohedral tilings of the plane, which is a finite number in 2D (93 in total).

These tilings have unique boundary patterns that define the necessary operations to copy the FD around the plane to make a pattern with a given symmetry group.  This means isohedral tilings can be interpreted as ring-like SDAs which envelop some cargo, and using only the information around its perimeter, be programmed to self-assemble the cargo into a given crystallographic symmetry!

## Isohedral Tiles are SDAs for Symmetry

Orbifolds describe symmetry
Cut open orbifolds to get IH tiles
Tiles = core (colloid) + boundary.
Boundary = derivable!

## 2D Math in a 3D World

We need to be careful - some practical considerations

Soft matter paper

# Conclusions

Q: from G&S - how can you create a tile that can carry any pattern?  Only answer this question partially, a more complete answer is subject of ongoing research

Future work in 3D
Other "lego blocks"

