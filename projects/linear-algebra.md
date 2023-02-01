---
layout: home2
permalink: /projects/linear-algebra/
title: Linear Algebra 
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-1-23
comments: false
---


### Vector Length

The **vector length** of an arbitrary vector $\textbf{v}$ given by $|\textbf{v}| = \sqrt{v_1^2 + ... + v_n^2}$.

### Unit Vector

A **unit vector** is a vector of length one.

Ex 1: Find a unit vector in the direction of $\textbf{v} = (-1,5,2)$.

We can compute the length of $\textbf{v}$ with $|\textbf{v}| = \sqrt{-1^2 + 5^2 + 2^2} = \sqrt{1 + 25 + 4} = \sqrt{30}$. Put $\textbf{u} = \frac{\textbf{v}}{|\textbf{v}|} = (\frac{-1}{\sqrt{30}}, \frac{5}{\sqrt{30}}, \frac{2}{\sqrt{30}})$. Now, $\textbf{u}$ is a unit vector in the direction of $\textbf{v}$.

### Matrix Multiplication

Without loss of generality, matrix multiplication functions in the following format. \\[\begin{pmatrix} \text{ row 1 } \\ \text{ row 2 } \end{pmatrix} \begin{pmatrix} \mid & \mid & \mid \\ \text{ col 1} &\text{col 2} &\text{col 3 } \\ \mid & \mid & \mid\end{pmatrix} = \begin{pmatrix} \text{ row 1 } \cdot \text{ col 1 } & \text{ row 1 } \cdot \text{ col 2 } & \text{ row 1 } \cdot \text{ col 3 } \\  \text{ row 2 } \cdot \text{ col 1 } & \text{ row 2 } \cdot \text{ col 2 } & \text{ row 2 } \cdot \text{ col 3 }\end{pmatrix}\\]Ex 1: Let $A = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix}$ and $\textbf{v} = \begin{pmatrix} -1 \\ 5 \\ 2 \end{pmatrix}$. Calculate $A \textbf{v}$. \\[A\textbf{v} = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix} \begin{pmatrix} -1 \\ 5 \\ 2 \end{pmatrix} = \begin{pmatrix} 1 \cdot -1 + 2 \cdot 5 + 3 \cdot 2 \\ 4 \cdot -1 + 5 \cdot 5 + 6 \cdot 2 \end{pmatrix} = \begin{pmatrix} -1 + 10 + 6 \\ -4 + 25 + 12 \end{pmatrix} = \begin{pmatrix} 15 \\ 33 \end{pmatrix}\\]
### Gaussian Elimination

**Gaussian elimination**, also known as row reduction, is an algorithm for solving systems of linear equations. Examples of this algorithm consist of the following.

Ex 1: Solve the matrix equation $A\textbf{x} = \begin{pmatrix} 1 & 2 & 4 \\ 0 & 1 & 5 \\ -2 & -4 & -3 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} -2 \\ 2 \\ 9 \end{pmatrix} = \textbf{v}$.

Put $A$ and $\textbf{v}$ into an augmented matrix. Using Gaussian elimination, put $\text{row 3} = \text{row 3} + 2 \cdot \text{row 1}$: \\[\left[ \begin{array}{ccc|c} 1 & 2 & 4 & -2\\ 0 & 1 & 5 & 2 \\ -2 & -4 & -3 & 9\end{array} \right] \to \left[ \begin{array}{ccc|c} 1 & 2 & 4 & -2\\ 0 & 1 & 5 & 2 \\ 0 & 0 & 5 & 5\end{array} \right]\\]Now, since we have an upper triangular matrix, we can write: \\[\begin{flalign*} 1 \cdot x_1 + 2 \cdot x_2 + 4 x_3 &= -2 \\ x_2 + 5 x_3 &= 2 \\ 5x_3 &= 5 \end{flalign*}\\]This implies that $x_3 = 1$. So: $x_2 + 5(1) = 2 \implies x_2 = -3$. Then: $x_1 + 2 \cdot -3 + 4 \cdot 1 = -2 \implies x_1 - 6 + 4 = -2 \implies x_1 = 0$. So: $(x_1, x_2, x_3) = (0,-3,1)$.

### PA = LU

The **PA = LU** factorization method is a well-known numerical method for solving types of linear equations against multiple input.

