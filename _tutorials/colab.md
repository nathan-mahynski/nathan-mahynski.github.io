---
title: "Colab vs. Conda"
excerpt: "A quick-start quide to getting set up in the cloud and running locally."
header:
  teaser: /assets/img/colab_logo.png
  image: /assets/img/colab_header.png
tags:
  - python
  - colab
  - jupyter
  - machine learning
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

[Google colab](https://colab.research.google.com/notebooks/intro.ipynb){:target="_blank"} is essentially a web-based [Jupyter notebook](/tutorials/jupyter_best_practices/){:target="_blank"}.  It links to GitHub and/or a Google Drive account allowing you to save your work in the cloud and run on google's servers.  It also gives you free access to GPU and TPU resources.  Google provides a number of [introductions](https://colab.research.google.com/notebooks/basic_features_overview.ipynb){:target="_blank"} to Colab, but if you are already familiar with python and Jupyter (and markdown) you are ready to go.

One purpose of this guide is to be a < 5 minute quick-start to get going so you can use and write (python) code, do data science, machine learning, etc. on-the-go or from whatever remote location you please.

Colab also lets you work together (as the name suggests) with other, the same as any other google document, for example.

Here is link to [download](template.ipynb) an example Jupyter notebook to build from, which contains some of the code samples below.

In contrast, if you don't work remotely (from your resources) you may prefer to configure things locally.  Working locally alo has many other benefits, and is still the norm for most developers.  I will also review a basic setup on [anaconda](https://www.anaconda.com/products/individual) which is a very useful package that allows you to create and manage different virtual environments (technically Colab also has anaconda available!).

# Colab 

## Quick-start

1. In your browser, navigate to [https://colab.research.google.com/](https://colab.research.google.com/){:target="_blank"}.
2. File > New Notebook. 
3. Sign in to your Google account, if prompted.
4. Name your notebook (you can create a directory and move files around on Google Drive as if it were a normal file later, if you wish).
5. Choose hardware acceleration under Edit > Notebook Settings > Hardware Accelerator.  Both [GPUs](https://en.wikipedia.org/wiki/Graphics_processing_unit){:target="_blank"} and [TPUs](https://en.wikipedia.org/wiki/Tensor_Processing_Unit){:target="_blank"} are currently supported.
6. All [Jupyter magics](https://ipython.readthedocs.io/en/stable/interactive/magics.html){:target="_blank"} are available; also, because you can perform shell commands be prepending the `!` symbol to a command, you to use `!wget`, for example, to import data from a website.
7. Install any repos you need.  By default things like numpy, scipy, matplotlib, pandas, [fastai](https://www.fast.ai/){:target="_blank"}, and torch are available.  If you want to import other things, like cartopy for example, you may get an error like this.
<img style="float: right" src="cartopy_error.png" width=600px>
* Conveniently, links to stackoverflow are already provided.  Here we have a link to "install cartopy" since Colab recognizes this; if not, you may get an "open examples" link instead. Clicking the link opens a "snippets" panel at the left which shows how to proceed, and other suggestions.
* Here, the solution is to have a cell that looks like this:
~~~ python
!apt-get -qq install python-cartopy python3-cartopy
import cartopy
~~~
* In general, you can use `!pip install <package-name>` to install packages, or you can install a repo via `!git clone <your-repo>`; you can also use `!apt-get install <package-name>` is Colab is running on Debian linux and uses aptitude as its package manager. Importantly, if you are cloning a repo you may need to install dependencies via requirements.tx file.  This can be done "as usual":
~~~ python
!git clone my_repository
!cd my_repository/; pip install -r requirements.txt
import my_repository
~~~
8. As discussed already, the "snippets" menu (the "<>" symbol on the left) is another great way to quickly add code.  You can search keywords right there to see what others have done, and use as needed.
9. Mounting your google drive (see snippets for up to date API)
* First execute a cell that looks like below.  This will give you a web link to visit where you will sign in and authorize access to you account, and in return you will get an authorization token to paste into the cell that results from this command.
~~~ python
from google.colab import drive
drive.mount('/gdrive')
~~~
* This mounts your google drive at `/gdrive/Mydrive/` so you can access or create files like this:
~~~ python
with open('/gdrive/My Drive/foo.txt', 'w') as f:
  f.write('Hello Google Drive!')
!cat '/gdrive/My Drive/foo.txt'
~~~
* You can also manually upload files by clicking on the "folder" icon on the left
10. Execute python scripts from your drive by `!python3 my_script.py`.

## Saving

There are 2 basic mechanisms for saving your work.

1. To save a copy in your Google drive, simply save as usual "ctrl + s" or "File > Save a copy in Drive".
2. "File > Save a copy in GitHub", then authorize GitHub access when prompted.  

While Drive does save file history, for better revision control GitHub is preferred.  But for most sandbox-level development and coding, Google Drive is an easier solution.  Note that 
[binder](https://mybinder.org/){:target="_blank"} is another popular option; however, Binder just lauches instances of notebooks saved to GitHub and any changes are lost when the session ends.  Using Colab you can edit and create more easily.  If you want to share you work without having to worry about collaborators editing or breaking something, binder might be preferable.  Of course, you can always save your work to a GitHub repo then generate the binder link to it for sharing if you like binder; however, when hosted on GitHub, users are served copies of the original.

You can also download the notebook locally as an .ipynb or .py file from the File menu.

## Recommendations

I like to have a template notebook with the above snippets in my google drive.  Here is an example you can [download](template.ipynb). This lets you quickly template new notebooks as you need them.  Of course, there is no shortage of other blogs and tutorials online on how to use Colab's features.  Here are a few:

* [Getting Started from Google](https://www.youtube.com/watch?v=inN8seMm7UI){:target="_blank"}
* [TutorialsPoint](https://www.tutorialspoint.com/google_colab/what_is_google_colab.htm){:target="_blank"}
* [Anne Bonner](https://towardsdatascience.com/getting-started-with-google-colab-f2fff97f594c){:target="_blank"}

# Anaconda

## Installation

The [anaconda individual edition](https://www.anaconda.com/products/individual) is free and can be downloaded [here](https://www.anaconda.com/products/individual).  This package has become extremely popular and, as such, the core package has come to include a lot of "standard" features and packages that not every developer needs; consequently, you may wish to opt for the stripped down version [miniconda](https://docs.conda.io/en/latest/miniconda.html) instead.  I recommend the latter as you can always install what you need, when you need it very easily.

~~~ bash
$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
$ bash Miniconda3-latest-Linux-x86_64.sh
~~~

> ["When installing Anaconda, you have the option to 'Add Anaconda to my PATH environment variable.' This is not recommended because the add to PATH option appends Anaconda to PATH. When the installer appends to PATH, it does not call the activation scripts."](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

Conda creates stand-alone environments and needs to solve the interdependencies of packages installed when adding or removing them.  For environments with many packages installed this creates a complex boolean satisfiability problem which [mamba](https://mamba.readthedocs.io/en/latest/index.html) has been developed to solve more quickly.  In short, you can [install mamba](https://anaconda.org/conda-forge/mamba) in a conda environment and then just replace all the `conda install` terms below with `mamba install` for a nice speed boost!

## Environments

Conda lets you create different environments (like different versions python or packages) so you can easily switch between different ones.  This lets you easily try new things our, maintain reverse compatibility, and stay-up-to-date without breaking your stride.  After installing, here is the basic way to create an environment:

~~~ bash
$ conda create -n my_env_name python=3.7
~~~

You can create environments with specific versions of packages by appropriately appending the above comman, as detailed [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)  It can be helpful to specify all the packages you want here since installing packages one at a time can sometimes lead to dependency conflicts, though I have rarely encountered this problem.

It can be very helpful to "silo" individual projects into individual environments.  You can maintain a [separate yaml file](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/04-sharing-environments/index.html) for each project, e.g., conda-env.yml, to save which codes (and versions) were used for each.  You can create this environment automatically:

~~~bash
$ conda env create -f conda-env.yml
~~~

Once created, you can activate and deactivate your environment as follows.  

~~~ bash
$ conda activate my_env_name
$ (my_env_name) conda install sklearn
$ (my_env_name) conda deactivate
$ 
~~~

The name of your environment is listed at the start of your prompy by default to let you know what environment is active. You can look at your environments:

~~~ bash
$ conda info --envs # OR
$ conda env list
~~~

Only packages you specifically installed therein are used while it is active so you can install different versions in different environments to test things out without conflict.  You can see what pacakges are installed in an environment:

~~~ bash
$ (my_env_name) conda list # If environment is active
$ conda list -n my_env_name # If environment is NOT active
~~~

It is possible to use pip and conda together, but it is recommended that you install as much as possible with conda, then install things with pip.  If additional modifications are needed later, it is recommended you just create a new environment. See the [user guide](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#using-pip-in-an-environment) for more details.

You can search for packages in the anaconda channel [here](https://anaconda.org/anaconda), and a common one to use is conda-forge.  You can search for your package of interest which brings you to a page with commands you can use to install it; for example, to install cartopy from conda-forge in your environment:

~~~ bash
$ conda activate my_env_name
$ (my_env_name) conda install -c conda-forge cartopy
~~~

## Integrating with Jupyter

After you create your environment, you may wish to use it as your kernel for you Jupyter notebook.  With Colab you have to install things every time you start a new session; if your environment contains a lot of packages that are not standard in Colab this can be a bit cumbersome.  The documentation for adding the environment as a kernel can be found [here](https://ipython.readthedocs.io/en/stable/install/kernel_install.html).  The basic steps are: (1) create your kernel as shown above, (2) install the ipykernel in that environment, (3) install the kernel.

~~~ bash
$ conda create -n my_env_name python=3.7
$ conda activate my_env_name
$ (my_env_name) conda install ... # All packages you care about
$ (my_env_name) conda install ipykernel
$ (my_env_name) python -m ipykernel install --user --name my_env_name --display-name "Python (my_env_name)"
~~~

This example installs the `my_env_name` conda environment as a kernel which will show up under Kernel > Change Kernel in your Jupyter notebook as "Python (my_env_name)".  Change the parameters above as you see fit.

Note that as you update packages in environment they become accessible in the notebook as well.  As a [best practice](/tutorials/jupyter_best_practices/), I recommend using [watermark](https://pypi.org/project/watermark/) or another tool to keep track of what version of different packages you are running.



