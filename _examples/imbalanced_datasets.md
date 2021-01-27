---
title: "Dealing with Imbalanced Datasets"
excerpt: "Using SMOTE and other methods to balance your data."
header:
  teaser: /assets/img/jupyter_logo.png
tags:
  - python
  - imbalanced
  - machine learning
  - smote
sidebar:
  - title: "Code"
    image: /assets/img/jupyter_logo.png
    image_alt: "logo"
    text: "python, jupyter"
---

<!-- 
https://www.analyticsvidhya.com/blog/2020/10/overcoming-class-imbalance-using-smote-techniques/
https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/
Rashcka book: resampling, "balancing parameters" is sklearn models 
https://datascience.stackexchange.com/questions/71741/how-to-apply-dataset-balancing-techniques-whilst-using-pipeline-in-sklearn
-->

{% include toc icon="gears" title="Table of Contents" %}

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/mahynski/mahynski.github.io/development?filepath=%2F_examples%2Fimbalanced_datasets%2Fexample.ipynb)

When building classifiers, class imbalance in the training set can be a big factor.  If a set is 80% A and 20% B, then a simple majority classifier (predict everything to be A) will have an 80% accuracy.  However, this is neither sensical, nor necessarily relevant for the real world if the balance is skewed relative to what is to be expected.  Dealing with class imbalance has been the subject (and continues to be) the subject of research, but there are many existing tools which can handle this problem reasonably well.

# Initial Approaches

