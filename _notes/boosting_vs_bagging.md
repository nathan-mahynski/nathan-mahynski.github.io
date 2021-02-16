---
title: "Boosting vs. Bagging"
excerpt: "Ensemble methods for training better ML models."
header:
  teaser: /assets/img/bag_cartoon.png
  image: /assets/img/boosting_vs_bagging_header.png
tags:
  - machine learning
  - ensemble
  - boosting
  - bagging
classes:
  - wide
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

["Bagging"](https://en.wikipedia.org/wiki/Bootstrap_aggregating) is an acronym for (B)oostrap (Agg)regat(ing); the idea is to average many simple models each trained on randomly drawn subsamples of the data (with replacement, hence bootstrap sample).  This averaging, or aggregating, reduces the variance and model overfitting tendencies.  Bagging is a "parallel" operation is the sense that each model in the ensemble is indendent and can therefore be trained separately.  [Bagging works best when you employ strong, complex models](https://scikit-learn.org/stable/modules/ensemble.html) that essentially overfit a boostrapped sample; when averaged over many different bootstraps, the error rate averages to zero (if fully independent).

<a href="https://raw.githubusercontent.com/PacktPublishing/Python-Machine-Learning-Second-Edition/master/Chapter07/images/07_06.png"><img style="float: center" src="raschka_07_06.png" width=600px></a>

Bagging is when we draw bootstrap samples; if subsets are drawn randomly, this is called "pasting" instead.  You can also draw random features ("random subspaces"), or combine this with pasting to get "random patches" - see the [sklearn documentation](https://scikit-learn.org/stable/modules/ensemble.html) for references and details.

Bagging is very good at reducing model variance, but poor at reducing bias; this is why you need to use complex models with high variance and low bias as base estimators (deep decision trees, for example).

See [L. Breiman, "Bagging predictors", Machine Learning, 24(2), 123-140, 1996](https://link.springer.com/article/10.1007/BF00058655) for the original work.

Randomly drawing from the data results in unique elements 63.2% of the time, while the other 37.8% of the data will end up being repeats (if you randomly resample N observations to create a bootstrap resample also of N observations).

## Bag of little bootstraps

Another idea that combines boostrap sampling and subsampling to estimate error bounds and other statistical properties with a greatly reduced computational demand. See "bag of little bootstraps" paper [here](https://arxiv.org/abs/1112.5016).

## Examples

1. [Random Forest (bagging with trees)](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier)
2. [(extremely) Randomized decision trees (cheaper version of random forests)](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.ExtraTreesClassifier.html#sklearn.ensemble.ExtraTreesClassifier)

A nice discussion of the differences can be found [here](https://towardsdatascience.com/an-intuitive-explanation-of-random-forest-and-extra-trees-classifiers-8507ac21d54b?gi=e3563b716238).

# Boosting

Boosting takes a series of weak learners (slightly better than random) and trains them in series to produce a strong model.  This is a "series" operation unlike the "parallel" operation that Bagging relies upon.  Boosting works best with many weak models, like shallow decision trees; this is in contrast to Bagging which relies on complex models (deep trees) to overfit the data in "different ways" when shown different subsets of the data.

Unlike bagging, boosting uses samples drawn without replacement.  The original boosting algorithm look like this:

1. Draw random subset of data and train a learner, $C_1$.
2. Draw a second random subset of the data and add 50% of the samples from step (1) that were misclassified by $C_1$; train a new classifier, $C_2$.
3. Find the samples in the original training set with $C_1$ and $C_2$ disragree on, and train a third classifier, $C_3$, using that data.
4. Combine all classifiers into a majority vote.

While it is possible for boosting to reduce bias and variance (not just variance) relative to bagging models, they are known for having high variance themselves.

## AdaBoost

One of the most popular boosting algorithms is AdaBoost (adaptive boosting) developed by [Freund and Schapire](https://link.springer.com/content/pdf/10.1007/BF00116037.pdf), for which they received the Goedel Prize in 2003.  The idea is to essentially weight incorrectly predicted points more in each iteration, so the model learns from the mistakes previously made in the ensemble. From [sklearn's documentation](https://scikit-learn.org/stable/modules/ensemble.html#adaboost):

> "The core principle of AdaBoost is to fit a sequence of weak learners (i.e., models that are only slightly better than random guessing, such as small decision trees) on repeatedly modified versions of the data. The predictions from all of them are then combined through a weighted majority vote (or sum) to produce the final prediction. The data modifications at each so-called boosting iteration consist of applying weights $w_1, w_2, \dots, w_N$, to each of the training samples. Initially, those weights are all set to $W_i = 1/N$, so that the first step simply trains a weak learner on the original data. For each successive iteration, the sample weights are individually modified and the learning algorithm is reapplied to the reweighted data. At a given step, those training examples that were incorrectly predicted by the boosted model induced at the previous step have their weights increased, whereas the weights are decreased for those that were predicted correctly. As iterations proceed, examples that are difficult to predict receive ever-increasing influence. Each subsequent weak learner is thereby forced to concentrate on the examples that are missed by the previous ones in the sequence."

Graphically, the algorithms can be summarized in the following figure from Raschka & Mirjalili:
<a href="https://raw.githubusercontent.com/PacktPublishing/Python-Machine-Learning-Second-Edition/master/Chapter07/images/07_09.png"><img style="float: center" src="raschka_07_09.png" width=600px></a>

The basic algorithm in pseudocode:
1. Initialize normalized weights, $\sum w_i = 1$.
2. For each boosting round, $j$:
 * Train, $C_j = f(X, y, w)$.
 * Predict, $\hat{y} = C_j(X)$.
 * Compute the error rate, $\epsilon = w \dot (y \neq \hat{y})$. (0 for correct classification, 1 for incorrect in classification tasks)
 * Compute the coefficient, $\alpha_j = 0.5 {\rm log} \left( \frac{1-\epsilon}{\epsilon} \right)$.
 * Update weights, $w = w \times {\rm exp} \left( - \alpha_j \times \hat{y} \times y \right)$. (assuming $y$ is binary vector of +1 or -1 for each class so that correct classifcation results in a positive number and a negative otherwise)
 * Re-normalize the weights.
3. Final prediction is a $\alpha$-weighted average of the $j$ different classifiers, $C_j$, for each point; $\hat{y} = \sum_j \alpha_j C_j(X)$.

There exist discrete and real-valued AdaBoosting methods - see sklearn's [AdaBoost classifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html#sklearn.ensemble.AdaBoostClassifier) and [AdaBoost regressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostRegressor.html#sklearn.ensemble.AdaBoostRegressor) documentation here for more discussion.  There is an explicit [example](https://scikit-learn.org/stable/auto_examples/ensemble/plot_adaboost_hastie_10_2.html#sphx-glr-auto-examples-ensemble-plot-adaboost-hastie-10-2-py) as well.

## Gradient Boosting

Again, the idea is to train an ensemble of weak learners in series to create a strong learner.  Following [Wikipedia's example](https://en.wikipedia.org/wiki/Gradient_boosting), the easiest way to illustrate the idea is to follow that of ordinary least squares regression.  In OLS, you are trying to minimize the mean squred error loss function:

$$
L =\frac{1}{N} \sum \left( y_i - F(x_i) \right)^2 
$$

Here, $F(x)$ is the model.  In gradient boosting the idea is that we have an imperfect model at each stage, $m < M$, out of $M$ total.  The error is then $h_m(x) = y - F_m(x)$; the idea is that at each stage a new estimator is added so that $F_{m+1}(x) = F_{m}(x) + h_m(x) = y$.  Gradient boosting fits $h_m(x)$ to the residual $y - F_m(x)$; other variants, like AdaBoost, just focus on correcting errors of the predecessor model.  Note that

$$
L \sim \left( y - F_m(x) \right)^2
$$

so that

$$
h_m(x) = - \frac{\partial L}{\partial F} \sim y - F_m(x)
$$

This interpretation that the residual is actually the gradient of the loss function with respect to the model.  Thus, a model is usually assumed which has some adjustable parameters (weighted sum of functions for example) that lets you take the derivative with respect to the function, $F$, and gradient descent can be employed.  This can be done using trees as the base models, which is what sklearn implements as [gradient boosted trees](https://scikit-learn.org/stable/modules/ensemble.html#gradient-tree-boosting). Gradient Tree Boosting is a generalization of boosting to arbitrary differentiable loss functions, and can be applied to both regression and classification tasks.

Some specialized, fast implementations exist elsewhere:

* [LightGBM](https://github.com/Microsoft/LightGBM)
* [XGBoost](https://xgboost.readthedocs.io/en/latest/)


