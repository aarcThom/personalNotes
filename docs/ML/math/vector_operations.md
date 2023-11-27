# Vector Operations

---

## Transpose Operation

You can transpose a column vector to a row vector and vice versa. This operation is denoted with a superscript *T*.
$$
[4,2,0]^T = 
\begin{bmatrix}
4, \\\ 
2,\\\ 
0 
\end{bmatrix}
$$

$$
\begin{bmatrix} 
3, \\\ 
-2,\\\
13 
\end{bmatrix}^T
= [3, -2, 13]
$$

### NumPy Implementation

You simply use `.T` to transpose a vector.

```python
v1 = np.array([[2, 3, 4]]) # row vector
v2 = v1.T # Column vector
print(v2)

>> array([[2],
>>       [3],
>>       [4]])
```



---

## Vector Addition and Subtraction

Algebraically, vector addition and subtraction happens element-wise. Vectors need to be the same dimension.

 
$$
[4, 5, 1, 0] + [-4, -3, 3, 10] = [0, 2, 4, 10]
$$

$$
\begin{bmatrix} 
4, \\\
2,\\\
0
\end{bmatrix}
-
\begin{bmatrix} 
6, \\\
-4,\\\
-60
\end{bmatrix}
+
\begin{bmatrix} 
2, \\\
-5,\\\
40
\end{bmatrix}
=
\begin{bmatrix} 
0, \\\
1,\\\
100
\end{bmatrix}
$$


Geometrically, you can add up vectors by *tip to tail* and subtract vectors by multiplying one vector by -1 before adding up *tip to tail*. In the below image, we are adding vector A to vector B.

![](assets/tip-to-tail.png)

### NumPy Implementation

```python
v1 = np.array([[3,2,1]])
v2 = np.array([[4,-8,12]])
v3 = v2 + v1
print(v3)

>> [[ 7 -6 13]]
```



---

## Vector Scalar Multiplication

When you multiply a vector by a scalar, you multiply each attribute by the scalar.
$$
3 \times [2, 4, 5] = [6, 12, 15]
$$
Geometrically, this changes the length but not the direction of the vector.

### NumPy Implementation

```python
v1 = np.array([[3, 4, 5]])
v2 = v1 * 3
print(v2)

>> [[ 9 12 15]]
```

---

## Vector Dot Product

The dot product is **extremely important for neural networks**. Each *unit* within a layer will output a value by first taking the *dot product* between the *weights* and *input*, adding a *bias* and then applying an *activation function*.

The dot product is a representation of *how similar two vectors are*. For a good explainer, using solar panels and Mario-Kart as an example, see [this link.](https://betterexplained.com/articles/vector-calculus-understanding-the-dot-product/)

### Denotation

The dot product can be denoted in a few different ways, but the two I've encountered the most are:
$$
{\textbf a} ~ \cdot ~ {\textbf b}
$$
or
$$
{\textbf a}^T{\textbf b}
$$

### Calculation

To compute the product, you multiply corresponding attributes of equal dimension vectors, and then take the sum.
$$
[1,4,7,7]~\cdot~[2,4,3,2]~=~ 1\times2~+~4\times4~+7\times3~+7\times2~=~53
$$

### NumPy Implementation

```python
v1 = np.array([3,2,1])
v2 = np.array([4,-8,12])
v3 = np.dot(v1, v2)
print(v3)

>> 8
```

