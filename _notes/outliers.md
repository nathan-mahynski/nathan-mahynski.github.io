---
title: "Outlier and Novelty Detection"
excerpt: "What happens when your data shifts?"
header:
  teaser: /assets/img/jupyter_logo.png
tags:
  - python
  - deep learning
  - outlier detection
  - distributional shift
  - novelty
---

<!-- Enter details at https://mybinder.org/, then copy the badge below -->

[![Made withJupyter](https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter)](https://jupyter.org/try)

Terminology
---
There are many fields of study, and associated tools, to detect "outliers."  A simple definition of an outlier is a point which falls outside the expected distribution of known examples (inliers).  Other terms found in statistics, machine learning, and AI reserch include: "novelty", "distributional shifts", "semantic shifts," etc.  There are subtle difference between these terms and how one goes about detecting them.

Let's start with outlier vs. novelty detection in machine learning, from [scikit-learn](https://scikit-learn.org/stable/modules/outlier_detection.html):

* Outlier detection: used to detect if a point belongs to a distribution determined by the consensus of other points.  This occurs when the training data is "dirty" and we need to clean it by removing unusual points.
* Novelty detection: used to determing if a NEW observation belongs to a previously determined distribution.  In this case, a model is trained with "clean" data only and predicts if a new point "matches" what it has seen before.

In the broader AI realm, "distributional shifts" refer to when test data starts to look different from the data used to train a model.  This can be due to several sources which are referred to differently.  Generally distinction is made based on whether or not the model should respond in a binary fashion (yes or no, this is an outlier) or should also be able to discriminate between known classes.  Here are some nice surveys:

1. [Generalized Out-of-Distribution Detection: A Survey](https://arxiv.org/abs/2110.11334) by Yang et al.
2. [A Unified Survey on Anomaly, Novelty, Open-Set, and Out-of-Distribution Detection: Solutions and Future Challenges](https://arxiv.org/abs/2110.14051) by Salehi et al.

* Out-of-distribution (OOD) - data is coming from a different distribution that what was trained on.  This can occur in 2 ways:
    * Semantic shift - new classes are appearing; e.g., a classifier was trained on cats and dogs, but is now being shown a bird.
    * Covariate shift - samples from known classes but a different domain are appearing; e.g., the model is trained to recognize strawberries from one locale, but they can be grown all over the world, or perhaps you have trained on only one species of cat.  Good training/test set creation is meant to combat this; cross-validation is also able to help ensure there are no covariate shifts between the test and training data.
* Closed-world assumption - where the test data is assumed to be drawn IID from the same distribution as the training data. This occurs when all the known classes can be sampled from during training, so that all samples in the test phase are from the same set of classes (with no covariate shift).
* Open set recognition (OSR) - model needs to be able to classify all classes from what is known, and be able to identify if something is "none of the above"

Resources
---
A centralized repo summarizing many resources. [anomaly-detection-resources](https://github.com/yzhao062/anomaly-detection-resources)

Notes
---
This is a living document with examples and implementations of OOD detection tools. [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/outliers/ood.ipynb)