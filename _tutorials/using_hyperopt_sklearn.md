---
title: "Using hyperopt-sklearn"
excerpt: "Automatic hyperparameter optimzation in scikit-learn."
header:
  teaser: /assets/img/brain_trig_ml.png
  image: /assets/img/using_hyperopt_sklearn_header.png
tags:
  - python
  - hyperopt-sklearn
  - automl
  - machine learning
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

[hyperopt-sklearn](https://github.com/hyperopt/hyperopt-sklearn) is a python package that serves as a wrapper around [hyperopt](http://hyperopt.github.io/hyperopt/) that extends its functionality to [scikit-learn](https://scikit-learn.org/stable/) that can perform automatic hyperparameter tuning and other pipeline optimization.  It is based on [hyperopt](http://hyperopt.github.io/hyperopt/) and was originally presented in [Komer B., Bergstra J., and Eliasmith C. "Hyperopt-Sklearn: automatic hyperparameter configuration for Scikit-learn" Proc. SciPy 2014.](http://conference.scipy.org/proceedings/scipy2014/pdfs/komer.pdf)  This is an [AutoML framework](/notes/automatic_machine_learning/) though it is not quite as powerful as others like [TPOT](https://github.com/EpistasisLab/tpot) or [auto-sklearn](https://automl.github.io/auto-sklearn/master/); however, it can be more transparent and easier to extract optimized models from.  The primary advantage that I find with this tool is its simplicity and how easy it is to modify existing code to make use of it.  They envision a workflow akin to:


~~~ python
from hpsklearn import HyperoptEstimator, svc
from sklearn import svm

# Load Data
# ...

if use_hpsklearn:
    estim = HyperoptEstimator( classifier=svc('mySVC') )
else:
    estim = svm.SVC( )

estim.fit( X_train, y_train )

print( estim.score( X_test, y_test ) )
~~~

# Installation

Pip is required to install the code:

~~~ bash
$ conda activate my_environment # optional
$ git clone git@github.com:hyperopt/hyperopt-sklearn.git
$ (cd hyperopt-sklearn && pip install -e .)
~~~

# Overview

hyperopt-sklearn estimators are designed to be compatibile with the sklearn estimator API.  Essentially, each hyperopt-sklearn estimator comes with a default search space and when `.fit()` is called it searches this space using a predefined approach to find the best hyperparameters.  sklearn's built in GridSearchCV can be used for similar tasks, but grid searching is coarse and not likely to find the [truly optimal parameters](https://scikit-learn.org/stable/modules/grid_search.html#randomized-parameter-search); this approach is good for a first pass, or for when evaluating the [performance of different pipelines to choose one](/examples/cross_validation/).  After selection, more fine-grained tuning can be required.  

> As an aside, if the best pipeline you have is something like a random forest, or other ensemble method, they are not very sensative to specific hyperparameters so you might be able to get away without bothering with this step.  For other models, like SVC's or methods with shrinkage parameters (or neural nets, etc.) the sensativity can be higher so it might be worth doing this.

Their documentation contains more [details](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/estimator.py#L435), but one generally defines a HyperoptEstimator with a few details as illustrated below.

The first step is to decide what classifier(s) or regressor(s) to examine, and what their search space should look like.  There are generic defaults yo can fall back on; in this example I specifically care about optimizing a random forest.

~~~ python
from hyperopt import hp

name = 'hyperopt_rf'
kwargs={'bootstrap':True,
        'oob_score':False,
        'n_jobs':-1,
        'random_state':0, # For reproducibility
        'verbose':False,
        'n_estimators':hp.uniformint(name+'.n_estimators', 10, 1000),
        'max_features':hp.pchoice(name+'.max_features', [
            (0.5, 'sqrt'),  # most common choice.
            (0.5, None),    # all features, less common choice.
            ]),
        'criterion':hp.pchoice(name+'.criterion', [(0.5,'gini'), (0.5,'entropy')]),
        'max_depth':None, # Will use hpsklearn's default seach space then
}
~~~

You can specify properties that are present in sklearn's API for a supported model.  In the above example there are several instances of where a parameter is not fixed, but a range of values is provided.  For example, `'n_estimators':hp.uniformint(name+'.n_estimators', 10, 1000)` instructs Hyperopt-sklearn to search using uniform random integers between 10 and 1000 to use as the number of estimators in the forest.  Discrete choices can be specified as `hp.pchoice` where the first field is the name, followed by a list of tuples which provide the probability of a given choice and the choice itself (cf. `max_features`).  Different types of choices, like hp.uniform for floats or hp.loguniform are documented in [Hyperopt](https://github.com/hyperopt/hyperopt). Hyperparameters that are not specified default to use hyperopt-sklearn's default search space for the given estimator; these can be found in the [components.py](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py) file in their package.

With the search space specified we can define the HyperoptEstimator.

~~~ python
from hyperopt import tpe

estim = HyperoptEstimator(classifier=hpsklearn.components.random_forest(name, **kwargs),
                          preprocessing=[], # NO PREPROCESSING 
                          algo=tpe.suggest, # Use tree parzen estimator
                          max_evals=5000, # Total number of configurations to evaluate
                          trial_timeout=300, # Allow this many seconds to fit each
                          seed=0, # For reproducibility
                          refit=True) # Refit on entire training set
~~~

For classification problems, the argument `classifier` is used, whereas for regression problems we specify `regressor` instead.  See more documentation [here](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/estimator.py#L435).

Note that when preprocessing, or other keywords above are "None" they perform their default search (which usually is a random search over everything implemented in the package); if you wish to deactivate any step, such as not performing any preprocessing as shown above, you must provide an empty list (or iterable) instead.  Preprocessors are also documented in their code in the [components.py](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py) file.

Models are sought out using various methods defined in Hyperopt including: (1) random searching, (2) annealing, (3) trees, (4) Gaussian process trees, and (5) tree-structured Parzen estimators.  The last one is usually recommended by the authors - details are presented in their paper [here](https://papers.nips.cc/paper/4443-algorithms-for-hyper-parameter-optimization.pdf).  Refer to hyperopt's documentation to choose different options.

Finally, we execute the search by calling fit().

~~~ python
estim.fit(X=X_train, 
	y=y_train,
	valid_size=0.0, # Don't need separate validation set, use all of training
	n_folds=10,  # Stratified by default
	cv_shuffle=True,
	random_state=0)
~~~

During the fitting, k-fold cross-validation is used to assess the performance of different models.  When n_folds = -1 it uses LOOCV instead of k-fold.  Stratification is also performed automatically in the case of classification problems.  If n_folds is None, a simple train/validation split is done; validation sets also seem to be used for assessing early stopping.  If neither of those cases are met (as above), I set valid_size=0, but you can leave the default=0.2 if you wish. cv_shuffle stipulates whether or not to do sample shuffling before splitting the data into train and valid sets; if not using a validation set this doesn't matter, but it is good set this to True out of habit (the default is False). Finally, the random_state should be set for reproducibilty.

The results can be viewed easily with

~~~ python
estim.score(X_test, y_test) 
print(estim.best_model())
~~~

Notably, this workflow of (1) preprocessing, (2) model fitting, (3) model evaluation is fairly simple and straightforward.  Codes like TPOT can produce more elaborate pipelines, which can be a good or a bad thing depending on what you are looking for.  This workflow mirrors the traditional approach taken when first approaching a problem so I tend to favor this simplicity, at least at the beginning of a project.  It is also nice since you may perform tests elsewhere and determine that you want to use a given model, but need to optimize just the model's hypeparameters; this is fairly straightforward with hyperopt-sklearn whereas other AutoML frameworks are often intended to be more complete and do not allow you do work on just certain parts of your model pipeline.

# Automatic searching

Default settings in the package allow it to automatically try all classifiers, or all regressors, for example.  If you don't have a particular model in mind you can try all the ones implemented with their default search spaces.  These can be specified as [`any_classifier`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1665), [`any_sparse_classifier`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1682), [`any_regressor`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1691),  [`any_sparse_regressor`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1708), [`any_preprocessing`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1973), or [`any_text_preprocessing`](https://github.com/hyperopt/hyperopt-sklearn/blob/master/hpsklearn/components.py#L1987).

~~~ python
estim = HyperoptEstimator(classifier=any_classifier('my_clf'),
                          preprocessing=any_preprocessing('my_pre'),
                          algo=tpe.suggest,
                          max_evals=100,
                          trial_timeout=120)
~~~

A list of available preprocessors, classifiers, and regressors is available in the [README](https://github.com/hyperopt/hyperopt-sklearn).

# Another Classification Example

Below is example script for doing classification with an [XGBoost](https://xgboost.readthedocs.io/en/latest/) classifier.

~~~ python
import hpsklearn
from hpsklearn import HyperoptEstimator
from hpsklearn.components import standard_scaler
from hyperopt import tpe
import numpy as np
import pickle
from sklearn.preprocessing import LabelEncoder
import pandas as pd
import os
import json

# Set OMP
os.environ["OMP_NUM_THREADS"] = "40"

# Import Training Data
X_train = pd.read_csv('X_train.csv')
y_train = pd.read_csv('y_train.csv')

# Use features selected from BorutaSHAP 
features = json.load(open('boruta_shap.accepted', 'r'))

# Select features and encode y
X_train = X_train[features]
enc = LabelEncoder()
y_train = pd.DataFrame(enc.fit_transform(y_train.values.ravel()), columns=y_train.columns)
pickle.dump(enc, open('encoder.pkl', 'wb'))

name = 'hyperopt_xgb'

from hyperopt import hp
kwargs = {'n_estimators':None, #hp.uniformint(name+'.n_estimators', 20, 200),
        'learning_rate':None, #hp.loguniform(name+'.learning_rate', np.log(1.0e-3), np.log(1.0e0)),
        'random_state':0,
        'max_depth':None, #hp.uniformint(name+'.max_depth', 3, 10),
        'subsample':hp.uniform(name+'.subsample', 0.5, 1.0),
        'reg_alpha':None, #hp.loguniform(name+'.reg_alpha', np.log(1.0e-3), np.log(1.0e3)),
        'reg_lambda':None, #0, # Use just L1 not L2
        #'eval_metric':'mlogloss', # default, so ok
        }

# Instantiate a HyperoptEstimator with the search space and number of evaluations
estim = HyperoptEstimator(classifier=hpsklearn.components.xgboost_classification(name, objective="multi:softmax", **kwargs),
                          preprocessing=[], # NO PREPROCESSING
                          algo=tpe.suggest, # Use tree parzen estimator
                          max_evals=5000, # Total number of models to evaluate
                          trial_timeout=300, # Allow this many seconds to fit each
                          seed=0, # For reproducibility
                          refit=True) # Refit on entire training set

# Search the hyperparameter space based on the data
estim.fit(X=X_train, y=y_train.values.ravel(),
          valid_size=0.0, # Don't need separate validation set, use all of training
          n_folds=10,  # Stratified by default
          cv_shuffle=True,
          random_state=0) # CV shuffling RNG seed, for reproducibility

# Model has already been refit on all of training set
pickle.dump(estim, open('fitted_model.pkl', 'wb'))
~~~

# More Resources

Some other blogs with nice discussions and tutorials:

* [Jason Brownlee, How To](https://machinelearningmastery.com/hyperopt-for-automated-machine-learning-with-scikit-learn/)
* [Will Koerhrsen, on Bayesian Optimization](https://towardsdatascience.com/an-introductory-example-of-bayesian-optimization-in-python-with-hyperopt-aae40fff4ff0)
* [Kris Wright, On Parameter Tuning](https://medium.com/district-data-labs/parameter-tuning-with-hyperopt-faa86acdfdce)
