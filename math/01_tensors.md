# Tensors

---

## Scalar - Rank-0 Tensor

### Definition

A simple numeric value such as *3* or *-4.5*. A **scalar** is a *rank-0 tensor* - i.e. it is a tensor with 0 dimensions - i.e. a tensor that contains only one number.

### Denotation

Denoted by an italic letter.

${ \it a} = 34$  

${\it b} = -24.2$

### NumPy / Python Implementation

In NumPy, a `np.array` of a single `float32` or `float64` is a scalar tensor.

```python
>>> x = np.array(12)
>>> x
>>> array(12)
>>> x.ndim
>>> 0
```

---

## Vector - Rank-1 Tensor

### Definition

A **vector** is an array of numbers. It is a rank-1 tensor in that it has one axis. The tensor is said to be *one-dimensional.* The vector itself will also have dimensionality. For instance a point $[2,3,0]$ is said to be a *3-dimensional vector but a 1-dimensional tensor*.

The list of scalar values contained within a vector are called **attributes**.

In architectural practice and 3D modelling, we are used to working with 3-dimensional points and vectors.

### Denotation

Denoted by a bold letter. The vector attributes are enclosed in square brackets.

${\bf a} = [0,1,-10]$  

${\bf b} = [-24.2,0,15,18.2]$

### NumPy / Python Implementation

In NumPy, a `np.array` of an *array* of scalar values is a vector.

```python
>>> x = np.array([1,2,3,-77,14])
>>> x
>>> array([1,2,3,-77,14])
>>> x.ndim
>>> 1
```