Ex 1: Find a permutation matrix $P$, a lower triangular matrix $L$, and an upper triangular matrix $U$ such that $PA = LU$, where $A = \begin{pmatrix} 1 & 0 & 1 \\ 2 & 2 & 2 \\ 3 & 4 & 5  \end{pmatrix}$.
Now, using a Gaussian elimination: \\[\begin{pmatrix} 1 & 0 & 1 \\ 2 & 2 & 2 \\ 3 & 4 & 5  \end{pmatrix} \to \begin{pmatrix}  1 & 0 & 1 \\ 0 & 2 & 0 \\ 3 & 4 & 5 \end{pmatrix} \to \begin{pmatrix} 1 & 0 & 1 \\ 0 & 2 & 0 \\ 0 & 4 & 2  \end{pmatrix} \to \begin{pmatrix}  1 & 0 & 1 \\ 0 & 2 & 0 \\ 3 & 4 & 5 \end{pmatrix} \to \begin{pmatrix} 1 & 0 & 1 \\ 0 & 2 & 0 \\ 0 & 0 & 2 \end{pmatrix} = U\\]So: \\[P = \begin{pmatrix}  1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} \text{, } \text{ } \text{ } \text{ } \text{ } \text{ } \text{ } E = E_{32}(2)E_{31}(3) E_{21}(2) = \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & -2 & 1\end{pmatrix} \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -3 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} \text{, } \text{ } \text{ } \text{ } \text{ } \text{ } \text{ } L = E^{-1} = E_{21}(-2) E_{31}(-3) E_{32}(-2) = \begin{pmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 3 & 0 & 1 \end{pmatrix}  \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 2 & 1\end{pmatrix} = \begin{pmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 3 & 2 & 1\end{pmatrix}\\]This implies that: \\[\begin{flalign*} PA &= LU \\  \begin{pmatrix}  1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 0 & 1 \\ 2 & 2 & 2 \\ 3 & 4 & 5  \end{pmatrix} &= \begin{pmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 3 & 2 & 1\end{pmatrix} \begin{pmatrix} 1 & 0 & 1 \\ 0 & 2 & 0 \\ 0 & 0 & 2 \end{pmatrix} \\  \begin{pmatrix} 1 & 0 & 1 \\ 2 & 2 & 2 \\ 3 & 4 & 5  \end{pmatrix}  &= \begin{pmatrix} 1 & 0 & 1 \\ 2 & 2 & 2 \\ 3 & 4 & 5  \end{pmatrix} \end{flalign*}\\]
### Matrix Transpose

The **transpose** of a matrix $A$ is given by \\[(A^T)_{ij} = A_{ji}\\]Ex 1: If $A = \begin{pmatrix} 1 & 2 & 3 \\ 0 & 0 & 4 \end{pmatrix}$, then $A^T = \begin{pmatrix} 1 & 0 \\ 2 & 0 \\ 3 & 4 \end{pmatrix}$.

The **transpose** of $A +B$ is $A^T + B^T$. The **transpose** of $AB$ is $(AB)^T = B^TA^T$. The **transpose** of $A^{-1}$ is $(A^{-1})^T = (A^T)^{-1}$.

### Matrix Inverse

The **inverse** of a matrix is denoted $A^{-1}$. It has the properties: $AA^{-1} = I$ and $A^{-1}A = I$.

Without loss of generality, the inverse product of two matrices $A,B$ is given by: \\[(AB)^{-1} = B^{-1}A^{-1}\\]A matrix is invertible if and only if it has independent columns/full rank.

A 2 by 2 matrix is invertible if and only if $ad - bc \neq 0$, as such: \\[\begin{pmatrix} a & b \\ c & d \end{pmatrix}^{-1} = \frac{1}{ad-bc}\begin{pmatrix} d & -b \\ -c & a\end{pmatrix}\\]Generally, the inverse of a matrix $A$ can be computed by creating an augmented matrix of the form $\begin{pmatrix} A & I\end{pmatrix}$. Using Gaussian elimination to row reduce $A$ to $I$ by performing row operations on both sides yields $\begin{pmatrix} I & A^{-1} \end{pmatrix}$. Note that the zero matrix is not invertible.

Ex: Can a square matrix with two identical rows be invertible?

No. An invertible matrix must have full rank. Two identical rows implies that this matrix is not full rank, since each column is not independent.

### Linear Independence

Vectors $\textbf{v}_1, ..., \textbf{v}_k$ are **linearly independent** if the only zero combination $c_1\textbf{v}_1 + ... + c_k\textbf{v}_k = 0$ has all $c_i = 0$. \\[\begin{pmatrix} 1 \\ 3 \\ 0\end{pmatrix} \text{ and } \begin{pmatrix} 5 \\ 5 \\ 0\end{pmatrix} \text{ are linearly independent. } \begin{pmatrix} 1 \\ 0\end{pmatrix} \text{ and } \begin{pmatrix} 0 \\ 1\end{pmatrix} \text{ are linearly independent}\\]We can test if two vectors are **linearly independent** with the following.

Put $v_1 = (1,5,0)$ and $v_2 = (3,3,0)$. These vectors are **linearly independent** if $a(1,5,0) + b(3,3,0) = (0,0,0)$ implies $a = b = 0$. Put independent\\[(a + 3b, 5a + 3b, 0) = (0,0,0) \text{ so } a + 3b = 5a + 3b = 0 \implies 4a = 0 \implies a = 0 \implies b = 0\\]Therefore, $v_1, v_2$ are **linearly independent**.

The columns of an arbitrary matrix $A \subseteq \mathbb{R}^m$ are independent when the only solution to $A\textbf{x} = \textbf{0}$ is $\textbf{0}$. In other words, the columns of $A$ are independent if $N(A) = \{\textbf{0}\}$.

The columns of $A$ are independent exactly when $n = r$.

### Linear Dependence

Vectors $\textbf{v}_1, ..., \textbf{v}_k$ are **linearly dependent** if the there exists a zero combination $c_1\textbf{v}_1, ..., c_k\textbf{v}_k = 0$ such that not all $c_i = 0$ .

Any set of $n$ vectors in $\mathbb{R}^m$ must be linearly dependent if $n > m$.

### Rank

The **rank** of a matrix is the maximum number of linearly independent column vectors.

If $A \subseteq \mathbb{R}^m$ is invertible, then $A$ is full rank.

### Vector Space

A **vector space** is a collection of vectors together with the operation of vector addition $\vec{\textbf{v}} + \vec{\textbf{w}}$ and scalar multiplication $c\vec{\textbf{v}}$ such that

(1) $\vec{\textbf{v}} + \vec{\textbf{w}} = \vec{\textbf{w}} + \vec{\textbf{v}}$
(2) $\vec{\textbf{v}} + (\vec{\textbf{w}} + \vec{\textbf{x}}) = (\vec{\textbf{w}} + \vec{\textbf{v}}) + \vec{\textbf{x}}$
(3) There exists a unique $\vec{\textbf{0}}$ vector such that $\vec{\textbf{v}} + \vec{\textbf{0}} = \vec{\textbf{v}}$ for all $\vec{\textbf{v}}$.
(4) For all vectors $\vec{\textbf{v}}$, there exists a unique vector $-\vec{\textbf{v}}$ such that $\vec{\textbf{v}} + (-\vec{\textbf{v}}) = \vec{\textbf{0}}$.
(5) $1 \cdot \vec{\textbf{v}} = \vec{\textbf{v}}$.
(6) $c(d\vec{\textbf{v}}) = (cd)\vec{\textbf{v}}$
(7) $c(\vec{\textbf{v}} + \vec{\textbf{w}}) = c\vec{\textbf{v}} + c\vec{\textbf{w}}$
(8) $(c + d)\vec{\textbf{v}} = c\vec{\textbf{v}} + d\vec{\textbf{v}}$

The result of each of these operations must remain in the **vector space**.  

Any set of vectors from a vector space will span a subspace of that space.

Ex 1: Give an example of a vector space $\textbf{V}$ that has $\{(1,3,0), (5,5,0)\}$ as a basis (these component vectors are shown above to be a basis).   

The vector space $\textbf{V} = \bigg\{ c_1\begin{pmatrix} 1 \\ 3 \\ 0\end{pmatrix} + c_2 \begin{pmatrix} 5 \\ 5 \\ 0 \end{pmatrix} : c_1, c_2 \in \mathbb{R} \bigg\}$ has basis $\bigg\{ \begin{pmatrix} 1 \\ 3 \\ 0 \end{pmatrix}, \begin{pmatrix} 5 \\ 5 \\ 0 \end{pmatrix} \bigg\} = \{(1,3,0), (5,5,0)\}$.

Ex 2: Is $\mathbb{R}^2$ a subspace of $\mathbb{R}^3$? Briefly explain your answer.

No. $(1,0) \not\in \mathbb{R}^3$ since $\mathbb{R}^3$ only contains elements of the form $(a,b,c)$ with $a,b,c \in \mathbb{R}$.

### Vector Subspace

A **vector subspace** is subset of a vector space satisfying vector addition and scalar multiplication.

Ex 1: Consider the vector space $P_2$ of polynomials of degree at most two, with vector addition and scalar multiplication given as usual. Which of the following sets are subspaces of $P_2$? \\[\text{1. } \{ax^2 + bx + c \text{ } | \text{ } a \text{ is odd}\} \text{ and 2. } \{ax^2 + bx + c \text{ } | \text{ } b = a+c\}\\](1) Not a subspace. $x^2$ lies in this set, but $x^2 + x^2 = 2x^2$ does not. Thus, set is not closed under vector addition.
(2) Is a subspace:
* Suppose $ax^2 + bx + c$, $a^{\prime}x^2 + b^{\prime}x + c^{\prime}$ are in the set so that $b = a + c$, $b^{\prime} = a^\prime + c^\prime$. Then \\[(ax^2 + bx + c) + (a^{\prime}x^2 + b^{\prime}x + c^{\prime}) = (a + a^{\prime})x^2 + (b + b^{\prime})x + (c + c^{\prime})\\]and $b + b^\prime = (a + a^{\prime}) + (c + c^{\prime})$ as required so vector addition is satisfied.
* Suppose $r$ is a scalar. Then $r(ax^2 + bx + c) = rax^2 + rbx + rc$ and $rb = ra + rc = r(a+c)$ as required.

Ex 2: Let $P$ be a plane in $\mathbb{R}^3$ defined by $x + y - 2z = 4$. This plane $P$ does not contain $(0,0,0)$ and is therefore not a subspace. Find two vectors in $P$ whose sum is not in $P$.

We have: $\vec{v}_1 = (0,4,0), \vec{v}_2 = (4,0,0) \in P$. However, $\vec{v}_3 = \vec{v}_1 + \vec{v}_2 = (0,4,0) + (4,0,0) = (4,4,0) \not\in P$.

### Span

The **span** of $S \subseteq \mathbb{R}^m$ is all combinations of vectors of $S$. \\[\text{If } S = \bigg\{\begin{pmatrix} 1 \\ 1\end{pmatrix}, \begin{pmatrix} 3 \\ 2 \end{pmatrix} \bigg\} \text{ then Span}(S) = \bigg\{c\begin{pmatrix} 1 \\ 1\end{pmatrix} + d\begin{pmatrix} 3 \\ 2 \end{pmatrix} : c,d \in \mathbb{R} \bigg\}\\]Similarly, one can find the **span** of the columns of a matrix as in the following. \\[\text{If } A = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} \text{ then the span of the columns of }A \text{ is } \bigg\{ c \begin{pmatrix} 1 \\ 0\end{pmatrix} + d \begin{pmatrix} 0 \\ 1\end{pmatrix} : c,d \in \mathbb{R}\bigg\} = C(A)\\]Notice that the **span** of the columns of the matrix is equivalent to the column space of the matrix.

By definition, to show that vectors - say $(2,1)$ and $(4,3)$ - span a vector space - say $\mathbb{R}^2$ - we need to prove the following without loss of generality.

$\forall  \text{ } \textbf{v} = (a,b) \in \mathbb{R}^2$, there exists $x,y \in \mathbb{R}$ such that $\textbf{v} = x(2,1) + y(4,3)$. This is true $\iff \begin{cases} 2x + 4y &= a \\ x + 3y &= b\end{cases}$  has a solution for any $a,b \in \mathbb{R}$.

Ex: When do 10 vectors span $\mathbb{R}^5$?

10 vectors span $\mathbb{R}^5$ when 5 of the vectors are linearly independent.

### Basis

The **basis** for a vector space is a sequence of vector that are (1) linearly independent and (2) span the space. For example: \\[\text{The basis of }\mathbb{R}^2 \text{ (all vectors of the form $\begin{pmatrix} a\\ b\end{pmatrix}$) is } \bigg\{\begin{pmatrix} 1 \\ 0\end{pmatrix}, \begin{pmatrix} 0 \\ 1 \end{pmatrix}\bigg\} \text{. The basis of all $2 \times 2$ matricies is }\bigg\{\begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix} \bigg\} \\]The **basis** of the column space of a matrix is the set containing the matrix's pivot columns after reducing to the matrix RREF form. \\[\text{The basis of the column space of the RREF matrix }R_0 = \begin{pmatrix} 1 & 3 & 0 & 0 & 5 \\ 0 & 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 & 3 \end{pmatrix}, \text{ is given by } \bigg\{ \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}, \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix}, \begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix}\bigg\}\\]The vectors $\textbf{v}_1,...,\textbf{v}_n$ are a basis for $\mathbb{R}^n$ exactly when they are the columns of an $n \times n$ invertible matrix. Thus, $\mathbb{R}^n$ has infinitely many different bases. When the columns are dependent, we keep only the pivot columns. These columns are independent and span the column space.

