---
title: "Intro Crystallographic Orbifolds"
excerpt: "How orbit manifolds describe symmetry and why they are so useful."
header:
  teaser: /assets/img/klein_bottle.png
tags:
  - symmetry
  - orbifolds
  - crystals
  - colloids
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

"An orbifold <a href="#thurston">[1]</a> consists of an underlying topological space with an embedded singular set that represents the <a href="https://en.wikipedia.org/wiki/Wyckoff_positions">Wyckoff sites</a> of the crystallographic group. An orbifold for a point group, plane group, or space group is derived by gluing together equivalent edges or faces of a crystallographic asymmetric unit" <a href="#johnson">[2]</a>.

As shown by William Thurston, John H. Conway and coworkers <a href="#conway">[3]</a>, there is a direct connection between crystallographic symmetry and orbifolds.  Orbifolds can be used to prove, for example, that there are 17 different wallpaper groups in 2D Euclidean space <a href="#conway2">[4</a>,<a href="#conway3">5]</a>.  Their uniqueness make them unambiguous descriptors of symmetry.  While the notation and language can be confusing at first, this schema is, in my opinion, a powerful and underappreciated tool for engineering patterns and self-assembly in soft matter physics and related applications.  

# Mathematical History

Topology has a long history in mathematics; it is essentially the study of how things are connected.  As a general prototype, the field studies <a href="https://en.wikipedia.org/wiki/Topology">"properties of geometric objects that are preserved under continuous deformations, such as stretching, twisting, crumpling and bending, but not tearing or gluing"</a>; the latter would change the connectivity by removing, or adding, connections, respectively. A homeomorphism shown below from wikipedia, illustrates the "inflation" of a sphere into a donut and a cow into a sphere; note that you cannot find a homeomorphism between the two of them - this would require tearing a pair of holes on either side of the cow and connecting them to form the "donut" shape. 

![Alt Text](https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Mug_and_Torus_morph.gif/200px-Mug_and_Torus_morph.gif)
![Alt Text](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Spot_the_cow.gif/200px-Spot_the_cow.gif)

The <a href="https://en.wikipedia.org/wiki/Poincar%C3%A9_conjecture">Poincare Conjecture</a> (now theorem), sometimes considered the <a href="https://www.math.unl.edu/~mbrittenham2/ldt/poincare.html">first conjecture ever made in topology</a> (the first <a href="https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg">theorems</a> are often attributed to Euler), states that every simply connected closed three-manifold is <a href="https://mathworld.wolfram.com/PoincareConjecture.html">homeomorphic to the three-sphere.</a> The proof was recently achieved by Grigoriy Perelman in 2002-2003, when he proved the more general Geometrization Conjecture by Thurston, of which the Poincare conjecture is a special case.  He was offered a <a href="https://www.claymath.org/millennium-problems/poincar%C3%A9-conjecture">millenium prize</a> from the Clay Mathematics Institute and a Fields medal in 2006 (both of which he <a href="https://en.wikipedia.org/wiki/Grigori_Perelman">declined</a>) for this solution.  To date, this is the only millenium prize that has been solved.

<a href="https://en.wikipedia.org/wiki/Crystallography">Crystallography</a>, roughly speaking, is the study of the arrangement of atoms in a crystal solid.  Crystals are described by an underlying (<a href="https://en.wikipedia.org/wiki/Bravais_lattice">Bravais</a>) lattice, on which symmetries act at lattice points.  By combining all possible <a href="https://en.wikipedia.org/wiki/Point_group">point symmetry groups<a/> consistent with the crystallographic restriction theorem (<a href="https://en.wikipedia.org/wiki/Crystallographic_point_group">32 total in 3D Euclidean space</a>) with all possible Bravais  lattices (14 in 3D, 5 in 2D, for example), and removing duplicates we arrive at all possible <a href="https://en.wikipedia.org/wiki/Space_group">space groups</a>.  The proof that there exist exactly 230 in 3D was known in the 1890's and was completed before the much easier proof of 17 "wallpaper" groups in 2D.  This original work was completed by Fedorov, Schoenflies; the same result was later reached by Barlow, and even later independently, by Polya and Niggli.

Naturally, there exists a connection between the way that crystals are "put together" and topology; the latter seeming to be a natural language for expressing that concept.  Indeed, Johnson and coworkers discussed the idea of combining geometric topology with structural crystallography as a field of "Structural Crystallographic Topology" in 1996. <a href="#johnson">[2]</a> Conway and Huson also discussed this notion in 2002 <a href="#conway3">[5]</a>.  More recently, in 2014, Hyde and coworkers have advocated this orbifold system, as it provides a new way to think about symmetric patterns and therefore, crystallography itself <a href="#hyde">[6]</a>.  To my knowledge, there has been little work to apply this to engineering problems in the soft matter physics community.

