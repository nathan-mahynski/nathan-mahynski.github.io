---
title: "EDA Suggestions"
excerpt: "Recommendations for exploratory data analysis."
header:
  teaser: /assets/img/rocket_cartoon.png
  image: /assets/img/eda_header.png
tags:
  - python
  - eda
  - machine learning
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

Exploratory data analysis (EDA) is ["is an approach to analyzing data sets to summarize their main characteristics, often with visual methods"](https://en.wikipedia.org/wiki/Exploratory_data_analysis) and is widely attributed to John Tukey.  During this phase one explores the data to look for unusual behavior and formulate hypotheses.  Testing and modeling comes later.  This is related but distinct from [data mining](https://en.wikipedia.org/wiki/Data_mining).

<a href="https://en.wikipedia.org/wiki/Exploratory_data_analysis"><img style="float: right" src="eda_flowchart.png" width=300px></a>

EDA is a superset of [initial data analysis (IDA)](https://en.wikipedia.org/wiki/Data_analysis#Initial_data_analysis) which is essentially just cleaning the data, imputing missing values, checking model assumptions (like normality), etc.  Much of IDA can be considered preprocessing, which [sklearn](https://scikit-learn.org/stable/modules/preprocessing.html) covers nicely.

If you are using python, EDA is easy in [seaborn](https://seaborn.pydata.org/index.html).

EDA can sometimes be controversial as there is a school of thought that suggests EDA can bias the data scientist at the outset toward false correlations or unsupported conclusions.  While this can be true, for most scientific applications the injection of background training/intuition at this stage is often more beneficial than harmful.  Still, you should always be wary of bias.

An example Jupyter notebook is available here which can be modified to your needs. [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nathan-mahynski/nathan-mahynski.github.io/public?filepath=%2F_examples%2Feda%2Feda_starting_point.ipynb)

EDA tools are constantly evolving and improving.  While the tools described below are good ways to get started, tools like [D-Tale](https://github.com/man-group/dtale) and [Lux](https://github.com/lux-org/lux) really make things very easy and fast.  

# Some Ideas

There are many EDA approaches so this will not be exhaustive.  If you are doing "simple" statistical modeling, checking model assumptions (normality, etc.), looking for outliers, etc. are probably the most important place to begin.  NIST's [engineering statistics handbook](https://www.itl.nist.gov/div898/handbook/eda/eda.htm) is a great reference and a good place to start; specifically, consider the [graphical techniques alphabet](https://www.itl.nist.gov/div898/handbook/eda/section3/eda33.htm) for different enlightening plots to make.

The following is just a (growing) list of approaches I have found useful, but are not remotely exhaustive.  Many of these are explicitly listed as part of [ml_inspector.data.InspectData](https://github.com/mahynski/ml_inspector/blob/master/data.py) and [ml_utils.eda](https://github.com/mahynski/ml_utils/tree/main/eda) which you can refer to for documenation and examples.

* [seaborn](https://seaborn.pydata.org/index.html) has lots of visualization tools; I regularly look at:
  * [grouped barplots](https://seaborn.pydata.org/examples/grouped_barplot.html)
  * [displots](https://seaborn.pydata.org/examples/faceted_histogram.html) to look at correlations between variables for classification tasks
  * [histplots](https://seaborn.pydata.org/examples/kde_ridgeplot.html) or [ridgeplots](https://seaborn.pydata.org/examples/kde_ridgeplot.html) to look at overlapping distributions
  * [boxplots](https://seaborn.pydata.org/examples/grouped_boxplot.html) 
  * [heatmaps](https://seaborn.pydata.org/examples/spreadsheet_heatmap.html) and [clustermaps](https://seaborn.pydata.org/examples/structured_heatmap.html) to visualize correlations

* Compute robust statistics to summarize, while minimizing the affect of any outliers you might have:
  * [Wikipedia's entry](https://en.wikipedia.org/wiki/Robust_statistics) discusses some examples.
  * It can be convenient to use [RobustScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html#sklearn.preprocessing.RobustScaler) instead of the [StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) in sklearn workflows.
  * Boxplots (listed above) are helpful for identifying outliers by using the quartile ranges to summarize the data.

* Compute non-parametric measures:
  * Spearman R(ank) order correlation - see [scipy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html#scipy.stats.spearmanr). sklearn has a nice [tutorial](https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance_multicollinear.html?highlight=spearman) of how to use this to establish multicollinearity between features.  An implementation is available [here](/examples/decorrelating_ml_features/).  In this example, you can choose to "cut" the dendrogram at some level which sets the level of decorrelation between the coarse-grained leaves.
  * See [ml_inspector.data.InspectData.cluster_collinear](https://github.com/mahynski/ml_inspector/blob/master/data.py#L116).

* Look for class imbalance during classification problems.
  * You may need to consider models that can automatically counter-weight to balance this (like trees or SVCs), or use
  * [SMOTE](/examples/imbalanced_datasets/), or other synthetic data generation approaches.
  * Use cluster/silhouette analysis to look for "natural groups" of data. See [ml_inspector.data.InspectData.cluster_elbow](https://github.com/mahynski/ml_inspector/blob/master/data.py#L24) and [ml_inspector.data.InspectData.cluster_silhouette](https://github.com/mahynski/ml_inspector/blob/master/data.py#L55).

~~~ python
import matplotlib.pyplot as plt
plt.bar(
    x=y.unique(),
    height=[np.sum(y==class) for class in y.unique()]
)
plt.title('Observations in database')
_ = plt.xticks(rotation=45)
~~~

* Use [PCA (unsupervised)](/examples/common_chemometrics/) in 2-3 dimensions to look for obvious trends on structure ([kernel PCA](https://scikit-learn.org/stable/modules/decomposition.html#kernel-pca) for more non-linear efforts).  Cross validation should be done to check the variability of the principal components (PC) if you decide there is something useful happening and need to explain it.  
  * For example, in a [Jupyter notebook](/tutorials/jupyter_best_practices/):

~~~ python
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

%matplotlib inline # 2D
pca = PCA(n_components=2)
ss = StandardScaler()
X_proj = pca.fit_transform(ss.fit_transform(X))

for class_ in y.unique():
    mask = y == class_
    plt.plot(X_proj[mask,0], X_proj[mask,1], 'o', label=class_)
plt.legend(loc='best')
plt.xlabel('PC 1')
plt.ylabel('PC 2')
plt.title('PCA on standardized dataset')

from mpl_toolkits.mplot3d import Axes3D # 3D
%matplotlib notebook

pca = PCA(n_components=3)
ss = StandardScaler()
X_proj = pca.fit_transform(ss.fit_transform(X))

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
for class_ in y.unique():
    mask = y == class_ 
    ax.scatter(X_proj[mask,0], X_proj[mask,1], X_proj[mask,2], label=class_)
ax.legend(loc='best')
ax.set_xlabel('PC 1')
ax.set_ylabel('PC 2')
ax.set_zlabel('PC 3')
ax.set_title('PCA on standardized dataset')
~~~

* Use [LDA (supervised)](/examples/common_chemometrics/) for classification tasks to see if there is any natural class division. Take care that you do not over interpret, since this is a supervised technique; cross-validation should always be done to validate any principal components (PCs) you consider relevant.  Looking at LDA PC coefficients can also be important. See this [stackexchange](https://stackoverflow.com/questions/45692017/lda-vectors-coefficient-interpretation) discussion and [this one](https://stats.stackexchange.com/questions/82497/can-the-scaling-values-in-a-linear-discriminant-analysis-lda-be-used-to-plot-e).
  * For example, in a [Jupyter notebook](/tutorials/jupyter_best_practices/):

~~~ python
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis as LDA

%matplotlib inline # 2D
ss = StandardScaler()
lda = LDA(n_components=2, store_covariance=True)
X_proj = lda.fit_transform(ss.fit_transform(X), y)

for class_ in sorted(y.unique()):
    mask = y == class_
    plt.plot(X_proj[mask,0], X_proj[mask,1], 'o', label=class_)
plt.legend(loc='best')
plt.xlabel('PC 1')
plt.ylabel('PC 2')
plt.title('LDA on standardized dataset')

from mpl_toolkits.mplot3d import Axes3D # 3D
%matplotlib notebook

ss = StandardScaler()
lda = LDA(n_components=3, store_covariance=True)
X_proj = lda.fit_transform(ss.fit_transform(X), y)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
for class_ in y.unique():
    mask = y == class_ 
    ax.scatter(X_proj[mask,0], X_proj[mask,1], X_proj[mask,2], label=class_)
ax.legend(loc='best')
ax.set_xlabel('PC 1')
ax.set_ylabel('PC 2')
ax.set_zlabel('PC 3')
ax.set_title('LDA on standardized dataset')
~~~

* Fit [decision trees](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html?highlight=decision%20tree#sklearn.tree.DecisionTreeClassifier) for classification problems (consider [OLS](/examples/common_chemometrics/) or [multilinear](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html?highlight=linear%20regression#sklearn.linear_model.LinearRegression) for regression tasks); there are [decision tree regressors](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html?highlight=decision%20tree#sklearn.tree.DecisionTreeRegressor) available as well.  If shallow trees work you are in luck - these are very explainable and often useful scientifically.
  * For example, in a [Jupyter notebook](/tutorials/jupyter_best_practices/):

~~~ python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=0, shuffle=True)

max_depth = np.arange(1,10)

test_acc, train_acc = [], []
for md in max_depth:
    tree = DecisionTreeClassifier(max_depth=md, class_weight=None) # Can adjust class weighting
    # No need to standardize
    tree.fit(X_train, y_train)
    
    test_acc.append(tree.score(X_test, y_test))
    train_acc.append(tree.score(X_train, y_train))
    
plt.plot(max_depth, test_acc, label='Test Accuracy')
plt.plot(max_depth, train_acc, label='Train Accuracy')
plt.legend(loc='best')
plt.ylabel('Accuracy')
plt.xlabel('Max depth')
~~~

* Fit a k-nearest neighbor [classifer](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html?highlight=k%20neighbors#sklearn.neighbors.KNeighborsClassifier) or [regressor](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html?highlight=k%20neighbors#sklearn.neighbors.KNeighborsRegressor) models.
  * It can be particularly helpful to do a [learning curve](https://github.com/mahynski/ml_inspector/blob/master/model.py#L360), in which you progressively add more and more information to the model, to see if it is converging if it is does/not level out.  If your accuracy does not seem to level out this means your model would continue to improve with more.  Your production model might not be a KNN, but if the KNN flattens out it often indicates that you have enough data to try more advanced learners.  
  * If K=1 is found during CV this may also be a sign that your data has sampled the phase space well.

~~~ python
from sklearn.neighbors import KNeighborsClassifier as KNN

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=0, shuffle=True)

ss = StandardScaler() # KNN should use standardized data usually
X_train = ss.fit_transform(X_train)
X_test = ss.transform(X_test)

nebrs = np.arange(1,10)

test_acc, train_acc = [], []
for n in nebrs:
    knn = KNN(n_neighbors=n, p=2, weights='uniform')
    knn.fit(X_train, y_train)
    
    test_acc.append(knn.score(X_test, y_test))
    train_acc.append(knn.score(X_train, y_train))
    
plt.plot(nebrs, test_acc, label='Test Accuracy')
plt.plot(nebrs, train_acc, label='Train Accuracy')
plt.legend(loc='best')
plt.ylabel('Accuracy')
plt.xlabel('Number of Neighbors')

from ml_inspector.model.InspectModel import learning_curve
learning_curve(knn, X=X_train, y=y_train, cv=5)
~~~

* Fit residuals of any simple models to a Gaussian.  If they fit, it suggests you have extracted all the information and all that is left is random noise. See [ml_inspector.model.InspectModel.plot_residuals](https://github.com/mahynski/ml_inspector/blob/master/model.py#L441)
* Plot a confusion matrix for simple models used for classification tasks; this reveals where you might struggle and where you might need to consider doing some feature engineering if different classes get confused with each other.  For example, [polynomial features](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html?highlight=polynomial%20features#sklearn.preprocessing.PolynomialFeatures) can automatically create products of features which can be helpful if there are correlations.  See [here](/tutorials/jensen_shannon/#binary-vs-ova) for example.  An [ROC curve](https://github.com/mahynski/ml_inspector/blob/master/model.py#L295) might be an alternative.
  * For example, in a [Jupyter notebook](/tutorials/jupyter_best_practices/):

~~~ python
from ml_inspector.model.InspectModel import confusion_matrix
confusion_matrix(knn, X_train, y_train)
~~~

* Look at the Jensen-Shannon divergence.  A longer discussion of this as an EDA tool is presented [here](/tutorials/jensen_shannon/).

# Some Examples of Red Flags

* For the multilabel classification task being done, there are 2 labels for each output.  The observation frequency in the dataset are plotted as a heatmap below.  Clearly, there is not much data.  In this case, it might be wise to only consider trying to model Label A: 30 or Label A: 32 data; this is because other labels are not sampled across all Label B values.  Thus, they might create a bias especially if labels and A and B are correlated, like height and weight, for example.

<img style="float: center" src="heatmap_1.png" width=600px>

* For the maximum (over all features) binary Jensen-Shannon divergences (JSD) depicted below it appears that classes 0 and 4 are not (easily) distinguishible.  Since this is a maximum JSD, you may need to consider (1) feature engineering to amplify differences (or correlations, etc.) that might be able to separate these classes, (2) considering these as the same class and merge them, or (3) collect more data or new features on those classes.

<img style="float: center" src="heatmap_2.png" width=400px>



