---
title: "Jensen-Shannon Divergence as an EDA Tool"
excerpt: "Using Jensen-Shannon divergence to search for good features and patterns."
header:
  teaser: /assets/img/divergence.png
tags:
  - machine learning
  - eda
  - feature engineering
  - decision trees
---

{% include toc icon="gears" title="Table of Contents" %}

# What is the Jensen-Shannon Divergence?

The [Jensen-Shannon divergence](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence) (JSD) is way to describe the difference between two distributions.  It is essentially the mean [Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) of a pair of distributions from their mean.

$$
JSD(P || Q) = \frac{KLD(P || M) + D(Q || M)}{2}
$$

where $M = \frac{P+Q}{2}$.  The Kullback-Leibler divergence (KLD) is defined as:

$$
KLD(P || Q) = \sum_x P(x) {\rm log}\left( \frac{P(x)}{Q(x)} \right)
$$

Importantly, $P$ and $Q$ are **normalized probability distributions** so be sure to normalize these values.  Thus, unlike the KL divergence, the JSD is symmetric, and importantly, if a base of 2 is used in the logarithm it is bounded by $0 < JSD < 1$. These are convenient properties as they allow you to quickly estimate how much information you might derive from a feature used in a decision tree, for example.  

# Developing an Intuition

A large JSD between 2 distributions suggests that one or more boundaries can be found to distinguish them.  It may be that one distribution is is "higher" that another, or that they might interwoven, as illustrated below.  For example, JSD = 1 for either of the instances shown below.

<img style="float: center" src="js_example1.png" width=600px>

While contrived, it is clear that a decision tree could select distribution A or B (i.e., class A or B), based on cutoffs of $x$.  For the example on the right, a simple stump at $x = 2$ would suffice; for the example on the left, a deeper tree with multiple splits (left and right) would be required, but possible.  Often, the former case (at right above) appears when examining different classes with different characteristic distributions of $x$; the latter (at left above) is more often a symptom of a poorly sampled class (not enough data) where the bins are too fine.  This will be addressed later.

These distributions are disjoint and "maximally different", so the JSD = 1; but when some overlap occurs, the JSD goes down.  In this example, each distribution has 3 bars where 2 are unique for each.  The resulting JSD = 2/3.

<img style="float: center" src="js_example2.png" width=600px>

When the distributions are identical, the JSD = 0.

# JS Divergence Reveals Plausible Tree Stumps

