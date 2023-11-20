# Tensors

---

## Scalar - Rank-0 Tensor

### Definition and Explanation

A simple numeric value such as *3* or *-4.5*. A **scalar** is a *rank-0 tensor* - i.e. it is a tensor with 0 dimensions - i.e. a tensor that contains only one number. They are called *scalars* because they *scale* vectors and matrices without changing the vector or matrix's direction. 

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

### Definition and Explanation

A **vector** is an array of numbers. It is a rank-1 tensor in that it has one axis. Geometrically,  a vector is a line defined by a *magnitude* (length) and direction. The dimensionality of the vector, is also the dimensionality of the coordinate system used to represent the vector. For instance, a 2 dimensional vector $[1,2]$ could be represented in a standard 2D XY grid.

The definition of a vector does not include its starting point. In other words, the vector $[1,2]$ indicates a positive direction (right 1 unit, up 2 units), and a magnitude ($\sqrt{5}$). This direction and magnitude tells use the location of the *head* (think of it as the arrow head) from the *tail* of the vector. However, the tail could start at any coordinate. A vector with its *tail* at the origin is said to start in the *standard position*.

The tensor is said to be *one-dimensional.* The vector itself will also have dimensionality. For instance a point $[2,3,0]$ is said to be a *3-dimensional vector but a 1-dimensional tensor*.

The list of scalar values contained within a vector are called **attributes**.

### Denotation

Denoted by a bold letter. The vector attributes are enclosed in square brackets.

${\bf a} = [0,1,-10]$  

${\bf b} = [-24.2,0,15,18.2]$

To reference a specific attribute within a vector, you use an italic value of the vector with an index like: ${\it a}^{(x)}$. So for the examples above, we could reference the respective first and last attributes like so:

${\it a}^{(0)} = 0$

${\it b}^{(3)} = 18.2$

### NumPy / Python Implementation

In NumPy, a `np.array` of an *array* of scalar values is a vector.

```python
>>> x = np.array([1,2,3,-77,14])
>>> x
>>> array([1,2,3,-77,14])
>>> x.ndim
>>> 1
```

### Vector Orientation

$$
Column \ Vectors:
\begin{bmatrix} 2, \\ 3,\\ 4 \end{bmatrix},
\begin{bmatrix} 0, \\ -1\end{bmatrix}
$$ {  \}


$$
Row \ Vectors: [2,3,4], [0,-1]
$$ {  \}