The "orbifold notation" for crystallographic groups is attributed to Conway <a href="#conway3">[5]</a>, though he does not take credit for the underlying philosophy. Conway claims his orbifold symbols are "just an elegant form of Macbeath's <a href="#macbeath">[7]</a> signature" <a href="#conway3">[5]</a>.
> "The philosophy that geometrical groups should be studied through their orbifolds in Bill Thurston's.  I claim originality only for the simple and elegant notation introduced here.  David Singerman tells me that Murray MacBeath has long described the orbifolds of surface groups in a less compact but essentially equivalent way." <a href="#conway">[3]</a>

The term "orbifold" itself originated with Thurston, but he also states he "cannot be blamed" for it:
> "This terminology should not be blamed on me. It was obtained by a democratic process in my course of 1976-77. An orbifold is something with many folds; unfortunately, the word 'manifold' already has a different definition. I tried 'foldamani,' which was quickly displaced by the suggestion of 'manifolded.' After two months of patiently saying 'no, not a manifold, a manifoldead,' we held a vote, and 'orbifold' won." <a href="#thurston">[1]</a>

A more detailed history of the mathematical evolution of orbifolds is presented in Johnson et al. <a href="#johnson">[2]</a> (see references therein):  
> "The V-manifold of Satake provided the first formal definition of what was later renamed orbifold and popularized widely by William Thurston. This concept was developed by Thurston into a major geometric topology discipline. Thurston’s unpublished Princeton class notes of 1978 entitled 'Three Dimensional Geometry and Topology,' which is being expanded into a book manuscript of the same title, and an article by Scott constitute the main general references on orbifolds. The first systematic study of crystallographic orbifolds was done by W. D. Dunbar in his 1981 Princeton dissertation, carried out under Thurston, and in which he derived and illustrated the singular sets for the 65 polar space groups using oriented orbifolds. The parts of his dissertation related to the underlying hypersphere space S$^3$ were published in 1988. The second major contribution to crystallographic orbifolds is the systematic development of orbifolds (both oriented and nonoriented) in Seifert fibered space in Bonahon and Siebenmann’s unpublished manuscript. Part of that manuscript related to Euclidean 3-orbifolds, but omitting direct discussion of crystallography, was published in 1985. A book on 'Classical Tesselations and Three-Manifolds' by Montesinos covers and expands certain aspects of Bonahon and Siebenmann's work. A nomenclature system for 2-orbifolds was published by John H. Conway of Princeton. Conway and Thurston have a nomenclature system for noncubic Euclidean 3-orbifolds based on the lifting of 2-Euclidean orbifolds to form Seifert fibered spaces."

# Connection to Symmetry

The history and terminology associated with orbifolds may seem a bit technical at first, however, the underlying principles are quite straightforward.  Here I will provide some examples to motivate the use of topology as an intuitive description of patterns.  The majority of this, and the remainder of this discussion is devoted to two dimensional (2D) Euclidean space.  One of the beauties of using orbifolds is that you can unify the description and enumeration of symmetry groups in non-Euclidean spaces, and with a little modification, can also also use them in 3D <a href="#conway">[4</a>,<a href="#hyde">6]</a>.

## A division of space

A formal mathematic definition of an orbifold is "a space modeled on the quotient of quotient of space by a finite group" <a href="#thurston">[1</a>,<a href="#hyde">6]</a>.  In other words, for 2D surfaces it is "the surface divided by the group". <a href="#conway">[3]</a>

When we divide 2 numbers, we have an intuition for what this means: a/b = c, implies that there are "c" of "b" in "a", e.g., there are 6 "2"s in 12.  Generalizing this to the division of an object, it is also clear how to divide, for example, a pie: pie / 4 = 1 slice. 

<img style="float: center" src="dividing_space.png" width=1000px>

> Anecdotally, there is a long mathematical history of <a href="https://en.wikipedia.org/wiki/Fair_cake-cutting">dividing cakes</a> (and pies), dating back to 1906 in this <a href="https://www.nature.com/articles/075173c0">Nature article</a>.  This is concerned with preserving the cake for as long as possible, whereas others have focused on dividing the cake in <a href="https://www.jstor.org/stable/2311357?seq=1">a fair way</a>. Conway has also contributed to the <a href="https://en.wikipedia.org/wiki/Selfridge%E2%80%93Conway_procedure">latter problem under various assumptions</a>.  Some recent developments are summarized <a href="https://www.quantamagazine.org/new-algorithm-solves-cake-cutting-problem-20161006/">here</a>.

