---
layout: single
author_profile: false
sidebar:
  nav: "resources"
classes:
  - landing
  - wide
excerpt: ""
title: "Resources"
permalink: /resources/
header:
  image_description: ""
  overlay_image: /assets/img/logo/wombo_splash.JPG
  overlay_filter: 0.35 # same as adding an opacity of 0.5 to a black background
#  caption: "www.nist.gov"
#  cta_label: "NIST Profile"
#  cta_url: "https://www.nist.gov/people/nathan-mahynski"
---

<!-- {% include toc icon="gears" title="Table of Contents" %} -->

<!--

# Research Opportunities
---
I am currently accepting postdoctoral applications for those interested in working on some of my research [focus areas](/research/).  There are also new opportunities for undergraduate and high school students who are interested in learning about research in chemical informatics and engineering.

1. [NRC Postdoctoral Opportunity](http://nrc58.nas.edu/RAPLab10/Opportunity/Opportunity.aspx?LabCode=50&ROPCD=506461&RONum=C0449&ROBaseMode=R100) You must be a US citizen to apply for this program.  [Contact me](mailto:nathan.mahynski@gmail.com) for details on how to apply.
2. [NIST Summer Undergraduate Research Fellowship (SURF) Program](https://www.nist.gov/surf).  This is an excellent opportunity for undergraduate students to get hands-on experience performing research to help prepare for an advanced degree in STEM.  Former alumni include:
  - Bliss Han, 2021, Brown Univ. Environmental Engineering
  - Daniel Markiewitz, 2021, Cornell Univ. Chemical Engineering -> Ph.D. @ Massachusetts Institute of Technology
  - Evan Pretti, 2018, Lehigh Univ. Chemical Engineering -> Ph.D. @ The University of California, Santa Barbara
  - Kamryn Kant, 2018, Clemson Univ. Chemical Engineering
  - Sally Jiao, 2017, 2018, Princeton Univ. Chemical Engineering -> Ph.D. @ The University of California, Santa Barbara
3. [NIST Summer High School Intern (SHIP) Program](https://www.nist.gov/ohrm/summer-high-school-intern-program)
-->

# Open Source Code
---

Open source codes used in my research can be found on [GitHub](https://www.github.com/mahynski) and major packages are summarized below.
<br/>

<!--
[![mahynski's github stats](https://github-readme-stats.vercel.app/api?username=mahynski&show_icons=true&theme=tokyonight&hide_border=true)](https://github.com/mahynski)
-->
<!-- ![1](https://github-readme-stats.vercel.app/api/top-langs/?username=mahynski&theme=tokyonight&hide_border=true) -->

* [pychemauth](https://github.com/mahynski/pychemauth) is a toolkit, implemented in python, to perform chemometric authentication using various types of data. These methods are designed to be compatible with [scikit-learn's](https://scikit-learn.org/stable/index.html) estimator API so that they can be deployed in pipelines and used with GridSearchCV, etc. These tools may be applied more generally, but this implementation is tailored to the needs of the chemometric authentication community.

* [PACCS](https://github.com/usnistgov/paccs) is primarily a colloidal crystal structure generation and optimization library written in Python. It supports stochastic optimization of periodic multicomponent systems in two and three dimensions, as well as the generation of candidate structures in two dimensions using wallpaper groups.  It was initially developed by Evan Pretti as his SURF project in 2018.

<a href="https://pages.nist.gov/feasst/"><img style="float: right;" src="/assets/img/feasst_logo.png"></a>

* The [Free Energy and Advanced Sampling Simulation Toolkit (FEASST)](https://pages.nist.gov/feasst/) is a free, open-source, modular program to conduct molecular and particle-based simulations with flat-histogram Monte Carlo methods, primarily developed by Wick Hatch at NIST.

# An Interactive Periodic Table
---

<!-- 
This is stored in the _includes directory.
Remember to remove the explicit DOCTYPE declaration at the top that Bokeh creates.
-->

{% include interactive_periodic_table.html %}

See the code to generate this [here](/examples/periodic_table).
