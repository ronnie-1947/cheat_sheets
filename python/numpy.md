---
description: >-
  NumPy stands for numerical Python. It's the backbone of all kinds of
  scientific and numerical computing in Python.
---

# Numpy

## Datatypes and Attributes

Important to remember the main type in NumPy is `ndarray`, even seemingly different kinds of arrays are still `ndarray`'s. This means an operation you do on one array, will work on another.

```python
# NumPy's main datatype is ndarray
import numpy as np

a1 = np.array([1, 2, 3])

# Declaring an array with 2 dimension
a2 = np.array([[1, 2.0, 3.3],
               [4, 5, 6.5]])

# Declaring an array with 3 dimension
a3 = np.array([[[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]],
               [[10, 11, 12],
                [13, 14, 15],
                [16, 17, 18]]])
```



<table><thead><tr><th width="193">Code</th><th>Description</th></tr></thead><tbody><tr><td>a1.shape</td><td>Shape of the ndarray, (rows|columns|dimensions)</td></tr><tr><td>a2.dtype</td><td>data type of the ndarray</td></tr><tr><td>a3.size</td><td>Number of elements in array</td></tr><tr><td>a3.ndim</td><td>dimension of ndarray</td></tr></tbody></table>

## Creating Arrays

#### Create a new array

```python
sample_array = np.array([1, 2, 3])
```

#### Create a new array of n-dim filled with random values

<pre class="language-python"><code class="lang-python"><strong># Array of 1 with 2 dimension 3 row and 4 columns
</strong><strong>ones = np.ones((2, 3, 4)) #Provide the shape
</strong><strong>
</strong># Array of 0 with 2 dimension 3 row and 4 columns
zeros = np.zeros((2, 3, 4))

# Create an array within a range of values
range_array = np.arange(0, 10, 2) # start at 0 end at 10 and skip by 2 ([0, 2, 4...])

# Random array where maxNum = 10
random_array = np.random.randint(10, size=(5, 3)) # Size = shape of the array

# Random array of floats (between 0 &#x26; 1)
np.random.random((5, 3))
</code></pre>

### Consistent pseudo-random numbers with seed

NumPy uses pseudo-random numbers, which means, the numbers look random but aren't really, they're predetermined.

For consistency, you might want to keep the random numbers you generate similar throughout experiments.

To do this, you can use [`np.random.seed()`](https://docs.scipy.org/doc/numpy-1.15.0/reference/generated/numpy.random.seed.html)

```python
# Set random seed to 0
np.random.seed(0)

# Make 'random' numbers
np.random.randint(10, size=(5, 3))
```

## View Arrays and Matrices

Array shapes are always listed in the format `(row, column, n, n, n...)` where `n` is optional extra dimensions.

### View a small portion

```python
# Get the first 2 values of the first 2 rows of both arrays
a3[:2, :2, :2]
a3

array([[[ 1,  2],
        [ 4,  5]],
        
       [[10, 11],
        [13, 14]]])
```

## Arithmatic Operations

```python
a1 = np.ones((3,3))

np.random.seed(3)
a2 = np.random.randint(0, 100, size=(3,3))
a2
array([[24,  3, 56],
       [72,  0, 21],
       [19, 74, 41]])

```

### Addition & Subtraction

<pre class="language-python"><code class="lang-python"><strong># Addition of 2 matrices with same shape
</strong><strong>a1 + a2
</strong>array([[25.,  4., 57.],
       [73.,  1., 22.],
       [20., 75., 42.]])

# Subtraction of 2 matrices with same shape 
a1 - a2
array([[-23.,  -2., -55.],
       [-71.,   1., -20.],
       [-18., -73., -40.]])
</code></pre>

### Multiplication and Division

```python
np.random.seed(3)
a2 = np.random.randint(0, 10, size=(2,3))
a3 = np.random.randint(0, 10, size=(2,3))
a2, a3

(array([[8, 9, 3],
        [8, 8, 0]]),
 array([[5, 3, 9],
        [9, 5, 7]]))
```

**Note:** Different number of dimensions (2, 3) vs. (2, 3, 3) <mark style="color:red;">will throw Error</mark>

```python
# Multiplication
a2 * a3
array([[40, 27, 27],
       [72, 40,  0]])
       
# Division
a2 / a3
array([[1.6       , 3.        , 0.33333333],
       [0.88888889, 1.6       , 0.        ]])

# Divide and floor (roundoff)
a2//a3
array([[1, 3, 0],
       [0, 1, 0]])

# Modulo
a2 % a3
array([[3, 0, 3],
       [8, 3, 0]])

# Square
a2 ** 2
array([[64, 81,  9],
       [64, 64,  0]])
```

## Dot Product (Matrices)

```python
np.dot(a1, a2.T) #Shape (2,3) & (3,2) will produce (2,2)
array([[20., 16.],
       [20., 16.]])
```

### Statistics

**What's mean?**

Mean is the same as average. You can find the average of a set of numbers by adding them up and dividing them by how many there are.

**What's standard deviation?**

[Standard deviation](https://www.mathsisfun.com/data/standard-deviation.html) is a measure of how spread out numbers are.

**What's variance?**

The [variance](https://www.mathsisfun.com/data/standard-deviation.html) is the averaged squared differences of the mean.

```python
# Find the mean
np.mean(a2)

# Find the max
np.max(a2)

# Find the standard deviation
np.std(a2)

# Find the variance
np.var(a2)

# The standard deviation is the square root of the variance
np.sqrt(np.var(a2))

# Demo of variance
high_var_array = np.array([1, 100, 200, 300, 4000, 5000])
low_var_array = np.array([2, 4, 6, 8, 10])

np.var(high_var_array), np.var(low_var_array)
```

## Reshape and Transpose

### Reshape

```python
a2
array([[1. , 2. , 3.3],
       [4. , 5. , 6.5]])
       
a2.shape
(2, 3)

a2.reshape(2, 3, 1)
```

### Transpose

```
a2.T
```

## Comparision

```
a1
array([1, 2, 3])

a2
array([[1. , 2. , 3.3],
        [4. , 5. , 6.5]])
```

```
a1 > a2
array([[False, False, False], [False, False, False]])
```

```
a1 >= a2
array([[ True,  True, False],
        [False, False, False]])
```

```
a1 > 5
array([False, False, False])
```

```
a1 == a2
array([[ True,  True, False],[False, False, False]])
```

## Sort array

* [`np.sort()`](https://numpy.org/doc/stable/reference/generated/numpy.sort.html) - sort values in a specified dimension of an array.
* [`np.argsort()`](https://numpy.org/doc/stable/reference/generated/numpy.argsort.html) - return the indices to sort the array on a given axis.
* [`np.argmax()`](https://numpy.org/doc/stable/reference/generated/numpy.argmax.html) - return the index/indicies which gives the highest value(s) along an axis.
* [`np.argmin()`](https://numpy.org/doc/stable/reference/generated/numpy.argmin.html) - return the index/indices which gives the lowest value(s) along an axis.
