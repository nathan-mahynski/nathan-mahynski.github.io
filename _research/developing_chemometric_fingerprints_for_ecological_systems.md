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

# Why Food Security Matters

## The Basic Problem

As globalization continues and international supply chains increase in complexity, it is increasingly difficult to reliably trace the source of commodities from the point of consumption or use back to their origin.  While this can be said of many consumer products, food and food products are some of the most high profile because of the risk their adulteration poses to human and environmental health. The U.S. Food and Drug Administration (FDA) reported nearly 800 food recalls from 2017 through 2019, of which 51% were attributed to the presence of undeclared allergens and 40% were based microbial contamination <a href="nist1251">[1]</a>. The Centers for Disease Control and Prevention estimates that each year, 1 in 6 Americans get sick and 3,000 die from consumption of contaminated foods and beverages.  This is because foods and related products may be (usually) unintentionally adulterated or contaminated with harmful chemicals (e.g., pesticides) or biological factors (e.g., mycotoxins) during the adulteration process.  The financial cost of global food fraud is estimated at 10-15 billion USD <a href="manning">[2]</a>, and an estimated 600 million people fall ill while 420,000 die globally every year after eating contaminated food <a href="gizaw">[3]</a>. These losses are only expected to increase in the coming years, due to globalization and the increasing vulnerability of supply chains <a href="robson">[4]</a>, which leads to the suspicion that more nefarious schemes to intentionally adulterate foods are on the rise. 

This so-called "food fraud" often involves "economically motivated adulteration" (EMA), which is the fraudulent addition of non-authentic substances or removal or replacement of authentic ones for the explicit purpose of deceiving the purchases for the economic gain of the seller <a href="johnson">[5]</a>. Some of the most salient examples in recent years include:

