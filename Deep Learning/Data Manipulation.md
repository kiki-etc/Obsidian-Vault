To begin anything, we first need to store and manipulate data.
<br>A **`tensor`** represents an n-dimensional array of numerical values, it resembles NumPy's `ndarray`, with a few features like automatic differentiation added. Tensors leverage GPUs to accelerate computation, whereas NumPy only runs on CPUs. Therefore, tensors are better for neural networks.
When only one axis is needed for the data, the tensor is called a ***vector***. With two axes, it's called a ***matrix***. For higher number of axes, we just refer to them as $n^{th}$ order tensors.
<br>A tensor can be initialized using `arange(n)`. This creates a vector of evenly spaced values (starting at 0-inclusive and ending at *n* non included). The default interval is one.
```Python
x = torch.arange(6, dtype=torch.float32)
```
`x` will be `tensor([0., 1., 2., 3., 4., 5.])`
<br>Each value in the tensor is called an *element* of the tensor. The number of elements can be inspected using the `numel` method. A tensor's *shape* (length along each axis) can be inspected using the `shape` method.
<br>The shape of a tensor can be changed without altering its size or values by invoking `reshape`. For example, the line `x.reshape(3, 4)` should have the following output:
```Python
tensor([[0., 1.,  2.,  3.],
	    [4., 5.,  6.,  7.],
	    [8., 9., 10., 11.]])
```
We can place a `-1` where we want the computer to infer the dimension. E.g., instead of `x.reshape(3, 4)` we could  call `x.reshape(3, -1)`.
<br>Tensors can be initialized to contain all 0s or 1s by using either the `zeros()` or `ones()` methods respectively, the dimensions need to be specified as parameters in an additional pair of brackets.
<br>The `randn` method can also be used. It creates a tensor with elements drawn from a standard normal distribution with mean 0 and standard deviation 1.
<br>Indexing and slicing work as they always have with `ndarrays` in NumPy. 
## Operations
The most useful are *elementwise* operations. These apply a standard scalar operation to each element of a tensor. For functions that take two tensors as inputs, elementwise operations apply some binary operator on each pair of corresponding elements.
Most unary scalar operators, $f:ℝ→ℝ$, can be applied elementwise. For example, $e^{x}$ can be applied elementwise as `torch.exp(x)`.
Binary scalar operators, $f:ℝ,ℝ→ℝ$, can also be applied elementwise. Here, two vectors (**u** and **v**) of the *same shape* produce a vector **c** where $c_{i}←u_{i},v_{i}$. 
<br>Multiple tensors can be concatenated to form larger ones. `torch.cat((X, Y), dim=0)` increases the vertical-axis length while `torch.cat((X, Y), dim=1)` increases the horizontal axis length.
<br>Using the `==` operator we can create a binary tensor by comparing corresponding elements.
## Broadcasting
Broadcasting works using the following two steps: expand one or both arrays by copying elements along axes with length 1 so that after this transformation, the two tensors have the same shape THEN perform an elementwise operation on the resulting arrays.
## Saving Memory
Allocating new memory can be a hassle when dealing with big data so it's helpful to perform calculation and updates *in place*. Secondly, if we do not update in place, we must be careful to update all references to a particular parameter, lest we spring a memory leak or inadvertently refer to "stale" parameters.
<br>We can assign the result of an operation to a previously allocated array by using slice notation. For example,
```Python
Z = torch.zeros_like(Y)
print('id(Z): ', id(Z))
Z[:] = X + Y
print('id(Z): ', id(Z))
```
If the value of X is not reused in subsequent computations, we can also use `X[:] = X + Y` to reduce the memory overhead of the operation.
# Data Preprocessing
Working with .csv files is very common. First we demonstrate using `pandas` to create a small .csv file we will manipulate later.
```Python
import os
import pandas

os.makedirs(os.path.join('..', 'data'), exist_ok=True)
data_file = os.path.join('..', 'data', 'house_tiny.csv')

with open(data_file, 'w') as f:
	f.write('''NumRooms,RoofType,Price
NA,NA,127500
2,NA,106000
4,Slate,178100
NA,NA,140000''')

data = pandas.read_csv(data_file)
print(data)
```
The output should look like this:
```
  NumRooms  RoofType   Price
0      NaN       NaN  127500
1      2.0       NaN  106000
2      4.0     Slate  178100
3.     NaN       NaN  140000
```
First step is separating input columns from the target column(s). Some imputation heuristics: for categorical fields, we can treat NaN as a category.
``` Python
inputs, targets = data.iloc[:,0:2], data.iloc[:, 2]
inputs = pandas.get_dummies(inputs, dummy_na=True)
```
Inputs will look like this:
```
  NumRooms RoofType_Slate RoofType_nan
0      NaN          False         True
1      2.0          False         True
2      4.0           True        False
3      NaN          False         True
```
For missing numerical values, a common heuristic is to replace the NaN entries with the mean. `inputs = inputs.fillna(inputs.mean())`
```
  NumRooms RoofType_Slate RoofType_nan
0      3.0          False         True
1      2.0          False         True
2      4.0           True        False
3      NaN          False         True
```
<br>We can now load the entries into tensors.
```Python
X = torch.tensor(inputs.to_numpy(dtype=float))
y = torch.tensor(taregets.to_numpy(dtype=float))
```
# Linear Algebra
Manipulating data often involves mathematical computations with tools from linear algebra. Scalars are implemented as tensors that contain only one element, for example, `torch.tensor(3.0)`. Vectors are implemented as first-order tensors. The default vector representation is vertical. We can access a tensor's elements via indexing.
To indicate that a vector contains *n* elements, we say  $x∈R^{𝑛}$, where *n* is the dimensionality of the vector. This attribute is accessible via Python's built-in `len` function. We can also access the length via the `shape` attribute, which is a tuple that indicates a tensor's length along each axis.
>[!note]- Order vs Dimensionality
>We use ***order*** to refer to the number of axes and ***dimensionality*** to refer to the number of components.
## Matrices
Matrices are second-order tensors, usually denoted by bold capital letters. The expression $A∈R^{m×n}$ indicates that a matrix contains m × n real-valued scalars, arranged as *m* rows and *n* columns. For example, `A = torch.arange(6).reshape(3, 2)` would look like this:
```Python
tensor([[0, 1],
	    [2, 3],
	    [4, 5]])
```
To transpose a matrix we use the `T` method. For example, `A.T` would look like this:
```Python
tensor([[0, 2, 4],
	    [1, 3, 5]])
```
## Tensors
Eventually, you need higher order tensors. Tensors will become especially important when working with images - each image arrives as a 3rd-order tensor with axes corresponding to the height, width and *channel*. At each spatial location, the intensities of each color (RGB) are stacked along the channel. Further, a collection of images is represented in code by a 4th-order, where distinct images are indexed along the first axis. Higher order tensors are constructed by growing the number of shape components.
<br>Elementwise operations produce outputs that have the same shape as their operands.
### Reduction
Often, we wish to express the sum of a tensor's elements. This can be done with the simple function: `x.sum()`, the result is a tensor scalar. Our libraries also allow us to specify the axes along which the tensor should be reduced. If we specify `axis=0`, since  the matrix reduces along `axis=0`, that axis will be missing from the shape of the output.
Another way to reduce is the *mean*, it's analogous to summing. For example, `A.mean()` and it can be also done along axes.
### Non-Reduction Sum
Assume A is as follows:
```Python
A = tensor([[0, 1, 2],
		    [3, 4, 5]])
```
Sometimes it can be useful to keep the order of the tensors involved unchanged when invoking the sum or mean.
```Python
sum_A = A.sum(axis=1, keepdims=True)
sum_A, sum_A.shape
```
The above should give the following output:
```Python
(tensor([[ 3.],
		 [12.]]),
 torch.size([2, 1]))
```
For instance, since `sum_A` keeps its two axes after summing each row, we can divide `A` by `sum_A` with broadcasting to create a matrix where each row sums up to 1. The output will look like this:
```Python
tensor([[0.0000, 0.3333, 0.6667],
	    [0.2500, 0.3333, 0.4167]])
```
If we want to calculate the cumulative sum of elements of A along some axis, we can do so. For example, `A.cumsum(axis=0)` should produce the following:
```Python
tensor([[0., 1., 2.],
	    [3., 5., 7.]])
```
### Dot Products
Given two vectors, their is a sum over the products of the elements at the same position.
```Python
x = tensor([0., 1., 2.])
y = torch.ones(3, dtype=torch.float32)
```
### Matrix-Vector Products
To start, we visualize our matrix in terms of its row vectors. The matrix vector product **Ax** is a column vector whose $i^{th}$ element is the dot product $a_{i}^{T}x$.
### Matrix-Matrix Products
Regular matrix multiplication. An a × n matrix multiplies an n × b to form an a × b matrix.
### Norms
Informally, the norm of a vector tells us how big it is. A norm is a function ∥·∥ that maps a vector to a scalar and satisfies the following three properties:
1. Given any vector **x**, if we scale (all elements of) the vector by a scalar $𝛼∈ℝ$, its norm scales accordingly
2. For any vectors **x** and **y**: norms satisfy the triangle inequality:
$∥x+y∥≤∥x∥+∥y∥$
3. The norm of a vector is nonnegative and it only vanishes if the vector is zero.

