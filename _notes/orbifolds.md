---
title: "Intro to Crystallographic Orbifolds"
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

For an introduction to symmetry, I recommend the "Math and Art of MC Escher" <a href="https://mathstat.slu.edu/escher/index.php/Math_and_the_Art_of_M._C._Escher">website</a>, which contains accessible discussions, illustrations, and examples that avoid a lengthy foray into complex mathematical language.

Prof. Frank Hoffman has a very nice YouTube course as well.  You can find it <a href="https://www.youtube.com/c/FrankHoffmann1000/featured">here</a>.

> "The orbifold idea is the most powerful way to achieve a conceptual understanding of these \[wallpaper\] groups and, in particular, it trivializes their enumeration." - Conway & Huson in Ref. <a href="#conway3">[5]</a>

# Mathematical History

Topology has a long history in mathematics; it is essentially the study of how things are connected.  As a general prototype, the field studies <a href="https://en.wikipedia.org/wiki/Topology">"properties of geometric objects that are preserved under continuous deformations, such as stretching, twisting, crumpling and bending, but not tearing or gluing"</a>; the latter would change the connectivity by removing, or adding, connections, respectively. A homeomorphism shown below from wikipedia, illustrates the "inflation" of a sphere into a donut and a cow into a sphere; note that you cannot find a homeomorphism between the two of them - this would require tearing a pair of holes on either side of the cow and connecting them to form the "donut" shape. 

![Alt Text](https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Mug_and_Torus_morph.gif/200px-Mug_and_Torus_morph.gif)
![Alt Text](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Spot_the_cow.gif/200px-Spot_the_cow.gif)

The <a href="https://en.wikipedia.org/wiki/Poincar%C3%A9_conjecture">Poincare Conjecture</a> (now theorem), sometimes considered the <a href="https://www.math.unl.edu/~mbrittenham2/ldt/poincare.html">first conjecture ever made in topology</a> (the first <a href="https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg">theorems</a> are often attributed to Euler), states that every simply connected closed three-manifold is <a href="https://mathworld.wolfram.com/PoincareConjecture.html">homeomorphic to the three-sphere.</a> The proof was recently achieved by Grigoriy Perelman in 2002-2003, when he proved the more general Geometrization Conjecture by Thurston, of which the Poincare conjecture is a special case.  He was offered a <a href="https://www.claymath.org/millennium-problems/poincar%C3%A9-conjecture">millenium prize</a> from the Clay Mathematics Institute and a Fields medal in 2006 (both of which he <a href="https://en.wikipedia.org/wiki/Grigori_Perelman">declined</a>) for this solution.  To date, this is the only millenium prize that has been solved.

<a href="https://en.wikipedia.org/wiki/Crystallography">Crystallography</a>, roughly speaking, is the study of the arrangement of atoms in a crystal solid.  Crystals are described by an underlying (<a href="https://en.wikipedia.org/wiki/Bravais_lattice">Bravais</a>) lattice, on which symmetries act at lattice points.  By combining all possible <a href="https://en.wikipedia.org/wiki/Point_group">point symmetry groups<a/> consistent with the crystallographic restriction theorem (<a href="https://en.wikipedia.org/wiki/Crystallographic_point_group">32 total in 3D Euclidean space</a>) with all possible Bravais  lattices (14 in 3D, 5 in 2D, for example), and removing duplicates we arrive at all possible <a href="https://en.wikipedia.org/wiki/Space_group">space groups</a>.  The proof that there exist exactly 230 in 3D was known in the 1890's and was completed before the much easier proof of 17 "wallpaper" groups in 2D.  This original work was completed by Fedorov, Schoenflies; the same result was later reached by Barlow, and even later independently, by Polya and Niggli.

Naturally, there exists a connection between the way that crystals are "put together" and topology; the latter seeming to be a natural language for expressing that concept.  Indeed, Johnson and coworkers discussed the idea of combining geometric topology with structural crystallography as a field of "Structural Crystallographic Topology" in 1996. <a href="#johnson">[2]</a> Conway and Huson also discussed this notion in 2002 <a href="#conway3">[5]</a>.  More recently, in 2014, Hyde and coworkers have advocated this orbifold system, as it provides a new way to think about symmetric patterns and therefore, crystallography itself <a href="#hyde">[6]</a>.  

The "orbifold notation" for crystallographic groups is attributed to Conway <a href="#conway3">[5]</a>, though he does not take credit for the underlying philosophy. Conway claims his orbifold symbols are "just an elegant form of Macbeath's <a href="#macbeath">[7]</a> signature" <a href="#conway3">[5]</a>.
> "The philosophy that geometrical groups should be studied through their orbifolds in Bill Thurston's.  I claim originality only for the simple and elegant notation introduced here.  David Singerman tells me that Murray MacBeath has long described the orbifolds of surface groups in a less compact but essentially equivalent way." <a href="#conway">[3]</a>

The term "orbifold" itself originated with Thurston, but he also states he "cannot be blamed" for it:
> "This terminology should not be blamed on me. It was obtained by a democratic process in my course of 1976-77. An orbifold is something with many folds; unfortunately, the word 'manifold' already has a different definition. I tried 'foldamani,' which was quickly displaced by the suggestion of 'manifolded.' After two months of patiently saying 'no, not a manifold, a manifoldead,' we held a vote, and 'orbifold' won." <a href="#thurston">[1]</a>