Every set of independent vectors can be extended to a basis. Every spanning set of vectors can be reduced to a basis.

For instance, given five vectors in $\mathbb{R}^7$ we find a basis for the space they span with the following. Put the five vectors into columns of a matrix $A$. Eliminate to find the pivot columns. These pivot columns are a basis for the column space.

Note: all bases for a vector space contain the same number of vectors.

Ex: Find the standard basis for all symmetric matrices in $\mathbb{R}^{2 \times 2}$. \\[\bigg\{ \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix}  \bigg\}\\]Ex: Show that the invertile $2 \times 2$ matrices span $\mathbb{R}^{2 \times 2}$.

The standard basis for $\mathbb{R}^{2 \times 2}$ is \\[\bigg\{ \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 1\\ 0 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 0 \\ 0 & 1\end{pmatrix}  \bigg\}\\]Notice that each element the standard basis is not invertible since each contains a column of zeros. However, there are infinite bases for $\mathbb{R}^{2 \times 2}$. Another basis for $\mathbb{R}^{2 \times 2}$ is \\[\bigg\{ \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}, \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}, \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \begin{pmatrix} 0 & 1 \\ -1 & 0 \end{pmatrix}\bigg\}\\]Notice that each element of this basis has independent columns. This just so happens to be the basis for all invertible matrices.

Ex: Find a basis for the space of polynomials $p(x)$ of degree $\leq 3$. Find a basis for the subspace $p(1) = 0$.

The standard basis for all polynomials of degree $\leq 3$ is $\{x^3,x^2, x, 1\}$. With the condition $p(1) = 0$, it is implied that \\[\begin{flalign*} p(x) &= ax^3 + bx^2 + cx + d \\ p(1) &= a + b + c + d = 0 \\ d &= -a-b-c\end{flalign*}\\]Therefore, basis is $\{x^3-1,x^2-1,x-1\}$.

### Dimension

The **dimension** of a vector space is the size/cardinality of the basis.

Let $V$ be a vector space whose dimension is finite and $W$ be a subspace of $V$. Then $\dim W \leq \dim V$.

Proof. The dimension of a vector space is the size of the basis. Put $m = \dim W$ and $n = \dim V$. Let $(w_1,...,w_n)$ be the basis for $W$ and $(v_1,...,v_n)$ be the basis for $V$. Then, elements of a basis must all be linearly independent so each $w_i \in (w_1,...,w_n)$ is linearly independent. Also, for all $i$, $w_i \in W \subseteq V$. So, $w_i \in V$ for all $i$. So, we can extend the basis of $W$ to the basis of $V$. Thus $n \geq m$ must be true because $(w_1,...,w_n) \subseteq (v_1,...,v_n)$, so the cardinality of the basis $W$ must be less than or equal to the cardinality of the basis $V$. $\square$

### Four Fundamental Subspaces

The **four fundamental subspaces** of an $m \times n$ matrix $A$ with rank $r$ are:
 
(1) *row space* denoted $C(A^T)$ with dim. $r$
(2) *column space* denoted $C(A)$ with dim. $r$
(3) *nullspace* denoted $N(A)$ with dim. $n - r$
(4) *left nullspace* denoted $N(A^T)$ with dim. $m -r$.

To find the *column space* and the *nullspace*, reduce $A$ to RREF, denoted $R_0$. Then, the pivot columns from $R_0$ are the independent columns from $A$ which span the *column space*. These columns can also be used to find the *nullspace* by multiplying the RREF matrix derived from $A$ by $\textbf{x}$ and setting it equal to $\textbf{0}$. Solve for the variables corresponding to the pivot columns (columns that are lin. indep.). Then write $\textbf{x}$ in terms of the pivot variables to find the nullspace. The cases for *row space* and *left nullspace* are similar, but involve reducing $A^T$ to RREF form.

### Column Space

The **column space** of $A$, denoted $C(A)$, consists of all linear combinations of the columns of $A$. \\[\text{If } A = \begin{pmatrix} 1 & 0 \\ 0 & 1 \\ 2 & 0 \end{pmatrix} \text{ then } C(A) = \bigg\{ c_1 \begin{pmatrix} 1 \\ 0 \\ 2\end{pmatrix} + c_2 \begin{pmatrix} 0 \\ 1 \\ 0\end{pmatrix} : c_1, c_2 \in \mathbb{R} \bigg\}\\]
Ex 1: What is the column space of the matrix $A = \begin{pmatrix} 0 & 1 & 0 & 0 \\ -1 & 0 & 2 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$? Is the vector $\textbf{v} = \begin{pmatrix} -1 \\ 5 \\ 2 \end{pmatrix}$.

Observe $-2 \cdot (0,-1,0) = (0,2,0)$. As such, $A$ has 3 independent columns and its column space is all of $\mathbb{R}^3$, i.e. $C(A) = \bigg\{ c_1 \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} + c_2\begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} + c_3\begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix} : c_1, c_2, c_3 \in \mathbb{R} \bigg\}$. Hence, we must have that $\textbf{v} \in C(A)$.

