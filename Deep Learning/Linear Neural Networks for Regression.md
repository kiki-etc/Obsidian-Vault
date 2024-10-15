Regression problems pop up whenever we want to predict a numerical value.
```Python
%matplotlib inline
import math, time, numpy, torch
from d2l import torch as d2l
```
# Linear Regression
Linear regression flows from a few simple assumptions. First, that the relationship between features x and target y is approximately linear, that the conditional mean $E[Y|X=x]$ can be expressed as a weighted sum of the features x. This setup allows that the target value may still deviate from its expected value on account of observation noise. We also assume that any such noise follows a Gaussian distribution.
We use superscripts to enumerate samples and targets, and subscripts to index coordinates. More concretely, $x^{(𝑖)}$ denotes the 𝑖th sample and $𝑥_j^{(𝑖)}$ denotes its 𝑗th coordinate.
## Model
A linear model is used to transform features into an estimate of the target. The assumption is that the expected value of the target (e.g., price) can be expressed as a **weighted sum of the features** plus a bias term. For example, a formula with area and age as features could look like this: $$\text{price} = w_{\text{area}} \cdot \text{area} + w_{\text{age}} \cdot \text{age} + b$$
$w_{area}$ and $w_{age}$ are **weights**. $b$ is the bias term, representing the offset when all features are zero.
<br>**Affine Transformation:**
An affine transformation refers to a combination of a **linear transformation** and a **translation** (shift). In the context of linear models, this allows us to map input features using a weighted sum and bias (offset). The goal in regression is to choose the weights and bias to minimize the prediction error.
<br>When handling datasets with many features, a **compact linear algebra notation** is used. The prediction can be written as: $$\hat{y} = w_1 x_1 + \dots + w_d x_d + b$$ 
or more compactly: $$     \hat{y} = \mathbf{w}^T \mathbf{x} + b$$
where:
- $\mathbf{x}$ is a vector of features.
- $\mathbf{x}$ is a vector of weights.

When working with multiple examples, we use a **design matrix** $\mathbf{X}$ that has one row per example and one column for each feature. Prediction for the entire dataset is expressed as: $$\hat{y} = \mathbf{Xw} + b$$
where broadcasting handles the addition of $b$ across all examples.
## Loss Function
The **loss function** quantifies how far the predicted output $\hat{y}$ is from the true value $y$.
**Squared Error** loss for a single example $i$: $$l^{(i)}(w, b) = \frac{1}{2} \left( \hat{y}^{(i)} - y^{(i)}\right)^2$$The constant $\frac{1}{2}$ simplifies differentiation and does not affect the final optimization.
For $n$ training examples, the total loss is the **average squared error**: $$L(w, b) = \frac{1}{n} \sum_{i=1}^{n} l^{(i)}(w, b)$$We aim to minimize this function by adjusting the model's weights $w^*$ and bias $b^*$ through optimization such that $$w^*,b^*=\text{argmin }L(w,b)$$
## Analytic Solution
Linear regression provides an optimization problem where we find the optimal parameters analytically by applying a formula as follows:
First, we subsume the bias $b$ into the parameter $\mathbf{w}$ by appending a column to the design matrix consisting of all 1s. Then our prediciton problem is to minimize $||\mathbf{y-Xw}||^2$. As long as the design matrix $\mathbf{X}$ has full rank (no feature is linearly dependent on the others), then there will be just one critical point on the loss surface and it corresponds to the minimum of the loss over the entire domain. Taking the derivative of the loss with respect to $\mathbf{w}$ and setting it equal to zero yields:
$$\delta_w||\mathbf{y-Xw}||^2=2\mathbf{X^T(Xw-y)}=0 \text{ and hence} \mathbf{X^Ty=X^T Xw}$$
Solving for $\mathbf{w}$ provides us with the optimal solution for the optimization problem. This solution
## Minibatch Stochastic Gradient Descent
Although some models are hard to optimize and can't always be solved analytically, they often yield better results when trained effectively. This makes the challenge of optimizing them worthwhile. The key method for optimizing deep learning models is **gradient descent**, where the model's parameters are iteratively updated to reduce the loss function by moving in the direction of the steepest decrease.
<br>There are various forms of gradient descent, from computing derivatives over the entire dataset (which can be slow) to using only a single sample at a time, known as **stochastic gradient descent (SGD)**. However, a more efficient strategy is **minibatch stochastic gradient descent**, where updates are made using small batches of data. This strikes a balance between computational efficiency and statistical performance, typically using batch sizes between 32 and 256, depending on the system's memory and hardware.
<br>In summary, mini-batch SGD proceeds as follows:
1. Initialize the values of the model parameters, typically at random
2. Iteratively sample random mini-batches from the data, updating the parameters in the direction of the negative gradient

