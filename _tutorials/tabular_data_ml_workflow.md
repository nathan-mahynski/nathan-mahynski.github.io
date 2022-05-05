---
title: "ML Workflow Guidelines for Tabular Data"
excerpt: "End-to-end best practices for doing ML with dense, tabular data."
header:
  teaser: /assets/img/workflow.png
  image: /assets/img/tabular_data_ml_workflow_header.png
tags:
  - python
  - feature engineering
  - machine learning
  - workflow
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

This is the workflow for tabular, dense data that has been the most useful to me.  Note that steps 5-7 can sometimes replaced by an Auto-ML framework like [auto-sklearn](https://automl.github.io/auto-sklearn/master/); however, this workflow gives you a little more control.


<a href="https://github.com/PacktPublishing/Python-Machine-Learning-Second-Edition/blob/master/Chapter01/images/01_09.png"><img style="float: right" src="/assets/img/01_09_rashka.png" width=400px></a>
This figure, from "Python Machine Learning: 2nd Ed." by Raschka & Mirjalili, illustates the overall workflow pattern.  Below, I discuss some specifics at different stages.

# 1. [Preprocessing] Exploratory Data Analysis (EDA)
Look at you data, plot things, look for patterns that might seem intuitive.  This is somewhat controversial in the data science community as this process can be influenced by a user's bias; however, for scientific applications this can be an invaluable step.  This notebook contains an example workflow that accompanies this [tutorial](/tutorials/eda/) on EDA methods that I have found helpful: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nathan-mahynski/nathan-mahynski.github.io/public?filepath=%2F_tutorials%2Feda%2Feda_starting_point.ipynb) 
 * See [ml_inspector.data](https://github.com/mahynski/ml_inspector) for tools to help visualize data.
 * The [seaborn](https://seaborn.pydata.org/) library has a lot of valuable tools to consider also.
 * Pre-processing is an art.  You may have to deal with missing data, imputation, standardization, etc.  See Rashka & Mirjalili's book, [chapter 3](https://github.com/rasbt/python-machine-learning-book-3rd-edition/tree/master/ch04) for some discussion and suggestions.  This is not exhaustive and you may have to try several different things.
 
# 2. [Preprocessing] Decorrelate your features
Other [notebooks and examples](/examples/decorrelating_ml_features/) illustrate how to, for example, use hierarchical clustering and the Spearman rank order correlation to create decorrelated data.
Down-select from all features to find the subset that is decorrelated.  You may want to try several different datasets / sets of features.
 * This method of decorrelating is unsupervised so we can use it on the entire dataset if we need to.  The results may be slightly biased toward this specific set of observations but should not be too bad if you have a reasonable amount of data.  This step is somewhat cursory anyway.
 * Note: if you are going to use a subspace method like PCA or LDA to find orthogonal axes as a pre-processing step in your pipeline, by definition, these (engineered, orthogonal) features are independent; however, these contrived axes can be hard to understand or explain at times. It may also turn out that you do not end up using such a projection in your final pipeline anyway.  You can still perform a decorrelation analysis even if you choose to use such a method, it just might not be necessary.

# 3. [Preprocessing] Balance classes after test/train split
This is only relevant for classification problems, but can help combat bias. [SMOTE](https://imbalanced-learn.org/stable/over_sampling.html?highlight=smote) is perhaps the most common method to balance a dataset, but simple resampling, using weights inside a specific ML model, or more advanced over/under sampling approaches can be done; see the [imblearn](https://imbalanced-learn.org/stable/index.html) package for examples and options.  **This should only be performed on the training set.**  This means a global test/train split must occur now; also, the training set (which will most likely be split further into training and validation folds with CV later on) reflects the actual sampled data / real world (hopefully).  This is also good because you will always have a completely held out test set to compare your final model against.  All cross validations and statistical methods to compare pipelines benefit from this; the only drawback is that you have slightly less data to train the model on.  Arguably, you **should not** put this data back in at the end and retrain the final production model on the re-balanced + test set because now the set is unbalanced again which was the entire thing you are trying to avoid in the first place.  Usually, nested CV, etc. being used to evaluate and compare pipelines up until that point would have seen balanced data and this means the production model may not behave as expected.
 * You should generally standardize your data before using SMOTE as well; this is because usually features exist on different scales or in different units.  SMOTE computes neighbors using a simple Euclidean distance metric and having features on different scales will bias this.  Good practice: (1) standardize train set, (2) SMOTE-balance train set, (3) transform train set back into de-standardized space before use.  Pipelines will generally apply their own standardization schemes next and so we do not want to interfere with that.  This way, we just generate reasonably realistic dummy data. 
 * Best practice is to actually include the resampling as part of your pipeline during CV.  This is because the model may learn to fit well to balanced or synthetic data but will be poor on imbalanced sets containing only "real" observations.  See [ml_utils.sklearn.imbalanced.ScaledSMOTEENN](https://github.com/mahynski/ml_utils/blob/main/sklearn/imbalanced.py) as an example.
 * See this [example](/examples/imbalanced_datasets/) for further illustration.
 * You may wish to look at other metric besides just accuracy if your dataset is unbalanced and you do not wish to balance it.

# 4. [Preprocessing] Perform BorutaSHAP
[BorutaSHAP](/examples/borutashap/) is a powerful tool to find, from the decorrelated feature set, which features have statistical power to predict the target.  Other feature selection algorithms are also possible.  Sometimes, methods such as shrinkage or regularization are incorporated directly into the model as a hyperparameter are tuned next so BorutaSHAP is just an additional "helper" function along the way which is not always necessary.
 * This is supervised so use this on the **training set only** to avoid "leaking" information into your model and creating bias.
 * More notes can be found [here](/notes/borutashap).
 * Works on raw input, not on PCA or LDA-based orthogonal features.
 * Best practice is to also include this in your pipeline to be part of the nested CV, etc. procedures.
 * See [ml_utils.sklearn.feature_selection.PipeBorutaSHAP](https://github.com/mahynski/ml_utils/blob/main/sklearn/feature_selection.py) for an example.

# 5. [Learning] Perform nested or repeated (k-fold) CV
 * See [here](/examples/cross_validation) for examples and notes on cross validation methods, including notes on statistical methods.
 * This estimates the generalization error of the joint hyperparameter optimization and model fitting stages to yield an estimate of what the best pipeline to use will be. A simple, coarse hyperparameter grid is usually sufficient at this stage to get reasonable comparison between methods.
 * See [ml_utils.sklearn.cross_validation.NestedCV](https://github.com/mahynski/ml_utils/blob/main/sklearn/feature_selection.py) for an example implementation.

# 6. [Evaluation] Perform statistical tests and choose final model 
Using the results of the last step, we can test for which pipeline or model is the "best".  The "one standard deviation" rule, or other heuristics may be helpful, and you should consider what you want to achieve and what "best" means given the situation: performance? explainability? reprodibility? simplicity?
 * See [ml_inspector.model](https://github.com/mahynski/ml_inspector/model.py) for tools to help evaluate different models.
 * Statistical tests, either hypothesis drive (frequentist) or Bayesian, can be done.
 * In many AutoML frameworks, the best performing pipeline is simply chosen without any statistical tests based on repeated CV; this can be fine, but usually these tests let you look for more parsimonious models that perform similarly to the best model, and are often better choices in terms of interpretability.

# 7. [Evaluation] Re-train the chosen pipeline
Re-train on the entire (i.e., SMOTE-balance) training set using simple CV to select the best hyperparameters.  The model's performance is estimated using the result from the previous step, not from this model.  However, it is possible to then test this model on the held-out test set (not rebalanced) to check if it matches up reasonably.  If not, investigate further.
  * Again, see this [example](/examples/cross_validation) for details.
  * Use a tool like [hyperopt-sklearn](https://github.com/hyperopt/hyperopt-sklearn) to rigorously optimize the hyperparameters.  The step 5 probably used (coarse) grid searching, which is fine for estimating the qualitative performance of different models and their generalization error, but your final model is worth optimizing more.

# 8. [Prediction] Predict and perform explainability analysis 
Use SHAP, global surrogate models, etc. to explain predictions.  This can be helpful for both the test and train sets, but we are usually interested evaluating and understanding predictions made on the training set.
 * See notes [here](/notes/interpretable_machine_learning) on interpretable ML.
 * You can use dashboards to help make your models more [comprehensible](/tutorials/configuring_explainerdashboard).
 * This [notebook](/notes/borutashap) which incorporates BorutaSHAP includes some example explanations.
 * The [SHAP](https://github.com/slundberg/shap) library also has plenty of nice examples.
