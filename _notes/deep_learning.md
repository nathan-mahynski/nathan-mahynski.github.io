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
These notes come from F. Chollet's [Deep Learning with Python](https://www.amazon.com/Learning-Python-Second-Fran%C3%A7ois-Chollet/dp/1617296864) book.  You can launch this on different platforms, but Colab's access to GPUs will likely make this more convenient:
* [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nathan-mahynski/nathan-mahynski.github.io/public?filepath=%2F_notes%2Fdeep_learning%2Fdeep_learning_notes.ipynb)

* [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/deep_learning_notes.ipynb)

Graph Neural Nets
---
Graph neural networks are unique enough to warrant their own set of notes.  These are notes on some helpful tools when dealing with graph data.
 * [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/graph_nn_tools.ipynb)

Embeddings and Collaborative Filtering
---
Embeddings are a way to perform sensible dimensionality reduction for neural networks.  There are a number of interpretations and explanations - the notebooks below goes through several of them.  Embeddings are critical to represeting categorical variables as continuous ones in an efficient manner.  Usually tabular data is best modeled with conventional machine learning tools like tree ensembles, but if the data includes high cardinality categorical variables or raw data (e.g., text or images) neural networks are usually a better choice.  Also, embeddings are the basis of collaborative filtering (recommendation systems) which is helpful to discover latent factors underlying similarity between categories. 
* [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nathan-mahynski/nathan-mahynski.github.io/blob/public/_notes/deep_learning/embeddings.ipynb)

Other Key Concepts
---
1. Attention 
 * Attention is a method that allows the model to focus on relevant parts of the input (context) in arbitrary (non-local) ways; high capacity models using attention mechanisms, trained with sufficient data, have surpassed recurrent neural network architectures as state-of-the-art (LSTM, etc. and even convolutions, too) on many tasks.
 * [Self-attention]((https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a)) is slightly different from attention, but [BERT](https://en.wikipedia.org/wiki/BERT_(language_model)#:~:text=Bidirectional%20Encoder%20Representations%20from%20Transformers,2018%20by%20researchers%20at%20Google.) and other transformers all use self-attention.
 * [Visualizing A Neural Machine Translation Model](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)

2. Transformers 
 * The concept of attention was introduced alongside transformer architectures in the original [``Attention is all you need'' paper](https://arxiv.org/abs/1706.03762).
 * [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) 
[![The Narrated Transformer](https://img.youtube.com/vi/-QH8fRhqFHM/0.jpg)](https://www.youtube.com/watch?v=-QH8fRhqFHM)
 * [Harvard's Annotated Transformer](http://nlp.seas.harvard.edu/annotated-transformer/)

General resources
---
* Google's [Machine Learning Courses](https://developers.google.com/machine-learning)
* [Tensorflow tutorials](https://www.tensorflow.org/tutorials)
* [Keras examples](https://keras.io/examples/)
* [PyTorch tutorials](https://pytorch.org/tutorials/beginner/basics/intro.html)
* [Tensor2Tensor](https://github.com/tensorflow/tensor2tensor) contains models to accelerate ML research.
