---
title: "Boosting vs. Bagging."
excerpt: "Ensemble methods for training better ML models."
header:
  teaser: /assets/img/bag_cartoon.png
tags:
  - machine learning
  - ensemble
  - boosting
  - bagging
---

{% include toc icon="gears" title="Table of Contents" %}

These notes come primarily from Chapter 7 in ["Python Machine Learning," Raschka and Mirjalili, Packt Publishing, 2nd Ed. (2017).](https://www.packtpub.com/product/python-machine-learning-second-edition/9781787125933).

# Ensemble Methods

Ensemble methods are meta estimators which combine multiple models to improve generalization accuracy.  This relies on having a diverse set of models so that their individual errors are compensated for by other models.  You can combine many different models (decision trees, logistic regression, SVC, etc.), or use the same model trained on different subsets of the same data (e.g., random forest).  The different constituents vote and the final prediction is due to this combination.  A common method is to just use the majority (or plurality) vote.  In the figure below (from Raschka & Mirjalili) the predictions, $P_i$ for model $i$, can also be weighted by, for example, their certainty; simple majority voting corresponds to identical weights for all participating models.

<a href="https://raw.githubusercontent.com/PacktPublishing/Python-Machine-Learning-Second-Edition/master/Chapter07/images/07_02.png"><img style="float: center" src="raschka_07_02.png" width=600px></a>

Ensemble methods are intuitive and quite clever; to see why consider a binary classification task where all base learners have some error rate, $\epsilon$.  If these are independent of each other, then we can express the probability that out of an ensemble of $n$ classifiers, $k$ or more of them will be incorrect as a binomial:

$$
P(y \ge k) = \sum_k^n {n \choose k} \epsilon^k (1-\epsilon)^{n-k} = \epsilon_{\rm ensemble}
$$

If we are using an ensemble of 11 classifiers and each is quite poor ($\epsilon = 0.25$, so 75% accuracy), then the chance that at least 6 of them are incorrect is $P \sim 0.034$; since at least 6 of them need to be wrong for a majority vote to produce the wrong prediction, we have now boosted the model from 75% to 96.6% accuracy!

The **key here is the decorrelation** of the different classifiers; essentially, we are "smearing out" the errors of one model assuming that other models are, on average, more accurate where that models goes wrong.  If they are correlated, they will all be wrong at the same time and so the average will also be wrong.  Technically, this also requires that the base learners error rate is better than random, $\epsilon > 0.5$, since a bias toward the "wrong" answer will be amplified just as a bias toward the "right" one will be.  The image below from Ch. 7 of Raschka & Mirjalili illustrates this.

<a href="https://raw.githubusercontent.com/PacktPublishing/Python-Machine-Learning-Second-Edition/master/Chapter07/images/07_03.png"><img style="float: center" src="raschka_07_03.png" width=600px></a>

scikit-learn provides more discussion in their [documentation](https://scikit-learn.org/stable/modules/ensemble.html), where you can find examples of how to use these tools automatically in sklearn.

# Bagging

"Bagging" is an acronym for (B)oostrap (Agg)regat(ing); the idea is to average many simple models each trained on randomly drawn subsamples of the data (with replacement, hence bootstrap sample).  This averaging, or aggregating, reduces the variance and model overfitting tendencies.  Bagging is a "parallel" operation is the sense that each model in the ensemble is indendent and can therefore be trained separately.  [Bagging works best when you employ strong, complex models](https://scikit-learn.org/stable/modules/ensemble.html) that essentially overfit a boostrapped sample; when averaged over many different bootstraps, the error rate averages to zero (if fully independent).

<a href="https://raw.githubusercontent.com/PacktPublishing/Python-Machine-Learning-Second-Edition/master/Chapter07/images/07_06.png"><img style="float: center" src="raschka_07_06.png" width=600px></a>

Bagging is when we draw bootstrap samples; if subsets are drawn randomly, this is called "pasting" instead.  You can also draw random features ("random subspaces"), or combine this with pasting to get "random patches" - see the [sklearn documentation](https://scikit-learn.org/stable/modules/ensemble.html) for references and details.

Bagging is very good at reducing model variance, but poor at reducing bias; this is why you need to use complex models with high variance and low bias as base estimators (deep decision trees, for example).

See [L. Breiman, "Bagging predictors", Machine Learning, 24(2), 123-140, 1996](https://link.springer.com/article/10.1007/BF00058655) for the original work.

## Bag of little bootstraps

https://arxiv.org/abs/1112.5016

# Boosting
Boosting takes a series of weak learners and trains them in series to produce a strong model.  This is a "series" operation unlike the "parallel" operation that Bagging relies upon.  Boosting works best with many weak models, like shallow decision trees; this is in contrast to Bagging which relies on complex models (deep trees) to overfit the data in "different ways" when shown different subsets of the data.

## AdaBoost

## Gradient Boosting

xgboost