However, if the "pie" is no longer a finite object, but an infinite periodic space, it becomes a little less intuitive.  If space is periodic, then whenever a cut is made, there must be an inifinite number of other symmetrically equivalent cuts being made at the same time according whatever rules are in place due to "periodicity".  At some point, those cuts should intersect and if, when they do, all the "chunks" are identical we can describe how those chunks can be mapped onto each other.  In fact, point symmetry describes how multiple cuts are equivalent to each other in the pie example.  For a crystal, the simplest imaginable chunk could be a unit cell.  A <a href="https://en.wikipedia.org/wiki/Bravais_lattice">"primitive cell"</a> is the smallest possible unit cell with discrete translational symmetry, i.e., it is the smallest "chunk" that can be translated around the plane or space and fill it completely; non-primitive unit cells are integer multiples of these. 

In the example above, it is clear that, if repeated in all directions, a rectangular-esque shape is excised by symmetrically equivalent neighbors above and below, and left and right.  This suggests the top and bottom are somehow equivalent, as are the left and right.  Topology helps us quantify and describe this more accurately.  This particular symmetry is known as p1 in the International or Hermann-Mauginn notation.  Primitive cells, or multiples thereof, have p1 symmetry in the sense that they are translations of each other and their edges are related by these operations.

## Wallpaper groups and symmetry

The chunks in the above 2D example are related only by translation, but other isometries are possible; combinations of such operations gives rise to the different symmetry groups.  As a result, the primitive cell usually contains multiple "chunks" related by these other symmetries.  Consequently, the unit cell is NOT the smallest "unit" of the crystal in terms of symmetry.  Formally, these chunks are referred to as fundamental domains (by mathematicians) or asymmetric units (by crystallographers).  The latter designation is adopted because it indicates that the fundamental domain (FD) contains no symmetry elements.  Symmetry elements are locations, such as points or planes, about which symmetry operations act. For example, rotation (operation) about a point (element). In fact, all symmetry elements exist along the boundary/surface of the FD; as we will later see, this means that the edges of the FD must be related to each other by virtue of those operations.  It was Conway who proved that that relation is unique for a given symmetry group.  This is particularly remarkable because there is (usually) no unique geometric shape for a FD; topologically, however, they will turn out to be the same if they belong to the same symmetry group!

FDs are critical to the description of crystals because they describe all of the "independent degrees of freedom." You can do anything inside the FD because it is going to be repeated, but all other locations in the crystal are describable as an operation (in topology, the "orbit") of points in a single FD.  It is useful to recalibrate your imagination to think of the FD as the smallest "unit" of the crystal, not the unit/primitive cell when it comes to symmetry.  In molecular simulations and computational applications, the simplicity of programming p1 boundary conditions is why most people work with crystals at this scale, rather than at the FD level.  The p1 primitive cell is its own fundamental domain; for all other wallpaper groups there are between 2 and 12 FD per primitive cell (see red boxes below)<a href="#pretti">[8]</a>. The number is fixed for a given groups and is always the same for that group, even though the specific tiles that form FDs might be different.

<a href="https://pubs.acs.org/doi/abs/10.1021/acs.jpca.0c00846"><img style="float: center" src="pc_example.png" width=1000px></a>

In 2D there are 4 different isometries of the Euclidean plane: translation, rotation, reflection, and glide reflection. Wallpaper groups describe the set of unique combinations of isometries of the Euclidean plane containing two linearly independent translations. These operations act on the fundamental domain to tessellate the plane. There are a total of 17 wallpaper groups, shown in the figures above and below; in the figure below, a FD is drawn with a black outline, with a chiral RGB trimer on top.  This is to illustrate how the groups tile space according to their operations.  Again, the tiles chosen below are not unique, but they are convenient for other purposes, which will be discussed later.

<a href="https://github.com/usnistgov/PACCS"><img style="float: center" src="wpgroups.png" width=1000px></a>

In the top row, only rotations are used to generate the plane; in the second, only reflections are used.  These operations only occur at the boundary of the FDs, i.e., along the black lines.  Check this for yourself as an exercise.  You should be able to see how the "knife" cut the space up into non-overlapping chunks without any gaps.  Moreover, pay attention to which edges are symmetrically equivalent.  In other words, for p1, since the cell is translated up and down, and also left and right, these sets of edges "overlap" between neighboring images.  These are, thus, "symmetrically equivalent."

