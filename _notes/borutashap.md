---
title: "BorutaSHAP"
excerpt: "Possibly the best feature selection algorithm."
header:
  teaser: /assets/img/selection_process.png
  image: /assets/img/borutashap_header.png
tags:
  - machine learning
  - feature engineering
  - boruta
  - SHAP
  - workflow
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

BorutaSHAP combines 2 of the leading machine learning methods to perform intelligent feature selection for tabular data with minimal hyperparameters or arbitrary thresholds.  The first is the original [Boruta feature selection algorithm](https://www.jstatsoft.org/article/view/v036i11), and the second is [SHAP](https://github.com/slundberg/shap), which is used to improve/replace one of the core steps in the original implementation.  Theses blog posts go into more detail about this:

* ["Boruta explained exactly how you wished someone explained to you" by Samuele Mazzanti](https://towardsdatascience.com/boruta-explained-the-way-i-wish-someone-explained-it-to-me-4489d70e154a)
* ["Is this the Best Feature Selection Algorithm 'BorutaShap'?" by Eoghan Keany](https://medium.com/analytics-vidhya/is-this-the-best-feature-selection-algorithm-borutashap-8bc238aa1677)

An accompanying Jupyter notebook can be found [here](/examples/borutashap/).

# What is Boruta?
---
[Boruta](https://www.jstatsoft.org/article/view/v036i11) was originally a package in R that was designed to test for the statistical significance of a feature relative to a randomized version of itself.  An FAQ page is available [here](https://notabug.org/mbq/Boruta/wiki/FAQ) accompanying their [main website](https://mbq.github.io/Boruta/). In scikit-learn you can often do feature selection using [SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html#sklearn.feature_selection.SelectFromModel) which (1) fits a model which allows you determine the "importance" of a feature based on some effect, then (2) select the top features by some criterion.  However, this threshold is essentially arbitrary and can be scaled to the user's purpose.  Boruta uses a random forest model and 2 innovations:

1. columns (features) compete, not against each other, but against randomized versions of themselves.
2. statistical significance is determined if a feature "wins" significantly more often than 50% of the time (null hypothesis).

In the original implementation, first the dataset is augmented with a shuffled ("shadow") version of each column.  Then, a random forest is trained on this data and [feature importances](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier.feature_importances_) extracted from this.  A feature gets a "hit" if its importance is greater than the most important shadow feature.  This is iterated many times.  The maximum uncertainty about a feature occurs if it is at 50%; after many trials, the binomial distribution should be followed.  If a feature gets hit such that it falls in the upper tail of the PMF function, then it is kept; falling in the lower tail implies rejection; falling in between these means indecision, which typically defaults to just keeping it.  A Bonferroni correction for multiple testing is applied.  **A significance level (alpha) still has to be chosen for this.**  Features are constantly removed during the iteration once one has been determined insignificant.

This is similar to [Bayesian comparison](https://baycomp.readthedocs.io/en/latest/introduction.html) which has a "region of practical equivalence" (ROPE).

Any model that returns a "feature importance" can be used with Boruta, not just random forests.

The authors of [BorutaPy](https://github.com/scikit-learn-contrib/boruta_py) offer some improvements of their own discussed therein.  Notably, the Bonferroni correction is often considered too harsh so alternatives are discussed.
- "Boruta is an all relevant feature selection method, while most other are minimal optimal; this means it tries to find all features carrying information usable for prediction, rather than finding a possibly compact subset of features on which some classifier has a minimal error."

Here is a brief and insightful explanation from the inventor, [Miron B. Kursa](/assets/docs/boruta_in_a_hurry.pdf).

# What is BorutaSHAP?
---
The primary drawback of Boruta is that it uses the built-in impurity-based (gini) feature importance from the trees in the random forest.  According to scikit-learn's documentation, "impurity-based feature importances can be misleading for high cardinality features (many unique values)."  The alternative suggested is permutation feature importances, however, these always suffer from the potential inclusion of unlikely data points due to correlations as discussed [here](/notes/automatic_machine_learning/) and illustrated [here](/examples/decorrelating_ml_features).  A better method might be to use SHAP to rank feature importances to determine "hits" instead.  This is what [BorutaSHAP](https://github.com/Ekeany/Boruta-Shap) does; specifically, it leverages the faster TreeSHAP approach since random forests are still used to determine the importances.

BorutaSHAP can work, in principle, with other models besides random forests; this is especially salient since it uses SHAP for feature importances now.  However, because SHAP is much faster with trees BorutaSHAP assumes the model will be tree-based, so gradient boosted trees, random forests, etc. are all valid at the moment.

# Suggested workflow
---
1. [Decorrelate dimensions](/examples/decorrelating_ml_features) using PCA or clustering based on Spearman's rank-order correlation.
2. Select a fixed number of decorrelated dimensions as some upper bound - the SHAP part of BorutaSHAP will work better if already decorrelated.  This bound sets the number of features and the degree of independence between them.
3. Run BorutaSHAP to identify the subset of features that are statistically significant.
4. Train ML model.  Consider an [AutoML framework](/notes/automatic_machine_learning/) such as [auto-sklearn](/tutorials/using_auto_sklearn/). Because BorutaSHAP is an "all relevant feature selection" not a "minimal optimal" one, things like L1/L2 regularization are not "double work" or philosphically inconsistent with its use, and may be incorporated into this pipeline freely.
5. Perform [explainability analysis](/notes/interpretable_machine_learning/) of your choosing, perhaps using a [SHAP dashboard](/tutorials/configuring_explainerdashboard/) and associated [SHAP tools](https://github.com/slundberg/shap).

An example Jupyter notebook is available [here](/examples/borutashap/) which illustrates this in action.