The $l_2$ norm is expressed as the sum of the squares of a vectors elements. The `norm` method calculates the $l_2$ norm.
```Python
u = torch.tensor([3.0, -4.0])
torch.norm(u)
```
The output will be `tensor(5.)`.
<br>The $l_1$ norm is associated with the Manhattan distance. It sums the absolute values of all a vector's elements. Compared to the $l_2$ norm, it is less sensitive to outliers. We compose the absolute value then use the sum operations: `torch.abs(u).sum()` should give `tensor(7.)`.
<br>The two $l$ norms are versions of the more general $l_p$ form.
$$∥x∥_p=\left(\sum_{n=1}^{n} |x_i|^p\right)^\frac{1}{p}$$
#### Frobenius Norm
This helps us with matrices, which can be more complicated to work with. It is defined as the square root of the sum of the squares of a matrix's elements:
$$∥X∥_F=\sqrt{\sum_{i=1}^{m} \sum_{j=1}^{n} x_{ij}^2}$$
The Frobenius norm behaves as if it were an $l_2$ norm of a matrix-shaped vector. Invoking `torch.norm(matrix)` will calculate the Frobenius norm of a matrix.
# Calculus, Probability and Statistics
The limiting procedure is at the root of both differential calculus and integral calculus.
```Python
%matplotlib inline
import numpy
from matplotlib_inline import backend_inline
from d2l import torch as d2l
```
#### Visualization Utilities
We can visualize the slopes of functions using the `matplotlib` library. The `use_svg_display` function tells `matplotlib` to output graphics in SVG format for crisper images. The comment `#@save` is a special modifier that allows us to save any code block (in a Jupyter notebook) to the `d2l` package so that we can invoke it in later cells without repeating the code.
```Python
def use_svg_display(): #@save
    """Set the figure size for matplotlib."""
    use_svg_displaay()
    d2l.plt.rcParams['figure.figsize'] = figsize
```
We can conveniently set figure sizes with `set_figsize`. Since the import statement from `matplotlib import pyplot as plt` was marked via `#@save` in the `d2l` package, we can call `d2l.plt`.
```Python
def set_axes(axes, xlabel, ylabel, xlim, ylim, xscale, yscale, legend):
	"""Set the axes for matplotlib"""
	axes.set_xlabel(xlabel), axes.set_ylabel(ylabel)
	axes.set_xscale(xscale), axes.set_yscale(yscale)
	axes.set_xlim(xlim),     axes.set_ylim(ylim)
	
	if legend:
		axes.legend(legend)
	axes.grid()
```
With these three functions, we can define a `plot` function to overlay multiple curves with the following function header:
```Python
def plot(X, Y=None, xlabel=None, ylabel=None, legend=[], xlim=None, ylim=None, xscale='linear', yscale='linear', fmts=('-', 'm--', 'g-.', 'r:'), figsize=(3.5, 2.5), axes=None):
```
>[!info]- Explanation of the `plot` parameters
>`X`
>- This represents the data for the x-axis. It can be a single array or a list of arrays if multiple lines are to be plotted.
>
>`Y`
>- This represents data for the y-axis.
>- If Y is not provided, the function assumes the data in X should be plotted on the y-axis, and automatically generates the x-values by defaulting to indices.
>
>`xlabel`
>- This represents the label for the x-axis.
>
>`ylabel`
>- The label for the y-axis
>
>`legend`
>- A list of labels for the different lines in the plot.
>
>`xlim` and `ylim`
>- Tuples that set the limits of the axes.
>
>`xscale` and `yscale`
>- Sets the scale of the respective axes. Typical values are `'linear'` and `'log'`
>
>`fmts`
>- A tuple specifying the format strings for the lines in the plot
>
>`figsize`
>- A tuple that determines the size of the figure
>
>`axes`
>- If provided, it uses the given axes object to plot the data.
>- If not, it uses the current active axes to plot the data.

