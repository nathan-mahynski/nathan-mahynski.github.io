---
title: "Setting up Colab"
excerpt: "A < 5 minute quickstart quide to Google Colab (Jupyter on-the-go)."
header:
  teaser: /assets/img/colab_logo.png
  image: /assets/img/colab_header.png
tags:
  - python
  - colab
  - jupyter
  - machine learning
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

[Google colab](https://colab.research.google.com/notebooks/intro.ipynb){:target="_blank"} is essentially a web-based [Jupyter notebook](/tutorials/jupyter_best_practices/){:target="_blank"}.  It links to GitHub and/or a Google Drive account allowing you to save your work in the cloud and run on google's servers.  It also gives you free access to GPU and TPU resources.  Google provides a number of [introductions](https://colab.research.google.com/notebooks/basic_features_overview.ipynb){:target="_blank"} to Colab, but if you are already familiar with python and Jupyter (and markdown) you are ready to go.

This is a < 5 minute quick start to get going so you can use and write (python) code, do data science, machine learning, etc. on-the-go or from whatever remote location you please.

Colab also lets you work together (as the name suggests) with other, the same as any other google document, for example.

Here is link to [download](template.ipynb) an example Jupyter notebook to build from, which contains some of the code samples below.

# Quick Start

1. In your browser, navigate to [https://colab.research.google.com/](https://colab.research.google.com/){:target="_blank"}.
2. File > New Notebook. 
3. Sign in to your Google account, if prompted.
4. Name your notebook (you can create a directory and move files around on Google Drive as if it were a normal file later, if you wish).
5. Choose hardware acceleration under Edit > Notebook Settings > Hardware Accelerator.  Both [GPUs](https://en.wikipedia.org/wiki/Graphics_processing_unit){:target="_blank"} and [TPUs](https://en.wikipedia.org/wiki/Tensor_Processing_Unit){:target="_blank"} are currently supported.
6. All [Jupyter magics](https://ipython.readthedocs.io/en/stable/interactive/magics.html){:target="_blank"} are available; also, because you can perform shell commands be prepending the `!` symbol to a command, you to use `!wget`, for example, to import data from a website.
7. Install any repos you need.  By default things like numpy, scipy, matplotlib, pandas, [fastai](https://www.fast.ai/){:target="_blank"}, and torch are available.  If you want to import other things, like cartopy for example, you may get an error like this.
<img style="float: center" src="cartopy_error.png" width=600px>
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
10. Execute python scripts from your drive by `!python3 my_script.py`.

# Saving

There are 2 basic mechanisms for saving your work.

1. To save a copy in your Google drive, simply save as usual "ctrl + s" or "File > Save a copy in Drive".
2. "File > Save a copy in GitHub", then authorize GitHub access when prompted.  

While Drive does save file history, for better revision control GitHub is preferred.  But for most sandbox-level development and coding, Google Drive is an easier solution.  Note that 
[binder](https://mybinder.org/){:target="_blank"} is another popular option; however, Binder just lauches instances of notebooks saved to GitHub and any changes are lost when the session ends.  Using Colab you can edit and create more easily.  If you want to share you work without having to worry about collaborators editing or breaking something, binder might be preferable.  Of course, you can always save your work to a GitHub repo then generate the binder link to it for sharing if you like binder; however, when hosted on GitHub, users are served copies of the original.

You can also download the notebook locally as an .ipynb  or .py file from the File menu.

# Recommendations

I like to have a template notebook with the above snippets in my google drive.  Here is an example you can [download](template.ipynb). This lets you quickly template new notebooks as you need them.  Of course, there is no shortage of other blogs and tutorials online on how to use Colab's features.  Here are a few:

* [Getting Started from Google](https://www.youtube.com/watch?v=inN8seMm7UI){:target="_blank"}
* [TutorialsPoint](https://www.tutorialspoint.com/google_colab/what_is_google_colab.htm){:target="_blank"}
* [Anne Bonner](https://towardsdatascience.com/getting-started-with-google-colab-f2fff97f594c){:target="_blank"}
