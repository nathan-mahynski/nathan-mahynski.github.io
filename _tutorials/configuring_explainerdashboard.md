---
title: "Configuring ExplainerDashboard for SHAP"
excerpt: "Making Explainable AI simple and accessible."
header:
  teaser: /assets/img/analytics_dashboard.png
  image: /assets/img/configuring_explainerdashboard_header.png
tags:
  - SHAP
  - explainable AI
  - dashboard
---

{% include toc icon="gears" title="Table of Contents" %}

Modern computational software and hardware have made it relatively easy to process data and train machine learning models; however, implementing these models requires trust of the end user, which means explainable AI methods, such as [SHAP](https://github.com/slundberg/shap), need to be leveraged.  In the realm of scientific research another common problem is that data science/analysis is often performed by one (or a team of) individual(s), but the audience or collaborators are those with more detailed scientific knowledge of a problem and less expertise on the data science end.  In order to collaborate and/or present the results to a more general audience, an interactive visualization tool is needed.   This enables scientists and engineers to absorb a model, perform 'what-if" analyses to test its limits, and extract scientific knowledge.  To that end, several dashboard tools have been developed.  This tutorial focuses on setting up [ExplainerDashboard](https://github.com/oegedijk/explainerdashboard) which I have found is particularly easy to configure and share with others.  This essentially lets you create a web app for your model and explanations in a few minutes (baseline).

As the authors state: "The library is designed to be modular so that it should be easy to design your own interactive dashboards with plotly dash, with most of the work of calculating and formatting data, and rendering plots and tables handled by explainerdashboard, so that you can focus on the layout and project specific textual explanations. (i.e. design it so that it will be interpretable for business users in your organization, not just data scientists). Alternatively, there is a built-in standard dashboard with pre-built tabs (that you can switch off individually)."

Full documentation can be found [here](https://explainerdashboard.readthedocs.io/en/latest/index.html) and should be regarded as primary source material.  The step-by-step instructions and examples below are taken fromn personal experience to illustrate quickly how to set up, share, and customize the interface.  Of course, this is biased toward what I have historically found the most helpful in my own work.

# Installation
---
First install the dashboard with conda (pip install is also available, see repo for details):

~~~ bash
$ conda create -n automl python=3.7
$ conda activate automl
$ conda install -c conda-forge explainerdashboard
~~~

# Baseline Example
---
This example, taken directly from the github website, illustrates a simple dashboard construction using python.  This is based on information from the Titanic, and is illustrative of most of the main features. Notably, here the model is trained and analyzed in a single script; this is generally not the case for production ML models which are usually developed at great computational expense beforehand.  Using pre-built models will be discussed in a subsequent section.  Parameter values below are documented [here](https://explainerdashboard.readthedocs.io/en/latest/explainers.html?highlight=shap#parameters).

~~~ python
from sklearn.ensemble import RandomForestClassifier
from explainerdashboard import ClassifierExplainer, ExplainerDashboard
from explainerdashboard.datasets import titanic_survive, titanic_names

# This returns a pandas DataFrame of information.
X_train, y_train, X_test, y_test = titanic_survive()
train_names, test_names = titanic_names()

# While not required, this should include a description for the columns in the DataFrames just loaded (see X_train.head()).
feature_descriptions = {
    "Sex": "Gender of passenger",
    "Gender": "Gender of passenger",
    "Deck": "The deck the passenger had their cabin on",
    "PassengerClass": "The class of the ticket: 1st, 2nd or 3rd class",
    "Fare": "The amount of money people paid", 
    "Embarked": "the port where the passenger boarded the Titanic. Either Southampton, Cherbourg or Queenstown",
    "Age": "Age of the passenger",
    "No_of_siblings_plus_spouses_on_board": "The sum of the number of siblings plus the number of spouses on board",
    "No_of_parents_plus_children_on_board" : "The sum of the number of parents plus the number of children on board",
}

# Fit a RF classifier to predict survival (0 or 1).
model = RandomForestClassifier(n_estimators=50, max_depth=5)
model.fit(X_train, y_train)

# See help(ClassifierExplainer) for full documentation on the inputs.
explainer = ClassifierExplainer(model, X_test, y_test, 
                                cats=['Deck', 'Embarked', {'Gender': ['Sex_male', 'Sex_female', 'Sex_nan']}],
                                descriptions=feature_descriptions, # defaults to None
                                labels=['Not survived', 'Survived'], # defaults to ['0', '1', etc]
                                idxs = test_names, # defaults to X.index
                                index_name = "Passenger", # defaults to X.index.name
                                target = "Survival", # defaults to y.name
                                )

# Launch a dashboard to visualize the explainer.
db = ExplainerDashboard(explainer, 
                        title="Titanic Explainer", # defaults to "Model Explainer"
                        whatif=False, # you can switch off tabs with bools
                        )

# Best to save the explainer.
explainer.dump("explainer.joblib")

# The dashboard can also be saved directly to a yaml file.
db.to_yaml("dashboard.yaml")

# Runs the dashboard as a website that is accessible at localhost:8050 (make sure the chosen port is open and unblocked).
db.run(port=8050)
~~~

This script can be executed from the command line:
~~~ bash
$ conda activate automl
$ python dashboard.py # Assuming the above was saved as dashboard.py
~~~

Things like [SHAP](https://github.com/slundberg/shap) require certain inputs to ensure the calculation proceeds as desired.  For ExplainerDashboard this is documented [here](https://explainerdashboard.readthedocs.io/en/latest/explainers.html?highlight=shap#parameters) and should be cross-referenced with the [underlying library](https://github.com/slundberg/shap).  For example, you can specify if the SHAP algorithm (kernel, tree, etc.), what dataset should be used (test vs. train), and what the model output is (log-odds vs. probability).

~~~ python
ClassifierExplainer(model, X_test, y_test,
        shap='linear', # default 'guess', can be 'tree', 'linear', 'deep', 'kernel'
        X_background=X_train, # set background dataset for SHAP calculations
        model_output='logodds', # set model_output to logodds (vs probability)
        cats=['Sex', 'Deck', 'Embarked'], # makes it easy to group onehotencoded vars
        idxs=test_names, # index with str identifier
        index_name="Passenger", # description of index
        descriptions=feature_descriptions, # show long feature descriptions in hovers
        target='Survival', # the name of the target variable (y)
        labels=['Not survived', 'Survived']) # show target labels instead of ['0', '1']
~~~

# Detailed Customization
---
Specific tabs and panels within each may be activated or deactivated using booleans when instantiated.  Elaborate examples can be found [here](https://explainerdashboard.readthedocs.io/en/latest/custom.html).
Here is a brief overview of the layout.

## Tabs

There are seven tabs that make up the default ["ExplainerDashboard"](https://explainerdashboard.readthedocs.io/en/latest/tabs.html)
~~~ python
ExplainerDashboard(explainer,
                    importances=False, 		# ImportancesComposite
                    model_summary=True,		# ModelSummaryComposite
                    contributions=True,		# IndividualPredictionsComposite
                    whatif=True,		# WhatIfComposite
                    shap_dependence=True,	# ShapDependenceComposite
                    shap_interaction=False,	# ShapInteractionsComposite
                    decision_trees=True)	# DecisionTreesComposite
~~~
The dashboard itself is composed of ExplainerComponents which are self-contained elements (a plot, table, dropdowns, etc.). These components are designed to interact with each other via connectors, so that when you select an index in one component, it that automatically updates the index in another component.

These component also make it easy to create custom dashboards.  From the documentation:

~~~ python
from explainerdashboard.custom import *

class CustomDashboard(ExplainerComponent):
   def __init__(self, explainer, title="Custom Dashboard"):
      super().__init__(explainer, title)
      self.shap_dependence = ShapDependenceComponent(explainer,
                         hide_title=True, hide_cats=True, hide_highlight=True,
                         cats=True, col='Fare')

   def layout(self):
      return html.Div([
         shap_dependence.layout()
      ])

ExplainerDashboard(explainer, CustomDashboard).run()
~~~

Individual kwargs for tabs are uniquely named and may be (de-)activated globally at instantiation.

~~~ python
ExplainerDashboard(explainer, 
        # importances tab:
        hide_importances=True,
        # classification stats tab:
        hide_globalcutoff=True, hide_modelsummary=True, 
        hide_confusionmatrix=True, hide_precision=True, 
        hide_classification=True, hide_rocauc=True, 
        hide_prauc=True, hide_liftcurve=True, hide_cumprecision=True,
        # regression stats tab:
        # hide_modelsummary=True, 
        hide_predsvsactual=True, hide_residuals=True, 
        hide_regvscol=True,
        # individual predictions tab:
        hide_predindexselector=True, hide_predictionsummary=True,
        hide_contributiongraph=True, hide_pdp=True, 
        hide_contributiontable=True,
        # whatif tab:
        hide_whatifindexselector=True, hide_whatifprediction=True,
        hide_inputeditor=True, hide_whatifcontributiongraph=True, 
        hide_whatifcontributiontable=True, hide_whatifpdp=True,
        # shap dependence tab:
        hide_shapsummary=True, hide_shapdependence=True,
        # shap interactions tab:
        hide_interactionsummary=True, hide_interactiondependence=True,
        # decisiontrees tab:
        hide_treeindexselector=True, hide_treesgraph=True, 
        hide_treepathtable=True, hide_treepathgraph=True,
        ).run()
~~~

It is also possible to indiscriminantly deactivate certain toggles and dropdowns by passing them as **kwargs.  However, this is applied globally so this is how it will be displayed everywhere.

~~~ python
ExplainerDashboard(explainer, 
                    no_permutations=True, # do not show or calculate permutation importances
                    hide_cats=True, # hide the group cats toggles
                    hide_depth=True, # hide the depth (no of features) dropdown
                    hide_sort=True, # hide sort type dropdown in contributions graph/table
                    hide_orientation=True, # hide orientation dropdown in contributions graph/table
                    hide_type=True, # hide shap/permutation toggle on ImportancesComponent 
                    hide_dropna=True, # hide dropna toggle on pdp component
                    hide_sample=True, # hide sample size input on pdp component
                    hide_gridlines=True, # hide gridlines on pdp component
                    hide_gridpoints=True, # hide gridpoints input on pdp component
                    hide_cutoff=True, # hide cutoff selector on classification components
                    hide_percentage=True, # hide percentage toggle on classificaiton components
                    hide_log_x=True, # hide x-axis logs toggle on regression plots
                    hide_log_y=True, # hide y-axis logs toggle on regression plots
                    hide_ratio=True, # hide the residuals type dropdown
                    hide_points=True, # hide the show violin scatter markers toggle
                    hide_winsor=True, # hide the winsorize input
                    hide_range=True, # hide the range subscript on feature input
                    hide_star_explanation=True, # hide the '* indicates observed label` text
)
~~~

# Using Pre-computed Models
---
The initial script example above computed the properties for the first time when this script was called.  However, conventionally this is done separately as an offline calculation and we only want to display the results for interaction later.  There are 2 options:

1. Wrap the explainer in a dashboard (which will calculate all properties needed for that particular dashboard) or 
2. simply calculate all properties with explainer.calculate_properties().

~~~ python
model = pickle.load(open('my_previously_fit_model.pkl', 'rb'))
explainer = ClassifierExplainer(model, X, y)

choice = 1 # or 2
if choice == 1:
   # Method 1:
   db = ExplainerDashboard(explainer)
   db.to_yaml("dashboard.yaml", explainerfile="explainer.joblib")
else:
   # Method 2:
   explainer.calculate_properties()

explainer.dump("explainer.joblib")
~~~

In method 1 above, we also exported the dashboard configuration to a yaml file.  While not necessary, when externally hosting this allows you to preserve any customizations you made to the ExplainerDashboard. The yaml file just points at the explainerfile so make sure the explainer.dump() uses the same filename (as shown).

# Deployment
---

## For personal use

During the initial phase of development and exploration, running the server locally is generally enough.  It even works in the cloud on colab, etc.  According to the documentation:

"You can start the dashboard with the standard dash.Dash() server or with the new notebook friendly JupyterDash server. The latter will allow you to keep working interactively in your notebook while the dashboard is running. Also, this allows you to run an explainerdashboard from within Google Colab!

The default dash server is started with mode='dash'. (except in Google Colab, where the default is mode='external') There are three notebook compatible options: mode='inline' for running the dashboard in an output cell in your notebook, mode='jupyterlab' for runnning the dashboard in jupyterlab pane, or mode='external' which runs the dashboard in a seperate tab:"

~~~ python
ExplainerDashboard(explainer).run() # default is either 'dash' or 'external' in colab
ExplainerDashboard(explainer, mode='dash').run()
ExplainerDashboard(explainer, mode='inline').run(port=8051)
ExplainerDashboard(explainer, mode='jupyterlab').run(8052)
ExplainerDashboard(explainer, mode='external').run()
~~~

## In production

In this case it is better to use a server like gunicorn (can also be done via pip).

~~~ bash
$ conda activate automl
$ conda install -c anaconda gunicorn
~~~

Clearly, a public-facing dashboard should come with all properties pre-calculated.  See the previous section for notes on storing these results.  Assuming you have stored your explainer and dashboard configuration in explainer.joblib and dashboard.yaml, respectively, then we just need to create a new file to load this and expose the flask server.  Create a file called my_dash.py:

~~~ python
from explainerdashboard import ClassifierExplainer, ExplainerDashboard

use_custom = False
if use_custom:
   db = ExplainerDashboard.from_config("dashboard.yaml") # You can also override yaml settings and pass new **kwargs
else:
   explainer = ClassifierExplainer.from_file("explainer.joblib")
   db = ExplainerDashboard(explainer)

# Need to define app so that gunicorn can find the flask server
app = db.flask_server()
~~~

The server (3 workers, preload before starting dashboard, bound to port 8050) is then created as follows:

~~~ bash
$ gunicorn -w 3 --preload -b localhost:8050 my_dash:app
~~~

The page is now at localhost:8050 which can be forwarded from another url, if desired.

It is also useful to have the server restart when underlying changes occur, for example, if a new model is provided.  These details are documented [here](https://explainerdashboard.readthedocs.io/en/latest/deployment.html#automatically-restart-gunicorn-server-upon-changes).

# ExplainerHub
---
This feature is currently in beta, but provides for the option of hosting a landing page which links to different ExplainerDashboards.  This also allows different users with different logins!  See the preliminary documentation for [now](https://explainerdashboard.readthedocs.io/en/latest/hub.html), and how to [set logins and passwords](https://explainerdashboard.readthedocs.io/en/latest/deployment.html#setting-logins-and-password).

# Other Notes
---
 * You can group onehot encoded categorical variables together using the cats parameter. 
 * You can either pass a dict specifying a list of onehot cols per categorical feature, or if you encode using e.g. pd.get_dummies(df.Name, prefix=['Name']) (resulting in column names 'Name_Adam', 'Name_Bob') you can simply pass the prefix 'Name' (cf. example script at the beginning).


