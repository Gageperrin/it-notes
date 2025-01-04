# Linear Algebra

These are notes compiled from Khan Academy's course on [Linear Algebra](https://www.khanacademy.org/math/linear-algebra).

## Vectors and Spaces

### Vectors

A vector has both **magnitude** and **direction**. Vectors can be used to represent $n$-dimensional space, with $n$ being the number of components in the vector.

Vectors are often notated with either **bold** characters or with a horizontal arrow above the letter.

For example, the vector $\mathbf{v}$ can be written as:

$$
\mathbf{v} = (5, 0)
$$

or in column form:

$$
\mathbf{v} = 
\begin{bmatrix}
5 \\ 
0
\end{bmatrix}
$$

#### Real Coordinate Space

Real coordinate space ($\mathbb{R}^n$) is the set of all $n$-tuples of real numbers. Each element in this set is a vector with $n$ components (i.e., dimensions), where each component is a real number.

A tuple is an ordered list of numbers. A 2-tuple is an ordered list of two numbers. This is how a two-dimensional vector is represented.

All the combined possible 2-tuples would then be 2-dimensional real coordinate space, $\mathbb{R}^2$.

A vector in 3-dimensional space would be:

$$
\mathbf{v} = 
\begin{bmatrix}
x_1 \\ 
x_2 \\ 
x_3
\end{bmatrix} \in \mathbb{R}^3
$$

To be real-valued, a tuple must have *real* numbers in its ordered list.

Real coordinate space can be extended to *n* dimensions.

#### Adding Vectors

To add two vectors, we add their corresponding components. If we have two vectors, $\mathbf{v}$ and $\mathbf{w}$, their sum is:

$$
\mathbf{v} = 
\begin{bmatrix}
v_1 \\ 
v_2 \\ 
v_3
\end{bmatrix}, \quad
\mathbf{w} = 
\begin{bmatrix}
w_1 \\ 
w_2 \\ 
w_3
\end{bmatrix}
$$

The sum of the vectors is:

$$
\mathbf{v} + \mathbf{w} = 
\begin{bmatrix}
v_1 + w_1 \\ 
v_2 + w_2 \\ 
v_3 + w_3
\end{bmatrix}
$$

Thus, vector addition is performed through the components.

#### Multiplying a Vector by a Scalar

When multiplying a vector by a scalar (a real number), we multiply each component of the vector by the scalar. If $\mathbf{v}$ is a vector and $c$ is a scalar, then:

$$
\mathbf{v} = 
\begin{bmatrix}
v_1 \\ 
v_2 \\ 
v_3
\end{bmatrix}
$$

Multiplying $\mathbf{v}$ by the scalar $c$ results in:

$$
c\mathbf{v} = 
\begin{bmatrix}
c v_1 \\ 
c v_2 \\ 
c v_3
\end{bmatrix}
$$

This operation scales the vector by the factor $c$, which can stretch or shrink the vector while keeping its direction unchanged (if $c$ is positive) or reversing it (if $c$ is negative).