Ex 2: Give examples of matrices whose column space is the vector space $\textbf{V} = \bigg\{ c_1\begin{pmatrix} 1 \\ 3 \\ 0\end{pmatrix} + c_2 \begin{pmatrix} 5 \\ 5 \\ 0 \end{pmatrix} : c_1, c_2 \in \mathbb{R} \bigg\}$.

Two examples are $\begin{pmatrix} 1 & 5 \\ 3 & 5 \\ 0 & 0\end{pmatrix}$ and $\begin{pmatrix} 1 & 0 \\ 0 & 1 \\ 0 & 0 \end{pmatrix}$.

### Row Space

The **row space** of $A$, denoted $C(A)$, consists of all linear cominations of the rows of $A$. \\[\text{If } A = \begin{pmatrix} 1 & 0 \\ 0 & 1 \\ 2 & 0 \end{pmatrix} \text{ then } C(A^T) = \bigg\{ c_1 \begin{pmatrix} 1 & 0 \end{pmatrix} + c_2 \begin{pmatrix} 0 & 1 \end{pmatrix} : c_1, c_2 \in \mathbb{R} \bigg\}\\]
### Nullspace

The **nullspace** of a $m \times n$ matrix $A$, denoted $N(A)$, is the set of all solutions to $A\textbf{x} = \textbf{0}$. So, if $\textbf{v} \in N(A)$ then $A\textbf{v} = \textbf{0}$.

Ex 1: Give an example of a matrix whose nullspace is $\textbf{V} = \bigg\{ c_1\begin{pmatrix} 1 \\ 3 \\ 0\end{pmatrix} + c_2 \begin{pmatrix} 5 \\ 5 \\ 0 \end{pmatrix} : c_1, c_2 \in \mathbb{R} \bigg\}$.

The nullspace of $A = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 1\end{pmatrix}$ is $\textbf{V}$. This is because of the following. Let $\vec{v} \in \textbf{V}$ be arbitrary. Then $\vec{v} = (a,b,0)$ for some $a,b \in \mathbb{R}$. Performing matrix multiplication on $A$ and $\vec{v}$ will always yield the zero vector $\textbf{0}$ as such: \\[\begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 1\end{pmatrix}\vec{v} = \textbf{0}\\]
### Left Nullspace

The **left nullspace** of a $m \times n$ matrix $A$, denoted $N(A^T)$, is the set of all solutions to $A^T\textbf{x} = \textbf{0}$. So, if $\textbf{v} \in N(A^T)$ then $A^T\textbf{v} = \textbf{0}$.

### Complete Solution to Ax = b