* In 1981 over 300 Spanish citizens died (and 20,000 were sickened) from a musculoskeletal disease termed ["toxic oil syndrome"](https://en.wikipedia.org/wiki/Toxic_oil_syndrome). It was traced to the consumption of colza oil intended for industrial purposes, not human consumption; however, to evade taxes it was illegally doped with aniline, then refined once imported.  It was sold as "olive oil" to consumers. There were a number of [issues proving the pathogenesis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1240833/) of TOS due to main components in this oil which has led to a number of alternative theories. 

* The [2008 "Chinese Milk Scandal"](https://en.wikipedia.org/wiki/2008_Chinese_milk_scandal) involved the addition of melamine to milk to boost apparent protein content so that it could pass quality assurance tests; [melamine](https://en.wikipedia.org/wiki/Melamine) is used in the industrial production of plastics, fertilizers and even concrete. This adulteration killed 6 babies and sickened nearly 300,000 Chinese citizen. More than 20 people were convicted for their roles in the scandal, and 2 were even executed.

* In 2009, over 600 people were sickened and 9 [killed by salmonella in peanut butter](https://www.wsj.com/articles/SB123491912215304853) as a result of intentional evasion of food safety practices.

* ["Horsegate"](https://foodfraudadvisors.com/horsemeat-scandal-analysis/) was uncovered in 2013 when Irish authorities revealed they had found horsemeat in "100% beef" hamburger patties.

* ["Project Honeygate"](https://www.reuters.com/article/usa-china-honey/u-s-charges-five-in-honeygate-anti-dumping-probe-idUSL1N0BKCRX20130220) refers to the operation which started in 2008 and concluded in the 2013 uncovering that Chinese honey was being illegally imported to the US to avoid tariffs associated with anti-dumping regulations. Some of this honey was found to contain chloramphenicol, an antibiotic considered unsafe for use in the US, and has been called the ["largest food fraud in US history."](https://www.bloomberg.com/news/articles/2013-09-19/how-germany-s-alw-got-busted-for-the-largest-food-fraud-in-u-dot-s-dot-history) In fact, honey is the third most adulterated food product because of its limited production and high value; some tests have even found evidence of [food fraud in more than 75% of honeys](https://www.foodsafetynews.com/2011/11/tests-show-most-store-honey-isnt-honey/) sold in US grocery stores.

* A recent [Indian Honeygate](https://www.indiatoday.in/india/story/honeygate-and-china-connection-cse-s-adulteration-investigation-reveals-shocking-details-1746416-2020-12-03) was reported in 2020 when an investigation reported that honey from 13 top brands in India showed signs of adulteration with Chinese syrups that can defeat conventional food safety and authenticity tests.

EMA is particularly rampant in certain areas.  For example, Manuka Honey is an iconic product of New Zealand, widely regarded for its purported health benefits; the country has an annual production of roughly 1,700 tons, yet in 2013 as much as 10,000 tons of “manuka honey” appeared on the global marketplace. <a href="zhou">[6]</a>.  This is because honey is difficult to produce and highly valued, creating a large economic driver for adulteration.  While crude oil and natural gas usually steal the spotlight as the no. 1 and no. 3 most traded commodities in the world, [coffee comes in at no. 2](https://www.bllnr.sg/leadership/what-are-commodities-and-what-are-the-top-10-most-traded-commodities-in-the-world).  Since a mature coffee tree [produces only 1-1.5 pounds](https://askinglot.com/how-much-is-a-coffee-tree) of roasted coffee per year, coffee (which the reader may be drinking at this very moment) is also particularly susceptible to EMA.

## The Basic Solution

Adulterations come in many forms and new ones are constantly being developed. "Targeted analysis" is a term that refers to methods by which a specific component or chemical is tested for in a product.  For example, one can perform a specific chemical test to check for the presence of known antibiotic or biomarker of a known adulterant.  Assuming this biomarker specifically and uniquely represents the target, its presence or absence yields a simple "yes or no" to the question of "is this product adulterated with this species?"  However, the situation is rarely that simple.  Moreover, most such tests are relatively slow and cannot be done in real time, say, at a port or other area of importation. If we assume we have a "pure" product made up of 2 components $a$ and $b$ in a certain ratio, then it is possible to artificially create this product by combining pure $a$ and pure $b$ in some mixing ratio. However, most products are highly complex chemical mixtures and their constituents are not readily available in pure form.  Perhaps, in this example, $a$ and $b$ are present in a lower grade forms of the product (1 and 2), but to pass a quality assurance tests the ratio must be adjusted.  It may be possible to find another product (3) with mostly $a$ and some other chemical(s) $c$, and simply add this to the lower grade form to increase $a$.  This is the idea behind adding melamine to diluted milk to boost the apparent protein (in actuality, nitrogen) content.  It is then possible to find volumes of the three ingredients, $V_1, V_2, V_3$, that can be combined to give the right amount of component volumes, $V_a, V_b$ to pass a test. 

$$
\overbrace{
\begin{bmatrix}
V_1, & V_2, & V_3 \\
\end{bmatrix}}^{V}

\overbrace{
\begin{bmatrix}
x_{1,a}, & x_{1,b}, & x_{1,c}=0 \\
x_{2,a}, & x_{2,b}, & x_{2,c}=0 \\
x_{3,a}, & x_{3,b}=0, & x_{3,c} \\
\end{bmatrix}}^{X}

=

\overbrace{
\begin{bmatrix}
V_{a} \\
V_{b} \\
V_{c} \\
\end{bmatrix}}^{Y}
$$

If the total volume, $V = V_a + V_b + V_c$, the ratio of the necessary components, $y = V_a / V_b$, and the available amount of one of the one of needed components, e.g., $V_a$, are known then it may be possible to solve the above matrix expression for the volumes of each ingredient ($V_1, V_2, V_3$) needed to create a fake product: $V = Y X^{-1}$.  This assumes the fractions ($X$) of components in each ingredient are known in advance.  If we are lucky, it may happen that $X$ is not invertible; however, this essentially amounts to saying that ingredients 1 and 2 are identical, i.e., that it is somehow impossible to find two ingredients with $a$ and $b$ in different amounts.  This is unlikely in practice, meaning this example can almost always be solved.  A targeted analysis can then be fooled by this because $a$ and $b$ are present in the right amount, but it will miss the presence of $c$ altogether since it is not tested for. Dangers arise when $c$ is a component that is harmful to human or environmental health, for example.

Enter, non-targeted analyses (NTA).  These tests provide a more holistic representation of a sample of interest. Examples include spectroscopy, such an [infrared (IR)](https://en.wikipedia.org/wiki/Infrared_spectroscopy) or [nuclear magnetic resonance (NMR)](https://en.wikipedia.org/wiki/Nuclear_magnetic_resonance_spectroscopy).  These produce a signal that is a complex result of all things present in the sample.  While it is sometimes harder to tease out information directly from this, this holistic "fingerprint" is likely to be affected by the presence of $c$, although it may be difficult to know exactly how in advance.  Consequently, NTA methods are generally perceived as the most promising approach to food authentication, though targeted analyses still play an important role.

Chemometric techniques, including aspects of what is currently enveloped by the term "machine learning", are generally applied to this NTA data to build models that can predict the authenticity of a given sample. An important distinction should be made, though, between "hard" and "soft" classification models. The former ("hard") are what are conventionally applied and developed in machine learning applications, usually via ["supervised learning"](https://scikit-learn.org/stable/supervised_learning.html) methods. In these methods, a model is shown a training set with several classes, such as one pure and two known adulterations (A1 and A2), and explicitly learn "boundaries" between them, usually after a dimensionality reduction step.  In the figure shown, these boundaries are straight red lines representing (hyper)planes.  Assuming that the training data is representative of real world variation it seems likely that this hard classifier will be very successful at distinguishing the "pure" samples, A1, and A2 from each other in the future.  However, this is not really the question we wish to ask.  In actuality, we want to know what it is that uniquely distinguishes "pure" or A1; hard classifiers are biased toward their training set in that they generally establish metrics that are optimal at distinguishing BETWEEN the known classes, rather than characterizing an individual class.  In other words, they find the boundary, not the "centroid" of the class. As a result, hard classifiers assign membership to one class and only one class; moreover, they cannot distinguish if something is "none" of the classes it knows about.

<img style="float: right" src="dim_red.png" width=400px>

Conversely, "soft" classifiers find conditional probabilities of class membership and assign (potentially multiple) membership based on that <a href="liu">[7]</a>. Soft methods can be used to build discrimination (hard) boundaries, but here I will use the term a bit more loosely to encompass what may be more precisely called "class modeling"; this modeling is used to characterize individual classes rather than differences between any of them. This soft methodology can, and must, <a href="pomerantsev">[8]</a> be applied to authentication problems. This is because we must establish a model to characterize only what we can faithfully observe and statistically sample well.  It reasonable to expect that one can collect samples of a validated class, such as "pure" honey; whereas, there are many ways to adulterate honey and new methods are constantly being developed, so you cannot hope to reasonably sample this "infinite" number of possibilities. This is also why "one-vs-rest", or effectively binarizing the data into "target" and "not-target" classes, is inappropriate; the "not-target" class can never be considered "well sampled." Soft/class modeling methods generally function by (1) performing a dimensionality reduction to find a characteristic subspace, then (2) drawing a statistical (probabilistic) boundary around the data observed, then (3) testing class membership by examining whether or not a sample falls inside that boundary.  One-class-classifiers (OCC, aka class models) can do this by only looking at a single class at a time <a href="perera">[9]</a>. The example above shows these soft boundaries as ellipses around the three known classes; a fourth in magenta illustrates why this is important.  The hard classifier whould indicate this is authentic and pure, but it is clearly suspicious.  Perhaps this is, indeed, pure and simply collected from a different part of the field or under different conditions (such as particularly hot or cold year) relative to the food in training set; or, perhaps this a new form of adulteration.  Clearly, more investigation is required and we would like our classifier to able to flag that "no" this is not the same as "pure", but it's not necessarily adulterated either.  This information is returned to the user so they can investigate further. Effectively, this means that "unknown unknowns" can be identified! This is why soft classifiers are generally required to solve authentication problems, though exceptions may be justifiable in certain scenarios. Some common chemometric examples in python can be found [here](/examples/common_chemometrics/).

Since class modeling / soft methods seek to characterize a class based on its most salient "features", the dimensionality reduction step plays a key role.  This step essentially determines the space in which characteristic aspects of the data manifest.  Historically, chemometric methods have relied upon linear approaches, like PCA, for a variety of reasons, but often for reasons of interpretability.  In general, one can increase the dimensionality of space, for example with kernels, to get [good separation between classes](https://en.wikipedia.org/wiki/Kernel_principal_component_analysis) (when supervised methods are used) in the case that they are not linearly separable in lower dimensions.  Of course, one must consider the ["curse of dimensionality"](https://en.wikipedia.org/wiki/Curse_of_dimensionality) which implies that exponentially more data is needed to train a classifier as the dimensionality increases.  Alternatively, non-linear boundaries may be sought after, or more powerful non-linear dimensionality reduction approaches could be used.  In either case, the interpretability of the boundary and/or the projected space becomes much harder. Being able to interpret a black box model is critical to adopting any model in the food authenticity space since this generally informs legal policy and other aspects that directly affect the everyday lives of most people.  Therefore, modern [interpetable machine learning](/notes/interpretable_machine_learning) are critical to understanding why a good model performs well, so that one can connect characteristic features of the food to its adulteration method (or lack thereof). Conversely, they can also reveal why inaccurate or weak models perform poorly, so that one can develop an intuition for the type of information that they may be lacking.  This can also be used to assess how and why classifications are made which can reveal biases or other unexpected problems in the model, often originating in the training data itself.

<img style="float: right" src="fig3.png" width=1000px>

All considered, the most optimal general approach to building authentication models seems to be:
1. use non-linear dimensionality reduction methods to build 
2. soft / class models that are as accurate as possible with (generally) minimal data, then
3. explain them with tools like [SHAP](shap.readthedocs.io/).  

This general pipeline forms the basic workflow we can apply to many problems.

## Steps Forward

<a href="https://pubs.rsc.org/en/content/articlehtml/2015/ay/c5ay02048d"><img style="float: right" src="analytical_methods_cover.png" width=400px></a>

Before any mathematical model can be built, accurate data must be collected.  This is actually somewhat of an interative process, as suggested by the workflow above, because it is not always clear what sort of measurement will provide sufficiently discriminatory information.  Often times, multiple different measurements need to be made and combined in a process generally referred to as "data fusion."  Ideally, NTA methods like spectroscopy can be leveraged to build "fingerprints" for known samples; however, this almost always requires a great deal of testing and feedback to identify methods which are sufficiently information rich as to be useful.  Targeted methods also require ongoing development and testing to screen for biomarkers and other specific indicators of adulterations.  A primary issue in the field of food authenticity and security is a lack of sufficient data.  It can be very expensive and time-consuming to collect data from different exemplifying regions or conditions, and therefore commands a great deal of attention.  Nonetheless, in the examples which follow from my own research, we have sought to tackle the search for biomarkers and other identifying chemometric signatures in a variety of different arenas using various types of chemometric data.

# Seabirds of the Northern Pacific Ocean

## The Data

## Taxonomical Trends

Binomial nomenclature

## Spatiotemporal Trends

# Recirculating Aquaculture Systems (RAS)

Hamlin lab
work from
proposal

smithsonian, U Maine, USDA partners

# Predicting Provenance

IAEA
provenance of food stocks
honey

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