## What is a crystallographic orbifold?

The term "orbifold" is short for orbit manifold <a href="#thurston">[1]</a>.  As already stated, an orbit manifold is topological object that describes the "division" of a space (2D Euclidean plane, for example) by the action of a finite group.  The fundamental domain is the region of space containing no symmetry operations and is this quotient.  In crystallographic topology, a wallpaper group's orbifold can derived by gluing together the FD to match up the symmetrically equivalent edges.<a href="#hyde">[6]</a>  Some examples are shown below.

<a href="https://www.nature.com/articles/s41467-019-10031-4"><img style="float: center" src="orbifold_examples.png" width=1000px></a>

For example, for p1 the pattern is created by shifting the cell "up" and "down", and "left" and "right".  Consequently, the left edge of a FD overlaps the right of the FD to its left, and so on.  This implies the left edge is symmetrically equivalent to the right, and the top with the bottom. If we wrap the FD's top to meet its bottom we create a cylinder; next, wrapping the left to meet the right we obtain a torus, as shown above.  This torus is the p1 group's orbifold.  Recall that this sort of cell is the type of cell used in conventional molecular simulations such as Monte Carlo or molecular dynamics.  Clearly, the orbifold effectively represents the boundary conditions on the FD; a p1 cell is sometimes said to have "toroidal" boundary conditions, though it has become such common practice that today they are referred to as "standard periodic boundary conditions" in most molecular simulation literature.

One can repeat this exercise for other groups.  The examples above show how this (orbi)folding process superimposes equivalent sites.

> "Orbifolding is simply the operation of wrapping, or folding in the case of mirrors, to superimpose all equivalent points. There are times when the orbifolding process itself is important since we may need to unfold the orbifold partially to obtain some other (covering) orbifold or to unfold it fully to obtain the original space (i.e., the universal cover). Covering orbifolds are related to the original orbifold as subgroups are related to groups. The universal cover of all Euclidean n-orbifolds is Euclidean n-space and that for spherical n- orbifolds is the n-sphere." <a href="#johnson">[2]</a>

Below, you can find the matching edges (according to color and arrow) indicated for all the FDs shown so far.  This can be found in Ref. <a href="#pretti">[8]</a>. You can do this yourself by cutting out the shape below and gluing the matching edges together.

<a href="https://pubs.acs.org/doi/abs/10.1021/acs.jpca.0c00846"><img style="float: center" src="boundary_conditions.png" width=1000px></a>

In fact, the concept of orbifolds is much more broad.  Orbifolds can be used to describe non-periodic objects, 2D periodic structures on curved surfaces, and with a little modification, can also help in 3D as well.  This largely developed by Conway and co-workers.

# A Step Back: Identifying Symmetry

At this point, we have discussed what a FD is and how by "gluing" it together we can obtain the orbifold that describes a given symmetry group.  This has been somewhat abstract, but is helpful a starting point.  Before proceeding further, it is enlightening to go back a bit and simply try to understand symmetry in patterns.

Pictures with examples of how to look for symmetry in patterns
https://mathstat.slu.edu/escher/index.php/Regular_Division_of_the_Plane_Drawings
Also some examples from Conway's book
Only draw symmetry elements - draw tiles and show edge matching later

I like to start with mirror lines - they are usually the most obvious (probably why Escher largely avoided them, but did not completely reject them still had kaleidoscopes!)