As alluded to above, a large JSD between classes for a feature suggests it might be a good feature to use, particularly in simple decision trees.  If the goal is to find a simple model like this, JSD can be a helpful feature selection step.  Note that because class information is used, this is a supervised technique and should only be performed on the training set (or appropriately using pipelines with cross validation).  For example, an sklearn workflow might [look like this](https://github.com/mahynski/ml_utils/blob/main/sklearn_ext/feature_selection.py):

~~~ python
pipeline = imblearn.pipeline.Pipeline(
		steps=[
			("smote", ScaledSMOTEENN(random_state=1)),
			("selector", JensenShannonDivergence(top_k=1,
				per_class=True,
				feature_names=X.columns)),
			("tree", DecisionTreeClassifier(random_state=1))
		])

param_grid = [{
		'smote__k_enn':[3, 5, 7, 10],
		'smote__k_smote':[3, 5, 7, 10],
		'smote__kind_sel_enn':['all', 'mode'],
		'tree__max_depth':np.arange(1,4+1),
		}]

ncv = ml_utils.cross_validation.NestedCV(k_inner=2, k_outer=5)
results = ncv.grid_search(pipeline, param_grid, X.values, y.values)
~~~

Assume we have some class data with 2 features distributed as follows:

<img style="float: center" src="js_example3.png" width=600px>

We can compute the JSD by doing a "one vs. all" (OvA) comparison; that is, construct the 2 distributions: one from a class of interest, the other from all other classes.  After normalizing each, we can compute the JSD to see if the class of interest is sufficiently disjoint along this feature to be considered insightful.  A threshold of 0.7 was chosen, and is shown below.  Clearly, Feature 2 seems to be a good way to distinguish class 2 from the other classes, as expected.

<img style="float: center" src="js_example4.png" width=600px>

With a lower threshold, you might also consider Feature 1 to be helpful for distinguishing class 0 from classes 1 and 2.  In the sklearn workflow given above, the [JensenShannonDivergence](https://github.com/mahynski/ml_utils/blob/main/sklearn_ext/feature_selection.py) is used to select the single best dimensions  (top_k=1) for each class; this way you don't have to worry about setting a threshold.  Clearly, this k can also be a hyperparameter that may be optimized also.  See the documentation from the [ml_utils](https://github.com/mahynski/ml_utils) package for details on using this feature selection method as shown.

While it might be straightforward to see such trends when you have a small number of features and classes, with many of them this can be a hard task.  JSD using this OvA approach reduces this to a single flaoting point number which can be visualized easily with a heatmap, for example.  

**In essence, a high JSD value for a feature with a chosen class means that class "sticks out" from the rest of the pack.**

# Identifying Clusters

Another thing that JSD can be helpful with is identifying when classes are very similar.  Clearly, things with a low JSD have nearly identical distributions which can be a simple and obvious check.  If, for a pair of classes, all their features have a low JSD you might consider these classes as indistinguishable.  That can be interpreted in two ways: either you don't have enough information to tell these classes apart, or you might benefit from considering these classes as a single "macroclass";  the latter interpretation suggests a more data-driven approach to determining what "classes" can be determined using the data, akin to unsupervised clustering like K-means, etc.  In this case, the use of class information a priori means this is a supervised approach. This means the OvA JSD can be an insightful exploratory data analysis (EDA) tool.  The [ml_utils.eda.screen.JSScreen](https://github.com/mahynski/ml_utils/blob/main/eda/screen.py) class provides tools to perform the calculations listed below.

Consider the following 4 classes with 2 features distributed as follows:

<img style="float: center" src="js_example5.png" width=600px>

Essentially, A is disjoint from B and C, which are sampled from the same distribution, and D is a broad distribution encompassing all three.

When we perform the OvA JSD calculation as described above, we can also perform "merges" where we combine classes to create a macroclass (A+B, for example) and see if that macroclass can then be distinguished from the other classes (which essentially also for a macroclass composed of all other classes).  Below is a summary of the JSD computed for both features for all combinations of classes.  Note that the figure is symmetric about its center because the JSD is a symmetric quantity ($JSD(A\|\|B) == JSD(B\|\|A)$) and since classes are partitioned disjointly (A+B) vs (remainder=C+D) is the same as (C+D) vs. (remainder=A+B).

<img style="float: center" src="js_example6.png" width=600px>

This suggests that A might be separable from classes (B,C,D) based on Feature 1; also that partitioning the classes in (A,B) or (C,D) makes sense.  Since C and D were sampled from the same distribution this is intuitive.  When we consider classes C and D on their own, they get confused with each other.  If you were to look at a confusion matrix of a classifier (say k nearest neighbor, for example) you would see a lot of off-diagonal entries corresponding to confusion between those classes.  If we lump them together, they do not compete against each other anymore, but instead are compared against the rest of the classes (A and B).  Because of this symmetry, it falls to the user to decide if the (A,B) set, the (C,D) set, or both are meaningful.  Clearly, because of the overlap, unsupervised clustering would not be able to pick up the connection between C and D.  

As already suggested, however, a KNN classifier's confusion matrix can provide a similar insight into class similarities.  A KNN model relies on some distance function (Euclidean vs. Mahattan, for example) which is not unique, though; moreover, this distance is computed using all the features at once, whereas here we can inspect each feature's similarity individually.

Assuming we had many features and wished to find a set of useful features to focus on, might consider that maximum JS divergence across all features for (macro)class; this gives a sense of how much we could hope to separate that (macro)class from the rest of the classes.  Because we may have many features which are not very useful, the max() is often more meaningful than the mean() of these.  A column-wise max() operation, sorted by value, looks like this (the errors are the standard deviation, which is not very meaningful for this example):

<img style="float: center" src="js_example7.png" width=600px>

Clearly, we see the (A) and (B,C) sets being indicated as potentially easily separable based on a single feature.  Their complements also appear with the same JSD.  Again, it is up to you to decide if A as a class is meaningful  or if (B,C,D) as a macroclass does.  Below are the binary distributions used for comparison for the top 4 (macro)classes based on the feature with the largest JSD (in this case it is always Feature 1); remember, these are normalized before computing the JSD.

<img style="float: center" src="js_example8.png" width=600px>

# Common Pitfalls

There are a few pitfalls to this approach. They essentially boil down to how to discretize the distributions to compute the KL divergence in the first place.  In practice, the distributions for a given feature are histogrammed into discrete bins, from which the KLD and JSD can be easily computed.  As a side note, in practice some small amount added to all bins (~1.0e-12) since numerically, we cannot divide by zero in the KLD calculation.  This is a small affect usually, should be noted.

## Bins are too coarse

Bins that are too coarse will make distributions seem identical.  For example, selecting $n_{\rm bins}=1$ means a single bin, which clearly will then encompass everything leading to JSD = 0 between any two distributions.  This is a silly example, and in practice, this limit arises more due to the presence of outliers.  Typically, we select the bin width after selecting $n_{\rm bins}$ to span the min and max of the observations; as a result, outliers will coarsen the bins.  In this example, classes A and B from above are compared before (left) and after (right) some outliers are added to A.  The divergences are, again symmetric, but are quite high (above the 0.7 threshold chosen earlier) for the case without the outlier; this is expected since A and B were purposefully prepared to be different.  However, when the outliers are added to A, the overlap between A and B becomes amplified because of bin coarsening, dramatically reducing this divergence which might make you miss the fact that Feature 1 is, in fact, a good feature to use.

<img style="float: center" src="js_example9.png" width=300px>
<img style="float: center" src="js_example10.png" width=300px>

## Bins are too fine

If we choose bin widths that are too small, each individual measurement (floating point number) becomes isolated to some arbitrarily small neighborhood and overlapping distributions begin to look as though they do not overlap at all.  This depends on the amount of data you have; more samples means you can tolerate smaller bins, but again, this is subjective and difficult to detemine in advance.  In the example below, we compute the OvA JSD calculations using $n_{\rm bins}=500$ (left) vs. $n_{\rm bins}=25$ for the B and C distributions above (designed to be overlapping).  When the number of bins is too high (small bin size) the divergence appears large (>0.7); whereas when a reasonable number is chosen the JSD is small (correct) because the two distributions are highly overlapping.

<img style="float: center" src="js_example11.png" width=300px>
<img style="float: center" src="js_example12.png" width=300px>

## In practice

In practice, having a bin width approach zero leads to $JSD \rightarrow 1$ while a bin width that becomes infinitely large cases $JSD \rightarrow 0$, in all causes for all distributions.  In practice, it is very difficult to assess what an appropriate bin width is then.  The easier approach in practice is to choose $n_{\rm bins}$; the bin width will be set by the bounds of the distributions and it is much easier to use existing techniques to remove potential outliers which would skew the results.  Even if the outliers are removed, choosing a bad bin width could still cause the JSD to jump toward unity.  Simply choosing a reasonable $n_{\rm bins} \sim 25$ is usually easier.  One way to remove outliers is to remove all points outside the IQR and use only this subset for the JSD-based comparisons.  Other approaches could also be devised.

The number of bins could be considered an adjustable hyperparameter if using the sklearn workflow, or something similar, shown above.  However, this is not necessaily a reasonable thing to do; it is better to choose some reasonable bin width (via the number of bins) and simply select the top $k$ features, treating $k$ as the hyperparameter; alternatively, you could set the threshold for deciding a feature is useful since the value used here (0.7) is arbitrary.


