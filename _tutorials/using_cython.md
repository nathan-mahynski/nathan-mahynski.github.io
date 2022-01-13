---
title: "Using Cython to Accelerate Python Code"
excerpt: "Fast scientific python code."
header:
  teaser: /assets/img/cython_logo.png
  image: /assets/img/cython_header.png
tags:
  - python
  - cython
  - high performance programming
classes:
  - wide
---

{% include toc icon="gears" title="Table of Contents" %}

# tl;dr

An initial disclaimer: "Premature optimization is the root of all evil!" Never optimize your code before your profile it. If you determine your code is too slow, and learn why after profiling (where it is spending time), you can begin optimization.  

This is very brief summary of [Cython](https://cython.readthedocs.io/en/latest/) and how to use it to **quickly** accelerate pure python code.  Cython is much more adaptable than what I discuss below; I have chosen to focus primarily on how you can use it to accelerate existing code, usually written during the course of prototyping, with **minimal** modification.  This is my primary use case for Cython, and it allows you to use your code in production for research much more easily than trying to develop optimized C/C++ from scratch.

In fact, if you are writing scientific code and working with numerical computations, [numba](/tutorials/using_numba/) often allows to accelerate code even more easily.  I recommend you check that out first. Cython is the next step if further speed up is required.  For maximum performance, consider a more extensive re-write of the pure python code to exploit all the benefits that Cython affords you.

In addition, using best python practice can also help you speed up your code.  Here are some [tips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips) you should follow.

Much of this is taken from [Cython's official documentation](https://cython.readthedocs.io/en/latest/), which is more extensive and should be referred to for more advanced applications.

# What is Cython?

Cython is a Python compiler, which means it can compile normal Python code (.py files) without any changes.  Generally, this increases the performance of the functions contained within compiled files (which are `import`-ed later by other code).

> "Cython is a programming language that makes writing C extensions for the Python language as easy as Python itself. It aims to become a superset of the Python language which gives it high-level, object-oriented, functional, and dynamic programming. Its main feature on top of these is support for optional static type declarations as part of the language. The source code gets translated into optimized C/C++ code and compiled as Python extension modules. This allows for both very fast program execution and tight integration with external C libraries, while keeping up the high programmer productivity for which the Python language is well known."

The easiest way to install Cython is by using pip or anaconda:

~~~bash
$ pip install Cython # or
$ conda install -c anaconda cython
~~~

# Compiling your code

## Individual files

Cython code needs to be compiled, unlike pure python code.  This happens in 2 stages:
1. Your .pyx or .py file is compiled by Cython into C code (.c file), then
2. The .c file is compiled with a C compiler to create a .so file which is what you actually import

The primary way this is accomplished is via a `setup.py` file which looks something like this:

~~~python
from setuptools import setup
from Cython.Build import cythonize

setup(
    name='Hello world app',
    ext_modules=cythonize("hello.pyx"),
    annotate=True, # This will be explained later
    zip_safe=False,
)
~~~

This `setup.py` file is like a Makefile for C/C++ projects.  The target of this example is a your other file called `hello.pyx`.  If you are trying to optimize your python code, you can generally just give your old .py file a new file extension (.pyx) if you intend to start modifying it (see below).  Cython can also compile pure python code (no need to change your file extension), but the speed up is usually modest (20-50\%), so it is best to at least exploit the basic tools discussed below, such as static typing.  Execute the compilation as follows:

~~~bash
$ python setup.py build_ext --inplace
~~~

## Jupyter

You can also compile from inside a Jupyter notebook. First load the Cython extension:

~~~python
%load_ext Cython
~~~

Then, prefix any cell with `%%cython` magic to compile is.  This compiles the first time you execute the cell.

~~~python
%%cython # --annotate # This annotate option will be explained later

a: cython.int = 0
for i in range(10):
    a += i
print(a)
~~~

# Optimizing the Code

## Static Typing

One of the best ways to use Cython to accelerate your python code is by indicating static type declarations using `cdef` statements.  At run-time python normally needs to figure out what variable types are, and perform other checks.  By declaring these types in advance, you can skip that overhead.  You can also turn off other "checks" with decorators (discussed later).  Together, these are some of the easiest ways to quickly accelerate pure python code without having to re-write anything.

Pure python syntax also allows static Cython type declarations, e.g., `i: x` to declare `x` as an integer.

 > "It must be noted, however, that type declarations can make the source code more verbose and thus less readable. It is therefore discouraged to use them without good reason, such as where benchmarks prove that they really make the code substantially faster in a performance critical section. Typically a few types in the right spots go a long way."

Compare this pure python code (example from Cython's documentation) saved in a file called `integrate_cy.py`:

~~~python
import cython

def f(x: cython.double):
    return x ** 2 - x

def integrate_f(a: cython.double, b: cython.double, N: cython.int):
    i: cython.int
    s: cython.double
    dx: cython.double
    s = 0
    dx = (b - a) / N
    for i in range(N):
        s += f(a + i * dx)
    return s * dx
~~~

to the Cython version (in a file called `integrate_cy.pyx`):

~~~python
import cython 

def f(double x):
    return x ** 2 - x

def integrate_f(double a, double b, int N):
    cdef int i
    cdef double s
    cdef double dx
    s = 0
    dx = (b - a) / N
    for i in range(N):
        s += f(a + i * dx)
    return s * dx
~~~

Unlike C/C++, function calls in python are expensive.  Cython can actually make it worse if you are not careful, because you may have to convert to and from python objects to call a function.  The solution is to actually declare a function to have C-style.

For example, in pure python:

~~~python
@cython.cfunc
@cython.exceptval(check=True)
def f(x: cython.double) -> cython.double:
    return x ** 2 - x
~~~

vs. Cython:

~~~python
cdef double f(double x) except *:
    return x ** 2 - x
~~~

Some form of except-modifier should usually be used, otherwise Cython will not be able to propagate exceptions raised in the function (or a function it calls). If the function returns a python object or if it is guaranteed that an exception will not be raised, you can leave out the modifier.  Importantly, if you declate a function using `cdef` (or with the `@cfunc` decorator) the function is no longer available from Python-space, since it does not know how to call it. Instead, you can change this to `cpdef` (or `@ccall` decorator) to make a function available to pure python functions.  This does add a small overhead, but not much, so it is generally easiest to just use the `cpdef` by default.

## Annotating

> "Because static typing is often the key to large speed gains, beginners often have a tendency to type everything in sight. This cuts down on both readability and flexibility, and can even slow things down (e.g. by adding unnecessary type checks, conversions, or slow buffer unpacking). On the other hand, it is easy to kill performance by forgetting to type a critical loop variable. Two essential tools to help with this task are profiling and annotation. Profiling should be the first step of any optimization effort, and can tell you where you are spending your time. Cython's annotation can then tell you why your code is taking time."

Using the `annotate` keyword produces an HTML output which highlights the generated Cython/C code in yellow on lines where python is being invoked.  The more yellow, the more interaction with python, the slower the code.  Optimally, you should just have "white" lines.

> "This report is invaluable when optimizing a function for speed, and for determining when to release the GIL: in general, a nogil block may contain only white code."

## Profiling

More details on [python profiling](https://docs.python.org/3/library/profile.html) can be found in python's offical documentation.  This is easy to read and implement if you are not already familiar with it.  This can be extended to profile Cython code as well. Consider the following example from Cython's official documentation:

~~~python
# calc_pi.py

def recip_square(i):
    return 1. / i ** 2

def approx_pi(n=10000000):
    val = 0.
    for k in range(1, n + 1):
        val += recip_square(k)
    return (6 * val) ** .5
~~~

You can profile this code to see how it runs with the following script.

~~~python
# profile.py

import pstats, cProfile

import calc_pi

cProfile.runctx("calc_pi.approx_pi()", globals(), locals(), "Profile.prof")

s = pstats.Stats("Profile.prof")
s.strip_dirs().sort_stats("time").print_stats()
~~~

Running code in the cProfile module will slow it down substantially (often by a factor of ~2) so always profiling using simple, otherwise easy to execute tests cases. Execute the profiling test:

~~~bash
$ python profile.py
~~~

The most important output to look at is the ``tottime`` column (total time spent on a function, including the time it spends calling other funtions).  Comparatively, Cython files need to be told to enable profiling in the first line of the file.  **Remove this during production to avoid slowing down your code unnecessarily.**

You can compile the code using a `setup.py` file as shown above, but for this example `pyximport` is used to compile it on the fly.  If you compile your code ahead of time, you don't need this.

~~~python
# profile.py

import pstats, cProfile

import pyximport # Add this line
pyximport.install() # Add this line

import calc_pi

cProfile.runctx("calc_pi.approx_pi()", globals(), locals(), "Profile.prof")

s = pstats.Stats("Profile.prof")
s.strip_dirs().sort_stats("time").print_stats()
~~~

You can disable profiling of an individual function with a decorator:

~~~python
@cython.profile(False)
cdef myFunction(double a, double b):
	return a*b
~~~

It is important to read your timings with the following understanding of how profiling works: calling a function during a profile run adds overhead to the function call itself. As a result, this overhead is **not** added to the time spent **inside** the called function, but instead is added to the time spent in the other functions which call it.  So inlining an removing the profiling for the `recip_square` function will make the calling function `approx_pi` take much less time during a profile.  This will reflect more accurate timings you will achieve during production.  You can using `%timeit` or other functions to compare. A final, optimzed version of the code might look like this:

~~~python
# cython: profile=True

# calc_pi.pyx

cimport cython

@cython.profile(False)
cdef inline double recip_square(int i) except -1.0:
    return 1. / (i * i)

def approx_pi(int n=10000000):
    cdef double val = 0.
    cdef int k
    for k in range(1, n + 1):
        val += recip_square(k)
    return (6 * val) ** .5
~~~

# Working with Numpy

Cython now uses typed memoryviews to integrate with numpy rather than adding types to numpy arrays.  Older codes may, thus, appear different from what is discussed below.

You need to import the numpy headers to use them in Cython code.

~~~python
cimport numpy
~~~

One major issue with using numpy arrays in Cython is that indices are often provided as python integers, not C ints; this means that have to be converted each time an array is accessed which dramatically slows things down.  In general, array lookups and assignments are usually the primary bottlenecks (besides loops which are always slow in python).  The alternative to using "[]" operators is the use [memoryviews](https://cython.readthedocs.io/en/latest/src/userguide/memoryviews.html#memoryviews).

> "In short, memoryviews are C structures that can hold a pointer to the data of a NumPy array and all the necessary buffer metadata to provide efficient and safe access: dimensions, strides, item size, item type information, etc… They also support slices, so they work even if the NumPy array isn’t contiguous in memory. They can be indexed by C integers, thus allowing fast access to the NumPy array data."

These are declared like:

~~~python
cdef int [:] foo         # 1D memoryview
cdef int [:, :] foo      # 2D memoryview
cdef int [:, :, :] foo   # 3D memoryview
~~~

These views are exactly that - they are essentially like pointers to the place in memory where that information is stored.  You can use memoryviews to retrieve and manipulate those values efficiently. Here is an example from the official Cython documentation on using memoryviews well.

~~~python
# compute_memview.pyx

import numpy as np

DTYPE = np.intc

cdef int clip(int a, int min_value, int max_value):
    return min(max(a, min_value), max_value)

def compute(int[:, :] array_1, int[:, :] array_2, int a, int b, int c):
    cdef Py_ssize_t x_max = array_1.shape[0]
    cdef Py_ssize_t y_max = array_1.shape[1]

    # array_1.shape is now a C array, no it's not possible
    # to compare it simply by using == without a for-loop.
    # To be able to compare it to array_2.shape easily,
    # we convert them both to Python tuples.
    assert tuple(array_1.shape) == tuple(array_2.shape)

    result = np.zeros((x_max, y_max), dtype=DTYPE)
    cdef int[:, :] result_view = result

    cdef int tmp
    cdef Py_ssize_t x, y

    # Do all the manipulation of the array using the memoryview
    for x in range(x_max):
        for y in range(y_max):
            tmp = clip(array_1[x, y], 2, 10)
            tmp = tmp * a + array_2[x, y] * b
            result_view[x, y] = tmp + c

    return result # You can return the array as normal!
~~~

There are two other factors that usually slow down the use of numpy arrays:
1. bounds checking, and
2. negative indices

For the second point, recall that numpy (and python) arrays can be accessed "from the end" like `my_array[-3]`.  This would not be valid C code and would be an invalid access.  Since that sort of thing is allowed in python, the interpreter needs to check for that.  The best practice is simply to change them to, e.g., `my_array[len(my_array)-3]`.  You can deactivate these with bottlenecks, if you are sure you have written your code correctly (previously tested!), with decorators:

~~~python
...
cimport cython
@cython.boundscheck(False)  # Deactivate bounds checking
@cython.wraparound(False)   # Deactivate negative indexing.
def compute(int[:, :] array_1, int[:, :] array_2, int a, int b, int c):
...
~~~

You can get even more speed up if you make things contiguous in memory.  Numpy does allow fortran (column-major) access, but here is an example for C (row major).  The memoryview is specified slightly differently, with a change in the last index as illustrated below.

~~~python
my_c_array = np.zeros((10, 10, 10), dtype=np.intc)
my_c_array = numpy.ascontiguousarray(my_c_array)
cdef int[:,:,::1] my_memview = my_c_array
~~~

A major innovation is the ability to used use ["fused types"](https://cython.readthedocs.io/en/latest/src/userguide/fusedtypes.html#fusedtypes).  They are basically like templates in C/C++ which allow you to define a function once; at compile time, multiple function declarations are generated, and at run time, the correct one is inferred based on the data being received. By comparing types in if-statements, you can even follow entirely different code paths depending on your data type! Here is an example:

~~~python
# cython: infer_types=True
import numpy as np
cimport cython

ctypedef fused my_type:
    int
    double
    long long

cdef my_type clip(my_type a, my_type min_value, my_type max_value):
    return min(max(a, min_value), max_value)

@cython.boundscheck(False)
@cython.wraparound(False)
def compute(my_type[:, ::1] array_1, my_type[:, ::1] array_2, my_type a, my_type b, my_type c):
    x_max = array_1.shape[0]
    y_max = array_1.shape[1]

    assert tuple(array_1.shape) == tuple(array_2.shape)

    if my_type is int:
        dtype = np.intc
    elif my_type is double:
        dtype = np.double
    elif my_type is cython.longlong:
        dtype = np.longlong

    result = np.zeros((x_max, y_max), dtype=dtype)
    cdef my_type[:, ::1] result_view = result

    cdef my_type tmp
    cdef Py_ssize_t x, y

    for x in range(x_max):
        for y in range(y_max):

            tmp = clip(array_1[x, y], 2, 10)
            tmp = tmp * a + array_2[x, y] * b
            result_view[x, y] = tmp + c

    return result
~~~

# Dealing with Classes

Cython supports a second kind of class, besides the "built-in type" called "extension types" or "cdef classes."

> "They are somewhat restricted compared to Python classes, but are generally more memory efficient and faster than generic Python classes. The main difference is that they use a C struct to store their fields and methods instead of a Python dict. This allows them to store arbitrary C types in their fields without requiring a Python wrapper for them, and to access fields and methods directly at the C level without passing through a Python dictionary lookup."

Normal python classes can inherit from cdef classes, but not the other way around.  In this example, the `cpdef` makes the `evaluate` function callable from Cython or python.

~~~python
# sin_of_square.pyx

from libc.math cimport sin

# Defining this function means you can declare a variable to be of this type
cdef class Function:
    # You need this to be defined so other classes efficienty inherit this
    cpdef double evaluate(self, double x) except *:
        return 0

cdef class SinOfSquareFunction(Function):
    cpdef double evaluate(self, double x) except *:
        return sin(x ** 2)
~~~

To make the class definitions visible to other modules, and thus allow for efficient C-level usage and inheritance outside of the module that implements them, we define them in a [`.pxd`](https://cython.readthedocs.io/en/latest/src/tutorial/pxd_files.html) file with the same name as the module.  This is basically like a header file in C/C++

~~~python
# sin_of_square.pxd

cdef class Function:
    cpdef double evaluate(self, double x) except *

cdef class SinOfSquareFunction(Function):
    cpdef double evaluate(self, double x) except *
~~~

One other thing: `None` is an allowable value for a variable in python which is checked for before it is used.  However, Cython will just try to use this without cehcking which can lead to a crash if you try it.  The most efficient, but bulky, way around this is to have a manual check as below.  It is possible to turn on compiler directives at the top of the file (`# cython: nonecheck=True`) but this will slow down the code.

~~~python
# integrate.pyx

from sin_of_square cimport Function, SinOfSquareFunction

# Observe how f is typed as a Function
def integrate(Function f, double a, double b, int N):
    cdef int i
    cdef double s, dx
    if f is None: # Since the argument is typed, we need to check whether it is None.
        raise ValueError("f cannot be None")

    s = 0
    dx = (b - a) / N

    for i in range(N):
        s += f.evaluate(a + i * dx)

    return s * dx

print(integrate(SinOfSquareFunction(), 0, 1, 10000))
~~~

## A note about class attributes

1. All attributes must be pre-declared at compile-time
2. Attributes are by default only accessible from Cython (typed access)
3. Properties can be declared to expose dynamic attributes to Python-space

For example:

~~~python
from sin_of_square cimport Function

cdef class WaveFunction(Function):
    # Not available in Python-space:
    cdef double offset

    # Available in Python-space:
    cdef public double freq

    # Available in Python-space, but only for reading:
    cdef readonly double scale

    # Available in Python-space:
    @property
    def period(self):
        return 1.0 / self.freq

    @period.setter
    def period(self, value):
        self.freq = 1.0 / value
~~~

# Common Decorators

For reference, here is a list of some common decorators that you can use to accelerate or clean up your code under different circumstances.

Python
* @lru_cache - caches the function to make subsequent function calls faster, especially important in recursion
* @jit - just in time
* @staticmethod - often used to define methods within a class as a "contained"; this exposes these decorated functions to be used globally
* @property

Cython
* @cython.ccall - creates a cpdef function
* @cython.locals(a=cython.int) - declares a local variable `a` and can be used to declare types for arguments
* @cython.inline - equivalent of C inline modifier
* @cython.final(True) - terminates the inheritance chain by preventing a type from being used as a base class, or a method from being overridden in subtypes. This enables certain optimizations such as inlined method call.
* @cython.returns(cython.int) - declares data type for return variable
* @cython.boundscheck(False)
* @cython.wraparound(False)
* @cython.exceptval(check=True)
* @cython.profile(False)
* @cython.cfunc