Determine all solutions to the equation $A\textbf{x} = \textbf{b}$ where \\[A = \begin{pmatrix} 2 & 4 & 6 & 4 \\ 2 & 5 & 7 & 6 \\ 2 & 3 & 5 & 2 \end{pmatrix} \text{, } \textbf{ x} = \begin{pmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{pmatrix} \text{, } \textbf{ b} = \begin{pmatrix} 4 \\ 3 \\ 5 \end{pmatrix} \\]First we must combine $A$ and $\textbf{b}$ in an augmented matrix and reduce this matrix to RREF form with the following gaussian elimination. \\[\begin{flalign*} \begin{pmatrix} 2 & 4 & 6 & 4 & 4 \\ 2 & 5 & 7 & 6 & 3 \\ 2 & 3 & 5 & 2 & 5\end{pmatrix} &\to \begin{pmatrix} 2 & 3 & 5 & 2 & 5 \\ 2 & 4 & 6 & 4 & 4 \\ 2 & 5 & 7 & 6 & 3 \end{pmatrix} \to \begin{pmatrix} 2 & 3 & 5 & 2 & 5 \\ 0 & 1 & 1 & 2 & -1 \\ 0 & 2 & 2 & 4 & -2 \end{pmatrix} \to \begin{pmatrix} 2 & 3 & 5 & 2 & 5 \\ 0 & 1 & 1 & 2 & -1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix} \to \begin{pmatrix} 2 & 2 & 4 & 0 & 6 \\ 0 & 1 & 1 & 2 & -1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix}\to \begin{pmatrix} 1 & 1 & 2 & 0 & 3 \\ 0 & 1 & 1 & 2 & -1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix} \to \begin{pmatrix} 1 & 0 & 1 & -2 & 4 \\ 0 & 1 & 1 & 2 & -1 \\ 0 & 0 & 0 & 0 & 0 \end{pmatrix}  \end{flalign*} \\]Notice that the first and second columns are the pivot columns. Now... \\[\begin{aligned} x_1 + x_3 + -2x_4 &= 4 \\ x_2 + x_3 + 2x_4 &= -1 \end{aligned} \implies \begin{pmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{pmatrix} = \begin{pmatrix} -x_3 + 2x_4 + 4 \\ -x_3 - 2x_4 - 1 \\ x_3 \\ x_4 \end{pmatrix} = \underbrace{x_3 \begin{pmatrix} -1 \\ -1 \\ 1 \\ 0 \end{pmatrix} + x_4 \begin{pmatrix} 2 \\ -2 \\ 0 \\ 1\end{pmatrix}}_{\textbf{x}_n} + \underbrace{\begin{pmatrix} 4 \\ -1 \\ 0 \\ 0 \end{pmatrix}}_{\textbf{x}_p} \\]This is the complete solution $\textbf{x} = \textbf{x}_n + \textbf{x}_p$. The nullspace is given by $\textbf{x}_n$ as such:  \\[\bigg\{x_3\begin{pmatrix} -1 \\ -1\\ 1 \\ 0 \end{pmatrix}  +x_4 \begin{pmatrix} 2 \\ -2\\ 0\\ 1 \end{pmatrix}: x_3,x_4 \in \mathbb{R}\bigg\}\\]

Thus, any element $\textbf{v} \in \textbf{x}_n$ will have the property $A\textbf{v} = \textbf{0}$ and the reason we are able to compute $\textbf{b}$ is due to $\textbf{x}_p$. We can quickly check that $A\textbf{x}_p = \textbf{b}$ holds with:\\[\begin{pmatrix} 2 & 4 & 6 & 4 \\ 2 & 5 & 7 & 6 \\ 2 & 3 & 5 & 2 \end{pmatrix}\begin{pmatrix} 4 \\-1 \\ 0 \\ 0 \end{pmatrix} = \begin{pmatrix} 4 \\ 3 \\ 5 \end{pmatrix} \implies \begin{aligned} 2 \cdot 4 + 4 \cdot (-1) + 0 + 0 &= 4 \\ 2 \cdot 4 + 5 \cdot (-1) + 0 + 0 &= 3 \\ 2 \cdot 4 + 3 \cdot (-1) + 0 + 0 &= 5 \end{aligned} \implies \begin{aligned} 4 &= 4 \\ 3 &= 3 \\ 5 &= 5 \end{aligned} \\]as required.

### Vector Spaces Consisting of Functions

Recall cos(0) = 1 and sin(0) = 0.

$\mathbb{R}^\mathbb{N} = \{f \text{ } | \text{ } f: \mathbb{N} \to \mathbb{R}\}$ is a vector space.  $\mathbb{R}^\mathbb{R} = \{f \text{ } | \text{ } f: \mathbb{R} \to \mathbb{R}\}$ is also a vector space.

The set of all functions of the form $y(x) = A \cos(x) + B \cos(2x) + C \cos(3x)$ has the span \\[\bigg\{ c\cos(x) + d\cos(2x) + e\cos(3x) : c,d,e \in \mathbb{R} \bigg\}\\]It has basis \\[\bigg\{ \cos(x), \cos(2x), \cos(3x) \bigg\}\\]
### Orthogonality

Two vectors are orthogonal when their dot product is zero: $\textbf{v} \cdot \textbf{w} = \textbf{v}^T\textbf{w} = \textbf{0}$. It follows that if these vectors are orthogonal, then we have that: $|\textbf{v}|^2 + |\textbf{w}|^2 = |\textbf{v} + \textbf{w}|^2$.

Subspaces $\textbf{V}$ and $\textbf{W}$ are orthogonal when $\textbf{v} \cdot \textbf{w} = \textbf{v}^T\textbf{w} = \textbf{0}$ for all $\textbf{v} \in \textbf{V}$ and $\textbf{w} \in \textbf{W}$.  

The nullspace $N(A)$ contains all vectors orthogonal to the row space $C(A^T)$ because every row of $A$ is perpendicular to every solution of $A\textbf{x} = \textbf{0}$. The left nullspace $N(A^T)$ is perpendicular to $C(A)$, the column space of $A$. When we want to solve $A\textbf{x} = \textbf{b}$ and cannot do it, this left nullspace $N(A^T)$ contains the error $\textbf{e} = \textbf{b} - A\hat{\textbf{x}}$ in the least squares solution $\hat{\textbf{x}}$.

Non-zero orthogonal sets of vectors are automatically linearly independent.

### Projection onto Subspaces

Projecting onto a line is given by the following. Let $n \in \mathbb{R}$. The projection of a vector $\textbf{b} \in \mathbb{R}^n$ onto the line through $\textbf{a} \in \mathbb{R}^n$ is the closest point $\textbf{p} = \textbf{a}\frac{\textbf{a}^T\textbf{b}}{\textbf{a}^T\textbf{a}}$. The orthogonal projection of the point $\textbf{b} = (2,3)$ onto the line $y = 2x$ is given by the following.

Line $y = 2x$ implies we can have $\textbf{a} = \begin{pmatrix} 1 \\ 2\end{pmatrix}$ since the line goes through the point $(1,2)$.

Then $\textbf{b} = \begin{pmatrix} 2 \\ 3 \end{pmatrix}$ and $\textbf{p} = \textbf{a}\frac{\textbf{a}^T\textbf{b}}{\textbf{a}^T\textbf{a}} = \frac{1 \cdot 2 + 2 \cdot 3}{1 \cdot 1 + 2 \cdot 2} \begin{pmatrix} 1 \\ 2\end{pmatrix} = \frac{8}{5}\begin{pmatrix} 1 \\ 2\end{pmatrix} = \begin{pmatrix} \frac{8}{5} \\ \frac{16}{5}\end{pmatrix}$. This $\textbf{p}$ that we have computed is the closest point on the line that goes through $\textbf{a}$ to the point $\textbf{b}$.

The error $\textbf{e} = \textbf{b} - \textbf{p}$ is perpendicular to $\textbf{a}$. In this case $\textbf{e} = \textbf{b} - \textbf{p} = (2,3) - (\frac{8}{5}, \frac{16}{5}) = (\frac{2}{5}, -\frac{1}{5})$.

The projection matrix $P$ is given by $\textbf{p} = \textbf{a} \hat{\textbf{x}} = \textbf{a}\frac{\textbf{a}^T\textbf{b}}{\textbf{a}^T\textbf{a}} = P\textbf{b}$ which implies $P = \frac{\textbf{a}\textbf{a}^T}{\textbf{a}^T\textbf{a}}$.

Projecting onto a subspace generally is given by the following. We can project a vector $\textbf{b}$ into the column space of $A$ by solving $A^TA\hat{\textbf{x}} = A^T\textbf{b}$ and $\textbf{p} = A\hat{\textbf{x}}$. In this case, the projection matrix $P$ is given by $P = A(A^TA)^{-1}A^T$ and $\textbf{p} = P\textbf{b}$.

When looking to find a linear combination of vectors closest to another vector $\textbf{b}$. Put the given vectors into a matrix $A$ and solve for $P$ (as given above). Then solve for $\textbf{p}$ (as given above). This is the closest point in the column space of $A$ to $\textbf{b}$. Writing this point in terms of the given vectors yields the solution.

- $A^TA$ is invertible if and only if $A$ has linearly independent columns.

### Least Squares Approximations

Solving $A^TA\hat{\textbf{x}} = A^T\textbf{b}$ gives the projection $\textbf{p} = A\hat{\textbf{x}}$ of $\textbf{b}$ onto the column space of $A$. If we are looking for the best horizontal line, we have that $\hat{\textbf{x}} = \begin{pmatrix} C \end{pmatrix}$. If we are looking for a line generally, we have that $\hat{\textbf{x}} = \begin{pmatrix} C \\D \end{pmatrix}$.

When $A\textbf{x} - \textbf{b}$ has no solution, $\hat{\textbf{x}} = (A^TA)^{-1}A^T\textbf{b}$ is the least squares solution. The least squares solution $\hat{\textbf{x}}$ makes $E = ||A\textbf{x}-\textbf{b}||^2$ as small as possible.

Setting partial derivatives of $E = ||||A\textbf{x}-\textbf{b}||^2||$ to zero $\bigg( \frac{\partial E}{\partial x_i} = 0 \bigg)$ also produces $A^TA\hat{\textbf{x}} = A^T\textbf{b}$. For example, suppose we need to find the closest line (not through the origin) to the points (0,6), (1,0), (2,0).

Then we have that $A = \begin{pmatrix} 1 & 0 \\ 1 & 1 \\ 1 & 2 \end{pmatrix}$, $x = \begin{pmatrix} C \\ D \end{pmatrix}$, and $\textbf{b} = \begin{pmatrix} 6 \\ 0 \\ 0 \end{pmatrix}$.

When $t = 0$ the first point is on the line $\textbf{b} = C + Dt$ if $C + D \cdot 0 = 6$. When $t = 1$ the second point is on the line $\textbf{b} = C + Dt$ if $C + D \cdot 1 = 0$. When $t = 2$ the third point is on the line $\textbf{b} = C + Dt$ if $C + D \cdot 2 = 0$. So... \\[E = e_1^2 + e_2^2 + e_3^2 = (C + D \cdot 0 - 6)^2 + (C + D \cdot 1 - 0)^2 + (C + D \cdot 2 - 0)^2\\]Now, by computing $\frac{\partial E}{\partial C}$, $\frac{\partial E}{\partial D}$ and setting them equal to zero... \\[\begin{flalign*} \frac{\partial E}{\partial C} &= 2(C + D \cdot 0 - 6) + 2(C + D \cdot 1) + 2(C + D \cdot 2) = 0 \implies 6C + 6D = 12 \\ \frac{\partial E}{\partial D} &= 2(C +D \cdot 0 - 6)(0) + 2(C + D \cdot 1)(1) + 2(C + D \cdot 2)(2) = 0 \implies 6C + 10D = 0 \end{flalign*}\\]Simplifying we get \\[\begin{flalign*} 3C + 3D &= 6 \\ 3C + 5D &= 0 \end{flalign*}\\]Solving yields $D = -3$ and $C = 5$. So, the best fit line is $y = C + Dt = 5 - 3t$.

Ex: Find the height $C$ of the best horizontal line to fit $\textbf{b} = (0,8,8,20)$.

Since we are looking for a horizontal line, put $A = \begin{pmatrix} 1 \\ 1\\ 1 \\1 \end{pmatrix}$. Then $\hat{\textbf{x}} = (A^TA)^{-1}A^T\textbf{b} = ... = \begin{pmatrix} 9 \end{pmatrix}$. This implies that $C = 9$ is the best horizontal line height. The four errors in $e$ are given by $e_1 = C - 0 = 9$, $e_2 = C - 8 = 1$, $e_3 = C - 8 = 1$, and $e_4 = C - 20 = -11$.

Ex: Find the best fit line through the origin $b = Dt$ to the points $(0,0),(1,8),(3,8),(4,20)$.

Here, we are not looking for a horizontal line, but since we want to go through the origin, we cannot have an intercept so no ones column. We have $\hat{\textbf{x}} = \begin{pmatrix} D \end{pmatrix}$ Put $A = \begin{pmatrix} 0 \\ 1 \\ 3 \\ 4 \end{pmatrix}$, $\textbf{b} = \begin{pmatrix} 0 \\ 8 \\ 8 \\20 \end{pmatrix}$. So $A^TA\hat{\textbf{x}} = A^T\textbf{b} \implies 26D = 112 \implies D = 56/13$. So the best fit line throught the origin $b = Dt$ is given by $b = \frac{56}{13}t$.

### Orthonormal Matrices

A set of vectors $q_1,...,q_n$ are called orthogonal when the dot products $q_i \cdot q_j$ are zero. Set of orthogonal unit vectors (unit vectors are vectors with length 1) are called **orthonormal**. Recall vector length is given by $|\vec{v}| = \sqrt{v_1^2 + ... + v_n^2}$. It follows that a set of vectors $q_1,...,q_n$ is orthonormal when: \\[q_i \cdot q_j = \begin{cases} 0, i \neq j \\ 1, i = j\end{cases}\\]If $Q$ is a matrix with orthonormal columns $q_1,...,q_n$ then $Q^TQ = I$. For example, permutation matrices have orthonormal columns. On a seperate note, the inverse of a permutation matrix is its transpose. This is also true for all *square* orthonormal matrices i.e. $Q^T = Q^{-1}$. Therefore, we also have $QQ^T = QQ^{-1} = I$ for all *square* matrices $Q$ (so to check if $Q$ is not orthonormal, one could check these equalities). Going forward, it will often be assumed that $Q$ is square.

Now, if $Q^TQ = I$ and $\vec{x}, \vec{y} = \mathbb{R}^n$. Then
1. $||Q\vec{x}|| = \sqrt{Q\vec{x} \cdot Q\vec{x}} = \sqrt{(Q\vec{x})^TQ\vec{x}} = \sqrt{\vec{x}^TQ^TQ\vec{x}} = ||\vec{x}||$
2. $Q\vec{x} \cdot Q\vec{x} = (Q\vec{x})^TQ\vec{y} = \vec{x}^TQ^TQ\vec{y} = \vec{x}^TI\vec{y} = \vec{x}^T\vec{y} = \vec{x} \cdot \vec{y}$

There is an interesting factorization of $A = QR$. Without loss of generality... \\[\begin{flalign*} A &= QR \\ \begin{pmatrix} \phantom{a} & \phantom{a} & \phantom{a} \\ \textbf{a} & \textbf{b} & \textbf{c} \\ \phantom{a} & \phantom{a} & \phantom{a} \end{pmatrix} &= \begin{pmatrix} \phantom{a} & \phantom{a} & \phantom{a} \\ \textbf{q}_1 & \textbf{q}_2 & \textbf{q}_3 \\ \phantom{a} & \phantom{a} & \phantom{a} \end{pmatrix} \begin{pmatrix} \textbf{q}_1 \cdot \textbf{a} & \textbf{q}_1 \cdot \textbf{b} & \textbf{q}_1 \cdot \textbf{c} \\ \textbf{0} & \textbf{q}_2 \cdot \textbf{b} & \textbf{q}_2 \cdot \textbf{c} \\ \textbf{0} & \textbf{0} & \textbf{q}_3 \cdot \textbf{c} \end{pmatrix} \end{flalign*}\\]So $R = Q^TA$. It follows for least squares that $R^TR\hat{\textbf{x}} = R^TQ^T\textbf{b}$ or $R\hat{\textbf{x}} = Q^T\textbf{b}$ or $\hat{\textbf{x}} = R^{-1}Q^T\textbf{b} = (Q^TA)^{-1}Q^T\textbf{b}$.

### Gram Schmidt

The Gram-Schmidt Process is a process for creating orthonormal vectors. It works as follows.

Suppose you have a set of vectors that you want to make orthonormal. Let the first vector be $\textbf{a}$, the second $\textbf{b}$, the third $\textbf{c}$, etc...

Put $\textbf{A} = \textbf{a}$.

Put $\textbf{B} = \textbf{b} - \frac{\textbf{A}^T\textbf{b}}{\textbf{A}^T\textbf{A}}\textbf{A}$.

Put $\textbf{C} = \textbf{c} - \frac{\textbf{A}^T\textbf{c}}{\textbf{A}^T\textbf{A}}\textbf{A} - \frac{\textbf{B}^T\textbf{c}}{\textbf{B}^T\textbf{B}}\textbf{B}$.

As so forth. Once you have transformed your vectors as such. Normalize each vector in the new set. For instance, with these three vectors, one would do the following.

Put $\textbf{A} = \frac{\textbf{A}}{||\textbf{A}||}$.

Put $\textbf{B} = \frac{\textbf{B}}{||\textbf{B}||}$.

Put $\textbf{C} = \frac{\textbf{C}}{||\textbf{C}||}$.

Now $\{\textbf{A}, \textbf{B}, \textbf{C}\}$ are a set of orthonormal vectors created from the set $\{\textbf{a}, \textbf{b}, \textbf{c}\}$. We say that $\{\textbf{A}, \textbf{B}, \textbf{C}\} = \{\textbf{q}_1,\textbf{q}_2,\textbf{q}_3\}$ if the elements of the set are columns of an orthonormal matrix $Q$.

### Determinants

The determinant of a 2x2 matrix is given by $\text{det} \begin{pmatrix} a & b \\ c & d\end{pmatrix} = ad - bc$. Larger cases follow from this case.

The determinant of a 3x3 matrix is given by $\text{det} \begin{pmatrix} a & b & c \\ d & e & f \\ g & h & i \end{pmatrix} = a \cdot \text{det} \begin{pmatrix} e & f \\ h & i \end{pmatrix} - b \cdot \text{det}  \begin{pmatrix} d & f \\ g & i \end{pmatrix} + c \cdot \text{det} \begin{pmatrix} d & e \\ g & h \end{pmatrix}$.

The determinant of a 4x4 matrix is given by

$\text{det} \begin{pmatrix} a & b & c & d \\ e &f & g & h \\ i & j & k & l \\ m & n & o & p \end{pmatrix} = a \cdot \text{det} \begin{pmatrix} f & g & h \\ j & k & l \\ n & o & p \end{pmatrix} - b \cdot \text{det} \begin{pmatrix} e & g & h \\ i & k & l \\ m & o & p \end{pmatrix} + c \cdot \text{det} \begin{pmatrix} e & f & h \\ i & j & l \\ m & n & p \end{pmatrix} - d \cdot \text{det} \begin{pmatrix} e & f & g \\ i & j & k \\ m & n & o \end{pmatrix}$.

Only square matrices have determinants.

Properties of determinants include: $\text{det} (A^T) = \text{det} (A)$, $\text{det} (AB) = (\text{det} (A))(\text{det} (B))$, and $|\text{det} (I)| = 1$. More properties: not additive i.e. $\text{det}(A+B) \neq \text{det}(A) + \text{det}(B)$, each row exchange of a matrix reverses the sign of a determinant, $\text{det}(\frac{1}{2}A) = \frac{1}{2}^n\text{det}(A)$ where $n$ is the dimension of $A$ ($n \times n$), $\text{det}(A^2) = \text{det}(A)^2$, and $\text{det}(A^{-1}) = \frac{1}{\text{det}(A)}$.  

Invertible matrices have $\text{det}(A) = \pm (\text{product of pivots})$ since if $PA = LU$, then $\text{det}(A) = (\text{det}(L))(\text{det}(U)) = \text{det}(U)$ and $\text{det}(P) = \pm 1$ because it is an invertible permutation matrix.  

A matrix $A$ is invertible if and only if $\text{det}(A) \neq 0$. This is because $\text{det}(A) = 0$ implies that the columns of $A$ are dependent.  Matrices are invertible if and only if their columns are independent and we are done.

The determinant of an upper or lower triangular matrix is the product of the terms on the diagonal. The determinant of a diagonal matrix is the product of the terms on the diagonal.

Ex: \\[\text{ det}\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 2 & 0 & 0 \\ 0 & 0 & 3 & 0 \\ 0 & 0 & 0 & 4 \end{pmatrix} = \text{ det}\begin{pmatrix} 1 & 1 & 1 & 1 \\ 0 & 2 & 1 & 1 \\ 0 & 0 & 3 & 1 \\ 0 & 0 & 0 & 4 \end{pmatrix} = \text{det}\begin{pmatrix} 1 & 0 & 0 & 0 \\ 1 & 2 & 0 & 0 \\ 1 & 1 & 3 & 0 \\ 1 & 1 & 1 & 4\end{pmatrix} = 1 \cdot 2 \cdot 3 \cdot 4 = 24\\]

### Linear Transformations

A linear transformation is a function $T:V \to V$ which satisfies vector addition and scalar multiplication as follows.

(1) $T(\textbf{v} + \textbf{w}) = T(\textbf{v}) + T(\textbf{w})$ for all $\textbf{v}, \textbf{w} \in \mathbb{R}$.
(2) $T(c\textbf{v}) = cT(\textbf{v})$ for all $c \in \mathbb{R}$.

Consequently, we have that $T(c\textbf{v} + d\textbf{w}) = cT(\textbf{v}) + dT(\textbf{w})$ and $T(\textbf{0}) = \textbf{0}$. Common examples of linear transformations include $T_\theta : \mathbb{R}^2 \to \mathbb{R}^2$ (which is rotation by angle $\theta$) and $T_{\text{ref}} : \mathbb{R}^2 \to \mathbb{R}^2$ (which is reflection across the x-axis so $T_{\text{ref}}(x,y) = (x,-y)$).  

Proof (of the latter). Let $\textbf{x},\textbf{y},\textbf{z},\textbf{w} \in \mathbb{R}^n$ and $c \in \mathbb{R}$. Now: \\[T_{\text{ref}}((\textbf{x},\textbf{y}) + (\textbf{z},\textbf{w})) = T_{\text{ref}}((\textbf{x}+\textbf{z},\textbf{y}+\textbf{w})) = (\textbf{x}+\textbf{z},-\textbf{y}-\textbf{w}) = (\textbf{x},-\textbf{y}) + (\textbf{z},-\textbf{w}) = T_{\text{ref}}((\textbf{x},\textbf{y})) + T_{\text{ref}}((\textbf{z},\textbf{w}))\\]so vector addition is satisfied. Also: \\[T_{\text{ref}}(c(\textbf{x},\textbf{y})) = T_{\text{ref}}((c\textbf{x},c\textbf{y})) = (c\textbf{x}, -c\textbf{y}) = c(\textbf{x}, -\textbf{y}) = cT_{\text{ref}}(\textbf{x}, \textbf{y})\\]so scalar multiplication is satisfied.

The composition of two linear transformations is a linear transformation.

Proof. Let $T_1, T_2$ be linear transformations from $V \to V$. Let $c,d \in \mathbb{R}$, and $\textbf{v}, \textbf{w} \in \mathbb{R}$. Then:

(1) $T_1(T_2(\textbf{v} + \textbf{w})) = T_1(T_2(\textbf{v}) + T_2(\textbf{w})) = T_1(T_2(\textbf{v})) + T_1(T_2(\textbf{w}))$ so vector addition is satisfied.
(2) $T_1(T_2(c\textbf{v})) = T_1(cT_2(\textbf{v})) = cT_1(T_2(\textbf{v}))$ so scalar multiplication is satisfied.

This proves that $T_1 \circ T_2$ is a linear transformation. $\square$  

Ex 1: Suppose a linear transformation $T$ transforms $(1,1)$ to $(2,2)$ and $(2,0)$ to $(0,0)$. Find $T((2,2))$.

Let $(a,b) \in \mathbb{R}^2$ be a combination of vectors $(1,1)$ and $(2,2)$. Then $(a,b) = x(1,1) + y(2,0) = (x+2y,x)$. So: $a = x + 2y$ and $b = x$. Therefore $x = b$ and $y = (a-b)/2$. \\[\begin{flalign*} %&\implies (a,b) = b T((1,1)) + \frac{a-b}{2}T((2,0)) \\
&\implies T((a,b)) = bT((1,1)) + \frac{a-b}{2}T((2,0)) = b(2,2) + \frac{a-b}{2} (0,0) = (2b,2b) \end{flalign*}\\]So: \\[T((2,2)) = (2\cdot2,2\cdot2) = (4,4)\\]Alternatively we can use $\textbf{b} = T(\vec{\textbf{x}}) = A \vec{\textbf{x}}$. Here: $(2,2)$ and $(0,0)$ correspond to $\textbf{b}$; $(1,1)$ and $(2,0)$ correspond to $\vec{\textbf{x}}$. So: \\[\begin{flalign*} A &= \textbf{b} \vec{\textbf{x}}^{-1} \\ &= \begin{pmatrix} 2 & 0 \\ 2 & 0\end{pmatrix} \begin{pmatrix} 1 & 2 \\ 1 & 0 \end{pmatrix}^{-1} \\ &=\begin{pmatrix} 2 & 0 \\ 2 & 0\end{pmatrix} \begin{pmatrix} 0 & 1 \\ \frac{1}{2} & -\frac{1}{2} \end{pmatrix} \\ &= \begin{pmatrix} 0 & 2 \\ 0 & 2 \end{pmatrix} \end{flalign*}\\]So: \\[\begin{pmatrix} 0 & 2 \\ 0 & 2 \end{pmatrix}\begin{pmatrix}  2 \\ 2\end{pmatrix} = \begin{pmatrix}  4 \\ 4\end{pmatrix}\\]as required.

Ex 2: Let $\textbf{v},\textbf{w} \in \mathbb{R}^2$. Is $T(\textbf{v})=(0,1)$ a linear transformation? No since it does not satisfy scalar multiplication: \\[T(\textbf{v} + \textbf{w}) = T(\textbf{v}) + T(\textbf{w}) = (0,1) + (0,1) = (0,2) \neq (0,1)\\]Is $T(\textbf{v}) = (v_2,v_1)$ a linear transformation? Yes since it satisfies (1) vector addition and (2) scalar multiplication.

(1) $T(\textbf{v} + \textbf{w}) = T(\textbf{v}) + T(\textbf{w}) = (v_2,v_1) + (w_2,w_1) = (v_2 + w_2, v_1 + w_1) = T((v_1 + w_1, v_2 + w_2)) = T(\textbf{v} + \textbf{w})$ as required.
(2) $T(c\textbf{v}) = cT(\textbf{v}) = c(v_2,v_1) = (cv_2,cv_1) = T(cv_1, cv_2) = T(c\textbf{v})$ as required.

### Eigenstuff

An eigen vector $\textbf{v}$ of a square matrix $A$ is a vector satisfying the equation: $A\textbf{v} = \lambda\textbf{v}$ for some $\lambda \in \mathbb{R}$ called the eigen value. So $A$ stretches or compresses $\textbf{v}$ without changing its direction. For instance: \\[\begin{flalign*} A\textbf{v} &= \lambda \textbf{v}\\ \begin{pmatrix} 1 & 1 \\ 1 & -1\end{pmatrix} \begin{pmatrix} 1 + \sqrt{2} \\ 1\end{pmatrix} &= \sqrt{2} \begin{pmatrix} 1 + \sqrt{2} \\ 1\end{pmatrix} \end{flalign*}\\]Notice: $A(c\textbf{v}) = cA\textbf{v} = c\lambda\textbf{v} = \lambda(cA)$.

To find the eigen values of a matrix the fundamental equation is \\[A\textbf{v} = \lambda \textbf{v} \iff A\textbf{v} - \lambda \textbf{v} = \textbf{0} \iff \underbracket{(A - \lambda I)}_{ \text{cols are dep} }\textbf{v} = \textbf{0}\\]Ex 1: Keeping $A = \begin{pmatrix} 1 & 1 \\ 1 & -1\end{pmatrix}$, solve the determinant equation $\text{det}(A - \lambda I) = 0$ for the eigen value $\lambda$.

We are given that $\text{det}(A - \lambda I) = 0 \implies \begin{pmatrix} 1 & 1 \\ 1 & -1\end{pmatrix} - \begin{pmatrix} \lambda & 0 \\ 0 & \lambda\end{pmatrix} = \begin{pmatrix} 1-\lambda & 1 \\ 1 & -1-\lambda\end{pmatrix}$. So, using the equation for a $2 \times 2$ determinant: \\[\begin{flalign*} (1-\lambda)(-1-\lambda) - 1 &= 0 \\ (1-\lambda)(-1-\lambda) &= 1 \\ (1-\lambda)(1+\lambda) &= -1 \\ 1(1-\lambda) + \lambda(1 - \lambda) &= -1 \\ 1 - \lambda + \lambda - \lambda^2 &= -1 \\ 1 - \lambda^2 &= -1 \\ -\lambda^2 &= -2 \\\lambda^2 &= 2 \\ \lambda &= \pm \sqrt{2} \end{flalign*}\\]To find the eigen vector $\textbf{v}$, we have that \\[(A - \sqrt{2} I) \textbf{v} = \textbf{0} \implies \bigg( \begin{pmatrix} 1 & 1 \\ 1 & -1\end{pmatrix} - \begin{pmatrix} \sqrt{2} & 0 \\ 0 & \sqrt{2} \end{pmatrix} \bigg) \textbf{v} = \textbf{0} \implies \begin{pmatrix} 1 - \sqrt{2} & 1 \\ 1 & -1 - \sqrt{2}\end{pmatrix} \textbf{v} = \textbf{0}\\]Solving the system of equations reveals that \\[\textbf{v} = \begin{pmatrix} v_1 \\ v_2 \end{pmatrix} = \begin{pmatrix} 1 \\ -1 + \sqrt{2}\end{pmatrix}\\]Notice: $\lambda$ is an eigen value $\iff (A - \lambda I)$ is singular (recall a square matrix is singular $\iff$ its determinant is zero/it does not have an inverse).  Now, The equation given by $\text{det}(A - \lambda I) = 0$ is a polynomial of degree $n$ called the characteristic polynomial whose variable is $\lambda$ (not $x$).

Facts: If $A$ is a diagonal/triangular matrix, the eigen values are diagonal entries. The product of the $n$ eigen values of $A$ is the determinant of $A$. The sum of the $n$ eigen values of $A$ equals the sum of the $n$ diagonal entires of $A$.

When $A$ is squared, the eigenvectors stay the same but the eigen values are squared.

Ex 2: Find eigenvalues for the matrix $A = \begin{pmatrix} 1 & 4 \\ 2 & 3\end{pmatrix}$.

Set $\text{det}(A - \lambda I) = 0$. So $\text{det}\bigg(\begin{pmatrix} 1 & 4 \\ 2 & 3 \end{pmatrix} - \begin{pmatrix} \lambda & 0 \\ \lambda & 0  \end{pmatrix}\bigg) = 0$. Using the equation for $2 \times 2$ determinant: $(1-\lambda)(3- \lambda) - 8 = 0 \implies (\lambda + 1)(\lambda - 5) = 0$. So $\lambda = -1,5$. Then $(A - \lambda I)\textbf{v} = \textbf{0}$. So for $\lambda = 1$: $(A + I)\textbf{v} = \textbf{0} \implies (\begin{pmatrix} 2 & 4 \\ 2 & 4 \end{pmatrix})\textbf{v} = \textbf{0}$. So $v_1 + 2v_2 = 0$. Set $v_1 = 1$. Then $\textbf{v} = (-2,1)$. Other case similar.

### Diagonalizing Matrices

The process of writing $A = X\Lambda X^{-1}$ (which implies $AX = X\Lambda$) is called diagonalizing $A$. It requires that the matrix $X$ be invertible. In other words, the eigenvectors must be linearly independent. Fact: eigenvectors corresponding to distinct eigenvalues are always linearly independent.

Diagonalize the matrix $A = \begin{pmatrix} 2 & 2 \\ 0 & 3\end{pmatrix}$.

Upper triangular so we have that $\lambda = 2,3$ are the eigenvalues.

For $\lambda = 2$: $\begin{pmatrix} 2 & 2 \\ 0 & 3 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} =\begin{pmatrix} 2x \\ 2y \end{pmatrix} \implies y = 0, x \in\mathbb{R}$. Set $x = 1$. So $\lambda_1 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$.