Now we can plot the function $u=f(x)$ and its tangent line $y=2x-3$ at $x=1$ using the following block of code:
```Python
x =np.arange(0,3,0.1)
plot(x, [f(x), 2*x - 3], 'x', 'f(x)', legend=['f(x)', 'Tangent line'])
```
## Automatic Differentiation
Automatic differentiation, often shortened to *autograd*, uses a framework that builds a computational graph that tracks how each value depends on the others as we pass data through each successive function.
<br>Suppose we want to differentiate $y=2x^Tx$ with respect to the column vector $x$. Assuming `x` was created like so: `x = torch.arange(4.0, requires_grad=True)`. Before we calculate the gradient of x, we need to be efficient with the way we store it.
>[!note]-
>The gradient of a scalar-valued function with respect to a vector **x** is a vector-valued with the same shape as **x**
><br>Also, the `requires_grad_()` function in PyTorch is used to set the `requires_grad` attribute of a tensor, indicating whether the tensor should be tracked for gradient computation. It can be modified in place.
><br>The `grad_fn` attribute helps keep track of the operations that created a tensor so that PyTorch can compute gradients during back-propagation. If you perform further operations that require gradients, PyTorch will use this information to calculate the gradients correctly. For example, if `y` has a `grad_fn`, it means that `y` is a ***leaf tensor*** (i.e., it’s not created from other tensors) and that it has requires_grad=True. **This indicates that gradients will be computed for this tensor when you call `backward()` on it or on any tensor that depends on it**.

