---
title: "Counting Crystals in Two Dimensions"
excerpt: "Leveraging symmetry to efficiently enumerate important patterns in 2D."
header:
  image: 
  teaser: /assets/img/wallpaper_groups.png
gallery:
  - url: http://dx.doi.org/10.1038/s41467-019-10031-4
    image_path: /assets/img/nat_comm_2019.png
    alt: "Using symmetry to elucidate the importance of stoichiometry in colloidal crystal assembly."
  - url: https://dx.doi.org/10.1021/acs.jpca.0c00846
    image_path: /assets/img/crystal_bc_ex.png
    alt: "Symmetry-based crystal structure enumeration in two dimensions."
  - url: http://dx.doi.org/10.1039/C9SM02426C
    image_path: /assets/img/soft_matter_cover_2020.png
    alt: "Grand canonical inverse design of multicomponent colloidal crystals."
classes:
  - wide
tags:
  - symmetry
  - orbifolds
  - crystals
  - colloids
  - two-dimensions
  - machine learning
---

{% include toc icon="gears" title="Table of Contents" %}

{% include gallery caption="" %}

# tl;dr

This post is accompaniment to several manuscripts:

1. ["Using symmetry to elucidate the importance of stoichiometry in colloidal crystal assembly," N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, Nature Commun. <b>10</b> 2028 (2019).](https://dx.doi.org/10.1038/s41467-019-10031-4) - Also see the ``Behind the paper'' [post](https://chemistrycommunity.nature.com/users/255993-nathan-mahynski/posts/48551-using-symmetry-to-elucidate-the-importance-of-stoichiometry-in-colloidal-crystal-assembly).
2. ["Symmetry-based crystal structure enumeration in two dimensions," E. Pretti, V. K. Shen, J. Mittal, N. A. Mahynski, J. Phys. Chem. A <b>124</b>, 3276–3285 (2020).](https://doi.org/10.1021/acs.jpca.0c00846)
3. ["Python Analysis of Colloidal Crystal Structures (PACCS)," E. Pretti, N. A. Mahynski, https://github.com/usnistgov/PACCS (2020).](https://github.com/usnistgov/PACCS) 
4. ["Grand canonical inverse design of multicomponent colloidal crystals," N. A. Mahynski, R. Mao, E. Pretti, V. K. Shen, J. Mittal, Soft Matter <b>16</b>, 3187–3194 (2020).](https://dx.doi.org/10.1039/C9SM02426C)

In this series of papers we reconsidered the task of creating planar interfaces that require a unique surface pattern to be functional. In a system that self-assembles, such as an appropriate colloidal suspension, there are many possible configurations, but the one that we might hope to form spontaneously is the one with the lowest free energy.  However, a ensemble of possibilities needs to be generated in order to screen them and establish which is the most thermodynamically stable one.  To this end, we developed an algorithm, inspired by symmetry and the concept of [orbifolds](/notes/orbifolds), that can enumerate point patterns as quickly as possible in two Euclidean dimensions.  This enables the generation of different candidate patterns that are particularly relevant for atomic crystals made of atoms that are similar in size, or out of colloidal crystals made from similarly sized spherical constituents.  This method enumerates structures according to rules of symmetry; compared to random searching, this enables the enumeration of "important" candidate structures is trillions of times faster, reducing a calculation that would otherwise take the age of the universe to complete (13.8e9 years) to a mere 15 minutes.  This code is available in Ref. 3 above, and is further described in Ref. 2.

Many of the graphics below have been presented at scientific conferences including:

* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Physical Society March Meeting, Denver, CO USA (03/2021).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, GRC: Colloidal, Macromolecular & Polyelectrolyte Solutions, Ventura, CA USA (02/2020). 
* ``Grand canonical inverse design of multicomponent colloidal assemblies,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Institute of Chemical Engineers Annual Meeting, Orlando, FL USA (11/2019).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Institute of Chemical Engineers Annual Meeting, Pittsburgh, PA USA (11/2018).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, Foundations of Molecular Modeling and Simulation, Delavan, WI USA (07/2018). 

Please cite appropriately if you find this material to be helpful.

# 2D Materials from Programmable Matter

Two dimensional materials play an important role in a number of scientific fields and engineering applications.  For example, in creating responsive surface coatings, membranes with controllable porosity, or placing ligands to act as selective receptors for chemical separation or sensing tasks.  In general, these surfaces are built from molecular, or macromolecular, units and so must be programmed to self-assemble rather than rely on an external agent or force to organize them if these materials are to be produced in large quantities.  This chemical programming can potentially be achieved in a number of ways.  If the constituents are colloidal (nano)particles, they may be functionalized with various ligands in isotropic or anisotropic ways, their shape may be tuned (e.g., spherical or non-spherical), or external directing agents such as additives or fields (e.g., electrical or flow/shear) may be applied.  In the image below, the $\lambda$ values represent differently programmed interactions between the different pairs of colored spheres (blue-blue, blue-green, green-green), which spontaneously self-assemble into different structures based on (1) these values and (2) the ratio of the number of green to blue particles.  

<a href="https://doi.org/10.1038/s41467-019-10031-4"><img style="float: center" src="mahynski_10_2019.png" width=1000px></a>

DNA has proven to be a powerful ligand whose specific, progammable interactions based on sequence can be leveraged to create exquisite nanoscale devices and structures, sometimes collectively referred to as "DNA nanotechnology."  While DNA is not required, widespread interest has driven a great deal of research into it.  A typical design approach is to take a spherical nanoparticle and graft a certain type (sequence) of single-stranded DNA, sometimes with an initial double stranded region for stability, to its surface.  The number of different sequences and their arrangement on the surface (how the surface is "functionalized") control how the nanoparticle interacts with other nanoparticles, which may (or may not) be grafted with complementary strands that interact favorably creating an attraction between the two.  It is also possible to simply use the strands directly to create "origami" structures or other frameworks via these specific interactions, as illustrated in the figure below from [this paper](https://dx.doi.org/10.1021/acs.chemmater.6b02546) by Chandrasekaran and Levchenko.

<a href="https://dx.doi.org/10.1021/acs.chemmater.6b02546"><img style="float: center" src="chandrasekaran_28_2016.png" width=1000px></a>

While it is sometimes intuitive to understand how to functionalize a nanoparticle to obtain a desired structure via self-assembly, often it is very difficult to do so.  The tactic of "inverse design" is where one starts from a desired structure, such as one in the first image above, and works backwards to derive the interactions or funcationality that give this result (i.e., the $\lambda$ values).  The more conventional "forward design" is the Edisonian "trial and error" methodology in which interactions are tested and tweaked to seek out the desired result in a generally iterative fashion.  The latter is often how experiments proceed, however, via the application of theory and simulations one can proceed with the former.  Inverse design methods have proven incredibly useful and been used to reverse engineer many structures.  Generally this involves performing some sort of molecular simulation, such as molecular dynamics or Monte Carlo, measuring properties of the system to establish (1) the properties of the current configuration and (2) how we expect they will change in response to modifications to, e.g., functionality.  The details vary, but step (2) informs you how to update your current state to get closer to your goal; this is iterated until a result that is sufficiently close is achieved.

A couple of issues tend to arise, in practice, when performing inverse design.  First, is insufficient sampling.  This may mean different things within the context of the specific approach being used, but generally the issue is that you have not explored enough candidate parameter space to choose the best point from.  This can result from simulations not being run long enough, databases being too sparse, or a poor choice of a basis set of candidates.  Even then, inverse design can often produce physically unrealizable results. For example, a shape of nanoparticle that we cannot synthesize or perhaps a pair interaction potential that we do not understand how to create via known chemistry.  It is possible to reduce the search space to physically realizable realms, which is one way to help remedy this problem; unfortunately, this may mean that the space you confine yourself to does not contain a solution to creating the structure you want.  

Another solution is to try to use many "simple" components instead of a single "complex" one; if we have designed a system to assemble into a strange configuration using only one building block, it is often the case that the interaction between those blocks that causes this will be equally "strange".  For example, perhaps the potential contains unusual oscillations or has discontinuities, unlike a more "conventional" [Lennard-Jones potential](https://en.wikipedia.org/wiki/Lennard-Jones_potential) which is considered more tractable.  Instead, we could try to achieve this [complexity through diversity](https://doi.org/10.1039/C9SM02426C) by using multiple simpler "lego"-like building blocks that have interactions more akin to Lennard-Jones.  The issue that arises in that approach originates with [Gibbs Phase Rule](https://en.wikipedia.org/wiki/Phase_rule). The number of thermodynamically permissible phases grows linearly with the number of components.  In practice, we want to engineer the functionality of the nanoparticle such that at the conditions of interest, e.g., room temperature and atmospheric pressure, the only stable phase is the one we are targeting.  In principle, it is possible to have more than one which are stable and will coexist, which is usually not a desirable scenario.  As the number of components, so too does the number of allowable phases increasing the chance that we will struggle to engineer the system to exist in only one of them. As an aside, phase separation is also a problem in molecular simulations since there are issues with nucleation due to the finite size and length of simulations making it hard to know if the system would phase separate if run for longer or in a larger simulation box; both of which are generally kept as small as possible to make the iterations of inverse design as quick as possible.

# Using Symmetry to Count Configurations

## A Different Perspective on Symmetry

Forward design also suffers from the problem of sparsity.  Even if we look up 1 million different structures, evaluate their free energy using the chosen functionality, and predict the one with the lowest as our expectation of what will assemble, we cannot guarantee that those candidates contained the correct answer to begin with.  This is the first challenge that must be addressed.  Many current approaches follow along these lines by doing simulations to generate a set of plausible candidates, looking up structures in known databases, or doing some sort of other machine learning/data-driven approach to generate this list.  In the end, none of these approaches are very systematic and could be biased by data availability (e.g., perhaps all structures in a database only have a certain set of symmetries) or by the method (simulation) itself.

<img style="float: right" src="comb_exp.png" width=500px>

It is possible to enumerate structures more systematically, however, this is almost never done in practice due to the "combinatorial explosion" of possibilities. If you discretize an area into $N^2$ pixels (or voxels in 3D) and try to fill half of them ($m=N^2/2$, i.e., a 1:1 ratio of 2 components), as shown on the right, the number of possible arrangements exceeds Avogadro's number (6.022e23) by the time the lattice is $8 \times 8$.  If you counted these structures at 1 millions configurations per second that would take 6.022e17 seconds, or roughly the age of the universe (13.8e9 years = 4.35e17 seconds).

This number of seconds is also roughly equal to the number of arrangements of a $3\times3\times3$ Rubik's cube (4.3e19), so the task is within a few orders of magnitude of trying to count all of those configurations!  However, clearly Rubik's cubes can be solved much more quickly if you know the right pattern and rules to achieve your target arrangement.  So can we also be clever about how we count or arrange particles on our 2D lattice to achieve a similar speed up?

The answer is: yes. If we restrict ourselves to consider periodic crystals, then we are essentially looking for all ways to organize particles in all possible symmetries. In two Euclidean dimensions there are 4 [isometries](https://en.wikipedia.org/wiki/Isometry) of the plane: translation, rotation, reflection, and glide reflection (reflection about a line, followed by translation along it).  All unique combinations of these isometries represent symmetry groups, sometimes called ["wallpaper groups"](https://en.wikipedia.org/wiki/Wallpaper_group) in 2D.  There are only 17 wallpaper groups in 2D, which makes considering each of them individually tractable.  While there are many ways to represent wallpaper groups and crystallographic symmetry, the conventional approach is to look at the [fundamental domain](https://en.wikipedia.org/wiki/Fundamental_domain) (FD) or [asymmetric unit](https://en.wikipedia.org/wiki/Crystal_structure) of the crystal and use matrix operations to describe how symmetry makes copies of it.  The FD is the smallest, generally simply connected, region of space that encloses no symmetry elements; you can view it as the smallest piece of the puzzle that the crystal is going to be formed by tesselation.  

This focus on how operations move the "interior" of the FD around stands in contrast to the topological description of symmetry which instead focuses on the boundary.  By "interior" I mean that the mathematical description uses matrices ("generators") like those found on the [Bilbao crystallographic server](https://www.cryst.ehu.es/#planetop) to operate on the FD. However, when these symmetry operations occur, each FD touches copies of itself. These matches indicate symmetrically equivalent points.  One consequence of this is that the boundaries of the FD are not all independent; in other words, if you view the space like a domain of numbers, e.g., [A, B), where we indicate the inclusivity of a bound by the chosen bracket symbol ("[" vs. ")") then there should be some equivalent to indicate the bounds of the FD.  For example, if you repeat the p1 boundary conditions often used in molecular simulation (translate a rectangle up/down and left/right to repeat) then we should formally only include either the top or bottom in our FD, but not both.  Explictly, the domain is $\mathcal{D} = \left[ x \in [0, L_x), y \in [0, L_y) \right]$.  However, explicit consideration of this has not appeared [until recently](https://doi.org/10.1107/S0108767311007008) in computational packages used for crystallography despite this well-known shortcoming.

<img style="float: center" src="orbifolding.png" width=1000px>

In contrast to generators (more "barycentric" in philosophy), if one focuses only on those equivalent boundaries, one arrives at an equivalent description of symmetry but which is "boundary-centric"; if you fold the FD to match all equivalent points you obtain a surface called a crystallographic orbifold.  More information on the history and a more formal description of orbifolds can be found [here](/notes/orbifolds).  For example, in the figure above 3 different wallpaper groups are represented.  p1 results by copying the rectangle left and right, and up and down; this means the top and bottom edge are equivalent, as are the left and right.  Thus, we fold the rectangle to match them, which results in a torus.  The cm group requires that we take the right edge and "flip" it to match the left, producing a Moebius band.  A hybrid of the two results in pg, whose orbifold is a [Klein bottle](https://en.wikipedia.org/wiki/Klein_bottle) which results from "gluing" two Moebius bands together.  <img style="float: right" src="portal1.jpeg" width=350px> Essentially, this means that boundary condition is related to symmetry.  In some sense, this is analogous to the popular video game puzzle series ["Portal"](https://en.wikipedia.org/wiki/Portal_(video_game)) which is often cited as ["one of the greatest video games ever made"](https://en.wikipedia.org/wiki/List_of_video_games_considered_the_best).

John H. Conway proved that there is a unique orbifold for each wallpaper group, and Olaf Delgado-Friedrichs and Daniel Huson later showed that all possible FDs for a group can be derived by "cutting" the group's orbifold open so that it falls flat on the plane it represents.  As this suggests, there are more than one possible FD for each group, sometimes instead called isohedral tiles.  If we choose our FDs carefully, we can come up with a simple way to generate crystal patterns in an "even-handed" way.  This is the essence of the algorithm we devised in Refs. [1-3] above.  <img style="float: left" src="fd.png" width=400px> As shown in the figure at the left, we elect to create FDs by imagining them as rectangles, described by $L_1, L_2,$ and the angle $\alpha$.  Different groups will have different restrictions on the ratio of the two edges and the value of $\alpha$, but when created as described in Refs. [1-2] all of the nodes created by crossing lines on the boundaries will map exactly to other ones.  This means we can easily create a lattice consistent with the group's symmetry.  Different groups have a different number of FDs per [primitive cell](https://www.doitpoms.ac.uk/tlplib/crystallography3/unit_cell.php), but if we bound the number of nodes, we can create lattices for each group that have similar node densities.  This allows us to compare different lattices "fairly". 

> The general primitive cell in 2D is a parallelogram, but it is not obvious (at least to the author) that the FD should also follow the same prescription, especially given that the [International Tables for Crystallography](https://it.iucr.org/Ab/) use very different shapes to describe the wallpaper groups.  Upon rearrangement, however, they can be shown to be identical to those used in Refs. [1-2].

So what does this have to do with the "rules" used to solve a Rubik's cube and how can it help with the combinatorial explosion problem?

## Wyckoff Positions

By virtue of the fact that the FD is bounded, and therefore does not wholly include points on its edges, and that symmetry implies some points are redundant, different positions along the boundary do not all "count" the same.  All points in the interior of a FD belong to what is known as the "general Wyckoff position"; that is, we may assign those points a stoichiometric factor of "1" since they are entirely encompassed by the domain, whereas those on the edges may have values less than 1.  Those positions on the edges which have values less than 1 are known as "special Wyckoff positions" - note that not all boundary points are "special", but all special positions are on the edge.  In fact, an orbifold is essentially just a [topological space with an embedded set that represents the Wyckoff sites](http://oldwww.iucr.org/iucr-top/comm/ccom/School96/pdf/cj.pdf); again, these are equivalent descriptions, but from different perspectives.  The so-called "orbifolding" process simply bends and folds the FD at the these positions.

Consider the example below. In it, we consider a FD for the p6 (632 in orbifold notation) group where our domain is a triangle, as shown.  The triangle is disretized into lattice points indicated by the light gray lines.  Points on the edge are colored; those which are lighter (right half) are symmetrically equivalent to those on the left which are colored darker.  You can see how this results from the primitive cell shown with the 6 FDs that compose it.  In this case, only 10 indepdenent nodes exist.  Nodes such as the yellow, orange, and green ones would seem to only have "half" of themselves inside the domain; however, they get another "half" from their symmetrically equivalent position for a total factor of 1.  The clear points in the middle also have a factor of 1.  The red and purple sites have factors of 1/3 and 1/2, respectively, since this is the fraction they "sweep out" and "map" to themselves; that is, there are no other points to consider.  Finally, the magenta point sweeps out and angle of $(2\pi)/12$ on the FD, but since there are 2 sites it has a net factor of 1/6.  Note that in this case we have 3 special Wyckoff positions with factors of 1/6, 1/3, and 1/2 which correspond to its orbifold designation of "632".  Generally, these factors are referred to as "site multiplicities" and are normalized by the smallest number so that the point with the lowest multiplicity (magenta) has a multiplicity of 1, while the largest (general positions) have a multiplicity of 6.  More information can be found in Ref. [2].

<img style="float: center" src="sudoku.png" width=1000px> 

We are the faced with the question: "how many ways can I place particles on this grid with a certain stoichiometric ratio?"  In fact, this is essentially just a [constrain satisfaction problem](https://en.wikipedia.org/wiki/Constraint_satisfaction_problem); this general class of problems describes many puzzles such as crosswords, sudoku, and even Rubik's cubes as shown in 2011 [here](https://arxiv.org/abs/1105.1436).  Note that this "CSP" should not be confused with the "crystal structure prediction" problem which has the same acronym and, in fact, is also what we are solving here!

> Also, note that Rubik's cubes were [originally called "magic cubes"](https://arxiv.org/abs/1105.1436).  Any relation to [Conway's "Magic Theorem"](/notes/orbifolds) surrounding orbifolds?!

By using modern computational CSP algorithms we can solve all possible ways to place particles on symmetrically distinct positions so that they result in a crystal with a given stoichiometric ratio.  For example, to achieve a 1:2 ratio of blue to green particles we could place 1 green on a clear node in the middle and 1 blue on the purple node (1/2:1 = 1:2); we could also place 1 green and 2 blue particles on different clear nodes.  In the first example, there are three ways to realize this because there are three different clear nodes to place the green particle.  Similarly, the second example has $_3C_1 = 3!/(1!2!) = 3$ solutions depending on which site is chosen to host the lone blue particle.  Obviously, this "puzzle" is underspecified, unlike the sudoku puzzles you will find newspapers or other airplane-based literature.  The "solution index" in the histogram above essentially refers to which puzzle is being solved and the vertical axis to the number of possible solutions.  The different ways of labelling the points based on their physical location (edges vs. corners vs. face scheme; orange histogram) or their net stoichiometric factor (Wyckoff multiplicity; blue histogram) is what defines "which puzzle" we are solving. Ultimately, it does not matter as the total number of solutions is identical (sum of the histograms).  Each entry corresponds to a different solution and (usually) a different structure!

Importantly, note how stoichiometry is intrinsically linked to specific positions in the crystal.  In the fluid phase of matter stoichometry is also more "fluid" in that it can change more continuously simply because we can add 1 more "blue" particle to a sea of "green" ones and it will simply diffuse about, not being fixed to one specific location.  In crystals, while we can do something similar by adding particles to the general Wyckoff position, the special position is a unique concept specific to crystalline phases of matter.  As a result, [stoichiometry and symmetry are directly linked](https://dx.doi.org/10.1038/s41467-019-10031-4).

## Suppressing the Combinatorial Explosion
 
Let's review the procedure to sample different possible crystal structure candidates (see Ref. [2] for more details), which can be viewed as traversing levels of a tree:

<img style="float: right" src="tree.png" width=400px> 

* [gray] Select an upper bound on the size of the primitive cell so we can "fairly" compare different groups,
* [orange] Choose a wallpaper group,
* Construct the FD grid based on the first 2 choices,
* [blue] Choose a stoichiometry (for any number of components, there is no upper bound),
* [green] Enumerate solutions to the CSP,
* Sample from these solutions.

The last step of "sampling" we will discuss a more later on.  For now, let's reconsider the number of possible solutions to each CSP.  In general, we do not consider p1 to be part of an interesting ensemble of structures.  Because of its toroidal boundary conditions, we have no special Wyckoff positions in p1.  This is the group with the "lowest" symmetry and it is used conventionally to simulate fluid phases of matter with molecular dynamics or Monte Carlo methods; note that this is usually acceptable since the unit or primitive cell of any crystal has p1 symmetry, so we can handle crystals by simply simulating a larger piece of it.  In p1 the FD is equal to the primitive cell, whereas in all other groups the FD is between 1/2 - 1/12 of this size.  For a $N \times N$ grid with p1 symmetry, we just have $(N-1) \times (N-1)$ independent sites with factors of 1 - the number of possibilities explode combinatorial just as before.  This explosion happens for all groups; however, comparing unit cells of the same size, we have 1/2 or less the degrees of freedom (the FD) relative to p1.  This means the number of possibilities is determined by choosing from less that $(N^2/2)$ not $N^2$, which dramatically "delays" the explosion until the cell would be much bigger.

Consider that if we wanted to enumerate all possible configuration of particles on a lattice.  We might begin by writing a loop to place a blue particle in the first position, then have a second loop to place a green one in all the others one at a time.  We could repeat for 2 or more green particles, and for multiple blue particles placed in different locations.  Essentially, this is a giant "for" loop that would create many, many configurations.  However, only a few would have "high" symmetry if we fortuitious placed everythiing "just so". <img style="float: left" src="defect.png" width=300px> Consider the p6 example at the left; if the particle highlighted in yellow was placed anywhere else, say the location indicated by magenta, the symmetry of the crystal would be destroyed.  We essentially "downgrade" from p6 to p1.  Our algorithm is able to directly propose the high symmetry structures, without having to loop over all possibilities then go back and determine which ones have a certain symmetry or not, allowing us to skip the defective one.

In principle, since these high symmetry crystals are found "by luck" in the loop described above, the "gap" between the dashed curve (enumerated by the PACCS code) and the solid line (combinatorial explosion of the equivalent p1 grid) contains all the "low" or "near" symmetry candidates; structures that are characterized by image above where we misplaced a single particle. <img style="float: right" src="configs.png" width=400px> [Prof. David Wale's](https://www.ch.cam.ac.uk/person/dw34) "high symmetry hypothesis" suggests that structures with high symmetry tend to have either very low or very high energies due to structural correlations.  An approximate explanation is that symmetry repeats patterns; if we have a favorable local arrangement of particles (low energy) this is repeated and drives the energy; conversely, if we have a "bad" local arrangement the energy is driven upward because this is continuous repeated throughout the crystal. Structures with less symmetry repeat things less often (or not at all in p1) so you can some local arrangements that are "good" and some that are not, averaging out somewhere in the middle.  

Thus, if we focus on high symmetry candidates only, we can get an ensemble of structures that contain either very good or very bad guesses as to what configuration represents the ground state (most stable state at low temperatures). This is an ansatz, but since this reduces the number of structures we need to consider from somthing like Avogadro's number (order of 1e23) for a unit cell of $N_g^2 = 8 \times 8$ (think crystals with roughly 64 atoms or colloids per unit cell) to roughly 1e9, the improvement is staggering. As previously stated, counting the former at 1e6 structures/sec would take longer than the age of the universe, whereas the latter would take only 15 minutes at the same rate!  Telling which ones are "good" and which are "bad" is a simple matter of evaluating the energy, or relevant free energy.

Importantly, it it still not very reasonable to have a code that  simply "dumps" out 1e9 structure to disk as even modern hard drives can be ovewhelmed by this operation.  The code in Ref. [3] instead uses [python generators](https://wiki.python.org/moin/Generators) which have essentially "memorized" the tree shown earlier for the problem at hand.  In truth, because each next step in the tree is deterministic based on the symmetry and stoichiometry chosen, and the sampling parameter (random number generator state) of the tree the code simply returns the next structure "in line" each time the generator is queried because it knows how to get to next leaf in the sequence.  This essentially provides an implicit mapping between integers up to $N$ when there are $N$ CSP solutions, so that you can sample all these integers in a given order and be returned a structure at each point in the sequence.  This makes the code deployable essentially regardless of the scale of the problem (upper bound on the size of the unit cell).

The reason the combinatorial explosion is suppressed is ultimately three-fold:
* We are working with the fundamental domain (1/2 - 1/12 the size of the primitive cell) instead of the primitive cell.
* We have removed the degrees of freedom from redundant (symmetrically equivalent) boundaries.
* We have exploited the Wyckoff multiplicity explicitly to enumerate structures with targeted stoichiometries.

## Sampling Crystal Structure Ensembles

Our algorithm allows us to directly and quickly generate crystalline candidates from whatever set of wallpaper groups and stoichiometries we may choose.  However, it may not always be desirable to explicitly count all possible structures (e.g., if $N_g$ is large this does become intractable).  Moreover, the generation routine up to this point still produces points on lattices, not in continuum space.  It is generally preferrable to relax the latter in continuum using the chosen set of interactions between colloids or nanoparticles.  This can be done with simple minimization, as well as other methods I will discuss next.  Regardless, because we are generally going to relax these initial candidates later on, we can tolerate an incomplete lattice enumeration as long as the ensemble is reasonable.

Our algorithm enables us to sample the tree in a stochastic fashion.  Following the analogy made earlier that the code essentially maps integers to strutures, we can simple choose integers in a different ways and generate different ensembles.  But should this be done randomly, or can we be somewhat intelligent about it?  Again, the answer is: yes.



you can also sample the tree stocahastically
define 'compexity" in terms of D_kl

# Exploring Phase Space

We can then explicitly choose to sample all the groups (except p1) and a range of relevant stoichiometries to build convex hulls of (free) energy.
This enables a forward design scheme that is systematic and fast!  INverse design is something we will consider in the next section


Look at example from paper using LJ-lambda potential

Use basin hopping
Minimize structures starting from perfect lattice but do nt constrain
This also allows you consider non-cystalline pahses in principle.

COnvex hull in any number of dimension - remember form beginning we had the issue of phase separation!

3 component example

We speculate that many structure seen experimentally are not reported beacuse they are believed to be "random" or kinetically trapped

# Optimizing the Exploration

Using GPR to optimize where you go looking.
Imclude video

Now we have come full cricle back to inverse design 

# Conclusions

Open to feedback and collaborations!
Also post-doc opportunities

3D - future work
anisotropic particles - forthcoming with derivable functionality