A more detailed history of the mathematical evolution of orbifolds is presented in Johnson et al. <a href="#johnson">[2]</a> (see references therein):  
> "The V-manifold of Satake provided the first formal definition of what was later renamed orbifold and popularized widely by William Thurston. This concept was developed by Thurston into a major geometric topology discipline. Thurston’s unpublished Princeton class notes of 1978 entitled 'Three Dimensional Geometry and Topology,' which is being expanded into a book manuscript of the same title, and an article by Scott constitute the main general references on orbifolds. The first systematic study of crystallographic orbifolds was done by W. D. Dunbar in his 1981 Princeton dissertation, carried out under Thurston, and in which he derived and illustrated the singular sets for the 65 polar space groups using oriented orbifolds. The parts of his dissertation related to the underlying hypersphere space S$^3$ were published in 1988. The second major contribution to crystallographic orbifolds is the systematic development of orbifolds (both oriented and nonoriented) in Seifert fibered space in Bonahon and Siebenmann’s unpublished manuscript. Part of that manuscript related to Euclidean 3-orbifolds, but omitting direct discussion of crystallography, was published in 1985. A book on 'Classical Tesselations and Three-Manifolds' by Montesinos covers and expands certain aspects of Bonahon and Siebenmann's work. A nomenclature system for 2-orbifolds was published by John H. Conway of Princeton. Conway and Thurston have a nomenclature system for noncubic Euclidean 3-orbifolds based on the lifting of 2-Euclidean orbifolds to form Seifert fibered spaces."

# Connection to Symmetry

The history and terminology associated with orbifolds may seem a bit technical at first, however, the underlying principles are quite straightforward.  Here I will provide some examples to motivate the use of topology as an intuitive description of periodic patterns.  The majority of this, and the remainder of this discussion is devoted to two dimensional (2D) Euclidean space.  One of the beauties of using orbifolds is that you can unify the description and enumeration of symmetry groups in non-Euclidean spaces, and with a some additional modifications, can also also use them in 3D <a href="#conway">[4</a>,<a href="#hyde">6]</a>.

## A division of space

A formal mathematic definition of an orbifold is "a space modeled on the quotient of quotient of space by a finite group" <a href="#thurston">[1</a>,<a href="#hyde">6]</a>.  In other words, for 2D surfaces it is "the surface divided by the group". <a href="#conway">[3]</a>

When we divide 2 numbers, we have an intuition for what this means: a/b = c, implies that there are "c" of "b" in "a", e.g., there are 6 "2"s in 12.  Generalizing this to the division of an object, it is also clear how to divide, for example, a pie: pie / 4 = 1 slice. 

<img style="float: center" src="dividing_space.png" width=1000px>

> Anecdotally, there is a long mathematical history of <a href="https://en.wikipedia.org/wiki/Fair_cake-cutting">dividing cakes</a> (and pies), dating back to 1906 in this <a href="https://www.nature.com/articles/075173c0">Nature article</a>.  This is concerned with preserving the cake for as long as possible, whereas others have focused on dividing the cake in <a href="https://www.jstor.org/stable/2311357?seq=1">a fair way</a>. Conway has also contributed to the <a href="https://en.wikipedia.org/wiki/Selfridge%E2%80%93Conway_procedure">latter problem under various assumptions</a>.  Some recent developments are summarized <a href="https://www.quantamagazine.org/new-algorithm-solves-cake-cutting-problem-20161006/">here</a>.

However, if the "pie" is no longer a finite object, but an infinite periodic space, it becomes a little less intuitive.  If space is periodic, then whenever a cut is made, there must be an inifinite number of other symmetrically equivalent cuts being made at the same time according whatever rules are in place due to "periodicity".  At some point, those cuts should intersect and if, when they do, all the "chunks" are identical we can describe how those chunks can be mapped onto each other.  In fact, point symmetry describes how multiple cuts are equivalent to each other in the pie example.  For a crystal, the simplest imaginable chunk could be a unit cell.  A <a href="https://en.wikipedia.org/wiki/Bravais_lattice">"primitive cell"</a> is the smallest possible unit cell with discrete translational symmetry, i.e., it is the smallest "chunk" that can be translated around the plane or space and fill it completely; non-primitive unit cells are integer multiples of these. 

In the example above, it is clear that, if repeated in all directions, a rectangular-esque shape is excised by symmetrically equivalent neighbors above and below, and left and right.  This suggests the top and bottom are somehow equivalent, as are the left and right.  Topology helps us quantify and describe this more accurately.  This particular symmetry is known as p1 in the International or Hermann-Mauginn notation.  Primitive cells, or multiples thereof, have p1 symmetry in the sense that they are translations of each other and their edges are related by these operations.

## Wallpaper groups and symmetry

The chunks in the above 2D example are related only by translation, but other isometries are possible; combinations of such operations gives rise to the different symmetry groups.  As a result, the primitive cell usually contains multiple "chunks" related by these other symmetries.  Consequently, the unit cell is NOT the smallest "unit" of the crystal in terms of symmetry.  Formally, these chunks are referred to as fundamental domains (by mathematicians) or asymmetric units (by crystallographers).  The latter designation is adopted because it indicates that the fundamental domain (FD) contains no symmetry elements.  Symmetry elements are locations, such as points or planes, about which symmetry operations act. For example, rotation (operation) about a point (element). In fact, all symmetry elements exist along the boundary/surface of the FD; as we will later see, this means that the edges of the FD must be related to each other by virtue of those operations.  It was Conway who proved that there is a unique underlying relationship for a given symmetry group.  This is particularly remarkable because there is (usually) no unique geometric shape for a FD; topologically, however, they will turn out to be the same if they belong to the same symmetry group!

