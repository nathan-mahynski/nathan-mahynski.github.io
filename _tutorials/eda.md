---
title: "EDA Suggestions"
excerpt: "Recommendations for exploratory data analysis."
header:
  teaser: /assets/img/rocket_cartoon.png
tags:
  - python
  - eda
  - machine learning
---

{% include toc icon="gears" title="Table of Contents" %}

## Summary

EDA is a subset of IDA
IDA = clean up data and fix issues
EDA = generate hypotheses about data
Subsequent modelling = test those hypotheses

NIST ITL has eda guidelines: https://www.itl.nist.gov/div898/handbook/eda/section1/eda11.htm

https://en.wikipedia.org/wiki/Exploratory_data_analysis

robust statistics
search for outliers - box plots

pairplots
look at class imbalance
cluster/silhouett analytes
correlation analysis
try to fit decision trees for classifiers, OLS for regressions; recusively eliminte features from trees to see effects
spearman rank order correlations, non-parametrics (should be evident in pairplot, but if you have too many to plot, use this to screen)

Jensen-Shannon divergence for initial test of distributions that separates classes

