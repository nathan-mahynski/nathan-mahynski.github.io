---
title: "Developing Chemometric Fingerprints for Ecological Systems"
excerpt: "The analytical chemistry inside seabirds, fish, and food."
header:
  image: 
  teaser: /assets/img/stamp.png
gallery:
  - url: /assets/images/unsplash-gallery-image-1.jpg
    image_path: assets/images/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
  - url: /assets/images/unsplash-gallery-image-2.jpg
    image_path: assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
  - url: /assets/images/unsplash-gallery-image-3.jpg
    image_path: assets/images/unsplash-gallery-image-3-th.jpg
    alt: "placeholder image 3"
classes:
  - wide
tags:
  - 
---

{% include toc icon="gears" title="Table of Contents" %}

# The Importance of Chemometric Authentication 

## The Basic Problem

As globalization continues and international supply chains increase in complexity, it is increasingly difficult to reliably trace the source of commodities from the point of consumption or use back to their origin.  While this can be said of many consumer products, food and food products are some of the most high profile because of the risk their adulteration poses to human and environmental health. The U.S. Food and Drug Administration (FDA) reported nearly 800 food recalls from 2017 through 2019, of which 51% were attributed to the presence of undeclared allergens and 40% were based microbial contamination <a href="nist1251">[1]</a>. The Centers for Disease Control and Prevention estimates that each year, 1 in 6 Americans get sick and 3,000 die from consumption of contaminated foods and beverages.  This is because foods and related products may be (usually) unintentionally adulterated or contaminated with harmful chemicals (e.g., pesticides) or biological factors (e.g., mycotoxins) during the adulteration process.  The financial cost of global food fraud is estimated at 10-15 billion USD <a href="manning">[2]</a>, and an estimated 600 million people fall ill while 420,000 die globally every year after eating contaminated food <a href="gizaw">[3]</a>. These losses are only expected to increase in the coming years, due to globalization and the increasing vulnerability of supply chains <a href="robson">[4]</a>, which leads to the suspicion that more nefarious schemes to intentionally adulterated foods are on the rise. 

This so-called "food fraud" often involves "economically motivated adulteration" (EMA), which is the fraudulent addition of non-authentic substances or removal or replacement of authentic ones for the explicit purpose of deceiving the purchases for the economic gain of the seller <a href="johnson">[5]</a>. Some of the most salient examples in recent years include:

* In 1981 over 300 Spanish citizens died (and 20,000 were sickened) from a musculoskeletal disease termed ["toxic oil syndrome"](https://en.wikipedia.org/wiki/Toxic_oil_syndrome). It was traced to the consumption of colza oil intended for industrial purposes, not human consumption; however, to evade taxes it was illegally doped with aniline, then refined once imported.  It was sold as "olive oil" to consumers. There were a number of [issues proving the pathogenesis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1240833/) of TOS due to main components in this oil which has led to a number of alternative theories. 

* The [2008 "Chinese Milk Scandal"](https://en.wikipedia.org/wiki/2008_Chinese_milk_scandal) involved the addition of melamine to milk to boost apparent protein content so that it could pass quality assurance tests; [melamine](https://en.wikipedia.org/wiki/Melamine) is used in the industrial production of plastics, fertilizers and even concrete. This adulteration killed 6 babies and sickened nearly 300,000 Chinese citizen. More than 20 people were convicted for their roles in the scandal, and 2 were even executed.

* In 2009, over 600 people were sickened and 9 [killed by salmonella in peanut butter](https://www.wsj.com/articles/SB123491912215304853) as a result of intentional evasion of food safety practices.

* ["Horsegate"](https://foodfraudadvisors.com/horsemeat-scandal-analysis/) was uncovered in 2013 when Irish authorities revealed they had found horsemeat in "100% beef" hamburger patties.

* ["Project Honeygate"](https://www.reuters.com/article/usa-china-honey/u-s-charges-five-in-honeygate-anti-dumping-probe-idUSL1N0BKCRX20130220) refers to the operation which started in 2008 and concluded in the 2013 uncovering that Chinese honey was being illegally imported to the US to avoid tariffs associated with anti-dumping regulations. Some of this honey was found to contain chloramphenicol, an antibiotic considered unsafe for use in the US, and has been called the ["largest food fraud in US history."](https://www.bloomberg.com/news/articles/2013-09-19/how-germany-s-alw-got-busted-for-the-largest-food-fraud-in-u-dot-s-dot-history) In fact, honey is the third most adulterated food product because of its limited production and high value; some tests have even found evidence of [food fraud in more than 75% of honeys](https://www.foodsafetynews.com/2011/11/tests-show-most-store-honey-isnt-honey/) sold in US grocery stores.

* A recent [Indian Honeygate](https://www.indiatoday.in/india/story/honeygate-and-china-connection-cse-s-adulteration-investigation-reveals-shocking-details-1746416-2020-12-03) was reported in 2020 when an investigation reported that honey from 13 top brands in India showed signs of adulteration with Chinese syrups that can defeat conventional food safety and authenticity tests.

EMA is particularly rampant in certain areas.  For example, Manuka Honey is an iconic product of New Zealand, widely regarded for its purported health benefits; the country has an annual production of roughly 1,700 tons, yet in 2013 as much as 10,000 tons of "manuka honey" appeared on the global marketplace <a href="zhou">[6]</a>.  This is because honey is difficult to produce and highly valued, creating a large economic driver for adulteration.  While crude oil and natural gas usually steal the spotlight as the no. 1 and no. 3 most traded commodities in the world, [coffee comes in at no. 2](https://www.bllnr.sg/leadership/what-are-commodities-and-what-are-the-top-10-most-traded-commodities-in-the-world).  Since a mature coffee tree [produces only about 2 pounds](https://www.ncausa.org/About-Coffee/What-is-Coffee#:~:text=The%20average%20coffee%20tree%20produces,world%20called%20the%20Coffee%20Belt.) of green beans per year, coffee (which the reader may be drinking at this very moment) is also particularly susceptible to EMA.

## Trust But Verify

How can we verify a food or product is what it says it is on its label? Adulterations come in many forms and new ones are constantly being developed. "Targeted analysis" is a term that refers to methods by which a specific component or chemical is tested for in a product.  For example, one can perform a specific chemical test to check for the presence of known antibiotic or biomarker of a known adulterant.  Assuming this biomarker specifically and uniquely represents the target, its presence or absence yields a simple "yes or no" to the question of "is this product adulterated with this species?"  However, the situation is rarely that simple.  Moreover, most such tests are relatively slow and cannot be done in real time, say, at a port or other area of importation. If we assume we have a "pure" product made up of 2 components $a$ and $b$ in a certain ratio, then it is possible to artificially create this product by combining pure $a$ and pure $b$ in some mixing ratio. However, most products are highly complex chemical mixtures and their constituents are not readily available in pure form.  Perhaps, in this example, $a$ and $b$ are present in a lower grade forms of the product (1 and 2), but to pass a quality assurance tests the ratio must be adjusted.  It may be possible to find another product (3) with mostly $a$ and some other chemical(s) $c$, and simply add this to the lower grade form to increase $a$.  This is the idea behind adding melamine to diluted milk to boost the apparent protein (in actuality, nitrogen) content.  It is then possible to find volumes of the three ingredients, $V_1, V_2, V_3$, that can be combined to give the right amount of component volumes, $V_a, V_b$ to pass a test. 

$$
\overbrace{
\begin{bmatrix}
V_1, & V_2, & V_3 \\
\end{bmatrix}}^{V}

\overbrace{
\begin{bmatrix}
x_{1,a}, & x_{1,b}, & x_{1,c}=0 \\
x_{2,a}, & x_{2,b}, & x_{2,c}=0 \\
x_{3,a}, & x_{3,b}=0, & x_{3,c}>0 \\
\end{bmatrix}}^{X}

=

\overbrace{
\begin{bmatrix}
V_{a} \\
V_{b} \\
V_{c} \\
\end{bmatrix}}^{Y}
$$

If the total volume, $V = V_a + V_b + V_c$, the ratio of the necessary components, $y = V_a / V_b$, and the available amount of one of the one of needed components, e.g., $V_a$, are known then it may be possible to solve the above matrix expression for the volumes of each ingredient ($V_1, V_2, V_3$) needed to create a fake product: $V = Y X^{-1}$.  This assumes the fractions ($X$) of components in each ingredient are known in advance.  If we are lucky, it may happen that $X$ is not invertible; however, this essentially amounts to saying that ingredients 1 and 2 are identical (strictly, that $x_{1,a}x_{2,b} - x_{2,a}x_{1,b} = 0$), i.e., that it is somehow impossible to find two ingredients with $a$ and $b$ in different amounts.  This is unlikely in practice, meaning this example can almost always be solved.  A targeted analysis can then be fooled by this because $a$ and $b$ are present in the right amount, but it will miss the presence of $c$ altogether since it is not tested for. Dangers arise when $c$ is a component that is harmful to human or environmental health, for example.

Enter: non-targeted analyses (NTA).  These tests provide a more holistic representation of a sample of interest. Examples include spectroscopy, such an [infrared (IR)](https://en.wikipedia.org/wiki/Infrared_spectroscopy) or [nuclear magnetic resonance (NMR)](https://en.wikipedia.org/wiki/Nuclear_magnetic_resonance_spectroscopy).  These produce a signal that is a complex result of all things present in the sample.  While it is sometimes harder to tease out information directly from this, this holistic "fingerprint" is likely to be affected by the presence of $c$, although it may be difficult to know exactly how in advance.  Consequently, NTA methods are generally perceived as the most promising approach to food authentication, though targeted analyses still play an important role.

Chemometric techniques, including aspects of what is currently enveloped by the term "machine learning", are generally applied to this NTA data to build models that can predict the authenticity of a given sample. An important distinction should be made, though, between "hard" and "soft" classification models. The former ("hard") are what are conventionally applied and developed in machine learning applications, usually via ["supervised learning"](https://scikit-learn.org/stable/supervised_learning.html) methods. In these methods, a model is shown a training set with several classes, such as one pure and two known adulterations (A1 and A2), and explicitly learn "boundaries" between them, usually after a dimensionality reduction step.  In the figure shown, these boundaries are straight red lines representing (hyper)planes.  Assuming that the training data is representative of real world variation it seems likely that this hard classifier will be very successful at distinguishing the "pure" samples, A1, and A2 from each other in the future.  However, this is not really the question we wish to ask.  In actuality, we want to know what it is that uniquely distinguishes "pure" or A1; hard classifiers are biased toward their training set in that they generally establish metrics that are optimal at distinguishing BETWEEN the known classes, rather than characterizing an individual class.  In other words, they find the boundary, not the "centroid" of the class. As a result, hard classifiers assign membership to one class and only one class; moreover, they cannot distinguish if something is "none" of the classes it knows about.

<img style="float: right" src="dim_red.png" width=400px>

Conversely, "soft" classifiers find conditional probabilities of class membership and assign (potentially multiple) membership based on that <a href="liu">[7]</a>. Soft methods can be used to build discrimination (hard) boundaries, but here I will use the term a bit more loosely to encompass what may be more precisely called "class modeling"; this modeling is used to characterize individual classes rather than differences between any of them. This soft methodology can, and must, <a href="pomerantsev">[8]</a> be applied to authentication problems. This is because we must establish a model to characterize only what we can faithfully observe and statistically sample well.  It reasonable to expect that one can collect samples of a validated class, such as "pure" honey; whereas, there are many ways to adulterate honey and new methods are constantly being developed, so you cannot hope to reasonably sample this "infinite" number of possibilities. This is also why "one-vs-rest", or effectively binarizing the data into "target" and "not-target" classes, is inappropriate; the "not-target" class can never be considered "well sampled." Soft/class modeling methods generally function by (1) performing a dimensionality reduction to find a characteristic subspace, then (2) drawing a statistical (probabilistic) boundary around the data observed, then (3) testing class membership by examining whether or not a sample falls inside that boundary.  One-class-classifiers (OCC, aka class models) can do this by only looking at a single class at a time <a href="perera">[9]</a>. The example above shows these soft boundaries as ellipses around the three known classes; a fourth in magenta illustrates why this is important.  The hard classifier whould indicate this is authentic and pure, but it is clearly suspicious.  Perhaps this is, indeed, pure and simply collected from a different part of the field or under different conditions (such as particularly hot or cold year) relative to the food in training set; or, perhaps this a new form of adulteration.  Clearly, more investigation is required and we would like our classifier to able to flag that "no" this is not the same as "pure", but it's not necessarily adulterated either.  This information is returned to the user so they can investigate further. Effectively, this means that "unknown unknowns" can be identified! This is why soft classifiers are generally required to solve authentication problems, though exceptions may be justifiable in certain scenarios. I discuss and illustrate some common chemometric examples [here](/examples/common_chemometrics/) implemented in python.

Since class modeling / soft methods seek to characterize a class based on its most salient "features", the dimensionality reduction step plays a key role.  This step essentially determines the space in which characteristic aspects of the data manifest.  Historically, chemometric methods have relied upon linear approaches, like PCA, for a variety of reasons, but often for reasons of interpretability.  In general, one can increase the dimensionality of space, for example with kernels, to get [good separation between classes](https://en.wikipedia.org/wiki/Kernel_principal_component_analysis) (when supervised methods are used) in the case that they are not linearly separable in lower dimensions.  Of course, one must consider the ["curse of dimensionality"](https://en.wikipedia.org/wiki/Curse_of_dimensionality) which implies that exponentially more data is needed to train a classifier as the dimensionality increases.  Alternatively, non-linear boundaries may be sought after, or more powerful non-linear dimensionality reduction approaches could be used.  In either case, the interpretability of the boundary and/or the projected space becomes much harder. Being able to interpret a black box model is critical to adopting any model in the food authenticity space since this generally informs legal policy and other aspects that directly affect the everyday lives of most people.  Therefore, modern [explainable machine learning](/notes/interpretable_machine_learning) methods are critical to understanding why a good model performs well, so that one can connect characteristic features of the food to its adulteration method (or lack thereof). Conversely, they can also reveal why inaccurate or weak models perform poorly, so that one can develop an intuition for the type of information that they may be lacking.  This can also be used to assess how and why classifications are made which can reveal biases or other unexpected problems in the model, often originating in the training data itself.

<!--<img style="float: right" src="fig3.png" width=1000px>-->

All considered, the most optimal general approach to building authentication models seems to be:
1. use (non-)linear dimensionality reduction methods to build 
2. soft / class models that are as accurate as possible with (generally) minimal data, then
3. explain them with tools like [SHAP](shap.readthedocs.io/).  

This general pipeline forms the basic workflow we can apply to many problems. In practice, many of the common chemometric approaches are still the easiest to explain and interpret making them stronger candidates for the basis of regulation.

## Steps Forward

<a href="https://pubs.rsc.org/en/content/articlehtml/2015/ay/c5ay02048d"><img style="float: right" src="analytical_methods_cover.png" width=400px></a>

Before any mathematical model can be built, accurate data must be collected.  This is actually somewhat of an interative process, as suggested by the workflow above, because it is not always clear what sort of measurement will provide sufficiently discriminatory information.  Often times, multiple different measurements need to be made and combined in a process generally referred to as "data fusion."  Ideally, NTA methods like spectroscopy can be leveraged to build "fingerprints" for known samples; however, this almost always requires a great deal of testing and feedback to identify methods which are sufficiently information rich as to be useful.  Targeted methods also require ongoing development and testing to screen for biomarkers and other specific indicators of adulterations.  A primary issue in the field of food authenticity and security is a lack of sufficient data.  It can be very expensive and time-consuming to collect data from different exemplifying regions or conditions, and therefore commands a great deal of attention.  Nonetheless, in the examples which follow from my own research, we have sought to tackle the search for biomarkers and other identifying chemometric signatures in a variety of different arenas using various types of chemometric data.

# Seabirds of the North Pacific Ocean

Coming Soon!

<!--
<img style="float: right" src="compounds.png" width=350px>

In this project, my collaborators and I sought to identify chemometric trends in anthropogenic toxins in the North Pacific ocean.  These toxins include [organochlorine pesticides](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5464684/), [per- and poly- fluoralkyl compounds (PFAS)](https://en.wikipedia.org/wiki/Per-_and_polyfluoroalkyl_substances), [polychlorinated biphenyls (PCBs)](https://en.wikipedia.org/wiki/Polychlorinated_biphenyl), and heavy metals such as mercury. 

> [In 1999, the U.S. Fish and Wildlife Service Alaska Maritime National Wildlife Refuge (USFWS-AMNWR), the U.S. Geological Survey Biological Resources Division (USGS-BRD), and NIST implemented the Seabird Tissue Archival and Monitoring Project (STAMP) to monitor contaminants in Alaska's marine environments. In 2010, the 111th Congress directed NIST to expand this and other programs into the U.S. Pacific Islands. STAMP was designed as an ongoing long-term effort to track geographic and temporal trends in environmental quality by collecting seabird eggs using standardized protocols, processing and banking the contents under conditions that ensure chemical stability during long-term (decadal) storage, and analyzing subsamples of the stored material to determine baseline levels of persistent bioaccumulative contaminants.](https://www.nist.gov/programs-projects/seabird-tissue-archival-and-monitoring-project-stamp)

<a href="https://www.grida.no/resources/5453"><img style="float: right" src="biomagnify.png" width=350px></a>

Seabird eggs are useful chemometric indicators because these birds exist near the top of the food chain, and act as a reservoir for these toxins via [biomagnification](https://en.wikipedia.org/wiki/Biomagnification).  Egg contents are fairly stable over long periods of time so they can be entered into animal [biobanks](https://en.wikipedia.org/wiki/Biobank) and analyzed at later dates.  Furthermore, because birds feed over broad areas they represent a reasonable average of the Pacific environment.

Overall, we found that chemometric signatures can, indeed, be correlated with a range of ecological and anthropogenic factors suggesting seabird eggs can act as biomonitors for a range of things related to regional, and potentially large scale, oceanographic changes.  Chemometric signatures manifest with enough statistical power to make these inferences which suggests that longitudinal monitoring and collection efforts on such samples may result in a wealth of information.

## The Data

<img style="float: right" src="seabirds.png" width=350px>

To understand any spatiotemporal trends, we started by curating analytical chemistry data for eggs collected from 1999-2010 on a subset of species and analytes that were measured regularly and reasonably systematically. Thousands of STAMP samples have been archived in the [NIST Biorepository](https://www.nist.gov/programs-projects/nist-biorepository) at [Hollings Marine Laboratory](https://www.nist.gov/mml/hollings-marine-laboratory). Longitudinal monitoring efforts of this nature provide invaluable data that enables the assessment of both wildlife and human exposures to environmental contaminants as these species often consume prey (e.g., fish) similar to, and from sources (e.g., oceanic) comparable to, human populations nearby. In certain areas, seabird eggs are also a significant part of subsistence diets for indigenous peoples. Chemometric profiles and related health implications are known to differ across species <a href="dietz">[10]</a>. Eggs, however, can be difficult to assign to a species unless the bird is observed on the nest from which the sample was collected due to similar appearance within a genus and [sympatric nesting behavior](https://en.wikipedia.org/wiki/Sympatry). This represents a large point of uncertainty for both wildlife managers and exposure researchers alike.

More details can be found in Refs. <a href="mahynski1">[11]</a> and <a href="mahynski2">[12]</a>, but here is a very basic summary of some of the main points.  The dataset essentially contained eggs from 5 different seabird species (Common murre, Thick-billed murre, Laysan albatross, Glaucous gull, and Glaucous-winged gull); a total of more than 400 different samples were analyzed for the presence of more than 100 different persistent bioaccumulative toxins, stable isotope ratios, and other chemical species. 

## Taxonomical Trends

Birds of a feather, do indeeed, flock together.  Within this dataset, an egg's genus (murre vs. gull vs. albatross) is relatively easy to distinguish (more than 97% accurate) using principal components analysis or similar methods.  However, these methods construct low dimensional spaces by linearly combining the available features to create axes that are, consequently, a function of all of the measurands.  This means you need to measure more than 100 different properties of a new sample if you want to use such a model to make a prediction about that egg's provenance.  Instead, non-linear methods, such as decision trees, can achieve similar performance with much less information.  Below is an example of a shallow decision tree that achieved similar performance with only 4 different analytes.  This greatly reduces the burden of **performing these tests** making it easier to implement management and testing strategies based on these models in practice.

<img style="float: center" src="genus_tree.png" width=1000px>

It would seem that species could be distinguished easily as well, however, due to the fact that murre species tend to "flock together" (exhibit [sympatric nesting](https://en.wikipedia.org/wiki/Sympatry), etc.) they are [congeneric](https://www.merriam-webster.com/dictionary/congener).  They seem to feed in similar areas on similar things, and live and nest together; since they exist in the same environment, barring major biological differences (metabolic pathways, etc.) which are not expected in such similar animals, we would anticipate that their chemometric signatures would reflect this environemental similarity.  Indeed, the distribution of all analytes between these different murre species are almost entirely overlapping.  <img style="float: right" src="species_lda.png" width=350px> Below is an example of a linear discriminant analysis (LDA) projection of these classes into three dimensional space.  The blue and orange points are the murres; they clearly overlap, but the other species tend to exist in different regions of this latent space.  This holds true in higher dimensions as well.  More advanced feature engineering approaches were unable to create signatures that had statisically significant distinguishing power.  Thus, considering the murres as a single species for biomonitoring efforts seems to be a reasonablte conclusion.  This simplifies collection and monitoring efforts since the particular species of murre does not seem to matter.

## Spatiotemporal Trends

Two particularly salient trends were noted: (1) total mercury concentration seems correlated with a sample's broad geographic region, and more generally (2) colony prediction models can be trained to be fairly accurate with their primary confusion coming from nearby colonies.  Both suggest that geographic and regional differences can be detected using chemometric signatures.  This is generally attributed to anthropogenic sources, such as mining.  Other geographical correlations with nitrogen isotopes can also be found, which are also attributed to such sources (agriculture).

<img style="float: center" src="total_hg.png" width=1000px>
 -->

# Oyster Provenance

Coming Soon!

# On the Origin of Strawberries

Coming Soon!

# The Problem with Honey

Coming Soon!

<!--
## The Problem with Honey

Although fraud has been reported in the majority of food products <a href="johnson">[5]</a>, according to the US Pharmacopeia’s Food Fraud Database honey is the third most popular food target for adulteration, behind only milk and olive oil. This is because honey is a natural food with high economic and sensorial value, health benefits, and low production <a href="fak">[13]</a>. With increasing demand and a relatively fixed production of authentic honey, the allure of trafficking in adulterated honey is increasing dramatically <a href="phipps">[14]</a>. According to the United Nations Food and Agriculture Organization the global number of beehives increased from roughly 70 million in 2001 to 91 million in 2017. This represents a modest 29% increase compared to global honey exports that increased 91% during a comparable timeframe (from 357,535 metric tons (MT) in 2001 to 682,792 MT in 2018). Although the exact amount of fraudulent honey in the world is debated, a nonprofit organization called the Honey Authenticity Project (HAP) has estimated that [one-third of internationally traded honey is either adulterated or completely fake](https://www.honeyap.org/). 

Honey can be adulterated directly by mixing it with raw sugars or syrups, by blending it with lower quality or synthetic honeys, or indirectly by "bee-feeding" with low-quality honey or industrial sugars <a href="cord">[15]</a>.  Another common practice is harvesting immature (unripe) honey then reducing the water content using vacuum dryers or filters; ion-exchange resins can also be used to remove residues of illicit substances (e.g., veterinary drugs) which has the side effect of also removing natural components that contribute to the aroma, taste, and beneficial properties of honey. All of these represent vectors for otherwise foreign harmful chemical (e.g., antibiotics, pesticides) and biological (e.g., mycotoxins) factors connected to a range of conditions including fatty liver, acute and chronic kidney injury, elevated body fat, increased inflammation, and toxicity <a href="fak">[13]</a>. Furthermore, intentionally mislabeling a honey’s botanical and geographical origin is also practiced to mislead consumers, as is often the case with [manuka honey](https://www.webmd.com/a-to-z-guides/manuka-honey-medicinal-uses).

Western Europe and the US are the predominant honey importers, while the major exporters include China, New Zealand, Argentina, and India. Many honey-exporting countries from the east (China, Ukraine, India, and Vietnam), showed stable exports between 2001 and 2009, followed by a steep increase; this is very difficult to justify given their levels of beekeeping activity, floral sources, and historical production <a href="garcia">[16]</a>. China was the US's biggest supplier until 2001 when incidences of low-quality honey with the presence of illegal and unsafe antibiotics and sugar syrups led to heavy US tariffs and antidumping regulations.

Honey quality and authenticity are determined by its sensory, chemical, physical, and microbiological characteristics. Traditionally, targeted methods such as isotopic measurements are used for the detection of adulteration with different sugars and to detect bee overfeeding <a href="tsa">[17]</a>. Chromatographic analysis and high-resolution mass spectrometry can be used for the detection of specific adulterants and origin assessment <a href="zhou2">[18]</a>, while immunological, molecular (genetic) techniques and physicochemical analysis have been mainly used to determine botanical origin. Targeted methods like this specifically test for a known set of markers and cannot provide the holistic representation needed to detect a broad range of continuously evolving contaminants and adulteration methods. Non-targeted multi-parameter screening techniques, however, are analytical methods that provide information about multiple aspects of the honey in one test. Vibrational and fluorescence spectroscopy are examples of non-targeted approaches for "fingerprinting" authentic honeys to detect adulteration <a href="tsa">[17]</a>. These methods can be used to "scan" samples quickly and accurately, without destroying the sample itself.

As discussed above, conventional classification models discriminate between known classes used during model training. The resulting model will always return a prediction that an observation belongs to one of these known classes; however, authentication must perform the more complex task of identifying an observation as being a known unadulterated (pure) sample, a known adulteration, or an unknown. These models are required for authentication, since novel adulteration methods are constantly being developed to defeat quality assurance tests. Class modeling techniques are "soft" classification methods designed to do this, and generally operate by performing a (semi)supervised dimensionality reduction to obtain a compressed subspace, and then create a statistical boundary around a region in that space which characterizes the training set.

This dimensionality reduction identifies the most characteristic aspects of the classes, enabling them to be separated from each other. As adulteration methods become more advanced, methods to detect these subtle changes is key, meaning that the data's structure must be carefully preserved during this step. The majority of food authentication studies currently rely on linear dimensionality reduction methods, such as principal components analysis (PCA); common examples include Soft Independent Modeling of Class Analogies (SIMCA) and Partial Least-Squares (PLS)-based methods (PLS may be considered nonlinear overall, but it employs linear dimensionality reduction internally). These were first developed within the chemometrics community over 40 years ago <a href="wold">[19]</a> though advances have been made in the intervening years.  A model that could convincingly be used to develop international trade and health policies should arguably have an accuracy bordering the "6 sigma" limit (99.99966%) to instill adequate consumer confidence; yet such models rarely achieve this, either due to a lack of data quality or quantity, or because of inherent model weaknesses, such as the inability to completely separate classes A1 and A2 in the cartoon shown in the introduction. With the recent growth of machine learning's popularity, novel classification and manifold-based learning models have emerged which now offer a wide range of approaches that have achieved this performance in many arenas, for example, in self-driving cars. Modern neural network architectures, such as variational autoencoders (VAE), and non-linear manifold reduction techniques including diffusion maps, the uniform manifold approximation and projection or UMAP, and pairwise controlled manifold approximation projection or PaCMAPs represent some of the leading new methods capable of finding semi-arbitrary subspaces which characterize the data while preserving, to varying degrees, its local and global properties <a href="wang">[20]</a>.  We hypothesize these agile methods will be able to preserve subtleties in the data much better than linear methods, which will enable us to develop novel and powerful soft classifiers to perform class modeling.  

The cost of such methods is their complexity, and inherent difficulty in understanding how and why they make their predictions. Thus, there is typically a preference for inherently interpretable (simpler, less accurate) models, especially when leveraging machine learning in high-stakes decision making. Therefore, it is insufficient to simply develop these next-generation models, they must also be trustworthy to ensure consumer confidence. Fortunately, recent advances have shown how game-theoretic principles, namely Shapley values, can be leveraged to [explain arbitrarily complex models](https://arxiv.org/abs/1705.07874) by economically assigning importance to features in a model; these are known as [SHapley Additive exPlanations (SHAP)](https://github.com/slundberg/shap) and can be used to explain the importance of any feature in any machine learning model’s decision making process.  SHAP essentially works by allocating optimal credit to each model feature (input) in terms of their contribution to a class assignment; e.g., if a model predicts with 85% certainty that a sample has been adulterated, it can quantitatively connect each feature (analyte concentration, or NMR peak value, for example) to numbers which sum to 0.85. 

This approach has been shown to unite many other types of model explanation methods and allows the scientist to understand why a prediction or class assignment is made, as well as how.  Being model agnostic, this approach may be applied to any model one develops. This is critical to understanding why a good model performs well, so that one can connect characteristic features of the honey to its adulteration method (or lack thereof). Conversely, it will also help reveal why inaccurate or weak models perform poorly, so that one can develop an intuition for the type of information that they may be lacking. This allows one determine if a chemometric signature (from NMR, for example) truly does contain enough information to perform authentication, or if additional measurements are necessary. SHAP has even been successfully deployed in surgical environments to help explain machine learning model predictions that anticipate hypoxaemia during surgery to the anaesthesiologists in real time <a href="lundberg">[21]</a>. Leveraging these explanation techniques in the food authenticity space would represent a major leap forward in developing trustworthy models for food authentication. 
-->

<!--
# IAEA / Stable Isotope and Trace Element (SITE) Database

# Slovenian Strawberries, etc. with Nives

# Colaboration with Chile
-->

# Recirculating Aquaculture Systems (RAS)

Coming Soon!

<!--
The United States is the largest importer of seafood and more than 90% of the seafood consumed in the US is imported. The US consumes roughly 500,000 tons of Atlantic salmon (Salmo salar) per year! Global demand continues to rise as well, however, capture fisheries are already at (or have exceeded) their sustainable yield limits. To meet the growing demand, aquaculture is perceived as being a sustainable route forward.  In fact, more than half of the US seafood imports are from aquaculture sources. Compared to terrestrial animal farming, aquaculture is a relatively young industry.  Modern aquaculture practices [began in Norway](https://seafoodfromnorway.us/Stories-from-Norway/the-gift-keeps-on-giving/innovating-a-new-industry---aquaculture/) in the late 20th century, and it was some time until this received more mainstream attention.

[Aquaculture](https://www.fda.gov/food/seafood-guidance-documents-regulatory-information/aquacultured-seafood) is often performed by breeding (broodstock) in net pens or tanks. Embryos are held in shallow trays and hatched under careful supervision before the yolk-sac fry (alevin) are transferred to ocean-based net pens to mature until harvest. This is a well established process with relative low infrastructure cost.  A rapidly growing alternative is to move entirely to a land-based system, called recirculating aquaculture systems (RAS). RAS facilities do not need to be located near (specific parts of) the ocean and enable fine control over the environment which helps avoid certain pathogens, e.g., sea lice, which can cause considerable economic harm in ocean-based counterparts.

Critical to RAS, and any aquaculture system, is an effective broodstock management program. Broodstock quality is correlated with offspring health, growth, fecundity, and disease resistance. Understanding the factors that comprise broodstock "quality" is central to developing a sustainable aquaculture industry in the future. Working with the [Hamlin Lab](https://umaine.edu/marine/people/heather-hamlin/) at the University of Maine, NIST, the US Department of Agriculture (USDA), and other federal partners we have set out to search for chemometric signatures that can be correlated with a fish's "quality"; i.e., predicting the likelihood of survival and growth of embryos and yolk-sac fry given traits of the parents. This can be used to inform hatchery managers of whether to cull or plan for a sub-optimal performance.
-->

<!--
The ability to reliably predict embryo survival is a considerable problem in the commercial aquaculture industry. <img style="float: right" src="embryo.png" width=350px> Specifically with regards to Atlantic salmon, survival rates of embryos in hatcheries have fallen from over 80% to roughly half that in recent years (data provided by [Cooke Aquaculture](https://aquaculturegrowsns.ca/)).  Embryo growth is a labor intensive process and represents a large investment for any commercial enterprise, so being able to reliably predict which fish to cull and which to breed is critical to developing an economical and sustainable RAS industry. 
-->

<!--
The National Cold Water Marine Aquaculture Center (NCWMAC) in Franklin, Maine, is the USDA’s largest applied Atlantic salmon selective breeding and genetic improvement facility in the US. We collected mucus swabs of broodstock Atlantic salmon classified as "brood" or "cull" during one of the USDA NCWMAC’s routine sorting events. Mucus swabs are less invasive than blood samples, are easier to collect, and we hypothesize that chemical signatures pertaining the metabalomic state (and overall health, or quality) of the fish can be ascertained from this.  In particular, we postulate that this non-targeted approach can be used to extract information that correlates with the fish quality class (brood or cull).
-->

# References

<p id="nist1251"><a href="https://doi.org/10.6028/NIST.SP.1251">[1] "Harnessing Measurement Science to Advance Food Safety", National Institute of Standards and Technology Special Publication 1251 (2020).</a></p>

<p id="manning"><a href="https://onlinelibrary.wiley.com/doi/abs/10.1111/1750-3841.13256">[2] Manning L., Soon J.M., "Food Safety, Food Fraud, and Food Defense: A Fast Evolving Literature," J. Food Sci. 81 (2016) 823–834.</a></p>

<p id="gizaw"><a href="https://link.springer.com/article/10.1186/s12199-019-0825-5">[3] Gizaw Z., "Public health risks related to food safety issues in the food market: A systematic literature review," Environ. Health Prev. Med. 24 (2019) 1–21.</a></p>

<p id="robson"><a href="https://www.sciencedirect.com/science/article/pii/S0956713520304321">[4] Robson K., Dean M., Haughey S., Elliott C., "A comprehensive review of food fraud terminologies and food fraud mitigation guides." Food Control 120 (2021) 107516.</a></p>

<p id="johnson"><a href="http://www.fredsakademiet.dk/ORDBOG/lord/food_fraud.pdf">[5] Johnson R., "Food fraud and economically motivated adulteration of food and food ingredients", Congressional Research Service, Washington D.C. (2014).</a></p>

<p id="zhou"><a href="https://www.nature.com/articles/s41598-018-32764-w">[6] Zhou X., Taylor, M.P., Salouros, H., Prasad, S., "Authenticity and geographic origin of global honeys determined using carbon isotope ratios and trace elements," Sci. Rep. 8 (2018) 1–11.</a></p>

<p id="liu"><a href="https://dx.doi.org/10.1198/jasa.2011.tm10319">[7] Liu Y., Zhang H.H., Wu Y. "Hard or soft classification? Large-margin unified machines," Journal of the American Statistical Association, 106 (2011) 166-177.</a></p>

<p id="pomerantsev"><a href="http://dx.doi.org/10.1016/j.trac.2016.01.010">[8] Rodionova O.Y., Titova A.V., Pomerantsev A.L., "Discriminant analysis is an inappropriate method of authentication," Trends in Analytical Chemistry, 78 (2016) 17-22.</a></p>

<p id="perera"><a href="https://arxiv.org/abs/2101.03064">[9] Perera P., Oza P., Patel V.M., "One-class classification: A survey," arXiv:2101.03064 (2021).</a></p>

<p id="dietz"><a href="https://www.sciencedirect.com/science/article/pii/S0048969719337337">[10] Dietz R., et al., "Current state of knowledge on biological effects from contaminants on arctic wildlife and fish," Science of the Total Environment 696 (2019) 133792.</a></p>
<!--
<p id="mahynski1"><a href="https://doi.org/10.6028/jres.126.028">[11] Mahynski N.A. et al., "Seabird Tissue Archival and Monitoring Project (STAMP) Data from 1999-2010," Journal of Research of the National Institute of Standards and Technology 126 (2021).</a></p>

<p id="mahynski2"><a href="">[12] Mahynski N.A., et al., "Chemometric Differentiation of Seabirds in the North Pacific Ocean," (in prep.)</a></p>

<p id="fak"><a href="https://www.mdpi.com/2304-8158/9/11/1538">[13] Fakhlaei R. et al. "The toxic impact of honey adulteration: A review," Foods, 9 (2020) 1–21.</a></p>

<p id="phipps"><a href="https://www.apiservices.biz/en/articles/sort-by-date-up-online/2612-international-honey-market-report-february-2021">[14] Phipps R., "International Honey Market Report - February 2021," American Bee Journal, 159 (2021) 1–6. Retrieved 12/17/2021. </a></p>

<p id="cord"><a href="https://doi.org/10.1016/j.aca.2004.10.018">[15] Cordella C., et al., "Detection and quantification of honey adulteration via direct incorporation of sugar syrups or bee-feeding: Preliminary study using high-performance anion exchange chromatography with pulsed amperometric detection (HPAEC-PAD) and chemometrics," Analytica Chimica Acta, 531 (2005) 239–248.</a></p>

<p id="garcia"><a href="https://doi.org/10.1080/0005772x.2018.1483814">[16] Garcia N.L., "The Current Situation on the International Honey Market," Bee World 95 (2018) 89–94.</a></p>

<p id="tsa"><a href="https://doi.org/10.1039/d1ra00069a">[17] Tsagkaris A.S., et al., "Honey authenticity: analytical techniques, state of the art and challenges," RSC Advances 11 (2021) 11273–11294.</a></p>

<p id="zhou2"><a href="https://doi.org/10.1016/j.foodchem.2013.08.117">[18] Zhou J., et al., "Floral classification of honey using liquid chromatography-diode array detection-tandem mass spectrometry and chemometric analysis," Food Chemistry 145 (2014) 941–949.</a></p>

<p id="wold"><a href="https://doi.org/10.1016/0031-3203(76)90014-5">[19] Wold S., "Pattern recognition by means of disjoint principal components models," Pattern Recognition 8 (1976) 127–139.</a></p>

<p id="wang"><a href="https://www.jmlr.org/papers/volume22/20-1061/20-1061.pdf">[20] Wang Y., et al., "Understanding How Dimension Reduction Tools Work: An Empirical Approach to Deciphering t-SNE, UMAP, TriMAP, and PaCMAP for Data Visualization," ArXiv:2012.04456 (2020) 1–63.</a></p>

<p id="lundberg"><a href="https://doi.org/10.1038/s41551-018-0304-0">[21] Lundberg S. M., et al., "Explainable machine-learning predictions for the prevention of hypoxaemia during surgery," Nature Biomedical Engineering 2 (2018) 749–760.</a></p>
-->
