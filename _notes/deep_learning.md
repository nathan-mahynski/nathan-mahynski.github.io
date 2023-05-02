---
title: "Deep Learning"
excerpt: "Notes and examples on creating deep learning models."
header:
  teaser: /assets/img/jupyter_logo.png
tags:
  - python
  - deep learning
  - keras
  - tensorflow
---

<!-- Enter details at https://mybinder.org/, then copy the badge below -->

[![Made withJupyter](https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter)](https://jupyter.org/try)

Deep Learning Basics
---
These notes are based on F. Chollet's [Deep Learning with Python](https://www.amazon.com/Learning-Python-Second-Fran%C3%A7ois-Chollet/dp/1617296864) book, but have been expanded to include updates, YouTube lectures, etc.  You can launch this on different platforms, but Colab's access to GPUs will likely make this more convenient:
* [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nathan-mahynski/nathan-mahynski.github.io/public?filepath=%2F_notes%2Fdeep_learning%2Fdeep_learning_notes.ipynb)

* [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/deep_learning_notes.ipynb)

Graph Neural Nets
---
Graph neural networks are unique enough to warrant their own set of notes.  These are notes on some helpful tools when dealing with graph data.
 * [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/graph_nn_tools.ipynb)

Embeddings and Collaborative Filtering
---
Embeddings are a way to perform sensible dimensionality reduction for neural networks.  There are a number of interpretations and explanations - the notebooks below goes through several of them.  Embeddings are critical to represeting categorical variables as continuous ones in an efficient manner.  For example, they can be used to [represent a node in a graph](https://keras.io/examples/graph/node2vec_movielens) with a vector of floats. Usually tabular data is best modeled with conventional machine learning tools like tree ensembles, but if the data includes high cardinality categorical variables or raw data (e.g., text or images) neural networks are usually a better choice.  Also, embeddings are the basis of collaborative filtering (recommendation systems) which is helpful to discover latent factors underlying similarity between categories. 
* [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/embeddings.ipynb)
* [fast.ai lesson](https://course.fast.ai/Lessons/lesson7.html)

Attention and Transformers
---
 The concept of attention was introduced alongside transformer architectures in the original ["Attention is all you need" paper](https://arxiv.org/abs/1706.03762). Attention is a method that allows the model to focus on relevant parts of the input (context) in arbitrary (non-local) ways; high capacity models using attention mechanisms, trained with sufficient data, have surpassed recurrent neural network architectures as state-of-the-art (LSTM, etc. and even convolutions, too) on many tasks.

 * [fast.ai lesson](https://course.fast.ai/Lessons/lesson24.html)

 [![Attention Is All You Need](https://img.youtube.com/vi/iDulhoQ2pro/0.jpg)](https://www.youtube.com/watch?v=iDulhoQ2pro)

Attention makes a direct connection between points in the input (e.g, sequence); as a result you can [view attention essentially like a graph](https://graphdeeplearning.github.io/post/transformers-are-gnns/) where each part of the input is connected to all others and the weight of each edge determines the "strength" (how much attention to pay) of the interaction. **Thus, transformers are a special case of graph neural networks!**

[![What are Transformer Neural Networks](https://img.youtube.com/vi/XSSTuhyAmnI/0.jpg)](https://www.youtube.com/watch?v=XSSTuhyAmnI)

 * [Self-attention](https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a) is slightly different from attention, but [BERT](https://www.youtube.com/watch?v=-9evrZnBorM&list=WL&index=5) and other transformers all have a component of self-attention.
 * [Visualizing A Neural Machine Translation Model](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)
 * [Harvard's Annotated Transformer](http://nlp.seas.harvard.edu/annotated-transformer/)
 * [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) and [video](https://www.youtube.com/watch?v=-QH8fRhqFHM)

There is a nice series by Rasa Algorithm Whiteboard here:
* [Transformers & Attention 1: Self Attention](https://www.youtube.com/watch?v=yGTUuEx3GkA&list=WL&index=1)
* [Transformers & Attention 2: Keys, Values, Queries](https://www.youtube.com/watch?v=tIvKXrEDMhk&list=WL&index=3)
* [Transformers & Attention 3: Multi Head Attention](https://www.youtube.com/watch?v=23XUv0T9L5c&list=WL&index=4)
* [Transformers & Attention 4: Transformers](https://www.youtube.com/watch?v=EXNBy8G43MM&list=WL&index=2)

Learning Rate Finder
---
A good learning rate can be critical to fitting models in a reasonable amount of time; this is especially true as the networks become more complicated, and transformers are particularly susceptible to poor fitting if they have a bad rate.  [fastai](https://fastai1.fast.ai/callbacks.lr_finder.html) has a nice learning rate finder, but others have built equivalents for [Keras](https://pyimagesearch.com/2019/08/05/keras-learning-rate-finder/).

![](https://b2633864.smushcdn.com/2633864/wp-content/uploads/2019/08/keras_learning_rate_finder_header.png?lossy=1&strip=1&webp=1)

One other best practice is to use [cyclical learning rates (CLR)](https://pyimagesearch.com/2019/07/29/cyclical-learning-rates-with-keras-and-deep-learning/), oscillating between a minimum and maximum rate several times, essentially to do better annealing. There are different types of schedules - a few are discussed in the linked blog post. The LR finder algorithm essentially trains the model for 1 epoch and looks at the loss - this is repeated for many rates to produce a plot which looks like the figure above (which is smoothed to give a better plot).  
* The minimum is starting to quickly decline
* The maximum is one order of magnitude less than the minimum

If not doing CLR, then the best guess is usually between these limits around where where the loss is decreasing the fastest (largest negative slope).

Some other feedback and thoughts on the subject:
* [How reliable is the learning rate finder method?](https://blog.dataiku.com/the-learning-rate-finder-technique-how-reliable-is-it)
* [An implementation in tensorflow](https://towardsdatascience.com/the-learning-rate-finder-6618dfcb2025)
* [PyTorch-lightning](https://pytorch-lightning.readthedocs.io/en/1.4.9/advanced/lr_finder.html) has a built-in method for this.

Physically Informed Neural Nets
---
Sometimes also referred to as "physics-guided", these are NN that are designed to reflect natural laws, or other constraints, known from a certain scientific domain.  There are a number of ways to "inform" a network, but the most common ways seem to be by (1) adding terms to the loss function to reflect divergence from conservation laws, or (2) designing the topology to reflect known interactions.

A nice review of PINNs is given in ["Physics-informed machine learning" by Karniadakis et al.](https://www.nature.com/articles/s42254-021-00314-5). 

1. There is an especially nice blog post by [Maziar Raissi](https://maziarraissi.github.io/research/1_physics_informed_neural_networks/) which gives examples and some links to several PINN libraries (there are many of these emerging and being standardized).

2. Non-linear topologies in Keras are easy. Here is [Keras' documentation](https://keras.io/guides/functional_api/#manipulate-complex-graph-topologies) and a nice [example from pyimagesearch](https://pyimagesearch.com/2019/02/04/keras-multiple-inputs-and-mixed-data/). 

[![](https://img.youtube.com/vi/POsGBquHFaY/0.jpg)](https://www.youtube.com/watch?v=POsGBquHFaY&list=WL&index=14)

[![](https://img.youtube.com/vi/ZuSx0pYAZ_I/0.jpg)](https://www.youtube.com/watch?v=ZuSx0pYAZ_I&list=WL&index=1)

[![](https://img.youtube.com/vi/AU_ewEJMaLc/0.jpg)](https://www.youtube.com/watch?v=AU_ewEJMaLc&list=WL&index=10)

Invariance and Equivariance
---
PINNs sometimes rely on encoding invariance that is present in the natural world.  Invariance and Equivariance are related concepts that are very important for scientific / physics-based NNs, but are are also highly relevant in areas such as computer vision.  The videos below do a nice job introducing various aspects of this concept and where it is relevant, especially as it extends to group convolutions.

Andrew White's [book](https://dmol.pub/) also contains 2 nice chapters on the subject:
* [Ch. 9 Input Data & Equivariances](https://dmol.pub/dl/data.html)
* [Ch. 10 Equivariant Neural Networks](https://dmol.pub/dl/Equivariant.html)

[![](https://img.youtube.com/vi/a4Quhf9NhMY/0.jpg)](https://www.youtube.com/watch?v=a4Quhf9NhMY&list=WL&index=17)

[![](https://img.youtube.com/vi/2bP_KuBrXSc/0.jpg)](https://www.youtube.com/watch?v=2bP_KuBrXSc&list=WL&index=19)

[![](https://img.youtube.com/vi/z2OEyUgSH2c/0.jpg)](https://www.youtube.com/watch?v=z2OEyUgSH2c&list=WL&index=11)

[![](https://img.youtube.com/vi/0_O8PdZBc5s/0.jpg)](https://www.youtube.com/watch?v=0_O8PdZBc5s&list=WL&index=12)

General resources
---
* Google's [Machine Learning Courses](https://developers.google.com/machine-learning)
* [Tensorflow tutorials](https://www.tensorflow.org/tutorials)
* [Keras examples](https://keras.io/examples/)
* [PyTorch tutorials](https://pytorch.org/tutorials/beginner/basics/intro.html)
* [Tensor2Tensor](https://github.com/tensorflow/tensor2tensor) contains models to accelerate ML research.
