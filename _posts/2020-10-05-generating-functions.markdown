
---
layout: post
title:  "Solving merge sort recurrence using generating functions and sympy"
date:   2020-05-07 11:47:03 +0100
categories: CS, Math
---

# The closed form for merge sort recurrence using sympy and generating functions

Here is the recurrence relation of merge sort. 

$$F_n = 2F_{n/2} + n - 1$$

This recurrence reflects the number fo comparisons required for sorting a list using merge sort algorithm.  It basically says the number of comparisons required for solving any problem with input size $n$ is two times the number of comparisons required for solving problems with half the input size $(n/2)$ plus $n-1$ comparisons. The following table shows the values this recurrence produces for the inputs of size $2^k$  for $k \in 0\ldots64$.

[Table of number of comparisons of merge sort](https://www.notion.so/ef57e7ba6faa47b0a7bb419dda6b1894)

 We define $C_N = F_{2^n}$. Let's assume $n$ is power of 2 and for simplicity we take $n = 2^N$.

$$C_N=2C_{N-1} + 2^N-1$$

We can convert it to a generating function by multiplying by $x^N$and summing over $N$. Lets define $A(x) = \sum_{N\geq0}C_Nx^N$

$$\sum_{N\geq0}C_Nx^N =2\sum_{N\geq0}C_{N-1}x^N+\sum_{N\geq0}2^Nx^n-\sum_{N\geq0}x^n$$

Next we factor out $x$ from $\sum_{N\geq0}C_{N-1}x^N$.

$$\sum_{N\geq0}C_Nx^N =2x\sum_{N\geq0}C_{N-1}x^{N-1}+\sum_{N\geq0}2^Nx^n-\sum_{N\geq0}x^n$$

From the standard generating functions we know the following identities.

$$\sum_{k\geq0}2^kx^k = \frac{1}{1 - 2x},  \sum_{k\geq0}x^k = \frac{1}{1 - x}$$

You can also check them in sympy, which give you the same result.

```python
f = 1/(1-x)
f.series()
```

By substituting the above identities and the equation above we get.

$$A(x) = 2xA(x) + \frac{1}{1-2x}-\frac{1}{1 - x}$$

If we now solve the equation for $A(x)$, we get.

$$(1 - 2x)A(x) = \frac{x}{\left(x - 1\right) \left(2 x - 1\right)} $$

Hence:

$$A(x)= -\frac{x}{\left(1 - x\right) \left(2 x - 1\right)^{2}}$$

If you get the Taylor series of $A(x)$ using sympy

```python
A = -(x/((x-1)*(2*x -1)**2))
A.series()
```

we get the following

$$A(x) = x + 5 x^{2} + 17 x^{3} + 49 x^{4} + 129 x^{5} + 321 x^{6} + 769 x^{7} + 1793 x^{8} + 4097 x^{9} + O\left(x^{10}\right)$$

Where each exponent shows the exponentiation of the number of nodes and the coefficient shows the number of comparisons. For example $5x^2$ shows for a list containing 4 nodes we need 5 comparisons and $129x^5$shows for a list of size 32 we need 129 comparisons. As you can see the coefficients match our numbers in the table given above.

Now we need to partition $A(x)$ to familiar series to find the closed form for each of the coefficients.  For that, we use partial fraction decomposition, which is easily done in sympy as follows.

```python
apart(A)
```

And we get this decomposition.

$$A(x) = \frac{2}{2 x - 1} + \frac{1}{\left(2 x - 1\right)^{2}} - \frac{1}{x - 1}$$

Now if we know the closed form for the coefficient of these series, then we find the closed form for the original recurrence relation.

The first and third terms' coefficients are straightforward and are $-2^{k+1}$ and $1$, respectively.

The second term is a bit tricky as it is a convolution of $1/(1-2x)$ with itself. 

$$A(z) B(z)=\sum_{n \geq 0}\left(\sum_{0 \leq k \leq n} a_{k} b_{n-k}\right) z^{n}$$

Let's look at the convolution of first three coefficients of this series with itself namely $a_1a_3+a_2a_2+a_3a_1$ if $a_i$s are the coefficients of the series. We know from above that $a_1, a_2, a_3$ are 1, 2, 4 respectively. 1*4 + 2*2 + 4*1 = 4 + 4 + 4. Trying other cases we can easily see that the closed form for coefficients is $(k+1)2^k$.

OK so the final closed form coefficient is the sum of the three terms: $2^{k} \left(k + 1\right) - 2^{k + 1} + 1 = k2^{k} - 2^{k} + 1$.

```python
In [1]: [k*2**k - 2**k + 1 for k in range(10)]
Out[1]: [0, 1, 5, 17, 49, 129, 321, 769, 1793, 4097]
```

The numbers again match the ones we have in the table.

As you can see, the numbers coincide with the numbers in the table above.

To verify it we can also put the $k2^{k} - 2^{k} + 1$ in the original recursion. 

Let's do some sympy:

```python
f = (x+1)*2**x -2**(x+1) + 1
(2 * f + 2*2**x - 1 - f.subs(x, x+1)).simplify() == 0
```

Now you can replace $2^k$ with $k$ and $k$ with $\log(k)$ and you get the similar results.

$$2F_{n/2} + n - 1 = 2(k\log(k)-k+1)+2k-1=2klog(k)+1$$

$$F_n = 2k\log(2k)-2k+1 = 2k(1+\log(k)) - 2k + 1 = 2k\log(k)+1$$