FDs are critical to the description of crystals because they describe all of the "independent degrees of freedom." You can do anything inside the FD because it is going to be repeated, but all other locations in the crystal are describable as an operation (in topology, the "orbit") of points in a single FD.  It is useful to recalibrate your imagination to think of the FD as the smallest "unit" of the crystal, not the unit/primitive cell when it comes to symmetry.  In molecular simulations and computational applications, the simplicity of programming p1 boundary conditions is why most people work with crystals at this scale, rather than at the FD level.  The p1 primitive cell is its own fundamental domain; for all other wallpaper groups there are between 2 and 12 FD per primitive cell (see red boxes below)<a href="#pretti">[8]</a>. The number is fixed for a given groups and is always the same for that group, even though the specific tiles that form FDs might be different.

<a href="https://pubs.acs.org/doi/abs/10.1021/acs.jpca.0c00846"><img style="float: center" src="pc_example.png" width=1000px></a>

In 2D there are 4 different isometries of the Euclidean plane: translation, rotation, reflection, and glide reflection. Wallpaper groups describe the set of unique combinations of isometries of the Euclidean plane containing two linearly independent translations. These operations act on the fundamental domain to tessellate the plane. There are a total of 17 wallpaper groups, shown in the figures above and below; in the figure below, a FD is drawn with a black outline, with a chiral RGB trimer on top.  This is to illustrate how the groups tile space according to their operations.  Again, the tiles chosen below are not unique, but they are convenient for other purposes, which will be discussed later.

<a href="https://github.com/usnistgov/PACCS"><img style="float: center" src="wpgroups.png" width=1000px></a>

In the top row, only rotations are used to generate the plane; in the second, only reflections are used.  These operations only occur at the boundary of the FDs, i.e., along the black lines.  Check this for yourself as an exercise.  You should be able to see how the "knife" cut the space up into non-overlapping chunks without any gaps.  Moreover, pay attention to which edges are symmetrically equivalent.  In other words, for p1, since the cell is translated up and down, and also left and right, these sets of edges "overlap" between neighboring images.  These are, thus, "symmetrically equivalent."

## What is a crystallographic orbifold?

The term "orbifold" is short for orbit manifold <a href="#thurston">[1]</a>. As already stated, an orbit manifold is topological object that describes the "division" of a space (2D Euclidean plane, for example) by the action of a finite group.  The fundamental domain is the region of space containing no symmetry operations and is this quotient.  In crystallographic topology, a wallpaper group's orbifold can derived by gluing together the FD to match up the symmetrically equivalent edges <a href="#hyde">[6]</a>. Some examples are shown below.

<a href="https://www.nature.com/articles/s41467-019-10031-4"><img style="float: center" src="orbifold_examples.png" width=1000px></a>

For example, for p1 the pattern is created by shifting the cell "up" and "down", and "left" and "right".  Consequently, the left edge of a FD overlaps the right of the FD to its left, and so on.  This implies the left edge is symmetrically equivalent to the right, and the top with the bottom. If we wrap the FD's top to meet its bottom we create a cylinder; next, wrapping the left to meet the right we obtain a torus, as shown above.  This torus is the p1 group's orbifold.  Recall that this sort of cell is the type of cell used in conventional molecular simulations such as Monte Carlo or molecular dynamics.  Clearly, the orbifold effectively represents the boundary conditions on the FD; a p1 cell is sometimes said to have "toroidal" boundary conditions, though it has become such common practice that today they are referred to as "standard periodic boundary conditions" in most molecular simulation literature.

One can repeat this exercise for other groups.  The examples above show how this (orbi)folding process superimposes equivalent sites.

> "Orbifolding is simply the operation of wrapping, or folding in the case of mirrors, to superimpose all equivalent points. There are times when the orbifolding process itself is important since we may need to unfold the orbifold partially to obtain some other (covering) orbifold or to unfold it fully to obtain the original space (i.e., the universal cover). Covering orbifolds are related to the original orbifold as subgroups are related to groups. The universal cover of all Euclidean n-orbifolds is Euclidean n-space and that for spherical n- orbifolds is the n-sphere." <a href="#johnson">[2]</a>

Below, you can find the matching edges (according to color and arrow) indicated for all the FDs shown so far.  This can be found in Ref. <a href="#pretti">[8]</a>. You can do this yourself by cutting out the shape below and gluing the matching edges together.

<a href="https://pubs.acs.org/doi/abs/10.1021/acs.jpca.0c00846"><img style="float: center" src="boundary_conditions.png" width=1000px></a>

In Chapter 9 of Ref. <a href="#conway2">[4]</a>, the authors also provide some examples, and some levity: "We suggest you buy a spare copy of the book before cutting out the patterns on the next few pages, so as to increase our royalties".

In fact, the concept of orbifolds is much more broad.  Orbifolds can be used to describe non-periodic objects, 2D periodic structures on curved surfaces, and with a little modification, can also help in 3D as well.  This largely developed by Conway and co-workers.

# A Step Back: Identifying Symmetry

At this point, we have discussed what a FD is and how by "gluing" it together we can obtain the orbifold that describes a given symmetry group.  This has been somewhat abstract, but is helpful a starting point.  Before proceeding further, it is enlightening to go back a bit and simply try to understand symmetry in patterns.  