For $\lambda = 3$: $\begin{pmatrix} 2 & 2 \\ 0 & 3 \end{pmatrix}\begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 3x \\ 3y \end{pmatrix} \implies x = 2, y \in\mathbb{R}$. Set $y = 1$. So $\lambda_2 = \begin{pmatrix} 2 \\ 1 \end{pmatrix}$.

Then $X = \begin{pmatrix} \lambda_1 & \lambda_2 \end{pmatrix} = \begin{pmatrix} 1 & 2 \\ 0 & 1 \end{pmatrix}$. $\Lambda = \begin{pmatrix} 2 & 0 \\ 0 & 3\end{pmatrix}$.

Now \\[\begin{flalign*} A &= X \Lambda X^{-1} \\ &= \begin{pmatrix} 1 & 2 \\ 0 & 1 \end{pmatrix} \begin{pmatrix} 2 & 0 \\ 0 & 3 \end{pmatrix} \begin{pmatrix} 1 & 2 \\ 0 & 1 \end{pmatrix}^{-1} \\ &= \begin{pmatrix} 1 & 2 \\ 0 & 1 \end{pmatrix} \begin{pmatrix} 2 & 0 \\ 0 & 3 \end{pmatrix} \begin{pmatrix} 1 & -2 \\ 0 & 1 \end{pmatrix} \\ &= \begin{pmatrix} 2 & 6 \\ 0 & 3 \end{pmatrix} \begin{pmatrix} 1 & -2 \\ 0 & 1 \end{pmatrix} \\ &= \begin{pmatrix} 2 & 2 \\ 0 & 3\end{pmatrix}\end{flalign*}\\]Notice that $A^n$ has the same eigenvectors in $X$ and its squared eigenvalues are in $\Lambda^n$. So $A^n = X \Lambda^n X^{-1}$.

Notice that if $A = X \Lambda X^{-1}$ and $B = Y \Lambda Y^{-1}$, then $A$ and $B$ have the same eigenvalues.

A matrix with no repeated eigenvalues will have linearly independent eigenvectors. In other words, eigenvectors corrseponding to distinct eigenvalues are always linearly independent.