We can now take the gradient of $y$ with respect to $x$ by calling its `backward` method. Then we can access the gradient via $x$'s `grad` attribute. This should give: `tensor(0., 4., 8., 12.])` because the differential should be $4x$.
### Backward for Non-Scalar Variables
When $y$ is a vector, the most natural representation of the derivative of $y$ with respect to a vector $x$ is the matrix called the ***Jacobian*** that contains the partial derivatives of each component of $y$ with respect to each component of $x$. Likewise, for higher-order $y$ and $x$, the result of differentiating could be an even higher order tensor.
<br>Because deep learning frameworks vary in how they interpret gradients of non-scalar tensors, PyTorch takes some steps to avoid confusion. Invoking backward on a non-scalar elicits an error unless we tell PyTorch how to reduce the object to a scalar. More formally, we need to provide some vector v such that backward will compute $v^⊤𝜕xy$ rather than $𝜕xy$. This argument (representing **v**) is named gradient. For a more detailed description, see [Yang Zhang’s Medium post](https://zhang-yang.medium.com/the-gradient-argument-in-pytorchs-backward-function-explained-by-examples-68f266950c29).
### Detaching Computation
Sometimes we want to do calculations outside of the recorded computational graph. This is helpful when we have auxiliary intermediate terms that do not contribute to learning or in any case where computing a gradient is not necessary. In this case we can use the `detach()` method or wrap the code in a `with torch.no_grad()` block.
1. Using `detach()`
```Python
x = torch.tensor([1.0, 2.0], requires_grad=True)
y = x * 2
z = y.detach()  # z will not track gradients
```
2. Using `with torch.no_grad()` block
```Python
# create two tensors
a = torch.tensor([1.0, 2.0, 3.0], requires_grad=True)
b = torch.tensor([4.0, 5.0, 6.0], requires_grad=True)

with torch.no_grad():
    c = a + b  # Perform addition without tracking gradients

# Print the result
print("Sum:", c)  # Output: Sum: tensor([5., 7., 9.])
```
### Gradient Control Flow
Programming allows for more complex computations that can depend on auxiliary variables or conditional statements based on intermediate results. A significant advantage of automatic differentiation is that it can compute gradients even when the function's path involves complex control flows, such as loops and conditionals.
<br>For example,
```Python
    def f(a):
        b = a * 2
        while b.norm() < 1000:
            b = b * 2
        if b.sum() > 0:
            c = b
        else:
            c = 100 * b
        return c
```
In this example, the number of iterations of the loop and the evaluation of the `if` statement depend on the input variable `a`.
<br>When the function `f(a)` is executed, it generates a specific computational graph based on the input. The gradient can be computed via:
```python
    a = torch.randn(size=(), requires_grad=True)
    d = f(a)
    d.backward()
```
This allows us to differentiate $f(a)$ with respect to $a$ dynamically, even though the exact form of the computational graph is unknown beforehand.
- **Real-World Applications**: Dynamic control flow is prevalent in deep learning applications (e.g., processing variable-length inputs in text). Automatic differentiation is crucial in these scenarios, enabling the computation of gradients without prior knowledge of the input structure.
- **Key Takeaway**: Automatic differentiation handles complex functions with dynamic behavior efficiently, making it essential for modern statistical modeling and deep
## Probability and Statistics
Consider the typical statistical problem of finding out the number of heads in a sequence of coin tosses. There are many ways to simulate this. We could, for example, invoke any random number generator. Python's `random.random` yields numbers in the interval \[0,1] where the probability of lying in any sub-interval \[a, b] ⊂ \[0,1] is equal to $b-a$. Thus we can get out 0 and 1 with a probability of 0.5 each by testing whether the returned float number is greater than 0.5:
```Python
num_tosses = 100
heads = sum([random.random() > 0.5 for _ in range(num_tosses)])
tails = num_tosses - heads
```
Printing out heads and tails could give an answer like `[44, 56]`.
<br>We can simulate multiple draws from a variable with a finite number of outcomes by calling the multinomial function, setting the first argument to the number of draws and the second as a list of probabilities associated with each of the possible outcomes.
```Python
unfair_die = torch.tensor([0.1, 0.2, 0.3, 0.2, 0.1, 0.1])
Multinomial(100, unfair_die).sample()
```
The result is a vector with the number of occurrences of each outcome, in the order that they were provided in the argument's list.
<br>Although the program knows the exact probabilities, the randomness guarantees that when we re-run the block of code, there is a slightly different output. This is less likely to occur the larger the sample size grows. We can visualize this by running the following code block:
```Python
fair_probs = torch.tensor([0.5, 0.5])

counts = Multinomial(1, fair_probs).sample((10000,))
cum_counts = counts.cumsum(dim=0)
estimates = cum_counts / cum_counts.sum(dim=1, keepdims=True)
estimates = estimates.numpy()

d2l.set_figsize((4.5, 3.5))
d2l.plt.plot(estimates[:, 0], label=("P(coin=heads)"))
d2l.plt.plot(estimates[:, 1], label=("P(coin=tails)"))
d2l.plt.axhline(y=0.5, color='black', linestyle='dashed')
d2l.plt.gca().set_xlabel('Samples')
d2l.plt.gca().set_ylabel('Estimated probability')
d2l.plt.legend();
```
<br>More formally, a ***probability function*** maps events onto real values $𝑃:A⊆S→[0,1]$. That is, the probability, $P(A)$, of an event $A$ in the given sample space $S$ is a real number between 0 and 1 inclusive.
$$P(A=a)=\sum\text{}_v\text{ }{P(A=a,B=v)}$$
For probabilities that continuous variables fall between certain intervals, we use probability densities. For multiple random variables, we involve conditionals.
<br>All formulas concerning expected value, variance and standard deviation apply.

>[!note]- The Central Limit Theorem
>The **Central Limit Theorem (CLT)** is a fundamental principle in statistics that states:
>1. **For any sufficiently large sample size**, the distribution of the sample mean will tend to follow a **normal distribution**, even if the population from which the sample is drawn is not normally distributed.
>2. Specifically, as the sample size increases, the **sampling distribution** of the sample mean becomes approximately normal with a mean equal to the population mean $mu$ and a standard deviation equal to the population standard deviation divided by the square root of the sample size: $$\text{Standard Error} = \frac{\sigma}{\sqrt{n}}$$
>3. This property holds as long as the samples are independent and identically distributed (i.i.d.), and the sample size is sufficiently large (usually \(n \geq 30\) is considered large enough).