When using a mini-batch $B$, we normalize by its size $|B|$, with the minibatch size and learning rate often defined by the user. These tunable parameters, which aren't updated during training, are known as **hyperparameters**. Techniques like **Bayesian optimization** can automatically tune them. After training for a set number of iterations or meeting a stopping criterion, we record the estimated model parameters. Due to randomness in mini-batch selection, the parameters won’t exactly minimize the loss, and convergence may not be exact in a finite number of steps.
<br>In linear regression, there's a global minimum as long as $\mathbf{X}$ is full rank, but deep networks often have complex loss surfaces with many minima and saddle points. Fortunately, the goal isn't to find the exact minimizers but any parameters that produce accurate predictions. In practice, deep learning models typically minimize training set loss effectively, but the real challenge lies in achieving **generalization**, where models perform well on unseen data. This focus on generalization is critical in deep learning, and we'll explore it further throughout the book.
# Object-Oriented Design for Implementation
Despite the simplicity of linear models, training a linear regression model involves processes similar to those used in more complex models. To streamline the implementation, an object-oriented design is proposed, where components are treated as objects. This approach organizes the system into three classes:
1. **Module**, which contains models, loss functions, and optimizers
2. **DataModule**, which manages data loaders for training and validation
3. **Trainer**, which coordinates training on different hardware

```Python
import time, numpy, torch
from torch import nn
from d2l import torch as d2l
```

**Challenges in OOP** in Jupyter notebooks arise from long class definitions, which can reduce readability. To combat this, functions can be registered as methods **after** the class is created, allowing the class implementation to be split into multiple code blocks.  
- Example: A utility function `add_to_class` registers a function as a method of an existing class.
    ```python
    @add_to_class(A)
    def do(self):
        print('Class attribute "b" is', self.b)
    ```
### Hyperparameters Class
- A utility class `HyperParameters` helps to manage and extend the parameters of a class, particularly in the constructor.
- By using `save_hyperparameters`, it saves all arguments passed to the class `__init__` method as attributes.

**Key Use Case**:
```python
    class B(d2l.HyperParameters):
        def __init__(self, a, b, c):
            self.save_hyperparameters(ignore=['c'])
```
- This ensures `a` and `b` are saved as class attributes, while `c` is ignored.
### ProgressBoard
The **ProgressBoard** class helps plot the progress of an experiment during training.
- It plots data points such as losses and validation metrics in real-time during model training, allowing for **interactive tracking**.
- Parameters like `xlabel`, `ylabel`, `xlim`, and `ylim` define the visual aspects of the plot.
Example:
```python
board.draw(x, np.sin(x), 'sin', every_n=2)
board.draw(x, np.cos(x), 'cos', every_n=10)
```
- This will plot the sine and cosine functions with different smoothness.
### Module Class
The `Module` class serves as the **base class** for all models implemented in deep learning projects. It contains:
- **`__init__`**: Initializes learnable parameters.
- **`training_step`**: Processes a batch of data and returns the loss.
- **`configure_optimizers`**: Defines which optimization method (e.g., SGD, Adam) to use for updating parameters.
- **`plot`**: Plots key values (e.g., loss) during training or validation.

**Note**: The `Module` class inherits from `nn.Module`, which is PyTorch’s base class for all neural networks.
### DataModule Class (Base Class for Data)
The `DataModule` class is responsible for handling the **data pipeline**.
  - **`train_dataloader`**: Returns a data loader for the training set.
  - **`val_dataloader`**: Returns a data loader for the validation set.
The dataloaders are **Python generators** that yield batches of data for training or validation.
### Trainer Class (Base Class for Model Training)
The `Trainer` class manages the process of training models on data, using instances of `Module` (model) and `DataModule` (data).
  - **`fit`**: Trains the model for a specified number of epochs.
  - **`prepare_data`**: Prepares the training and validation data loaders.
  - **`prepare_model`**: Prepares the model for training, links it to the trainer, and sets up the progress board for tracking.
  
Example Workflow:
```python
trainer.fit(model, data)
```
This method executes training across multiple epochs and monitors the loss.
### Concise Implementation of the Data Loader
We need a dataset with features X and labels y. Beyond that, we set `batch_size` in the built-in data loader and let it take care of shuffling examples efficiently.<br>
>[!caution]- Linear Regression Implementation (Dataset Loading and Training)
>See Jupyter Notebooks for example implementation. Make sure to run the code and understand all sections of it.

# Generalization
The phenomenon of fitting closer to our training data than to the underlying distribution is called *overfitting* and the techniques for combatting it are (often) called *regularization* methods.
## Training Error vs. Generalization Error
The training error is a statistic calculated on the training dataset and the generalization error is an expectation taken with respect to the underlying distribution. Formally, the training error is expressed as a sum: $$R_{\mathbf{emp}}[\mathbf{X,y,}f] = \frac{1}{n}\sum_{i=1}^{n}l(\mathbf{x}^{(i)},y^{(i)},f(\mathbf{x}^{(i)}))$$ while the generalization error is expressed as an integral: $$R_{[p,f]}=E_{(\mathbf{x},y)~\sim ~P}[l(\mathbf{x},y,f(\mathbf{x}))] = \int\int l(\mathbf{x},y,f(\mathbf{x}))p(\mathbf{x},y)~d\mathbf{x}dy$$
We can never find the generalization error exactly because we never know the true distribution of the infinite streams of unseen data. The central question of generalization is "When should we expect our training error to be close to the population error (and thus the generalization error)?".
This problem of generalization has some relationship with the complexity of our model. Complexity can be measured in diverse ways, for now we focus on the number of parameters and the range of values they're allowed to take.
# Weight Decay
In the standard 