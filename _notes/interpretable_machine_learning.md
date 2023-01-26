---
title: "Interpretable / Explainable Machine Learning"
excerpt: "Some notes on how to make black-box models explainable."
header:
  teaser: /assets/img/shap_logo.png
  image: /assets/img/interpretable_machine_learning_header.png
gallery:
  - url: https://github.com/slundberg/shap
    image_path: /assets/img/shap_logo.png
    alt: "SHAP Library"
  - url: https://github.com/interpretml
    image_path: /assets/img/iml_logo.png
    alt: "InterpretML"
  - url: https://christophm.github.io/interpretable-ml-book/
    image_path: /assets/img/iml_book_logo.png
    alt: "Interpretable Machine Learning by Christoph Molnar"
tags:
  - machine learning
  - interpretability
  - SHAP
  - python
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

<a href="https://christophm.github.io/interpretable-ml-book/agnostic.html"><img style="float: right" src="/assets/img/fig5.1_molnar.png" width=400px></a>

Machine learning has demonstrated previously unparalleled capabilities in predictive computational modelling.  However, to deploy these models in real-world scenarios it is necessary to be able to explain *why* a model makes a prediction not just its degree of certainty, for example.  This allows governments, corporations, and other users to reach a decision about whether a model is trustworthy or not, or may exhibit a bias.  It is particularly important in scientific research as this can allow a researcher to inspect the "logic" behind a model's prediction to understand if the model has made a rational new discovery.  This is an enormous area of research and these notes are not remotely comprehensive; they are largely based on Christoph Molnar's book "Interpretable Machine Learning: A Guide for Making Black Box Models Explainable" available [here](https://christophm.github.io/interpretable-ml-book/).  Unless otherwise stated, attribution should be assumed.

Almost all of the best models (most accurate) are ensembles, where many individuals models are averaged; even if each individual model is interpretable/understandable to a human, their average quickly becomes opaque.  Thus, interpretability methods are generally required.

A recent effort at NIST has been to develop standards for explainable AI - this has been condensed to roughly [4 principles](https://www.nist.gov/publications/four-principles-explainable-artificial-intelligence-draft):
* Explanations need to be delivered by models
* Those explanations need to be "meaningful "
* The explanations themselves need to be accurate (this is not the same as model accuracy)
* Knowledge limits 

This is an ongoing effort and will change over time.

If you are looking for any kind of high-performing model and simply want it to be accurate, [scikit-learn's cheat sheet](https://scikit-learn.org/stable/tutorial/machine_learning_map/index.html) is a nice map of what algorithm might be the most useful.  
![](https://scikit-learn.org/stable/_static/ml_map.png)

However, in practice that is often not enough.  We need to have either "explainable AI" or "interpretable AI" (which are not the same thing) to judge how much trust we should place in the model.  Cynthia Rudin has a nice YouTube talk on this subject, linked below:

[![](https://img.youtube.com/vi/I0yrJz8uc5Q/default.jpg)](https://youtu.be/I0yrJz8uc5Q)

Jay Alammar has a nice repository of [Explainable AI](https://ex.pegg.io/) tools and a [cheat-sheet](https://ex.pegg.io/Explainable-AI-cheat-sheet-v0.2.1080.png) that nicely summarizes things.
![](https://ex.pegg.io/Explainable-AI-cheat-sheet-v0.2.1080.png)

[![](https://img.youtube.com/vi/Yg3q5x7yDeM/default.jpg)](https://youtu.be/Yg3q5x7yDeM)

<!--{% include gallery  %}-->

## Interpretability Itself

"Interpretability is the degree to which a human can understand the cause of a decision." - [Tim Miller](https://arxiv.org/pdf/1706.07269)

### Types of interpretability 

1. Humans tend to prefer "short explanations (only 1 or 2 causes) that contrast the current situation with a situation in which the event would not have occurred." Contrasting with abnormal cases can be particularly helpful.
  - Good explanations are contrastive: "Humans usually do not ask why a certain prediction was made, but why this prediction was made instead of another prediction."  FInding archetypes or prototypes is a good way to proceed if you want to go this route.
  - [LIME](https://github.com/marcotcr/lime) is good at providing a small subset of reasons for a prediction.
  - Abnormal features, even if computed to have similar "weight" as others, should be included in an explanation.

2. A *complete causal attribution* provides an explanation for all factors.  When debugging ML models, searching for bias, or any legally-binding or public-facing application, this is the goal.  This is usually mutually exclusive to the first type above.

### What characterizes an interpretable model? 

There are several ways to make a model "interpretable":
1. Intrinsic: use a simple, naturally interpretable model, such as a decision tree or linear model.  
2. Post hoc: use model agnostic approaches that analyze the model after training.

Today, type (1) is generally considered "interpretable AI" while type (2) is considered "explainable AI".

Model agnostic approaches can be divided into 2 categories:
> 1. They explain global, overall behavior, such as:
>  - Partial Dependence Plots
>  - Feature Importance
>  - Global Surrogate Models
>  - Prototypes and Criticisms (explaination by example).
> 2. They explain individual predictions, such as:
>  - Local surrogate models, [LIME](https://github.com/marcotcr/lime)
>  - Shapley Values

A *black box model* is a model that does not reveal its inner workings, i.e., it cannot be understood just by looking at its parameters (like a neural net).  The opposite is sometimes called a *white box* which is interpretable.  Model agnostic explaination methods treat all models as black boxes so as to be indendent of their details, regardless of what color box they are in reality.

The output of intepretation methods can be broadly classified into:

1. Feature summary statistic/visual: such as a feature importance (floating point value).
2. Model internals: parameters such as learned weights (coefficients) in a linear model, or visualization of feature detectors in neural nets (as in transfer learning).  For simple linear models, these effectively reflect a summary statistics (class 1). 
3. Data examples: used in counterfactual explanations, for example.  A single point is explained by finding a similar one with some critical difference that results in a different prediction. Mostly used for text and images not tabular data because the data points themselves need to be interpretable (is the image a dog or cat?).
4. Intrisic model: approximate a complex, black box model with a simple, white box one which can be interpreted by class 2.

*Model-specific* tools depend on the model itself; for example, interpeting the weights in a linear model.  *Model agnostic* tools perform post hoc analysis, usually by comparing feature input and the model output/response as a pair.

*Scope* describes how broad the explanation is.  "Local" explanations explain a specific data point, where as "global" methods explain the entire model's behavior.  Global interpretability is more holistic: "you need the trained model, knowledge of the algorithm and the data. This level of interpretability is about understanding how the model makes decisions, based on a holistic view of its features and each of the learned components such as weights, other parameters, and structures."  This is the most difficult to achieve in practice, but is usually the goal in scientific applications because the holistic explanation usually (hopefully) reflects the underlying physics which generates the observations being made.  Local explanations "zoom in" to a single data point and try to explain that particular model outcome; predictions can locally vary linearly with inputs, as a Taylor series expansion might suggest.  In such a case, it is possible that local explanations can be even more accurate than a global one.

The [Rashomon Effect](https://en.wikipedia.org/wiki/Rashomon_effect) is an effect where different models might reach the same conclusion but for different reasons; therefore, you can have different explanations.  For example, if you train a SVC and a linear regression model to predict the same target, you might hope they produce similar explanations if they are modeling a real, physical system.  High *consistency* is desirable if the models rely on similar relationships, which is generally the case in scientific applications.

"Fidelity" refers to how well the explanation approximates the black box model - this is critical since only in the case of hi fidelity is the explanation sensical.

## Instrinsically Iterpretable Models 
[Back to Top](#top){: .btn .btn--warning}

"The easiest way to achieve interpretability is to use only a subset of algorithms that create interpretable models. Linear regression, logistic regression and the decision trees are commonly used interpretable models."


Algorithm 		|	Linear	| Monotone	| Interaction	| Task	
--------------------	|	------	| --------	| -----------	| ----------
Linear regression	| 	Yes	| Yes		| No 		| regr
Logistic regression 	|	No 	| Yes 		| No 		| class
Decision trees 		|	No	| Some 		| Yes 		| class,regr
RuleFit 		|	Yes	| No 		| Yes 		| class,regr
Naive Bayes 		|	No	| Yes 		| No 		| class
k-nearest neighbors 	|	No	| No 		| No 		| class,regr
====================	|	======  | ========	| ===========	| ==========


### The Generalized Linear Model (GLM)

An introduction to GLMs can be found [here](https://online.stat.psu.edu/stat504/lesson/6/6.1) and [here](https://web.stat.tamu.edu/~suhasini/teaching613/chapter9.pdf).

The linear regression model assumes that some prediction, y, may be expressed as a weighted sum of its features with an error $$\epsilon$$ that is normally distributed.

{% raw %}
$$
y = \beta_0 + \beta_1x_1 + \dots + \beta_px_p + \epsilon
$$
{% endraw %}

This makes 3 principle assumptions:
1. y follows a Gaussian distribution
2. features are independent
3. features and the output, y, have a true linear relationship

The "effect" is defined as the product of $$\beta_ix_i$$ and gives the quantitative change in $$y$$ given a unit change in $$x_i$$ **assuming all other features remain fixed**.  Non-dimensionalization (standardization) will place all factors on the same scale and make it more clear which coefficient/term is more/less important than others more directly.  Of course, standardization scales by the standard deviation which may only make sense if the observations vary normally, so there may be other ways to scale the data in a more intuitive and meaningful way depending on the application.  However, looking at the effect essentially just circumvents the need to care about this and looks at the impact directly, so it can be a preferred initial course of action in the absence of any understanding about the underlying nature of the data, or process that generated it.

If assumption 1 is violated, a GLM can handle this my use of a non-linear function to transform the left hand side of the equation.  This is called a [link function, g](https://en.wikipedia.org/wiki/Generalized_linear_model#Link_function).

{% raw %}
$$
g \left( E_Y(y \vert x) \right) = \beta_0 + \beta_1x_1 + \dots + \beta_px_p = x^T\beta,
$$
{% endraw %}

where $$x$$ is the matrix of observations, $$\beta$$ is the vector of coefficients, and $$E_Y$$ is a probability distribution from the [exponential family](https://en.wikipedia.org/wiki/Exponential_family#Table_of_distributions). In the classic linear model where a Gaussian distribution is assumed for y, the link function is just the identity function. Under the GLM framework, the concept generalizes to any distribution from the exponential family and arbitrary link functions.

For example, if modeling a distribution of integers (e.g., number of coffees consumed in a day) we would use the Poisson distribution, for which the link function is the natural logarithm: $$ {\rm ln} \left( E_Y(y \vert x) \right) = x^T\beta $$.  Logistic regression, which models the log-odds-ratio, is a Bernoulli distribution which carries a logit link function:

{% raw %}
$$
{\rm ln} \left( \frac{E_Y(y \vert x)}{1 - E_Y(y \vert x)} \right) = {\rm ln} \left( \frac{P(y=1 \vert x)}{1 - P(y=1 \vert x)} \right) = x^T\beta.
$$
{% endraw %}

> "Each distribution from the exponential family has a canonical link function that can be derived mathematically from the distribution. The GLM framework makes it possible to choose the link function independently of the distribution. How to choose the right link function? There is no perfect recipe. You take into account knowledge about the distribution of your target, but also theoretical considerations and how well the model fits your actual data. For some distributions the canonical link function can lead to values that are invalid for that distribution. In the case of the exponential distribution, the canonical link function is the negative inverse, which can lead to negative predictions that are outside the domain of the exponential distribution. Since you can choose any link function, the simple solution is to choose another function that respects the domain of the distribution."

Assuming we assumed a Poisson distribution and took its canonical link function (natural logarithm), to interpret the weights we need to invert the link function.  Now weights are multiplicative, not additive:

{% raw %}
$${\rm ln}E_Y = x^T\beta$$
{% endraw %}
{% raw %}
$$E_Y = {\rm exp} \left( x^T\beta \right) = \Pi_i^p {\rm exp}(\beta_i x_i)$$
{% endraw %}

So for each unit increase of $$x_i$$, the expected value of y is multiplied by $${\rm exp}(\beta_i x_i)$$; thus, if the factor is greater than 1, y increases, and when the factor is less than 1, it decreases. A handy consequence to remember is that if a factor is categorical and one-hot-encoded (either 0 or 1), then this factor tells you how many times greater y is when the category is true (1) than when it is false (0).

If assumption 2 is violated, we may simply multiply columns together to get a product and retrain the model.  This is perhaps the easiest to deal with.  However, this complicates interpretation because now a feature contributes to multiple terms in the GLM and so its effect is a net of these effects.  You may, for example, have 2 columns: $$x_i$$ and $$x_ix_j$$.  The interpretation in a linear model of the effect of $$x_i$$ requires that all other terms remain fixed, but that is not possible because $$x_j$$ would have to counter the change to keep their product fixed, but in that case the $$x_j$$ term would change.  The importance is essentially a sum of that associated with $$x_i$$ and the product term.  If $$x_j$$ is a binary variable (0 or 1) the interpretation is that there are 2 importances (slopes) for $$x_i$$: one if $$x_j$$ is 0 and another if it is 1; if it is not binary, the interpretation is harder.

If assumption 3 is violated, there are number of remedies.  
1. Nonlinear transformation of the feature (take natural logarithm, for example)
2. Categorization of the feature (bin into histograms, each of which is a fictitious category)
3. Generalized Additive Models (GAMs)

[GAMs](https://github.com/dswah/pyGAM) extend the GLM by assuming the outcome, y, can be modeled as a functions of each feature:

{% raw %}
$$
g \left( E_Y(y \vert x) \right) = \beta_0 + f_1(x_1) + \dots + f_p(x_p).
$$
{% endraw %}

This is actually just a generalization of options 1 and 2.  Generally, each function is assumed to be a sum of many spline functions, and regularization is used to keep them from overfitting.  Codes like [pyGAM](https://github.com/dswah/pyGAM) have these implemented.  It seems like random forests and other models have surpassed this, though, and so GAMs are not used as much these days.

### Non-linear Methods

Decision trees are simple, non-linear models that are trivial to interpret.  There are many ways to create trees, though CART (Classification and Regression Tree) is probably the most common.  Scikit-learn implements an [optimized version of this](https://scikit-learn.org/stable/modules/tree.html#tree-algorithms-id3-c4-5-c5-0-and-cart).

>"The overall importance of a feature in a decision tree can be computed in the following way: Go through all the splits for which the feature was used and measure how much it has reduced the variance or Gini index compared to the parent node. The sum of all importances is scaled to 100. This means that each importance can be interpreted as share of the overall model importance."

See examples of calculating this from scikit-learn [here](https://scikit-learn.org/stable/auto_examples/ensemble/plot_forest_importances.html).

Advantages of Decision Trees:
1. The data ends up in distinct groups that are often easier to understand than points on a multi-dimensional hyperplane as in linear regression. 
2. The tree structure also has a natural visualization, with its nodes and edges.
3. Trees create good "human-friendly" explanations. 
  - Counterfactuals are easy: "If a feature had been greater / smaller than the split point, the prediction would have been y1 instead of y2." 
  - The tree explanations are contrastive. 
  - If the tree is shallow the resulting explanations are selective. A tree with a depth of three requires a maximum of three features and split points to create the explanation for the prediction of an individual observation. The truthfulness of the prediction depends on the predictive performance of the tree. 
  - There is no need to transform features or perform standardization to rescale. 

Another option is the [RuleFit](https://github.com/christophM/rulefit) algorithm by [Friedman and Popescu](https://projecteuclid.org/euclid.aoas/1223908046) automatically detects interaction effects in the form of decision rules. It learns a sparse linear model with the original features plus a number of new features that are decision rules, which capture the interactions between the original features; these new features are derived from decision trees. "Any algorithm that generates a lot of trees can be used for RuleFit, for example a random forest. Each tree is decomposed into decision rules that are used as additional features in a sparse linear regression model."
* Also comes with a feature importance measure, calculated from the weights of the regression model, to identify linear terms and their importance. These importance measures are then combined for each of the original features, since they are generally used multiple times.
* Introduces partial dependence plots to show the average change in prediction by changing a feature.
* Decision rules are binary; 1 = all conditions met, else 0.
* LASSO used to regularize the addition of decision rules.

There are 2 steps to RuleFit: rule generation, and model fitting.
1. Rule Generation: these are binary, for example, "if x1 < a and x2 > b then 1 else 0".  Rules are constructed by decomposing fitted decision trees by following the path from stump; trees of random depth are fitted so rules of varying coplexity are generated. Generally, gradient boosting is used to fit an ensemble of such trees, but any ensemble algorithm can be used.  Because you fit the trees to y, the rules are assumed to be "good" for the task at hand. The number of rules generated, K, from M trees in the ensemble, each with $$t_m$$ terminal nodes is: $$K = \sum_{m=1}^M 2(t_m-1)$$
2. Fitting: with rules generated from an ensemble of decision trees.  K is usually very large and needs to be reduced.  First, the original features are [winsorized](https://en.wikipedia.org/wiki/Winsorizing) then normalized by scaling (akin to standardization) to place them on an even footing with the rule-based features.  A linear model is then fit to set of all new features with a LASSO penalty. 

Local feature importances are then the coefficient from the model multiplied by a relevant "standard deviation term".  For the rule-based features, this term depends on the number of times the rule applies (is 1 not 0); since a feature may contribute to many rules, each feature's importance is the sum of that from the linear term and those from the rules, weighted by the inverse of the number of features constituting each rule.  Summing over all instances gives the global importance of a feature.

RuleFit is a non-linear model with good interpretability; however, this goes down if you have too many rules, so a strong regularization from LASSO is generally required, and the depth of trees used should not be too large (3 or less, maybe).  It is claimed to perform as well as random forests, but communal experiences seem less supportive of this conclusion. There are also major issues if rules overlap, or are not disjoint. [Skope-Rules](https://github.com/scikit-learn-contrib/skope-rules) is a similar idea but tries to remove duplicated rules.

## Model-Agnostic Methods 
[Back to Top](#top){: .btn .btn--warning}

Model agnostic methods can be applied independent of the model used, and therefore are more general. They are also particularly helpful when using multiple models to perform the same task and they need to be compared.

> "Desirable aspects of a model-agnostic explanation system are:
> * Model flexibility: The interpretation method can work with any machine learning model, such as random forests and deep neural networks.
> * Explanation flexibility: You are not limited to a certain form of explanation. In some cases it might be useful to have a linear formula, in other cases a graphic with feature importances.
> * Representation flexibility: The explanation system should be able to use a different feature representation as the model being explained. For a text classifier that uses abstract word embedding vectors, it might be preferable to use the presence of individual words for the explanation."

-- See ["Model-agnostic interpretability of machine learning."](https://arxiv.org/abs/1606.05386)

### Partial Dependence Plots (PDP) 
---
See [scikit-learn user guide](https://scikit-learn.org/stable/modules/partial_dependence.html#partial-dependence) for more details.
  * global method which shows the marginal effect feature(s) have on the predicted outcome of a machine learning model.
  * work by marginalizing the model output over the distribution of the features in set C, so that the function shows the relationship between the features in set S we are interested in.
  * assumes that features in C are not correlated with the features in S.

$$
\hat{f}_{x_S}(x_C) = E_{x_C} \left[ \hat{f}(x_S, x_C) \right] = \int \hat{f}(x_S, x_C) dP(x_C) \approx \frac{1}{n} \sum_{i=1}^n\hat{f} \left( x_S, x_C^{(i)} \right)
$$

PDPs represent the average prediction if we force all data points to assume a specific value for a given feature.  This value is changed systematically to produce a curve. These can also be plotted with 2D heatmaps to show pairs of effects at once, but effectively this is the limit.

Feature independence is the most significant assumption for PDPs.  Since we effectively take each data point and just change one feature to be some value, we generate artificial data points that might be unrealistic.  For example, if the data includes the height and weight of a person, it is unrealistic that a very short person may have a large weight.

### Individual Conditional Expectation (ICE) plots 
---
  * display one line per instance that shows how the instance's prediction changes when a feature changes.
  * equivalent of PDP for individual data instances (PDP = average of ICE lines) which show *heterogeneity* in the feature effect, if it exists.
  * PDP best if features decorrelated, but ICE more informative if they are correlated.
  * still suffer from potentially unrealistic data points created during calculation.

"Similar to a PDP, an individual conditional expectation (ICE) plot shows the dependence between the target function and an input feature of interest. However, unlike a PDP, which shows the average effect of the input feature, an ICE plot visualizes the dependence of the prediction on a feature for each sample separately with one line per sample." - [scikit-learn user guide](https://scikit-learn.org/stable/modules/partial_dependence.html#individual-conditional-expectation-ice-plot)

If you look at the ICE curves and they all have the same "course" or trajectory then this suggests there are not obvious interactions and a PDP plot is fine.

### Accumulated Local Effects (ALE) plots 
---
  * describe how features influence model prediction on average. 
  * faster and unbiased alternative to PDPs.
  * examine differences in predictions around some small window value for a feature (approximates a gradient).

See [Apley, et al.](https://rss.onlinelibrary.wiley.com/doi/pdf/10.1111/rssb.12377?casa_token=lLdKRhyBuUIAAAAA:TkCvuLB_dxF4vUcpZDNhgs0Ux5b6xKGzBXijwvOJZfx5jnaM8bJazJp_jzpaGXTmleT0kpCPVZWtFW0) for more details.
A python implementation is available [here](https://github.com/blent-ai/ALEPython).

<a href="https://christophm.github.io/interpretable-ml-book/ale.html#motivation-and-intuition"><img style="float: center" src="/assets/img/fig5.12_molnar.png" width=800px></a>

The image above, from Molnar's book, illustrates the calculation of ALE for feature x1, which is correlated with x2. First, the x1 is divided into intervals (z's), usually generated as quantiles of the data. Second, for each of the data points that fall in each interval, calculate the prediction difference between when x1 takes its upper and lower limit of the interval. Third, average these differences (gradients) within each interval.  This produces the average "local effect." Finally, we accumulate (sum) the averages over all intervals to effectively integrate. The ALE curve is the running sum (integral) up to the given interval.  Usually this curve is also centered at zero so its interpretation is relative to the mean prediction of a model.

> "The value of the ALE can be interpreted as the main effect of the feature at a certain value compared to the average prediction of the data. For example, an ALE estimate of -2 at xj=3 means that when the j-th feature has value 3, then the prediction is lower by 2 compared to the average prediction."

ALE is also less sensitive to model results that fall outside the data's observed (joint) distribution because it accounts for the correlation.  We do not usually expect a model to perform well when extrapolated to such regions, but it might be relevant to know that the model is behaving strangely there.  PDPs can reveal this wierd behavior, whereas ALE will not.  Depending on your application this can be good or bad.

There is no easy way to choose the number of intervals to use; too small and the ALE plots might not be very accurate (not enough observations), but too large, the curve can become "blocky" and loses its "local" character.

Higher order ALE plots are possible (average over "grids" rather than intervals), but the curse of dimensionality makes it more difficult to get a reliable number of measurements per grid, which can lead to noise.

> "Even though ALE plots are not biased in case of correlated features, interpretation remains difficult when features are strongly correlated. Because if they have a very strong correlation, it only makes sense to analyze the effect of changing both features together and not in isolation. This disadvantage is not specific to ALE plots, but a general problem of strongly correlated features."

Regardless, "As a rule of thumb: Use ALE instead of PDP."

### Permutation Feature Importance
---
  * measures the change in model prediction error after shuffling a feature's values.
  * still very dependent on feature interactions, since permutation can potentially lead to unrealistic data points (same as PDP).
  * random reshuffling should be performed many times to get a good average PFI.

The original paper from 2018 can be found [here](http://arxiv.org/abs/1801.0148).  A standard implementation from scikit-learn is available [here](https://scikit-learn.org/stable/modules/generated/sklearn.inspection.permutation_importance.html#sklearn.inspection.permutation_importance).

>Whether you should use the test or train data set to compute the feature importances is somewhat unclear.  Generally, it would be safer to use the training data since the training data was used to create the model and, thus, may be biased.  On the other hand, the PFIs would be self-consistent with the way the model was trained.  If the model has seen enough data to be reasonable and the test and training sets are similar (train was representative of test), then this shouldn't matter too much.  This, of course, can always be tested.  Since the training set is generally much larger than the test set, the training set seems to be used by various authors.  This is especially true when doing SHAP methods which rely on getting accurate marginal probabilities from the data.  In fact, this seems to be true for many examples available online, including from scikit-learn, even for other methods like PDP.

PFI automatically takes into account all interactions with other features. "By permuting the feature you also destroy the interaction effects with other features. This means that the permutation feature importance takes into account both the main feature effect and the interaction effects on model performance. This is also a disadvantage because the importance of the interaction between two features is included in the importance measurements of both features. This means that the *feature importances do not add up to the total drop in performance, but the sum is larger*. Only if there is no interaction between the features, as in a linear model, the importances add up approximately."  SHAP methods later will add up correctly.

Also, because of correlation the model may have access to "information" about the true answer through pathways that are not broken by permuting only one feature.  As a result, it may appear that after permutation the model performance did not suffer greatly and that this feature was irrelevant.  Repeating this exercise for its other correlated features results in the same answer, and you might decide to remove them all due to the low PFI.  Clearly, you have just lost all this information.  Working with decorrelated features is the best way to avoid this (see below for notes on decorrelating features).

### Global Surrogate
---
  * a surrogate interpretable model (tree, for example) that is trained to approximate the output of a black box model
  * effectively a coarse-graining of the system

To obtain a surrogate model:
  1. Select the dataset X (may be same used for training black box, or a new dataset from the same distribution).
  2. Get the predictions of the black box model.
  3. Select an interpretable model type (linear model, decision tree, etc.).
  4. Train the interpretable model on X and the black box model's predictions.
  5. Measure how well the surrogate model replicates the predictions of the black box model.
  6. Interpret the surrogate model.

Note: ther performance of the black box model is irrelevant; as long as the surrogate matches the black box well ($$R^2 \gtrsim 0.7$$, but no clear cutoff), we assume it is a good explanation. It is important only that the surrogate and black box match each other reasonable well for you to be comfortable accepting the surrogate as a valid explanation.

Most importantly, remember that you are draw ingconclusions about the model and not about the data, since the surrogate model never sees the real outcome.

### Local Surrogate (LIME)
---
  * [Local interpretable model-agnostic explanations (LIME)](https://github.com/marcotcr/lime) are local surrogate models trained to explain individual predictions.

LIME generates a new dataset of permuted samples and corresponding predictions of the black box model. LIME then trains an (any) interpretable model, which is weighted by the proximity of the sampled instances to the instance of interest.  Mathematically, this can stated as:

{% raw %}
$$
{\rm explanation}(x) = {\rm arg min}_{g \in G} L(f, g, \pi_x) + \Omega(g)

L(f, g, \pi_x) = \sum_{x' \in X'} \left[ f(x')-g(x') \right]^2 \pi_x(x')
$$
{% endraw %}
where X' is the neighborhood of the x.

"The explanation model for instance x is the model, g, (e.g. linear regression model) that minimizes the loss, L (e.g. mean squared error), which measures how close the explanation is to the prediction of the original model, f (e.g. an xgboost model), while the model complexity $$\Omega(g)$$ is kept low (e.g. prefer fewer features). G is the family of possible explanations, for example all possible linear regression models. The proximity measure πx defines how large the neighborhood around instance x is that we consider for the explanation. In practice, LIME only optimizes the loss part. **The user has to determine the complexity, e.g. by selecting the maximum number of features that the linear regression model may use.**"  However, regularization techniques like LASSO or forward/backward [feature selection](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.feature_selection) can be used to determine these.

The recipe for training local surrogate models:

  1. Select an instance of interest to explain.
  2. Perturb your dataset and get the black box predictions for these new points. For tabular data, LIME creates new samples by perturbing each feature individually, drawing from a normal distribution with mean and standard deviation taken from the feature.
  3. Weight the new samples according to their proximity to the instance of interest. Defining a meaningful neighborhood around a point is very difficult. LIME currently uses an exponential smoothing kernel to define the neighborhood, but there is no evidence/procedure to fully define this.
  4. Train a weighted, interpretable model on the dataset with the variations.
  5. Explain the prediction by interpreting the local model.

<a href="https://christophm.github.io/interpretable-ml-book/lime.html#lime-for-tabular-data"><img style="float: center" src="/assets/img/fig5.33_molnar.png" width=800px></a>

The above figure is from Molnar's book, and illustrates the LIME algorithm for tabular data. "A) Random forest predictions given features x1 and x2. Predicted classes: 1 (dark) or 0 (light). B) Instance of interest (big dot) and data sampled from a normal distribution (small dots). C) Assign higher weight to points near the instance of interest. D) Signs of the grid show the classifications of the locally learned model from the weighted samples. The white line marks the decision boundary (P(class=1) = 0.5)."

It is easier to interpret categorical feature effects than numerical features. One solution is to categorize the numerical features into bins (perhaps quantiles?).

> "When using Lasso or short trees, the resulting explanations are short (= selective) and possibly contrastive. Therefore, they make human-friendly explanations. This is why I see LIME more in applications where the recipient of the explanation is a lay person or someone with very little time. It is not sufficient for complete attributions, so **I do not see LIME in compliance scenarios where you might be legally required to fully explain a prediction.** Also for debugging machine learning models, it is useful to have all the reasons instead of a few."

Note that black box models may use uninterpretable transformations (e.g., PCA) of raw data to make predictions, but LIME models can use the raw features making the explanations more interpretable. They do not have to be the same!

"Another really big problem is the instability of the explanations. In this [article](https://arxiv.org/pdf/1806.08049) the authors showed that the explanations of two very close points varied greatly in a simulated setting. Instability means that it is difficult to trust the explanations, and you should be very critical."

[SHAP is closely related to LIME with the primary differences being in how $$\Omega(g)$$ and $$\pi_x$$ are chosen. In LIME these are done heuristically: $$\Omega(g)$$ is the number of non-zero weights in the linear model
and $$\pi_x(x')$$ is defined using a cosine or l2 distance. SHAP uses game theory to define these quantities so that they staisfy certain properties.](https://dl.acm.org/doi/pdf/10.1145/3375627.3375830)

**Also of great importance: it has been recently shown that LIME (and also SHAP) can be fooled by manipulating the perturbation scheme. See the paper [here](https://dl.acm.org/doi/pdf/10.1145/3375627.3375830).** 

### Scoped Rules (Anchors)
---
  * model-agnostic explanation of individual predictions by finding a decision rule that "anchors" the prediction sufficiently. 
  * an "anchor" implies that changes in other feature values do not affect the prediction.
  * uses reinforcement learning and graph search algorithms to avoid local minima traps.
  * results in rules like "IF a AND b THEN PREDICT y WITH PRECISION x% AND COVERAGE %z".
  * very easy to understand.

Originally proposed by [Ribeiro et al.](http://homes.cs.washington.edu/~marcotcr/aaai18.pdf) in 2018.  The associated python code can be found [here](https://github.com/marcotcr/anchor) and this is also integrated into [Alibi](https://github.com/SeldonIO/alibi).

"Like its predecessor, the anchors approach deploys a perturbation-based strategy to generate local explanations for predictions of black-box machine learning models. However, instead of surrogate models used by LIME, the resulting explanations are expressed as easy-to-understand IF-THEN rules, called anchors."

> "Given an instance x to be explained, a rule or an anchor A is to be found, such that it applies to x, while the same class as for x gets predicted for a fraction of at least $$\tau$$ of x’s neighbors where the same A is applicable. A rule’s precision results from evaluating neighbors or perturbations using the provided machine learning model."

"In their paper, the authors compare both of their algorithms and visualize how differently these consult an instance's neighborhood to derive results. For this, the following figure depicts both LIME and anchors locally explaining a complex binary classifier (predicts either - or +) using two exemplary instances. LIME’s results do not indicate how faithful they are as LIME solely learns a linear decision boundary that best approximates the model given a perturbation space D. Given the same perturbation space, the anchors approach constructs explanations whose coverage is adapted to the model’s behavior and clearly express their boundaries. Thus, they are faithful by design and state exactly for which instances they are valid."

<a href="https://christophm.github.io/interpretable-ml-book/anchors.html"><img style="float: center" src="/assets/img/fig5.37_molnar.png" width=800px></a>

"While choosing the default perturbation space is a comfortable choice to make, it may have a great impact on the algorithm and can thus lead to biased results."  In general, there are many hyperparameters to set in this approach and is a significant drawback.  If the neighborhood, etc. are well-chosen intuitive answers can result, but it is difficult to know what this is and/or how to choose in advance.

### Shapley Values 
---
  * assuming each feature is a "player" in a game where the prediction is the payout, Shapley values (from coalitional game theory) describe how to fairly distribute the payout, i.e., explain the prediction, among the features.
  * attributions to each feature sum up "correctly".
  * the Shapley value is the (weighted) average marginal contribution of a feature value across all possible coalitions. 
  * can be used for classification (probabilities) and regression.

Coined by [Lloyd Shapley in 1953](http://www.library.fa.ru/files/Roth2.pdf#page=39) which is the core solution to game theory - he received a Nobel Prize for this in 2012.

<!--
We are seeking to attribute the difference between the model's prediction for a single instance and the average prediction of the model. So if we have 3 features in a model with a mean prediction of 100, and an instance where the prediction is 90, how do we attribute the value of -10 among the three "players"?  A valid explanation might be (10,-15,-5) since the sum is -10. The attribution to each feature must always sum up exactly to this difference.

To compute the Shapley value you need to examine all possible "coalitions" of the features.  Consider an example with 4 features where we want to estimate the marginal contribution of feature 4 to a coaltion of features (1,2).

1. Fix the value of features in a coalition; 1 and 2 in this case.  Fix the value of the feature (4) you want to investigate as well.
2. For missing features, (feature 3) randomly draw an instance from the data set and use those value(s) to fill in missing columns.
3. Predict the result.
4. Predict result again if the feature you are explaining (feature 4) has also been replaced by that of the randomly drawn sample.
5. The marginal contribution is the difference between these two predictions.
6. Repeat many times to get a better average for this coalition.
7. Repeat steps 1-6 for all possible coalitions.
8. Shapley value = weighted average of all the marginal contributions to all possible coalitions (not originally including this feature). The weight is related to the number of different possible ways to make a coalition of a certain size.

> "Be careful to interpret the Shapley value correctly: The Shapley value is the average contribution of a feature value to the prediction in different coalitions. The Shapley value is NOT the difference in prediction when we would remove the feature from the model."
-->

The contribution of the $$j^{th}$$ feature, $$\phi_j(\hat{f}(x))$$, to a given prediction, $$\hat{f}(x)$$, with a linear model where

{% raw %}
$$
\hat{f}(x) = \beta_0 + \beta_1x_1 + \dots + \beta_px_p,
$$
{% endraw %}

is given by:

{% raw %}
$$
\phi_j(\hat{f}) = \beta_jx_j - E(\beta_jX_j).
$$
{% endraw %}

Here $$E(\beta_jX_j)$$ is the expected feature effect for feature $$j$$.  If we sum over all features for a given instance:

{% raw %}
$$
\sum_{j=1}^p\phi_j(\hat{f}) = \left[ \beta_0 + \sum_{j=1}^p \beta_jx_j \right] - \left[ \beta_0 + \sum_{j=1}^p E(\beta_jX_j) \right]
= \hat{f}(x) - E(\hat{f}(X))
$$
{% endraw %}

Thus, the contribution is the predicted value minus the average predicted value.  The Shapley value, from coalition game theory, extends this to arbitrary models, not just simple linear ones.  Assuming the model prediction (less the mean) is the "payout", then the "players" are the feature values.  Accoring to [Wikipedia](https://en.wikipedia.org/wiki/Shapley_value): "The Shapley value is one way to distribute the total gains to the players, assuming that they all collaborate. It is a 'fair' distribution in the sense that it is the only distribution with certain desirable properties listed below."

{% raw %}
$$
\phi_j(\hat{f}) = \frac{1}{p} \sum_{S \in \{x_1,\dots,x_p\};\{x_j\} } \left[ \frac{1}{ {p-1 \choose |S|} } \right] \left[ v(S \cup x_j) - v(S) \right]
$$
{% endraw %}

where $$v(S \cup x_j)$$ is the prediction when feature values not part of S have been marginalized:

{% raw %}
$$
v(S) = \int \hat{f}(x_1, \dots, x_p) d{\rm P}_{x \notin S } - E_X\left( \hat{f}(X) \right).
$$
{% endraw %}

The Shapley value can be interpreted as

{% raw %}
$$
\phi_j(\hat{f}) = \frac{1}{ {\rm number\ of\ players}} \sum \frac{ {\rm marginal\ contribution\ of\ feature\ j\ to\ coalition\ i} }{ {\rm number\ of\ coalitions\ excluding\ j\ of\ this\ size} }
$$
{% endraw %}

From Wikipedia: "The formula can be interpreted as follows: imagine the coalition being formed one actor at a time, with each actor demanding their contribution $$\left[ v(S \cup x_j) - v(S) \right]$$ as a fair compensation, and then for each actor take the average of this contribution over the possible different permutations in which the coalition can be formed."  Stated another way: "An intuitive way to understand the Shapley value is the following illustration: The feature values enter a room in random order. All feature values in the room participate in the game (= contribute to the prediction). The Shapley value of a feature is the average change in the prediction that the coalition already in the room receives when the feature value joins them."

The Shapley value is the **only way** to distribute the payout such that the following axioms are always true:

1. Efficiency: the sum of the Shapley values of all features equals the value of the "grand coalition", i.e., $$\sum_{j=1}^p \phi_j = \hat{f}(x) - E_X(\hat{f}(X))$$. LIME does not have this property.
2. Symmetry: if 2 features contribute equally to all coalitions then their Shapley values are equal.
3. Dummy (null player): A features which does not affect the prediction, regardless of coalition, has $$\phi_j = 0$$.
4. Additivity (linearity): Combining games results in the sum of the Shapley value from each game for each feature.

"Suppose you trained a random forest, which means that the prediction is an average of many decision trees. The Additivity property guarantees that for a feature value, you can calculate the Shapley value for each tree individually, average them, and get the Shapley value for the feature value for the random forest."

Because of the combinatorial term which provides the "weight" in the average, exact evaluation of the Shapley value can be very computationally expensive, which is the main disadvantage.  As a result, [approximate Monte Carlo methods](https://link.springer.com/article/10.1007/s10115-013-0679-x) are typically employed.

> "The Shapley value of a feature value is not the difference of the predicted value after removing the feature from the model training. The interpretation of the Shapley value is: Given the current set of feature values, the contribution of a feature value to the difference between the actual prediction and the mean prediction is the estimated Shapley value."

Again the Shapley value suffers from inclusion of unrealistic data instances being used during marginalized when features are correlated (simulation when a feature value is absent from a coalition).  This is achieved by sampling values from the feature's marginal distribution, which is fine as long as features are independent.  It is possible to perform conditional sampling (features are sampled conditional on the features already in the coalition), however, this **violates the symmetry axion** and means this is no longer a Shapley value.  See [Sundararajan and Najmi (2020)](http://proceedings.mlr.press/v119/sundararajan20b.html) and [Janzing et al. (2020)](http://proceedings.mlr.press/v108/janzing20a.html).

Perhaps the best way to use Shapley values is to only use decorrelated features, for example, by doing [hierarchical clustering based on Spearman rank order correlations](https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance_multicollinear.html#handling-multicollinear-features) to find a feature subset that is sufficiently decorrelated.  See notes below for more on this.

### SHAP
---
  * SHapley Additive exPlanations: based on Shapley values
  * inspired by local surrogate models (like LIME)

Developed by [Lundberg and Lee](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions.pdf) - see the [github repo](https://github.com/slundberg/shap) for more citations and details on the implementation from the authors.

"One innovation that SHAP brings to the table is that the Shapley value explanation is represented as an additive feature attribution method, a linear model. That view connects LIME and Shapley Values. SHAP specifies the explanation as:"

{% raw %}
$$
g(z') = \phi_0 + \sum_{j=1}^M \phi_jz_j'
$$
{% endraw %}

where $$g$$ is the explanation model, $$M$$ is the maximum coalition size, and $$z' \in {0,1}^M$$ maps whether or not a feature belongs to the coalition.  For the case of aggregating pixles into superpixels, for example this may be more important (called "simplified features" in original publication); however, for tabular, dense data all features are present and playing in the "final coalition" so all $$z' = 1$$.  The $$\phi_j$$ is the feature attribution, which is computed in such a way that it becomes the Shapley value.

#### KernelSHAP
---

For each instance, $$x$$, from the dataset:

1. Sample $$K$$ total random coalitions, $$z_k' \in {0,1}^M$$ where $$k \in {1, \dots, K}$$ and $$M$$ is the maxium coalition size.
2. Use the function $$h_x(z_k')$$ to map the coalition to a set of feature values, then predict the output, $$f(h_x(z_k'))$$, for each coalition.
3. Compute the weight for each $$z_k'$$ from the SHAP kernel (discussed below).
4. Fit the weighted linear model (below); the coefficients on each term are the (estimated) Shapley value for 

The function $$h_x(z_k')$$ maps a coalition \[sequence of 0's and 1's in $$z_k'$$ = (0, 1, 1, 0, ...)\] to a valid data instance by using the values from the instance $$x$$ - you take values where there are "1"s and fills all "0"s with values from a (different) randomly sampled data instance.  For tabular data, the figure below taken from Molnar explains. This sampling from the marginal distribution means, again, ignoring correlations and being subject to the affects of using unlikely data instances.  However, as discussed in the Shapley value section previously, for this to be a true Shapley value, it has to be done this way. 

<a href="https://christophm.github.io/interpretable-ml-book/shap.html#kernelshap"><img style="float: center" src="/assets/img/fig5.48_molnar.jpeg" width=800px></a>

"The big difference to LIME is the weighting of the instances in the regression model. LIME weights the instances according to how close they are to the original instance. The more 0's in the coalition vector, the smaller the weight in LIME. SHAP weights the sampled instances according to the weight the coalition would get in the Shapley value estimation."

> "Small coalitions (few 1's) and large coalitions (i.e. many 1's) get the largest weights. The intuition behind it is: We learn most about individual features if we can study their effects in isolation. If a coalition consists of a single feature, we can learn about the features' isolated main effect on the prediction. If a coalition consists of all but one feature, we can learn about this features' total effect (main effect plus feature interactions)."


The SHAP kernel is given by

{% raw %}
$$
\pi_x(z') = \frac{M-1}{{M \choose |z'|} |z'| (M-|z'|)} = \frac{1}{M} \frac{1}{M-2 \choose |z'|-1}
$$
{% endraw %}

where \|z'\| is the number of features present in the sampled coalition, z' (number of "1"s).  This looks similar to the weight used in computing a Shapley value, and in fact, "Lundberg and Lee show that linear regression with this kernel weight yields Shapley values. If you would use the SHAP kernel with LIME on the coalition data, LIME would also estimate Shapley values!"  We simply need to minimize the normal squared error loss function with this weight.

{% raw %}
$$
L(f,g,\pi_x) = \sum_{z' \in Z} \left[ f(h_x(z')) - g(z') \right]^2 \pi_x(z')
$$
{% endraw %}

"We can be a bit smarter about the sampling of coalitions: The smallest and largest coalitions take up most of the weight. We get better Shapley value estimates by using some of the sampling budget K to include these high-weight coalitions instead of sampling blindly. We start with all possible coalitions with 1 and M-1 features, which makes 2 times M coalitions in total. When we have enough budget left (current budget is K - 2M), we can include coalitions with two features and with M-2 features and so on. From the remaining coalition sizes, we sample with readjusted weights."

#### TreeSHAP
---
  * model specific alternative (faster) to KernelSHAP - computes in polynomial time instead of exponential time.
  * can produce non-intuitive feature attributions.

This approach uses conditional expectation not marginal expectation; the problem is that irrelevant features which should get a SHAP value of 0 can be non-zero if the feature is correlated with a feature that does influence the prediction.  

As an example, here is how to compute the expected prediction for one tree, given an instance x and chosen feature subset determined by $$z'$$. If we conditioned on all features ($$z$$ is all "1"s) then the expectation is the prediction from the terminal node that x belongs to (a "normal" use of decision tree).  Conditioned on no features ($$z'$$ all "0"s), just use a weighted average of all terminal nodes in the tree. If $$z'$$ has some "0"s and some "1"s, then: (1) ignore predictions of unreachable nodes, i.e., ones the decision path can't reach given the values in the instance selected, $$x$$, indicated by $$z'$$; then (2) average predictions from remaining terminal nodes weighted by node sizes (number of training samples in that node). 

However, the TreeSHAP package can/does(?) use the marginal distribution.  This is still unclear to me.

#### Using SHAP
---

The python package [here](https://github.com/slundberg/shap) documents all the procedures, and examples of different use cases.

1. (Global) Feature Importance Plots - indicates which features are the most important for the model
A global feature importance can be found by summing individual Shapley values across the data set: $$I_j = \sum_{i=1}^n |\phi_j^{(i)}|$$. "SHAP feature importance is an alternative to permutation feature importance. There is a big difference between both importance measures: Permutation feature importance is based on the decrease in model performance. SHAP is based on magnitude of feature attributions."
**But remember: "All effects describe the behavior of the model and are not necessarily causal in the real world."**

2. Summary Plot - combines feature importance with feature effects to get a sense of the distribution of the Shapley values for each feature.
"SHAP dependence plots are an alternative to partial dependence plots and accumulated local effects. While PDP and ALE plot show average effects, SHAP dependence also shows the variance on the y-axis. Especially in case of interactions, the SHAP dependence plot will be much more dispersed in the y-axis."

3. Dependence Plot - shows the actual form of the relationship between the value (magnitude) of a feature and its impact on the model predictions.
Note that it has been recently shown that LIME (and also SHAP) can be fooled by manipulating the perturbation scheme. See the paper [here](https://dl.acm.org/doi/pdf/10.1145/3375627.3375830).

4. Stacked Force Plots - cluster data with similar explanations.
The goal of clustering is to find groups of similar instances which is usually based on feature values; however, these are usually on different scales (i.e. height vs weight or color).  Computing distances (such as Euclidean) can hard to justify; hwoever, all SHAP values have the same "unit" (whatever the unit of the prediction space is). Instead, SHAP clustering works by clustering on Shapley values of each instance, i.e., you cluster based on explanation similarity. 

Note:
  * "With SHAP, global interpretations are consistent with the local explanations, since the Shapley values are the "atomic unit" of the global interpretations. If you use LIME for local explanations and partial dependence plots plus permutation feature importance for global explanations, you lack a common foundation."
  * However, SHAP can be [fooled by adversial attack](https://dl.acm.org/doi/abs/10.1145/3375627.3375830). "It is possible to create intentionally misleading interpretations with SHAP, which can hide biases. If you are the data scientist creating the explanations, this is not an actual problem (it would even be an advantage if you are the evil data scientist who wants to create misleading explanations). It is a disadvantage as the receiver of an explanation, as you can be less sure about their truthfulness."  This can erode trust in this approach by the public or whoever is "consuming" the explanation, though.
  * Explainers for other things like deep neural nets also exist, as documented in the [python package](https://github.com/slundberg/shap).
  * KernelSHAP is model agnostic but slow.

## Explanation by Example
[Back to Top](#top){: .btn .btn--warning}
  * select particular instances of the dataset to explain the behavior of machine learning models or to explain the underlying data distribution.
  * mostly model-agnostic, because they make any machine learning model more interpretable.
  * only make sense if we can represent an instance of the data in a humanly understandable way (works well for images, hard for tabular data).
  * works well if there are only a handful of features or if we have a way to summarize an instance.

The k-nearest neighbors (KNN) method works explicitly with example-based predictions since it "memorizes" the dataset. A knn model makes a prediction based on the k-most-similar data points and returns the average as a prediction. 

### Counterfactual explanations
----
  * tells us how an instance has to change to significantly change its prediction. 
  * can learn about how the model makes its predictions and can explain individual predictions.
  * there are model-specific and model-agnostic versions.
  * contrastive to a chosen instance and selective (usually focus on a small number of features).
  * subject to "Rashomon effect" - i.e., can be multiple, contradictory explanations.
  * opposite of Anchors / Scoped Rules.

"A counterfactual explanation describes a causal situation in the form: 'If X had not occurred, Y would not have occurred'. For example: 'If I hadn't taken a sip of this hot coffee, I wouldn't have burned my tongue'." For ML models, the features are the causes of the prediction.  **A counterfactual explanation describes the smallest change to the feature values that changes the prediction to a predefined output.**

The "Rashomon effect" can be addressed either by (1) reporting all counterfactual explanations or (2) by having a criterion to evaluate and select the best one.  A good counterfactual should:
1. produce the predefined prediction as closely as possible,
2. be as similar as possible to the instance's feature values which is being explained (low Manhattan or Gower distance),
3. change as few features as possible relative to instance (low L0 norm),
4. have feature values that are likely.

There are different methods to generate counterfactual explanations, but they essentially are all based on these sort of criterion, and define a loss function which should be mininized to locate the desired explanation (differ based on loss function and optimization method).

Code repos can be found:
[Alibi](https://github.com/SeldonIO/alibi)
[Mace](https://github.com/amirhk/mace)
[Dice](https://github.com/interpretml/DiCE)

#### Method by Wachter et al.
----
Wachter et al. introduced counterfactual explanations as an explanation method [here](https://heinonline.org/hol-cgi-bin/get_pdf.cgi?handle=hein.journals/hjlt31&section=29) in 2017.  For counterfactual, $$x'$$, with desired outcome, $$y'$$ we have

{% raw %}
$$
L(x,x',y',\lambda) = \lambda \left[ \hat{f}(x') - y' \right]^2 + \sum_{j=1}^p \frac{|x_j-x_j'|}{MAD_j}
$$
{% endraw %}

where the Manhattan distance between the instance and counterfactual (each with p features) is scaled by the median absolute deviation over the dataset, $$MAD_j = {\rm median}(\|x_{i,j} - {\rm median}(x_{l,j})\|)$$.  This is more robust to outliers than, say, a Euclidean distance.  A larger $$\lambda$$ implies we want counterfactuals that are closer to the data instance being explained, while a lower value means we prefer explanations that have more similar feature values; however, it is not obvious how to select a value for this. Wachter et al. instead suggest select a tolerance for how far away the prediction of the counterfactual instance is allowed to be from y' (desired): $$ \|\hat{f}(x') - y'\| < \epsilon $$. 

General algorithm:
1. Select x to be explained, the desired outcome y', a tolerance $$\epsilon$$ and a (low) initial value for $$\lambda$$.
2. Choose random instance as couterfactual.
3. Optimize the loss with the initially sampled counterfactual as starting point.
4. While $$\| \hat{f}(x') - y' \| > \epsilon$$: 
 - increase $$\lambda$$.
 - Optimize the loss with the current counterfactual as starting point.
 - Return the counterfactual that minimizes the loss.
5. Repeat steps 2-4 and return the list of counterfactuals or the one that minimizes the loss.

However, this does handle categorical features well, though Wachter et al. suggest some approaches.

#### Method by Dandl et al.
---
Essentially defines a loss function for each of the 4 objectives listed, but rather than combining into a single, weighted function as in Wachter et al., all are optimized simultaneously using the **Nondominated sorting genetic algorithm.** (NSGA-II)
1. create an initial group of counterfactual by randomly changing some of the features compared to the instance, x, being explained.
2. evaluate each candidate using the four objective functions; from them, randomly select some candidates where fitter ones are more likely to be chosen.
3. crossover to produce "offspring" (crossover categorial, average numerical features).
4. mutate that generation.
5. select the best half from each group (parents, children) by using crowding distance sorting algorithm (based on objective values); the "best" are the most promising and/or the more diverse half.
6. repeat until we approach a diverse set of promising candidates with low objective values.

"Nondominated means that none of the counterfactuals has smaller values in all objectives than the other counterfactuals. We can think of our counterfactuals as a set of trade-off solutions."

"The counterfactual method creates a new instance, but we can also summarize a counterfactual by reporting which feature values have changed. This gives us two options for reporting our results. You can either report the counterfactual instance or highlight which features have been changed between the instance of interest and the counterfactual instance."

"The counterfactual method does not require access to the data or the model... offer a balance between explaining model predictions and protecting the interests of the model owner."

### Adversarial examples
---
  * small, intentional feature perturbations that cause a machine learning model to make a false prediction.
  * counterfactuals used to fool machine learning models. 
  * emphasis is on changing the prediction, NOT explaining it.

Primarily the subject of cybersecurity, etc. research.

### Prototypes
---
  * representative instances (prototypes) and criticisms (counterexamples of prototypes).
  * model agnostic - can even be used to summarize data without any ML model.
  * prototypes and criticisms are always actual instances from the data, unlike  counterfactuals.

"A prototype is a data instance that is representative of all the data. A criticism is a data instance that is not well represented by the set of prototypes."  Below is a figure illustrating both taken from Molnar's book.

<a href="https://christophm.github.io/interpretable-ml-book/proto.html"><img style="float: center" src="/assets/img/fig6.7_molnar.png" width=800px></a>

Clustering algorithms can be used to find prototypes, however, they rarely can find criticisms. The following is a review of the [MMD-critic method by Kim et al.](https://papers.nips.cc/paper/6300-examples-are-not-enough-learn-to-criticize-criticism-for-interpretability) which combines the two into a single framework.  Essentially, the method looks at the distribution of selected prototypes and the overall data distribution, then tries to minimize the discrepancy between the two; prototypes are then selected from regions of high probability, while criticisms come from those in low probability.  The number of each is specified at the beginning by the user.

To estimate probability distribution a maximum mean discrepancy function is used.  The MMD measures the difference between two distributions (data, $$x$$, and prototypes, $$z$$), which is the supremum over a function space of differences between the expectations according to the two distributions.  This requires a kernel, $$k$$, often taken to be the radial basis function, for example.

{% raw %}
$$
MMD^2 = \frac{1}{m^2} \sum_{i,j=1}^mk(z_i, z_j) -\frac{2}{mn}\sum_{i,j=1}^{m,n}k(z_i,x_j) + \frac{1}{n^2}\sum_{i,j=1}^n k(x_i,x_j)
$$
{% endraw %}

where, 

{% raw %}
$$
k(x,x') = {\rm exp} \left( -\gamma ||x-x'||^2 \right)
$$
{% endraw %} 

"The key to bringing MMD2 down to zero is the term in the middle, which calculates the average proximity between the prototypes and all other data points (multiplied by 2). If this term adds up to the first term (the average proximity of the prototypes to each other) plus the last term (the average proximity of the data points to each other), then the prototypes explain the data perfectly."

In the figure below (from Molnar) the data is shown in black (density estimated by shaded background) with the prototypes indicated in red; the bottom right is the best choice.

<a href="https://christophm.github.io/interpretable-ml-book/proto.html"><img style="float: center" src="/assets/img/fig6.8_molnar.png" width=800px></a>

Prototypes are found by a greedy search:
1. Start with empty set of prototypes.
2. While $$n_p < m$$ (desired number of prototypes): for each point in the dataset, test how much MMD2 is reduced if it were selected, then select the point that minimizes MMD2.
3. Return the list of prototypes.

Criticisms use the kernel function to identify how much the data and prototype distributions differ at a particular point.  Criticisms have a large (positive or negative) value for the witness function:

{% raw %}
$$
{\rm witness}(x) = \frac{1}{n} \sum_{i=1}^mk(x, x_i) - \frac{1}{m}\sum_{j=1}^m k(x,z_j)
$$
{% endraw %}

"If the witness function for a point x is close to zero, the density function of the data and the prototypes are close together, which means that the distribution of prototypes resembles the distribution of the data at point x. A negative witness function at point x means that the prototype distribution overestimates the data distribution (for example if we select a prototype but there are only few data points nearby); a positive witness function at point x means that the prototype distribution underestimates the data distribution (for example if there are many data points around x but we have not selected any prototypes nearby)."

"Like prototypes, criticisms are also found through greedy search. But instead of reducing the overall MMD2, we are looking for points that maximize a cost function that includes the witness function and a regularizer term. The additional term in the optimization function enforces diversity in the points, which is needed so that the points come from different clusters."

Using the prototypes, you can create a "nearest-prototype model" (akin to KNN) to explain black box predictions by maximizing the kernel (minimizing distance) value for a point in question relative to all prototypes.  Another option is to predict the output for the prototypes and criticisms using a ML model, then analyze them; which ones were right/wrong, etc.?  The prototypes and criticisms act simply as representative points to test the model with.  In that sense, prototypes are akin to using the [Nystroem method](https://scikit-learn.org/stable/modules/generated/sklearn.kernel_approximation.Nystroem.html#sklearn.kernel_approximation.Nystroem) for kernel approximation.

It seems like the number of prototypes could be selected using a k-medoids or other cluster-counting approach (silhouette coefficient); however, the number of criticisms is less obvious - it seems they are generally taken as equal.

Code for the MMD-critic approach can be found [here](https://github.com/BeenKim/MMD-critic).

### Influential Instances
---
  * data points that were the most influential for the parameters of a prediction model.
  * one of the best debugging tools for machine learning models.
  * should be based on ["robust statistics"](https://en.wikipedia.org/wiki/Robust_statistics) to avoid affects due to outliers and model assumption violations.
  * deletion diagnostics are model-agnostic.

A training instance may be considered "influential" if, upon deletion from the training set, the model somehow changes "significantly". Outliers can be very influential.  "This is useful to know for debugging the data. Is there a problematic instance? Are there measurement errors? The influential instances are the first ones that should be checked for errors, because each error in them strongly influences the model predictions." There are basically 2 ways to find influential instances: by deleting an instance and retraining, or by "upweighting" an instance.

**It is generally helpful to train an interpretable model (such as a decision tree) to then explain the difference between influential and non-influential points (binary classification) after the influential ones have been identified.**

You should also check the influential instances for "correctness", i.e., no bad measurements were made - by definition, these instances are the most important and therefore merit the most attention.  This could be used for some sort of automated control / checking of new data in online settings, I think.

(1) Deletion Diagnostics
  * DFBETA: only for parametric models (neural nets, linear regression, logistic regression, etc. but not decision trees, for example).
{% raw %}
$$
{\rm DFBETA}_i = \beta - \beta^{(-i)},
$$
{% endraw %}
  where $$\beta^{(-i)}$$ refers to the model parameter when the model is retrained with instance $$i$$.
  * Cook's distance: only for regression (or where MSE can be defined) where we have $$p$$ features.
{% raw %}
$$
D_i = \frac{ \sum_{j=1}^n (\hat{y}_j - \hat{y}_j^{(-1)})^2}{p \times {\rm MSE}}
$$
{% endraw %}
  * In general, a simple influence measure (which could be based on any norm, really)
{% raw %}
$$
I^{(-i)} = \frac{1}{n} \sum_{j=1}^n \vert| \hat{y}_j - \hat{y}_j^{(-i)} \vert|
$$
{% endraw %}

(2) Influence functions
  * [Koh and Liang (2017)](https://arxiv.org/abs/1703.04730) suggested using influence functions, a method of robust statistics, to measure how an instance influences model parameters or predictions.
  * requires a loss function that is twice differentiable with respect to its parameters (no trees, but neural nets work, for example).
  * the loss function is modified to increase the weight on a given data instance, so that when minimization is performed you can get different model parameters.
  * influence is essentially a quadratic expansion at that point in parameter space - uncertainties are always associated with such an expansion.
  * more complicated than deletion diagnostics but does not require retraining the entire model for each point in the dataset.
  * can be used to create adversarial training data.

Influence function [code](https://github.com/kohpangwei/influence-release) is available, while deletion diagnostics are simple enough to manually program.

### KNN Model
---
  * a "lazy" learner which memorizes the dataset.
  * perhaps the simplest "example-based" explanation method.

## Notes on Decorrelating Features
[Back to Top](#top){: .btn .btn--warning}

In order to devise a set of decorrelated features one can take a number of different approaches.  It is possible to compute the [Friedman H statistic](https://projecteuclid.org/download/pdfview_1/euclid.aoas/1223908046) but this is very expensive.  A python toolkit for doing with gradient boosted trees is available [here](https://pypi.org/project/sklearn-gbmi/).  This will tell how correlated pairs of features are, for subsequent analysis.
Alternatively, one can perform clustering based on the Spearman's rank-order correlation and subsequent hierarchical clustering, then select features from different clusters as decorrelated. See this [example](https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance_multicollinear.html?highlight=spearman) from scikit-learn.  This subset of features can also be tested with the H statistic as a check.  An example Jupyter notebook illustrating this can be found [here](/examples/decorrelating_ml_features/).

Note that PCA, for example, can produce decorrelated features, by definition; though these principle components may be difficult to interpret themselves.

Also see Variable Interaction Networks (VIN) by [Hooker (2004)](https://dl.acm.org/doi/pdf/10.1145/1014052.1014122) and partial dependence based feature interaction by [Greenwell et al. (2018)](https://arxiv.org/pdf/1805.04755).

## Dashboards for SHAP and LIME
[Back to Top](#top){: .btn .btn--warning}

* [InterpretML](https://github.com/interpretml/interpret-community) from Microsoft
* [ExplainerDashboard](https://github.com/oegedijk/explainerdashboard) - see [Tutorial](/tutorials/configuring_explainerdashboard/) for setting this up.


