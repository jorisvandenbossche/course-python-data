---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Python the basics: numpy

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is largely based on material of the *Python Scientific Lecture Notes* (https://scipy-lectures.github.io/), adapted with some exercises.

```python run_control={"frozen": false, "read_only": false}
%matplotlib inline
```

# Numpy -  multidimensional data arrays


## Introduction


NumPy is the fundamental package for scientific computing with Python. It contains among other things:

* a powerful N-dimensional array/vector/matrix object
* sophisticated (broadcasting) functions
* function implementation in C/Fortran assuring good performance if vectorized
* tools for integrating C/C++ and Fortran code
* useful linear algebra, Fourier transform, and random number capabilities

Also known as *array oriented computing*. The recommended convention to import numpy is:

```python run_control={"frozen": false, "read_only": false}
import numpy as np
```

In the `numpy` package the terminology used for vectors, matrices and higher-dimensional data sets is *array*. Let's already load some other modules too.



```python run_control={"frozen": false, "read_only": false}
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('darkgrid')
```

## Showcases


### Roll the dice


You like to play boardgames, but you want to better know you're chances of rolling a certain combination with 2 dices:

```python run_control={"frozen": false, "read_only": false}
def mydices(throws):
    """
    Function to create the distrrbution of the sum of two dices.
    
    Parameters
    ----------
    throws : int
        Number of throws with the dices
    """
    stone1 = np.random.uniform(1, 6, throws) 
    stone2 = np.random.uniform(1, 6, throws) 
    total = stone1 + stone2
    return plt.hist(total, bins=20) # We use matplotlib to show a histogram
```

```python run_control={"frozen": false, "read_only": false}
mydices(100) # test this out with multiple options
```

### Cartesian2Polar


Consider a random 10x2 matrix representing cartesian coordinates, how to convert them to polar coordinates

```python run_control={"frozen": false, "read_only": false}
# random numbers (X, Y in 2 columns)
Z = np.random.random((10,2))
X, Y = Z[:,0], Z[:,1]

# distance
R = np.sqrt(X**2 + Y**2)
# angle
T = np.arctan2(Y, X) # Array of angles in radians
Tdegree = T*180/(np.pi) # If you like degrees more

# NEXT PART (now for illustration)
#plot the cartesian coordinates
plt.figure(figsize=(14, 6))
ax1 = plt.subplot(121)
ax1.plot(Z[:,0], Z[:,1], 'o')
ax1.set_title("Cartesian")
#plot the polar coorsidnates
ax2 = plt.subplot(122, polar=True)
ax2.plot(T, R, 'o')
ax2.set_title("Polar")
```

### Speed


Memory-efficient container that provides fast numerical operations:

```python run_control={"frozen": false, "read_only": false}
L = range(1000)
%timeit [i**2 for i in L]
```

```python run_control={"frozen": false, "read_only": false}
a = np.arange(1000)
%timeit a**2
```

```python run_control={"frozen": false, "read_only": false}
#More information about array?
np.array?
```

## Creating `numpy` arrays


There are a number of ways to initialize new numpy arrays, for example from

* a Python list or tuples
* using functions that are dedicated to generating numpy arrays, such as `arange`, `linspace`, etc.
* reading data from files


### From lists


For example, to create new vector and matrix arrays from Python lists we can use the `numpy.array` function.

```python run_control={"frozen": false, "read_only": false}
# a vector: the argument to the array function is a Python list
V = np.array([1, 2, 3, 4])
V
```

```python run_control={"frozen": false, "read_only": false}
# a matrix: the argument to the array function is a nested Python list
M = np.array([[1, 2], [3, 4]])
M
```

The `v` and `M` objects are both of the type `ndarray` that the `numpy` module provides.

```python run_control={"frozen": false, "read_only": false}
type(V), type(M)
```

The difference between the `v` and `M` arrays is only their shapes. We can get information about the shape of an array by using the `ndarray.shape` property.

```python run_control={"frozen": false, "read_only": false}
V.shape
```

```python run_control={"frozen": false, "read_only": false}
M.shape
```

The number of elements in the array is available through the `ndarray.size` property:

```python run_control={"frozen": false, "read_only": false}
M.size
```

Equivalently, we could use the function `numpy.shape` and `numpy.size`

```python run_control={"frozen": false, "read_only": false}
np.shape(M)
```

```python run_control={"frozen": false, "read_only": false}
np.size(M)
```

Using the `dtype` (data type) property of an `ndarray`, we can see what type the data of an array has (always fixed for each array, cfr. Matlab):

```python run_control={"frozen": false, "read_only": false}
M.dtype
```

We get an error if we try to assign a value of the wrong type to an element in a numpy array:

```python run_control={"frozen": false, "read_only": false}
#M[0,0] = "hello"  #uncomment this cell
```

```python run_control={"frozen": false, "read_only": false}
f = np.array(['Bonjour', 'Hello', 'Hallo',])
f
```

If we want, we can explicitly define the type of the array data when we create it, using the `dtype` keyword argument: 

```python run_control={"frozen": false, "read_only": false}
M = np.array([[1, 2], [3, 4]], dtype=complex)  #np.float64, np.float, np.int64

print(M, '\n', M.dtype)
```

Since Numpy arrays are *statically typed*, the type of an array does not change once created. But we can explicitly cast an array of some type to another using the `astype` functions (see also the similar `asarray` function). This always create a new array of new type:

```python run_control={"frozen": false, "read_only": false}
M = np.array([[1, 2], [3, 4]], dtype=float)
M2 = M.astype(int)
M2
```

Common type that can be used with `dtype` are: `int`, `float`, `complex`, `bool`, `object`, etc.

We can also explicitly define the bit size of the data types, for example: `int64`, `int16`, `float64`, `float128`, `complex128`.


Higher order is also possible:

```python run_control={"frozen": false, "read_only": false}
C = np.array([[[1], [2]], [[3], [4]]])
print(C.shape)
C
```

```python run_control={"frozen": false, "read_only": false}
C.ndim # number of dimensions
```

### Using array-generating functions


For larger arrays it is inpractical to initialize the data manually, using explicit python lists. Instead we can use one of the many functions in `numpy` that generates arrays of different forms. Some of the more common are:


#### arange

```python run_control={"frozen": false, "read_only": false}
# create a range
x = np.arange(0, 10, 1) # arguments: start, stop, step
x
```

```python run_control={"frozen": false, "read_only": false}
x = np.arange(-1, 1, 0.1)
x
```

#### linspace and logspace

```python run_control={"frozen": false, "read_only": false}
# using linspace, both end points ARE included
np.linspace(0, 10, 25)
```

```python run_control={"frozen": false, "read_only": false}
np.logspace(0, 10, 10, base=np.e)
```

```python run_control={"frozen": false, "read_only": false}
plt.plot(np.logspace(0, 10, 10, base=np.e), np.random.random(10), 'o')
plt.xscale('log')
```

#### random data

```python run_control={"frozen": false, "read_only": false}
# uniform random numbers in [0,1]
np.random.rand(5,5)
```

```python run_control={"frozen": false, "read_only": false}
# standard normal distributed random numbers
np.random.randn(5,5)
```

#### zeros and ones

```python run_control={"frozen": false, "read_only": false}
np.zeros((3,3))
```

```python run_control={"frozen": false, "read_only": false}
np.ones((3,3))
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Create a vector with values ranging from 10 to 49 with steps of 1
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
np.arange(10, 50, 1)
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Create a 3x3 identity matrix (look into docs!)
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
np.identity(3)
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
np.eye(3)
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Create a 3x3x3 array with random values
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
np.random.random((3, 3, 3))
```

------------


## File I/O


Numpy is capable of reading and writing text and binary formats. However, since most data-sources are providing information in a format with headings, different dtypes,... we will use for reading/writing of textfiles the power of **Pandas**.


### Comma-separated values (CSV)


Writing to a csvfile with numpy is done with the savetxt-command:

```python run_control={"frozen": false, "read_only": false}
a = np.random.random(40).reshape((20, 2))
np.savetxt("random-matrix.csv", a, delimiter=",")
```

To read data from such file into Numpy arrays we can use the `numpy.genfromtxt` function. For example, 

```python run_control={"frozen": false, "read_only": false}
a2 = np.genfromtxt("random-matrix.csv", delimiter=',')
a2
```

### Numpy's native file format


Useful when storing and reading back numpy array data, since binary. Use the functions `numpy.save` and `numpy.load`:

```python run_control={"frozen": false, "read_only": false}
np.save("random-matrix.npy", a)

!file random-matrix.npy
```

```python run_control={"frozen": false, "read_only": false}
np.load("random-matrix.npy")
```

## Manipulating arrays


### Indexing


<center>**MATLAB-USERS:<br> PYTHON STARTS AT 0!**


We can index elements in an array using the square bracket and indices:

```python run_control={"frozen": false, "read_only": false}
V
```

```python run_control={"frozen": false, "read_only": false}
# V is a vector, and has only one dimension, taking one index
V[0]
```

```python run_control={"frozen": false, "read_only": false}
V[-1:]  #-2, -2:,...
```

```python run_control={"frozen": false, "read_only": false}
# a is a matrix, or a 2 dimensional array, taking two indices 
# the first dimension corresponds to rows, the second to columns.
a[1, 1]
```

If we omit an index of a multidimensional array it returns the whole row (or, in general, a N-1 dimensional array) 

```python run_control={"frozen": false, "read_only": false}
a[1]
```

The same thing can be achieved with using `:` instead of an index: 

```python run_control={"frozen": false, "read_only": false}
a[1, :] # row 1
```

```python run_control={"frozen": false, "read_only": false}
a[:, 1] # column 1
```

We can assign new values to elements in an array using indexing:

```python run_control={"frozen": false, "read_only": false}
a[0, 0] = 1
a[:, 1] = -1
a
```

### Index slicing


Index slicing is the technical name for the syntax `M[lower:upper:step]` to extract part of an array:

```python run_control={"frozen": false, "read_only": false}
A = np.array([1, 2, 3, 4, 5])
A
```

```python run_control={"frozen": false, "read_only": false}
A[1:3]
```

Array slices are *mutable*: if they are assigned a new value the original array from which the slice was extracted is modified:

```python run_control={"frozen": false, "read_only": false}
A[1:3] = [-2,-3]

A
```

We can omit any of the three parameters in `M[lower:upper:step]`:

```python run_control={"frozen": false, "read_only": false}
A[::] # lower, upper, step all take the default values
```

```python run_control={"frozen": false, "read_only": false}
A[::2] # step is 2, lower and upper defaults to the beginning and end of the array
```

```python run_control={"frozen": false, "read_only": false}
A[:3] # first three elements
```

```python run_control={"frozen": false, "read_only": false}
A[3:] # elements from index 3
```

```python run_control={"frozen": false, "read_only": false}
A[-3:] # the last three elements
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Create a null vector of size 10 and adapt it in order to make the fifth element a value  1
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
vec = np.zeros(10)
vec[4] = 1.
vec
```

----------------------


### Fancy indexing


Fancy indexing is the name for when an array or list is used in-place of an index: 

```python run_control={"frozen": false, "read_only": false}
a = np.arange(0, 100, 10)
a[[2, 3, 2, 4, 2]]
```

In more dimensions:

```python run_control={"frozen": false, "read_only": false}
A = np.arange(25).reshape(5,5)
A
```

```python run_control={"frozen": false, "read_only": false}
row_indices = [1, 2, 3]
A[row_indices]
```

```python run_control={"frozen": false, "read_only": false}
col_indices = [1, 2, -1] # remember, index -1 means the last element
A[row_indices, col_indices]
```

We can also index masks: If the index mask is an Numpy array of with data type `bool`, then an element is selected (True) or not (False) depending on the value of the index mask at the position each element: 

```python run_control={"frozen": false, "read_only": false}
B = np.array([n for n in range(5)])  #range is pure python => Exercise: Make this shorter with pur numpy
B
```

```python run_control={"frozen": false, "read_only": false}
row_mask = np.array([True, False, True, False, False])
B[row_mask]
```

```python run_control={"frozen": false, "read_only": false}
# same thing
row_mask = np.array([1,0,1,0,0], dtype=bool)
B[row_mask]
```

This feature is very useful to conditionally select elements from an array, using for example comparison operators:

```python run_control={"frozen": false, "read_only": false}
AR = np.random.randint(0, 20, 15)
AR
```

```python run_control={"frozen": false, "read_only": false}
AR%3 == 0
```

```python run_control={"frozen": false, "read_only": false}
extract_from_AR = AR[AR%3 == 0]
extract_from_AR
```

```python run_control={"frozen": false, "read_only": false}
x = np.arange(0, 10, 0.5)
x
```

```python run_control={"frozen": false, "read_only": false}
mask = (5 < x) * (x < 7.5)  # We actually multiply two masks here (boolean 0 and 1 values)
mask
```

```python run_control={"frozen": false, "read_only": false}
x[mask]
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Swap the first two rows of the 2-D array `A`?
</div>

```python run_control={"frozen": false, "read_only": false}
A = np.arange(25).reshape(5,5)
A
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
#SWAP
A[[0, 1]] = A[[1, 0]]
A
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Change all even numbers of `AR` into zero-values.
</div>

```python run_control={"frozen": false, "read_only": false}
AR = np.random.randint(0, 20, 15)
AR
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
AR[AR%2==0] = 0.
AR
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Change all even positions of matrix `AR` into zero-values
</div>

```python run_control={"frozen": false, "read_only": false}
AR = np.random.randint(1, 20, 15)
AR
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
AR[1::2] = 0
AR
```

------------------------


### Some more extraction functions


where function to know the indices of something

```python run_control={"frozen": false, "read_only": false}
x = np.arange(0, 10, 0.5)
np.where(x>5.)
```

With the diag function we can also extract the diagonal and subdiagonals of an array:

```python run_control={"frozen": false, "read_only": false}
np.diag(A)
```

The `take` function is similar to fancy indexing described above:

```python run_control={"frozen": false, "read_only": false}
x.take([1, 5])
```

## Linear algebra


Vectorizing code is the key to writing efficient numerical calculation with Python/Numpy. That means that as much as possible of a program should be formulated in terms of matrix and vector operations.


### Scalar-array operations


We can use the usual arithmetic operators to multiply, add, subtract, and divide arrays with scalar numbers.

```python run_control={"frozen": false, "read_only": false}
v1 = np.arange(0, 5)
```

```python run_control={"frozen": false, "read_only": false}
v1 * 2
```

```python run_control={"frozen": false, "read_only": false}
v1 + 2
```

```python run_control={"frozen": false, "read_only": false}
A = np.arange(25).reshape(5,5)
A * 2
```

```python run_control={"frozen": false, "read_only": false}
np.sin(A) #np.log(A), np.arctan,...
```

### Element-wise array-array operations


When we add, subtract, multiply and divide arrays with each other, the default behaviour is **element-wise** operations:

```python run_control={"frozen": false, "read_only": false}
A * A # element-wise multiplication
```

```python run_control={"frozen": false, "read_only": false}
v1 * v1
```

If we multiply arrays with compatible shapes, we get an element-wise multiplication of each row:

```python run_control={"frozen": false, "read_only": false}
A.shape, v1.shape
```

```python run_control={"frozen": false, "read_only": false}
A * v1
```

Consider the speed difference with pure python:

```python run_control={"frozen": false, "read_only": false}
a = np.arange(10000)
%timeit a + 1  

l = range(10000)
%timeit [i+1 for i in l] 
```

```python run_control={"frozen": false, "read_only": false}
#logical operators:
a1 = np.arange(0, 5, 1)
a2 = np.arange(5, 0, -1)
a1>a2  # >, <=,...
```

```python run_control={"frozen": false, "read_only": false}
# cfr. 
np.all(a1>a2) # any
```

Basic operations on numpy arrays (addition, etc.) are elementwise. Nevertheless, It’s also possible to do operations on **arrays of different sizes** if Numpy can transform these arrays so that they all have the same size: this conversion is called **broadcasting**.

```python run_control={"frozen": false, "read_only": false}
A, v1
```

```python run_control={"frozen": false, "read_only": false}
A*v1
```

```python run_control={"frozen": false, "read_only": false}
x, y = np.arange(5), np.arange(5).reshape((5, 1)) # a row and a column array
```

```python run_control={"frozen": false, "read_only": false}
distance = np.sqrt(x ** 2 + y ** 2)
distance
```

```python run_control={"frozen": false, "read_only": false}
#let's put this in a figure:
plt.pcolor(distance)    
plt.colorbar()  
```

### Matrix algebra


What about matrix mutiplication? There are two ways. We can either use the `dot` function, which applies a matrix-matrix, matrix-vector, or inner vector multiplication to its two arguments: 

```python run_control={"frozen": false, "read_only": false}
np.dot(A, A)
```

```python run_control={"frozen": false, "read_only": false}
np.dot(A, v1) #check the difference with A*v1 !!
```

```python run_control={"frozen": false, "read_only": false}
np.dot(v1, v1)
```

Alternatively, we can cast the array objects to the type `matrix`. This changes the behavior of the standard arithmetic operators `+, -, *` to use matrix algebra. You can also get `inverse` of matrices, `determinant`,...  

We won't go deeper here on pure matrix calculation, but for more information, check the related functions: `inner`, `outer`, `cross`, `kron`, `tensordot`. Try for example `help(kron)`.




### Calculations


Often it is useful to store datasets in Numpy arrays. Numpy provides a number of functions to calculate statistics of datasets in arrays. 

```python run_control={"frozen": false, "read_only": false}
a = np.random.random(40)
```

Different frequently used operations can be done:

```python run_control={"frozen": false, "read_only": false}
print ('Mean value is', np.mean(a))
print ('Median value is',  np.median(a))
print ('Std is', np.std(a))
print ('Variance is', np.var(a))
print ('Min is', a.min())
print ('Element of minimum value is', a.argmin())
print ('Max is', a.max())
print ('Sum is', np.sum(a))
print ('Prod', np.prod(a))
print ('Cumsum is', np.cumsum(a)[-1])
print ('CumProd of 5 first elements is', np.cumprod(a)[4])
print ('Unique values in this array are:', np.unique(np.random.randint(1,6,10)))
print ('85% Percentile value is: ', np.percentile(a, 85))
```

```python run_control={"frozen": false, "read_only": false}
a = np.random.random(40)
print(a.argsort())
a.sort() #sorts in place!
print(a.argsort())
```

### Calculations with higher-dimensional data


When functions such as `min`, `max`, etc., is applied to a multidimensional arrays, it is sometimes useful to apply the calculation to the entire array, and sometimes only on a row or column basis. Using the `axis` argument we can specify how these functions should behave: 

```python run_control={"frozen": false, "read_only": false}
m = np.random.rand(3,3)
m
```

```python run_control={"frozen": false, "read_only": false}
# global max
m.max()
```

```python run_control={"frozen": false, "read_only": false}
# max in each column
m.max(axis=0)
```

```python run_control={"frozen": false, "read_only": false}
# max in each row
m.max(axis=1)
```

Many other functions and methods in the `array` and `matrix` classes accept the same (optional) `axis` keyword argument.


<div class="alert alert-success">
    <b>EXERCISE</b>: Rescale the 5x5 matrix `Z` to values between 0 and 1:
</div>

```python run_control={"frozen": false, "read_only": false}
Z = np.random.uniform(5.0, 15.0, (5,5))
Z
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
# RESCALE:
(Z - Z.min())/(Z.max() - Z.min())
```

----------------


## Reshaping, resizing and stacking arrays


The shape of an Numpy array can be modified without copying the underlaying data, which makes it a fast operation even for large arrays.

```python run_control={"frozen": false, "read_only": false}
A = np.arange(25).reshape(5,5)
n, m = A.shape
B = A.reshape((1,n*m))
B
```

We can also use the function `flatten` to make a higher-dimensional array into a vector. But this function create a copy of the data (see next)

```python run_control={"frozen": false, "read_only": false}
B = A.flatten()
B
```

## Stacking and repeating arrays


Using function `repeat`, `tile`, `vstack`, `hstack`, and `concatenate` we can create larger vectors and matrices from smaller ones:


### tile and repeat

```python run_control={"frozen": false, "read_only": false}
a = np.array([[1, 2], [3, 4]])
```

```python run_control={"frozen": false, "read_only": false}
# repeat each element 3 times
np.repeat(a, 3)
```

```python run_control={"frozen": false, "read_only": false}
# tile the matrix 3 times 
np.tile(a, 3)
```

### concatenate

```python run_control={"frozen": false, "read_only": false}
b = np.array([[5, 6]])
```

```python run_control={"frozen": false, "read_only": false}
np.concatenate((a, b), axis=0)
```

```python run_control={"frozen": false, "read_only": false}
np.concatenate((a, b.T), axis=1)
```

### hstack and vstack

```python run_control={"frozen": false, "read_only": false}
np.vstack((a,b))
```

```python run_control={"frozen": false, "read_only": false}
np.hstack((a,b.T))
```

## IMPORTANT!: View and Copy


To achieve high performance, assignments in Python usually do not copy the underlaying objects. This is important for example when objects are passed between functions, to avoid an excessive amount of memory copying when it is not necessary (techincal term: pass by reference). 

```python run_control={"frozen": false, "read_only": false}
A = np.array([[1, 2], [3, 4]])

A
```

```python run_control={"frozen": false, "read_only": false}
# now B is referring to the same array data as A 
B = A 
```

```python run_control={"frozen": false, "read_only": false}
# changing B affects A
B[0,0] = 10

B
```

```python run_control={"frozen": false, "read_only": false}
A
```

If we want to avoid this behavior, so that when we get a new completely independent object `B` copied from `A`, then we need to do a so-called "deep copy" using the function `copy`:

```python run_control={"frozen": false, "read_only": false}
B = np.copy(A)
```

```python run_control={"frozen": false, "read_only": false}
# now, if we modify B, A is not affected
B[0,0] = -5

B
```

```python run_control={"frozen": false, "read_only": false}
A
```

Also reshape function just takes a view:

```python run_control={"frozen": false, "read_only": false}
arr = np.arange(8)
arr_view = arr.reshape(2, 4)
```

```python run_control={"frozen": false, "read_only": false}
print('Before\n', arr_view)
arr[0] = 1000
print('After\n', arr_view)
```

```python run_control={"frozen": false, "read_only": false}
arr.flatten()[2] = 10  #Flatten creates a copy!
```

```python run_control={"frozen": false, "read_only": false}
arr
```

## Using arrays in conditions


When using arrays in conditions in for example `if` statements and other boolean expressions, one need to use one of `any` or `all`, which requires that any or all elements in the array evalutes to `True`:

```python run_control={"frozen": false, "read_only": false}
M
```

```python run_control={"frozen": false, "read_only": false}
if (M > 5).any():
    print("at least one element in M is larger than 5")
else:
    print("no element in M is larger than 5")
```

```python run_control={"frozen": false, "read_only": false}
if (M > 5).all():
    print("all elements in M are larger than 5")
else:
    print("all elements in M are not larger than 5")
```

## Some extra applications:


### Polynomial fit

```python run_control={"frozen": false, "read_only": false}
b_data = np.genfromtxt("./data/bogota_part_dataset.csv", skip_header=3, delimiter=',')
plt.scatter(b_data[:,2], b_data[:,3])
```

```python run_control={"frozen": false, "read_only": false}
x, y = b_data[:,1], b_data[:,3] 
t = np.polyfit(x, y, 2) # fit a 2nd degree polynomial to the data, result is x**2 + 2x + 3
t
```

```python run_control={"frozen": false, "read_only": false}
x.sort()
plt.plot(x, y, 'o')
plt.plot(x, t[0]*x**2 + t[1]*x + t[2], '-')
```

---------------------_


<div class="alert alert-success">
    <b>EXERCISE</b>: Make a fourth order fit between the fourth and fifth column of `b_data`
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
x, y = b_data[:,3], b_data[:,4] 
t = np.polyfit(x, y, 4) # fit a 2nd degree polynomial to the data, result is x**2 + 2x + 3
t
x.sort()
plt.plot(x, y, 'o')
plt.plot(x, t[0]*x**4 + t[1]*x**3 + t[2]*x**2 + t[3]*x +t[4], '-')
```

-------------------__


However, when doing some kind of regression, we would like to have more information about the fit characterstics automatically. Statsmodels is a library that provides this functionality, we will later come back to this type of regression problem.


### Moving average function

```python run_control={"frozen": false, "read_only": false}
def moving_average(a, n=3) :
    ret = np.cumsum(a, dtype=float)
    ret[n:] = ret[n:] - ret[:-n]
    return ret[n - 1:] / n
```

```python run_control={"frozen": false, "read_only": false}
print(moving_average(b_data , n=3))
```

However, the latter fuction implementation is something we would expect from a good data-analysis library to be implemented already. 

The perfect timing for **Python Pandas**!


## REMEMBER!

* Know how to create arrays : **array, arange, ones, zeros,...**.

* Know the shape of the array with array.shape, then use **slicing** to obtain different **views** of the array: array[::2], etc. Adjust the shape of the array using reshape or flatten it.

* Obtain a subset of the elements of an array and/or modify their values with **masks**

* Know miscellaneous operations on arrays, such as finding the mean or max (array.max(), array.mean()). No need to retain everything, but have the **reflex to search in the documentation** (online docs, help(), lookfor())!!

* For advanced use: master the indexing with arrays of integers, as well as broadcasting. Know more Numpy functions to handle various array operations.


## Further reading


* http://numpy.scipy.org
* http://scipy.org/Tentative_NumPy_Tutorial
* http://scipy.org/NumPy_for_Matlab_Users - A Numpy guide for MATLAB users.
* http://wiki.scipy.org/Numpy_Example_List
* http://wiki.scipy.org/Cookbook


### Acknowledgments and Material


* J.R. Johansson (robert@riken.jp) http://dml.riken.jp/~rob/
* http://scipy-lectures.github.io/intro/numpy/index.html
* http://www.labri.fr/perso/nrougier/teaching/numpy.100/index.html