It can help to keep in mind we are looking for the "smallest" region (FD) possible that contains no symmetry, itself.  Mirrors are always boundaries; rotation centers are either on and edge or corner.  Note that rotation centers can result from intersecting mirrors.  If there are mirrors, take those; if not, we have a "corner" rotation point.  Glide reflections are a little more tricky to identify; they result from a reflection + translation so we are looking for mirror images that are not related by a mirror plance. Conway actually called this a "miracle"!  Translations are the simplest to identify (according to Conway: wonderous "wandering" = "wonder-ring = "o" symbol)


This means something very important - instead of thinking of groups as matrices, we can vied them as "rings" decorated with maps that indicate how parts of the ring are related to each other.  Create ring, add decorations until "cost" reaches a threshold. this is what conway proved.

# The Power of Orbifolds

## Conway's Naming Convention

Emphasice that (Conway, Huson 2002) an orbifold is more than just a simple manifodl because it has angles defined on it.

## Proof of 17 Wallpaper Groups (2D)

Remarkably, Conway proved that the topological properties of the orbifolds for all crystals are unique and singular for each symmetry group.  In other words, there is one and only one orbifold for each symmetry group (even though there are often many fundamental domains).  This "Magic Theorem", is truly that.  

first proof in 1891 (https://en.wikipedia.org/wiki/Space_group)

"Top down" approach (Surgery on a sphere) connects to "bottom up" (folding FD)
Gauss Bonnet theorem

topologies (p1=torus, p2=pillowcase, p3-p6=turnover)

## Topologically Distinct Domains

Since the orbifold is unique to each symmetry group, and can be formed by folding up a FD, we can derive other "tiles" of the plane by looking at different ways of reversing this folding, i.e., cutting them open again.  Unsurprisingly, there are multiple ways of cutting open orbifold and this can be shown to correspond to different fundamental domains.  

There are some rules for cutting...

These are the different so-called "Heesch types" after the mathematician Heesch who, among other things, studied tilings and their connection to symmetry; the renowned Dutch artist M. C. Escher also undertook a study of these tilings.
- http://www.eschertile.com/
- http://jwilson.coe.uga.edu/EMT668/EMAT6680.2002.Fall/AllenL/MATH%207200/Escher%20Project/Tesselmania/tesselmania.html

Different cuttings to produce different isohedral tiles (some of them are FD)
Point to papers coming out?

For most applications, it doesn't matter how you select the tile - you can cut the same crystal many ways --> "headless horse" situation
See ETH zurich java applet for example where you use a single tile
ITCA uses a single tile


https://mathstat.slu.edu/escher/index.php/Math_and_the_Art_of_M._C._Escher


Draw colored edges from the previous symetry examples


https://mathstat.slu.edu/escher/index.php/Regular_Division_of_the_Plane_Drawings
"Over the course of his career, Escher filled five folio notebooks with sketches of periodic tessellations. These were not intended for public consumption, but instead sources of ideas and design patterns he would use in creating his finished graphic works. Escher numbered his sketches, from 1 to 137. " - https://mathstat.slu.edu/escher/index.php/Regular_Division_of_the_Plane_Drawings

# Generating Crystals

PACCS - have separate post about paper on this
Wykoff positions and stoichiometry (a bit about this) --> remember wyckoff positions originally mentioned in tldr?
High symmetry hypothesis from David Wales
See paper by Doye, also isopointal sets by Glotzer student

# Beyond

So far, have focused discussion on 2D Euclidean space but is extensible to 3D and to non-Euclidean spaces (Hyde paper and Conway's book).
As already discussed, this is connected to tiling theories. <a href="#conway">[1]</a>

# In summary

* Orbifolds describe symmetrically unique (think independent degrees of freedom)

# Citations

<p id="thurston">[1] Thurston, William P. <it>The geometry and topology of three-manifolds.</it> Princeton, NJ: Princeton University, 1979.</p>
<p id="johnson">[2] Johnson, Carroll K., Michael N. Burnett, and William D. Dunbar. "Crystallographic Topology and its Applications." <it>Crystallographic Computing</it> 7 (1996).</p>
<p id="conway">[3] Conway, J. H. in <it>Groups, combinatorics and geometry.</it> Vol. 165. pp 438-447, Cambridge University Press, 1992</p>
<p id="conway2">[4] Conway, John H., Heidi Burgiel, and Chaim Goodman-Strauss. <it>The Symmetries of Things.</it> CRC Press, 2008.</p>
<p id="conway3">[5] Conway, John H., and Daniel H. Huson. "The Orbifold Notation for Two-dimensional Groups." <it>Structural Chemistry</it> 13.3 (2002): 247-257.</p>
<p id="hyde">[6] Hyde, S. T., S. J. Ramsden, and Vanessa Robins. "Unification and Classification of Two-dimensional Crystalline Patterns using Orbifolds." <it>Acta Crystallographica Section A: Foundations and Advances</it> 70.4 (2014): 319-337.</p>
<p id="macbeath">[7] Macbeath, A. M. "The classification of non-euclidean plane crystallographic groups." <it>Canadian Journal of Mathematics</it> 19 (1967): 1192-1205.</p>
<p id="pretti">[8] Pretti, Evan, et al. "Symmetry-Based Crystal Structure Enumeration in Two Dimensions." <it>The Journal of Physical Chemistry A</it> 124.16 (2020): 3276-3285.</p>

<!--
2D Crystallographic Tiling

# Heesch Types
Escher
Heesch Tiles - 28 tiles from certain wallpaper groups

# Fundamental Domains from Orbifolds

Conway's symbols on circles - "shortcut" method to get FD for groups

# Fundamental Domain Tilings

Can find all 46 FDs for groups by making transformations in the plane.

# Non-FD Tilings

The other 47 "molecular" units.

-->