In what follows I will classify some of M. C. Escher's sketches (from his private folios) according to their symmetry.  I will disregard differences in color, as this introduces some additional complexity.  More information can be found in Ref. <a href="#conway2">[4]</a>. There are 4 fundamental "features" of symmetric patterns, or <a href="https://en.wikipedia.org/wiki/Isometry">isometries</a> of the plane: translation, rotation, reflection, and glide reflection.  Conway and co-workers call these "wonders", gyrations, kaleidoscopes, and "miracles." A glide reflection is a reflection about a line followed by translation along that line.

> "Over the course of his career, Escher filled five folio notebooks with sketches of periodic tessellations. These were not intended for public consumption, but instead sources of ideas and design patterns he would use in creating his finished graphic works. Escher numbered his sketches, from 1 to 137." - From [here](https://mathstat.slu.edu/escher/index.php/Regular_Division_of_the_Plane_Drawings)

There are a number of heuristics you can find online that can be used to identify the wallpaper group of a pattern.  For example, see the decision tree found <a href="https://mathstat.slu.edu/escher/index.php/Wallpaper_Patterns">here</a>.  However, these methods are less clear to me than using the orbifold method.  In the former, classification is based on the existence of certain symmetries that end up being generators, essentially; in the latter, we focus on the FD of the pattern and look at what its boundaries are made up of.  I find the second to be much more intuitive!

It can help to keep in mind we are looking for the "smallest" region (FD) possible that contains no symmetry, itself. Mirrors are always boundaries; rotation centers are either on and edge or corner.  Note that rotation centers can result from intersecting mirrors. If there are mirrors, take those; if not, we have a "corner" rotation point.  Glide reflections are a little more tricky to identify; since they result from a reflection + translation we are looking for mirror images that are not related by a mirror plane. 

Here's how I proceed:
1. First, I like to start by looking for mirror lines - they are usually the most obvious.  This is not great if you are an artist and is seemingly why M. C. Escher largely avoided them.  
2. Next, I look for for rotation centers.  Be aware: intersecting mirror planes create rotation centers; for example if you have 2 intersecting, orthogonal mirrors creating 4 quadrants, then Q1 and Q3 are 180 degree rotations of each other, as are Q2 and Q4.  If you see a rotation center, be sure it isn't due to multiple mirror planes passing through that point.  
3. Finally, look for mirror images that have are connected but are not related by a mirror plane, i.e., you can draw a line between the 2 parts of the image without going through a mirror.
4. If there are none of the above, look for translations.

Let's look at some examples. These are taken from the "Math and Art of MC Escher" website <a href="https://mathstat.slu.edu/escher/index.php/Regular_Division_of_the_Plane_Drawings">here</a>.  I have demarcated the symmetries I see with green lines and shapes.

<img style="float: center" src="p1-symm-Regular-division-105.png" width=300px>
<img style="float: center" src="p2-symm-455px-Regular-division-01.png" width=300px>
<img style="float: center" src="p2-symm-Regular-division-11.png" width=300px>

Above, we have an example of p1, and 2 examples of p2. In the first, only translation is present and the Pegasus is merely "shifted" in 2 orthogonal directions to fill the plane.  In the second two examples, we see there are only 2-fold rotation centers (ovals) present.  You can see where Escher originally drew his guide lines and made points to denote where he would place those points of symmetry.

<img style="float: center" src="p3m1-symm-Regular-division-85.png" width=300px>
<img style="float: center" src="p31m-symm-448px-Regular-division-04.png" width=300px>
<img style="float: center" src="p2gg-symm-451px-Regular-division-10.png" width=300px>

In this row of examples, first we see a case where only intersecting mirror planes are used (p3m1).  The motif is composed of 3 "halves" of animals: a lizard, fish, and bat.  Their biaxial symmetry enables one to use only a half of a single animal to generate the other half; it is purely artistic talent that enables the three to together inside a single bounded triangle.  The second example also contains a motif (person) with a mirror plane, however, in this case a 3-fold rotation center is present allowing different motifs to nest perfectly inside each other (p31m).  The final example is one with glide planes (dashed lines) which bound 2-fold rotation centers (p2gg, or pgg for short).

> "Some animal motifs, generally larger animals, are usually seen from the front or side, and so look silly when viewed upside down or at an angle. Escher was careful, at least in his later work, to avoid symmetries containing rotation when working with such animals. On the other hand, Escher writes: "When a rotation does take place, then the only animal motifs which are logically acceptable are those which show their most characteristic image when seen from above" <a href="#doris">[9]</a>. For example, insects and lizards occur frequently in Escher's work when rotation symmetry is present. Although Escher understood symmetry well and and knew of the mathematical classification of wallpaper symmetry groups from Polya's work, he was interested in creating new patterns rather than analyzing existing work." - Excerpt from [here](https://mathstat.slu.edu/escher/index.php/Wallpaper_Patterns)

Allegedly, it was Escher's brother-in law who knew of George Polya and originally mentioned Polya's work to him. Escher hand-copied some of Polya's papers into his own notebooks to study the theory of symmetry. Polya was Conway's mentor and had a great influence on Conway's interests; Polya himself invented a naming scheme for crystallographic symmetries, though it is not among those commonly used today. In the next section I will review Conway's naming convention and why it is so elegant.

# The Power of Orbifolds

Now I will illustrate how simple naming and classifying symmetry is using Conway's notation.  To do so, first I will review the naming convention itself; this should alo make it clear how one can easily identfy a FD for a pattern.  Next, I will go over how this naming scheme is related to the proof that there are only 17 distinct wallpaper groups.  Finally, this excercise should explicitly illustrate the connection between symmetry, orbifolds, and tilings of the plane using only a single type of tile.

Conway and Huson have provided a very accessible paper on the subject of orbifiold notation for 2D groups in Structural Chemistry (See Ref. <a href="#conway3">[5]</a>), which I will refer the reader to for more information.

## The Orbifold Naming Convention

As we have already seen, the orbifold is the "surface divided by the group" and may be found by folding or wrapping the FD up to match up symmetrically equivalent points on its edges. The orbifold symbol is given generally as:

$\circ \dots \circ ABC \dots * abc \dots * def \dots \times \dots \times$

Here, the letters represent numbers while the symbols ($$\circ, *, \times$$), are known as the "characters" of the orbifold symbol.  By convention, the above order is typically followed.  First, the "wonder-rings" ($\circ$) are shown, which indicate the presence of translational symmetry.  Second, letters are used to denote the presence of rotational centers ($2\pi/A$, for example) which manifest as "cone points" on the orbifold. Third, the $*abc$ clause(s) which indicate kaleidoscopes (intersecting mirrors) are written; it is possible to have multiple clauses because you can technically have parallel mirrors ($**$) which do not intersect and so are not technically kaleidoscopes.  Overall, this part of the symbol represents all the possible configurations of mirror planes.  The numbers which follow the asterisk denote "corner points," which indicate angles along the boundary curve of $\pi/a$ (angle between intersecting mirrors), for example.  Finally, the crosses denote crosscaps, which represent glide reflections. To prove that this list of 4 is complete requires more topology; while it is not too detailed, it is beyond the scope of this post.  The reader is referred to Chapter 8 of Ref. <a href="#conway2">[4]</a> for the proof.

This number of orbifold "words" is infinite, as a generic orbifold can contain any number of the above symbols. There are only a few words that correspond to known crystallographic groups and their grammar will be derived in the next section.

> "The orbifold idea is the most powerful way to achieve a conceptual understanding of these groups and, in particular, it trivializes their enumeration." - Conway & Huson in Ref. <a href="#conway3">[5]</a>

So what do these actually look like?  The examples below are taken from Ref. <a href="#conway3">[5]</a> which show this orbifolding process.  The patterns are shown to the left, with the orbifold shown on the right of each instance.  If we look at the isometries present, following the guidelines given above, you can easily see the fundamental domains in these examples.  If you focus on the FD, you can see which parts of the FD are equivalent to others and which "unique" symmetries are present.  If you list those unique symmetries (remember, a periodic pattern keeps on repeating indefinitely so you could never write down *all* of them) you obtain the orbifold symbol.

<img style="float: center" src="conway_huson_examples.png" width=1000px>

While the torus and Moebius strip are straightforward, I want to emphasize the different "points" in the first case (c2mm or 2*22).  In this example we have 1 cone point (2 fold rotation) and 2 corner points (mirrors intersecting at $\pi/2$).  In a generic manifold, the homeomorphisms illustrated at the beginning of this post which "inflate the cow" would seem to suggest that we can just "bend" away those corner points.  However, as Conway and Huson caution: "an orbifold is slightly more than a topological manifold, because it inherits a metric from the original surface, which means, in particular, that angles are defined on it" <a href="#conway3">[5]</a>.  In other words, that angle is alway present locally.  It is possible to bend or deform the edges leading away from it without ruining the orbifold, but it is important that the local curvature is preserved.  To my understanding, this is a critical feature that enables the proof presented in the next section, since it involves integrating the Gaussian curvature of the orbifold.

Here is a list of all orbifolds for each the wallpaper groups from Ref. <a href="#johnson">[2]</a>.  n-gons indicate n-fold rotation centers, dashed lines are glide planes, and thick lines are mirror; "regular" lines are where the surface is folded up, indicating the boundary of the FD if it is not otherwise indcated by one of these other lines.

<img style="float: center" src="orbifold_summary.png" width=1000px>

In the table, the symmetry diagram is shown on top, with the orbifold below.

## Proof of 17 Wallpaper Groups (2D) using Orbifolds

Remarkably, Conway proved that the topological properties of the orbifolds for all crystals are unique and singular for each symmetry group. In other words, there is one and only one orbifold for each symmetry group (even though there are often many fundamental domains).  This "Magic Theorem" (his words, not mine), is truly that.  

As he and co-workers write <a href="#conway2">[4]</a>: "Any repeating pattern can be folded into an orbifold on some surface. So to prove that \[the\] list of possible orbifolds is complete, we only have to show that we've considered all possible surfaces. ... any surface can be obtained from a collection of spheres by punching holes that introduce boundaries ($*$) and then adding handles ($\circ$) or crosscaps ($\times$). Since all possible surfaces can be described in this way, we can conclude that all possible orbifolds are obtainable by adding corner points to their boundaries and cone points to their interiors."  Recall, that these points are there to encode rotational symmetries, and as stated, we need to simply add those (and preserve their curvature on the surface) to represent all isometries present in the 2D plane!  Again, this is why an "orbifold is slightly more than a topological manifold."

Importantly, this works for all surfaces of constant curvature. The 2D Euclidean plane has zero curvature (it is "flat"), but there are also planes with constant positive (spherical), or negative (hyperbolic) curvature which are also perfectly describable with orbifolds in this way. See Ref. <a href="#hyde">[6]</a> for a longer discussion on this unification.  There are some additional caveats in those cases, and Euclidean space turns out to be the easiest to work with.

Let's go through a sketch of the proof. Conway and others have previously presented proofs in a number of places, for examples, Refs. <a href="#conway">[3</a>,<a href="#conway2">4</a>,<a href="#hyde">6]</a>. You can find more details therein.  

As already stated, all surfaces can be obtained by performing "surgery" on a sphere.  We push out cones to represent rotation centers, and cut holes to create boundaries; if we glue together multiple boundaries (in a "reverse" fashion) we can make crosscaps (representing glide reflections), and if we glue on handles we can encode translations.  If we attach nothing to the boundary, it is a line along which a reflection occurs.

Importantly, all of these surgical operations change the <a href="https://en.wikipedia.org/wiki/Euler_characteristic">Euler characteristic</a> of the sphere you start from. The [Gauss-Bonnet theorem](https://en.wikipedia.org/wiki/Gauss%E2%80%93Bonnet_theorem) connects the curvature of a surface (geometry) to its Euler characteristic (topology).  Thus, if we start changing the surface's integral ("net") curvature via "surgery" we have a direct relationship to the topological properties of that surface.

$\int_M K dA + \int_{\partial M} k_g ds = 2 \pi \chi(M) $

where $\chi(M)$ is the Euler characteristic of the (compact, 2D Riemannian) manifold $M$, with boundary $\partial M$; here $k_g$ is the [geodesic curvature](https://en.wikipedia.org/wiki/Geodesic_curvature) of the boundary.  For geodesic polygons,

$\int_M K dA = (2-p)\pi + \sum \nu_i$,

where $\nu_i$ are the interior angles of the polygon.  A sketch of the proof using these facts is attached [here](proof_sketch.pdf).  The essence is that orbifold features are input surgically on the sphere, we each of which reduces the curvature, and therefore the Euler characteristic.   Surface with a characteristic (and therefore, integral curvature) of zero correspond to surfaces that can be made by folding up that "flat plane", and are therefore, all the possible orbifolds.

For a generic orbifold word $\circ^{\alpha} ABC \dots * abc \dots * def \dots \times^{\beta}$ we have:

$\chi = 2 - \left[ 2\alpha + \beta + \frac{A-1}{A} + \frac{B-1}{B} + \frac{C-1}{C} \dots + \left(1 + \frac{a-1}{2a} + \dots \right) + \left(1 + \frac{d-1}{2d} + \dots\right) \right]$

This unites a "top down" view (a sphere being surgically operated on) and a "bottom up" view (folding of a FD) to show they are representations of the same thing.  The above proof allows you to input different orbifold words and see what their $\chi$ is.  You can easily show with some testing (since all characters are integers > 1, and the bracketed term above is always positive) that there are only 17 ways to get to $\chi = 0$.  Try for yourself!

The allowable orbifold words, and visual representations, are:

1. $\circ$ (p1) - torus
2. 2222 (p2) - closed pillowcase (4-pointed pillowcase)
3. 333 (p3) - 3,3,3 turnover (3-pointed pillowcase)
4. 442 (p4) - 4,4,2 turnover (3-pointed pillowcase)
5. 632 (p6) - 6,3,2 turnover (3-pointed pillowcase)
6. *2222 (pmm) - rectangle
7. *333 (p3m1) - equilateral triangle
8. *442 (p4m) - right, isoceles triangle
9. *632 (p6m) - right, scalene triangle
10. 2*22 (cmm) - 4,4,2 turnover with a slit along one edge (4,4)
11. 3*3 (p31m) - 6,3,2 turnover with a slit along one edge (3,2)
12. 4*2 (p4m) - 4,4,2 turnover with a slit along one edge (4,2)
13. 22* (pmg) - open pillowcase, or a 2-pointed "hat"
14. xx (pg) - Klein bottle
15. ** (pm) - cylinder
16. 22x (pgg) - non-orientable football
17. *x (cm) - Moebius strip

You can find these, and more illustrations at [The Geometry Center](http://www.geom.uiuc.edu/docs/reference/CRC-formulas/node12.html#SECTION01240000000000000000). Conway's proof using orbifolds <a href="#federov">[10]</a> was not the first proof that there are 17 wallpaper groups; in fact, this list was proven to be complete in 1891, [only after](https://en.wikipedia.org/wiki/Space_group) the proof of 230 groups in three dimensions was completed.  Below are some examples of orbifolds taken from Ref. <a href="conway2">[4]</a>.

<img style="float: center" src="sot_orbifold_examples.jpg" width=1000px>

Conway uses various analogies to explain the orbifold notation and how orbifolds trivialize the description of repeating patterns on surface of constant curvature.  In Ref. <a href="#conway">[3]</a> he uses the analogy of an amusement park called ["SymmetryLand"](https://books.google.com/books?hl=en&lr=&id=HmoDWHA8lE8C&oi=fnd&pg=PA438&dq=the+orbifold+notation+for+surface+group&ots=T6sLvtY_gg&sig=-JJayjcjsNKStLsy8N_i8hX3u4g#v=onepage&q=the%20orbifold%20notation%20for%20surface%20group&f=false) in which the cost of tickets ($2) serves as analogy for the "cost" of performing the surgical operations to encode features in the orbifold.

Hyde <it>et al.</it> in Ref. <a href="#hyde">[6]</a> also present a nice summary, derivation, and explanation and cataloging of the orbifold notation in different spaces, beyond just Euclidean space.

## Topologically Distinct Domains

If crystallographic orbifolds are (1) just "folded up" FDs that, in Euclidean dimensions, correspond to all surfaces with a $\chi = 0$, and (2) they are unique for each wallpaper group, then it follows that one should be able to recover the FD if one is presented with an orbfold simply by "cutting" it back open.  Indeed, this approach was pioneered by Daniel Huson and Olaf Delgado-Friedrichs <a href="conway2">[4]</a>.  

> "A graph on the orbifold will be the boundary of a fundamental domain if it cuts the orbifold into a topological disk that has no internal cone point and can be opened flat onto the plane."

For example, the p6 (632) group is illustrated below in chapter 25 of Ref. <a href="conway2">[4]</a>:

<img style="float: center" src="632_SOT_example.jpg" width=1000px>

In other words, if you take the orbifold for a given wallpaper group, all the possible FDs correspond to all the different ways you could cut it back open to fall "flat" back on the plane.  This means that the graph has to go through each symmetry operator.  You can intuitively understand this since the FD is also known as the "asymmetric unit", i.e., it contains no symmetry within it; so the graph has to create a boundary on the orbifold that puts all the symmetry operators (features like cones) on itself so that none are contained "inside" the FD once it is unfolded.

There are a total of 46 such tiles that could be formed, which had been found previously through other methods.  Gruenbaum and Shepherd list these as "motif-transitive proper subgroups" of each wallpaper group which are primitive <a href="gs">[11]</a>. Both M. C. Escher, as an artist, and Heesch and Kienzele <a href="hk">[12]</a>, as mathematicians, have undertaken studies of this "regular division of the plane."  The latter were particularly concerned with tiles that could "interlock", i.e., those that do not require any straight edges.  Escher was also primarily focused on this case, though not exclusively.  A nice website devoted to the math and art of Escher can be found [here](https://mathstat.slu.edu/escher/index.php/Math_and_the_Art_of_M._C._Escher)

> "The use of imagery from life led to restrictions on possible choices for symmetry for aesthetic reasons, restrictions which Escher gradually evolved over time. Examining the Regular Division of the Plane Drawings, one finds that the vast majority fall into one of seven symmetry groups: p1, p2, p3, p4, p6, pg, and pgg. These are exactly the symmetry groups which have no reflection symmetry - only translation, rotation, and glide reflection. If two creatures meet on a line of mirror symmetry, they must have a flat edge, and recognizable figures from life rarely have perfectly straight edges. Because of this, Escher mostly avoided mirror symmetry, although he did create a few drawings where bilateral symmetry of the motif leads to overall mirror symmetry of the pattern" - See [here](https://mathstat.slu.edu/escher/index.php/Wallpaper_Patterns).

Heesch's naming system is constrained to these 7 groups as well, for the same reason: they must contain no mirror planes.  Interesting, mirror "images" are still possible as a result of glide planes.  The fact that reflections are possible without mirrors is why Conway refers to these operations as "miracles" <a href="conway2">[4]</a>.  In Heesch's system sides of a tile are labelled with letter: T, C, or G.  If an edge is a translated version of another edge, those labelled with a "T". If an edge results from a rotation (cyclic symmetry) about one corner of a neighbor, these edges are denoted with "C"s and are given a subscript to denote the angle of rotation: N for 360$^{\circ}/N$ degrees, though 2 is (180$^{\circ}$) is not indicated.  Moreover, if any edge is a (180$^{\circ}$) rotation of itself, it is just considered a single edge, not two.  Finally, if one edge is a result of a glide operation (flip and translate) the two edges are labelled with "G"s; if there are more than one glide pair they are given subscripts to disambiguate them, but otherwise have no subscript.  More details can be found [here](http://www.eschertile.com/), [here](http://www.tessellations.org/tess-symmetry7.shtml) and in the [Tesselmania!](http://jwilson.coe.uga.edu/EMT668/EMAT6680.2002.Fall/AllenL/MATH%207200/Escher%20Project/Tesselmania/tesselmania.html) program.

<img style="float: center" src="Heesch-tiles.png" width=1000px>

There are a total of 28 Heesch types, corresponding to all FDs for these 7 groups; thee image above is from Ref. <a href="hk">[12]</a>.  These are a subset of the 46 total tiles mentioned earlier.  These tiles are the same, so the crystal or tesselation that results is made from tiles of all the same "shape" or face, so are referred to as "isohedral."
 In fact, it is also possible to derive "composite" tiles which also tile the plane with the same shape.  If the FDs are the "atomic" unit of a crystal, is also possible to have topologically distinct units if several of them combine to form "molecular" units; such units contain the symmetry of the crystal.  For example, it is possible to have a tile with an internal mirror plane such that half of it forms a FD, and the mirror plane simply aligns with a mirror plane running through the crystal. These correspond to non-primitive subgroups in Ref. <a href="gs">[11]</a>.  

It turns out that you can also derive these isohedral tiles using the orbifolds as well.  You simply need to change the "cutting rules" used to find the ones that are FDs in order to get the rest.  Previously, we had to cut so that all the symmetry operations fell on the boundary we cut along; now, we may at most leave (1) a single cone point in the interior, or (2) a cone point on the boundary (a corner point).  We may also "ignore" parts of the boundary (mirrors) which makes them "part" of the tile.  Thus, all isohedral tiles can be enumerated directly from the orbifold.  There are 47 isohedral tiles that do not correspond to FDs, for a total of 93.  

Importantly, these cuttings do NOT change the property of the tiling.  These are simply different ways that you can divide the pattern (remember an orbifold is a "quotient" space!) into equal pieces.  For many applications, this does not make any difference; this is why "standard" FDs can be defined in the [International Tables for Crystallography](https://it.iucr.org/Ab/), for example.  One convention is just as good as another if you are describing the arrangement of points in space, such as atoms.  

However, it can sometimes a convenient choice can be very relevant for design.  It is common practice for crystallographers to define FDs "around" molecules to form their "outline". In the example below, taken from [here](http://pd.chem.ucl.ac.uk/pdnn/symm3/asymunit.htm), the outline of the molecule is qualitatively drawn to illustrate this, though in this case it is in 3D.

<img style="float: center" src="sg4asym4.gif" width=410px>

Below, on the right, is a mutation of a simple box with p1 symmetry into Escher's Pegasus depicted earler, taken from [here](http://www.eschertile.com/kids/animate.htm).  More such examples can be found at [eschertile.com](http://www.eschertile.com/kids/animate.htm).  For comparison, the isohedral tile (IH) is also shown.  It is clear that we could either divide the picture into squares, or into Pegasus-shaped objects.  Topologically they are the same, but one is far more intuitive that the other.

<img style="float: center" src="david-a6.gif" width=300px>
<img style="float: center" src="p1-Regular-division-105.jpg" width=225x>

As a final example, consider the "fish" below.  Here, I have drawn two possible divisions of space according to two different IH tiles.  IH52 is a more "conventional" cell you might find in the International Tables for Crystallography or another standard reference, while IH5 is the one used to actually generate the tesselation.

<img style="float: center" src="p2gg-451px-Regular-division-10.jpg" width=300px>

As a final counterexample, examine the picture below.  In it, Escher has drawn the same shape (tile) but inside he has draw two different motifs by sketching different lines and curves.  One appears to be a "flying fish" and other a bird.  However, careful examination reveals that they have the same outline.  Thus, they are the same tile.  Only the interior of the FD has been changed, which is perfectly acceptable in the context of art.  It violates no rules, because the "inside" of the FD represents all degrees of freedom, whereas the boundary encodes the symmetry.

<img style="float: center" src="Regular-division-80.jpg" width=1000px>

# In summary

Orbifolds provide a unique and general way to describe symmetry and tilings, and are particularly useful in 2D.  Extensions to 3D can be performed, but with some effort.  The interested reader should refer to Ref. <a href="conway2">[4] for an overview.  They make explicit the connection between tiling and symmetry and represent a different approach to understanding the mathematics which underpin these concepts.  This has application is design, material science, art, and engineering which in many regards, remains underutilized to this day.

* Orbifolds describe a "folded" fundamental domain (FD).
* The FD contains the symmetrically unique sites (think independent degrees of freedom).
* The FD, aka "asymmetric unit", has all the symmetry elements on its edge or boundary, which is usually only roughly half "independent" since these operations map one edge to a symmetrically equivalent one.
* The sequence of these pairings is how Heesch represented isohedral tilings.

# Citations

<p id="thurston">[1] Thurston, William P. <it>The geometry and topology of three-manifolds.</it> Princeton, NJ: Princeton University, 1979.</p>
<p id="johnson">[2] Johnson, Carroll K., Michael N. Burnett, and William D. Dunbar. "Crystallographic Topology and its Applications." <it>Crystallographic Computing</it> 7 (1996).</p>
<p id="conway">[3] Conway, J. H. in <it>Groups, combinatorics and geometry.</it> Vol. 165. pp 438-447, Cambridge University Press, 1992</p>
<p id="conway2">[4] Conway, John H., Heidi Burgiel, and Chaim Goodman-Strauss. <it>The Symmetries of Things.</it> CRC Press, 2008.</p>
<p id="conway3">[5] Conway, John H., and Daniel H. Huson. "The Orbifold Notation for Two-dimensional Groups." <it>Structural Chemistry</it> 13.3 (2002): 247-257.</p>
<p id="hyde">[6] Hyde, S. T., S. J. Ramsden, and Vanessa Robins. "Unification and Classification of Two-dimensional Crystalline Patterns using Orbifolds." <it>Acta Crystallographica Section A: Foundations and Advances</it> 70.4 (2014): 319-337.</p>
<p id="macbeath">[7] Macbeath, A. M. "The classification of non-euclidean plane crystallographic groups." <it>Canadian Journal of Mathematics</it> 19 (1967): 1192-1205.</p>
<p id="pretti">[8] Pretti, Evan, <it>et al.</it> "Symmetry-Based Crystal Structure Enumeration in Two Dimensions." <it>The Journal of Physical Chemistry A</it> 124.16 (2020): 3276-3285.</p>
<p id="doris">[9] Schattschneider, D. <it>M.C. Escher: Visions of Symmetry.</it> Thames & Hudson, 2004.</p>
<p id="federov">[10] Federov, E. <it>Symmetry in the plane.</it> Proceedings of the Imperial St. Petersburg Mineralogical Society, 28 (1891): 345-390.</p>
<p id="gs">[11] Gruenbaum, Branko, and Geoffrey Colin Shephard. Tilings and Patterns. Second Edition. Courier Dover Publications, 2016.</p>
<p id="hk">[12] Heesch, Heinrich, and Kienzle, Otto. <it>Flaechenschluss; System der Formen lückenlos aneinanderschliessender
Flachteile.</it> Berlin: Springer, 1963. 


