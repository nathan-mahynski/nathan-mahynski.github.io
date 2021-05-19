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

In this series of papers we reconsidered the task of creating planar interfaces that require a unique surface pattern to be functional. In a system that self-assembles, such as an appropriate colloidal suspension, there are many possibilities, but the one that we might expect is the one with the lowest free energy.  However, a ensemble of possibilities needs to be generated in order to screen them for the most thermodynamically stable one.  To this end, we developed an algorithm, inspired by symmetry and the concept of [orbifolds](/notes/orbifolds), that can enumerate point patterns as quickly as is possible in two Euclidean dimensions.  This enables one to generate different candidate patterns that are particularly relevant for atomic crystals made of atoms that are similar in size, or out of colloidal crystals made from similarly sized spherical constituents.  This method enumerates structures according to rules of symmetry; compared to random searching, this enables the enumeration of "important" candidate structures trillions of times faster, reducing a calculation that would otherwise take the age of the universe to complete (13.8e9 years) to a mere 15 minutes.  This code is available in Ref. 3 above, and is further described in Ref. 2.

Many of the graphics below have been presented at scientific conferences including:

* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Physical Society March Meeting, Denver, CO USA (03/2021).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, GRC: Colloidal, Macromolecular & Polyelectrolyte Solutions, Ventura, CA USA (02/2020). 
* ``Grand canonical inverse design of multicomponent colloidal assemblies,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Institute of Chemical Engineers Annual Meeting, Orlando, FL USA (11/2019).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, American Institute of Chemical Engineers Annual Meeting, Pittsburgh, PA USA (11/2018).
* ``Symmetry-based discovery of multicomponent, two-dimensional colloidal crystals,'' N. A. Mahynski, E. Pretti, V. K. Shen, J. Mittal, Foundations of Molecular Modeling and Simulation, Delavan, WI USA (07/2018). 

Please cite appropriately if you find this material to be helpful.

# 2D Materials from Programmable Matter

2D materials important in a number of areas
Progammable matter for self-assembly, but scale is an issue
Inverse design has a couple problems: (1) produces physically unrealizable potentials.  the solution might to be use many simple components and achieve complexity through diversity, but (2) gibbs phase rule/phase separation is an issue with many practice aspects of inverse design due to cell size.
Other things like floppy box MC exist to just "try" and see what you get; you can also look at databases or use ML inspired approaches.  but at the end of the day these are all somewhat random and not systematic.
Instead we need to have a way to enumerate plasible candidates quickly and systematically

# Using Symmetry to Count Configurations

## Suppressing the Combinatorial Explosion

FD smaller than unit cell so combinatorial explosion suppressed

Furthermore, symmerty, as described by orbifolds reveals symmatrically equivalent edges so furhter reduces DoF

However, need to even compare across groups - if you cleverly choose the FD correclty you can do this.  We use a parallelogram divided along long acxis

Boundary condition ~ symmetry

## Wyckoff Positions

Orbifolds are basically manifolds with embedded wyckoff sites (see post, part from Johnson paper).  Others have used Wyckoff and isopointal sets (glotzer). Also paper by Doye
All sites have stoichiometric factor = mukltiplicity (general vs. special)

## Putting it Together

Can define colloidal sudoku puzzle and recusively solve
This creates a tree that cen be defined in the memory state of computer
you can also sample the tree stocahastically
define 'compexity" in terms of D_kl

## Why is this "better"?

High symmetry hypothesis from David Wales
Can directly generate high symmetry candidates


# Exploring Phase Space

Look at example from paper using LJ-lambda potential

# Optimizing the Exploration

Using GPR to optimize where you go looking.
Imclude video

# Conclusions

Open to feedback and collaborations!