First of all, you can use an alternative metric to accuracy.  Accuracy is subject to the issues with the majority class just illustrated; other metrics like precision or recall might be more helpful depending on your application. Second, you could employ class balancing methods within the ML model you are developling.  For example, [trees](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html?highlight=decision%20tree#sklearn.tree.DecisionTreeClassifier) and [SVCs](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) can weight the error of misprediction by the inverse of class frequency in the training set.  scikit-learn implements these as the `class_weight=balanced` option - refer to the documentation for more details.  This is particularly nice since methods based on ensembles of these atomic classifiers, like random forests, also inherit this built in capability.  Since RF's are almost always one of, if not the best, method for classifying dense, tabular data we can often rely on this feature; it can even be treated as hyperparameter during cross validation to test its importance and impact automatically.  In fact, this is such an effective tool for such data that I have rarely encountered a case when a class-balanced RF isn't the final model selected when operating on dense tabular data for scientific applications.  

# When Those Fail...

Unfortunately, not all models can handle this.  Unsupervised methods (e.g., PCA) in particular ignore class labels and if one class is highly sampled the data structure can be biased toward that region of phase space, resulting in what will usually become a poor model in production.  As a result, you can resort to other methods to balance the classes which will allow you more failry compare pipelines involving classifiers that cannot automatically balance classes with those that can.  These methods generally involve re-sampling the dataset, but there are a number of different ways to do so.

The simplest is to simply [resample](https://scikit-learn.org/stable/modules/generated/sklearn.utils.resample.html?highlight=resample) (draw with replacement) the minority classes until all classes have the same number of data point; the repeated reuse of the data can amplify bias toward these specific observations, however.  This is referred to as "oversampling" since it boosts the minority classes. It is also possible to undersample the majority class(es) by randomly removing some points.  Generally, the latter is less popular since data is usually very precious and we want to make as much use of it as possible.  It is also common practice to combine the two to shrink the majority class(es) a little, and amplify the minority class(es) a little so they meet somewhere in the middle.

Instead of re-using "real" data, it is also possible to create synthetic data.  There are a number of approaches, but perhaps the most common is [Synthetic Minority Over-sampling TEchnique (SMOTE)](http://www.jair.org/index.php/jair/article/view/10302); a nice python library [imblearn](https://imbalanced-learn.org/stable/index.html) implements this, and alternatives, and has excellent examples and tutorials. SMOTE works by looking at the k nearest neighbors of a point (which belong to the same class), selecting one randomly, then choosing a random distance to move along the vector connecting the two, essentially interpolating between them.  There are a number of variants of SMOTE, but the vanilla version is common; it does have a number of issues:

1. It is unclear what k to choose, and also, this imposes a minimum number of examples that must be in a dataset (k=10 won't work if you only have 9 observations of a class).
2. It uses Euclidean distance, which (usually) means features need to be on the same scale to make sense.
3. It results in a "stringy" datasets where points follow "lines" between points which is a bit artificial; moreover, noise is introduced when points of classes are very near near other and tend to make it harder to recognize decision boundaries.

The first point is relatively simple to solve if you treat k as a hyperparameter and optimize it with CV.  This means that SMOTE should really be part of your overall data modeling pipeline, not just a single preprocessing step.  With enough data, you can split the data into test/train sets; then, you should balance **only the training set** - we wish to train on data that doesn't bias the model fitting, but we need to look at only the real world data to assess.  If you generated synthetic data for testing you cannot be sure those points are meaningful.  The test data should always remain imbalanced.

The second point can actually be solved by standardizing the data before using SMOTE; this essentially non-dimensionalizes the data and places it all on the same scale.  For example, if you have a feature of height in millimeters, and weight in kilograms where people are being measured, the height feature will have a much larger magnitude.  SMOTE uses Euclidean distance to determine the nearest neighbors, so in this case differences in weight do not really contribute; the k nearest neighbors would really just be the k nearest samples with most similar height.  Thus, the data would make a certain minority class seem like they all have very similar heights, which could very easily confuse a model trained on that data.  The solution is (1) standardize, then (2) SMOTE resample, then (3) de-standardize so the resulting dataset now has synthethic data in its original units, but which has been resampled in a more even fashion across the features.  It is also important to stratify your test/train sets as well.  A final note is that outliers can affect standardization so [robust scaling](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) can be preferable to "standard scaling".

The third problem can be solved by combining the oversampling with undersampling.  [imblearn](https://imbalanced-learn.org/stable/combine.html#combine) currently implements 2 different methods: Tomek's links and edited nearest neighbors.  While subjective, SMOTE-ENN tends to clean up more noisy (re)samples than using Tomek's links.  Refer to their documentation for more information:

> "`SMOTE` allows to generate samples. However, this method of over-sampling does not have any knowledge regarding the underlying distribution. Therefore, some noisy samples can be generated, e.g. when the different classes cannot be well separated. Hence, it can be beneficial to apply an under-sampling algorithm to clean the noisy samples. Two methods are usually used in the literature: (i) Tomek’s link and (ii) edited nearest neighbours cleaning methods. Imbalanced-learn provides two ready-to-use samplers `SMOTETomek` and `SMOTEENN`. In general, `SMOTEENN` cleans more noisy data than `SMOTETomek`."

<a href="https://imbalanced-learn.org/stable/combine.html#combine"><img style="float: center" src="imblearn_smoteenn.png" width=600px></a>

# Implementation

Given the considerations above, best practice is to:

1. Do test/train split.
2. (robustly) Standardize training set.
3. SMOTEENN to balance the training set.
4. De-standardize to produce a balanced data set with synthetic in the original units.  This transform is based on the originally seen data, whose mean and other stats will be different from the newly balanced set.
5. Standardize the complete balanced training set.
6. Continue with pipeline...

Importantly, the above steps should actually be a part of a CV scheme - i.e., the test/train split (step 1) should be corresponding to one loop during a k-fold CV; this way, many different test/train splits are performed so the SMOTEENN balancing sees different data each time.  This allows the generlization error estimated from CV to include an error that comes from this resampling, which is excellent!

Examples of implementations are provided in the attached Jupyter notebook.  However, it is worth pointing out that in order to do this as part of the standard "pipeline" workflow employed in sklearn, we actually must empliy imblearn's `pipeline`.  This is because the .fit() and .transform() methods expected during sklearn's workflow do not expect both X and y to be outputs (just X), whereas during the resampling stage, both are output.

## An Example Workflow
1. Ward clustering to decorrelate dimensions (this is unsupervised) - see [this example](/examples/borutashap/).
2. Do test/train split.
3. Standardize, SMOTEENN, de-Standardize to get balanced training set, leaving test set imbalanced (set aside).
4. Use BorutaSHAP on balanced training set to remove extraneous features.
5. Perform AutoML/k-fold CV/etc. to train model on training set.  This pipeline can include its own Standardization step for the data since it is in the original units.
6. With model hyperparameters optimized, retrain on entire training set (balanced).
7. Report performance by testing on the imbalanced test set.

Usually, however, we do not have enough data to justify either (1) a single test train split initially, or (2) a single SMOTE resampling of the data.  Often, this biases the model to the specific observations in the data set.  A better approach is to use nested k-fold CV to optimize the pipeline and estimate its error, where the pipeline includes these steps (and therefore, so does the error).

## A Better Workflow
1. Ward clustering to decorrelate dimensions (this is unsupervised) - see [this example](/examples/borutashap/).
2. Define pipeline of (SMOTEENN (incl. standarization and destandardization as described) --> Standardize --> BorutaSHAP --> Train Model).  Arguably, the scaling and BorutaSHAP steps could be swapped since BorutaSHAP uses trees which are not affected by data scaling; but this way, it selects on what the model is about to see, so for philosphical reasons I tend to put that right before model training.
3. Perform nested k-fold CV on the entire dataset - this breaks the set into multiple test/train splits, the each training set is further subdivided into sets for fitting and testing.  This allows you to set k for SMOTEENN, etc. as a hyperparameters that can be chosen, and the error associated with finding that choice is built in to the final error from the nested CV. See [here](/examples/cross_validation/) for more information and a nested CV example.
4. Repeat the nested CV for pipelines with other models and perform [statistical tests](/examples/cross_validation/) to choose the best.
5. Select the best pipeline and re-train on the entire training set with a single k-fold CV loop to choose the hyperparameters.
6. Re-fit best model/pipeline using all the data to be the production model. Report the generalization (error) performance not from step 5, but from step 4.

# A Final Exhortation

Finally, if you can avoid simulated data, it is generally better to do so.  Trees and other classes with built-in class weighting schemes tend to perform at least as well as SMOTE (or other resampling-based) balanced pipelines, but don't come with additional hyperparameters.  This is also nice because AutoML frameworks, while extensible, do not always support custom preprocessing as is needed to use SMOTE correctly.

