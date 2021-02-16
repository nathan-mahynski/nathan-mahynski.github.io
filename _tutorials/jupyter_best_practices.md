---
title: "Best Practices for Jupyter Notebooks"
excerpt: "Opinionated advice for the getting the most out of notebooks."
header:
  teaser: /assets/img/jupyter_logo.png
  image: /assets/img/jupyter_best_practices_header.png
tags:
  - jupyter
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

This is an opinionated guide to using [Jupyter notebooks](https://jupyter.org/), and it is certainly not the first.  Google even has a [manifesto](https://cloud.google.com/blog/products/ai-machine-learning/best-practices-that-can-improve-the-life-of-any-developer-using-jupyter-notebooks) on the subject. Depending on the application, clearly tastes will differ.  I generally do python prototyping to develop scientific code and workflows using Jupyter.  This means that functions and classes are usually initially spun up and then, when basically functional, converted to scripts or built into existing libraries for proper version control, unittesting, and issue tracking.  There is [ipytest](https://pypi.org/project/ipytest/) for testing inside the notebook, but I recommend [outsourcing to a repo using pre-commit](/tutorials/git_python_workflow/), for example, so this can be tracked with git. So the notebook is primarily for (1) initial testing and sandbox-level development, or (2) for scripting these together to analyze data.  Ther latter is where best practices are essential.

# Installation

1. Use [Anaconda](https://www.anaconda.com/) which already includes Jupyter, or
2. `$ pip3 install jupyter`

# Headless Operation
---

I like to login to my machines remote, spin up a jupyter server inside [screen](https://ss64.com/bash/screen.html), then go back to my browser to access these remotely.  To do this

You can do this simply from the command line:

~~~bash
$ jupyter notebook --no-browser --port=XXXX
~~~

Alternatively, you can configure this automatically in the configuration file.  See Jupyter's [documentation](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html) on this for more details.  In brief:

~~~bash
$ jupyter notebook --generate-config # generates vi ~/.jupyter/jupyter_notebook_config.py
~~~

Then change the following settings in `~/.jupyter/jupyter_notebook_config.py` before running `$ jupyter notebook` from the command line:

1. c.NotebookApp.ip = '0.0.0.0'
2. c.NotebookApp.open_browser = False
3. c.NotebookApp.port = 9999 # optional, change to something unique (besides default of 8888)
4. c.NotebookApp.notebook_dir = '/home/username/' # set where the notebook's head is (change username to your's)

# Extensions
---

Jupyter notebook extensions bring utilities and functional widgets to the table.

~~~bash
$ pip install jupyter_contrib_nbextensions # OR
$ conda install -c conda-forge jupyter_contrib_nbextensions
~~~

It is easiest to enable or disable extensions using the [configurator](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator).  After enabling, be sure to restart the notebook server.
~~~bash
$ pip install jupyter_nbextensions_configurator # OR
$ conda install -c conda-forge jupyter_nbextensions_configurator
$ jupyter nbextensions_configurator enable --user
~~~

From the server (localhost:8888 usually) click on the "Nbextensions" tab and enable the ones you like.  Play around to see what features work best for you.  Below is an example (especially important is the toc2, table of contents) - also consider enabling Autopep8, VariableInspector, and datestamper.
<img style="float: right" src="jupyter_nbextensions.png" width=600px>

[snippets](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/snippets/README.html) is a great tool if you regularly use a certain piece of code or header, etc. that you want to add easily to your notebook.  This is especially helpful for teaching and developing.

# The Header
---

* Use [watermark](https://github.com/rasbt/watermark) - this is a beautiful and handy piece of code that let's you display the version of the different libraries you have imported.  Especially for machine learning applications where substantial changes to default behavior and interfaces can happen quickly and behind the scenes, this is a very nice way to ensure you know what you are running under the hood.

~~~bash
$ pip install watermark
~~~

From your notebook, include the following cell to display the time, machine info, hostname, python and ipython versions, and name and version of all imported packages:
~~~python
%load_ext watermark
%watermark -t -m -h -v --iversions
~~~

* Enable autoreload - this means that when you edit or update an imported library (especially important when testing and debugging one) you don't have to restart the kernel to reload it; it will be done on-the-fly as needed.  Enter the following into a cell at the top of your notebook:

~~~python
%load_ext autoreload
%autoreload 2
~~~

# Help Functions
---

1. `help(foobar)` - load docstring about "foobar".
2. `?foobar` - display basic information about foobar including its docstring.
3. `??foobar` - display the source code of foobar.

# Useful Magic
---

* Profiling tools - memory and line profiling can be done with built in magic. Install using pip, then load the extensions. 

~~~bash
$ pip install line-profiler
$ pip install memory_profiler
~~~

~~~python
%load_ext line_profiler
%load_ext memory_profiler
~~~

See the documentation at `?lprun` or `?mprun` for how to use `%lprun` or `%mprun`.

* `%timeit` - runs the code line multiple times and returns an average and other statistics about its operation.

* Bash - bash commands can be run as `!<command>` or `%<command>` as if you issued `$ <command>` from the command-line.

* `%%pdb` - debugging interface loads when exception occurs.

# Additional Resources
---

Jeremy Howard has been leading development of [nbdev](https://github.com/fastai/nbdev) which is a self-contained development environment in the notebook.  It has met with some controversy surrounding whether or not this is a good idea, but it is a neat piece of code regardless.  

The [tqdm](https://github.com/tqdm/tqdm#ipython-jupyter-integration) progress bar is very helpful for on-the-fly profiling and understanding how long loops with take to complete.  This is particularly helpful for spinning up simepl examples to estimate how long tasks will take in production.

~~~bash
$ pip install tqdm # OR
$ conda install -c conda-forge tqdm
~~~


