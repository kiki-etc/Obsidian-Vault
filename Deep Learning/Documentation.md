This section provides some guidance for exploring the PyTorch API.
```Python
import torch
```
## Functions and Classes in a Module
To know which functions and classes can be called in a module, we invoke the `dir` function. For example, we can query all properties in the module for generating random numbers:
```Python
print(dir(torch.distributions))
```
Generally, we ignore functions that start and end with `__` (special objects in Python) or functions that start with a single `_` (usually internal functions).
## Specific Functions and Classes
For specific instructions on how to use a given function or class, we can invoke the `help` function. For example, for usage instructions for tensor's `ones` function we can use the following line of code:
```Python
help(torch.ones)
```
From the documentation, we can see that the `ones` function creates a new tensor with the specified shape and sets all the elements to the value of 1.
<br>In the Jupyter notebook, we can use `?` to display the document in another window. For example, `list?` will create content that is almost identical to `help(list)`, displaying it in a new browser window. In addition, if we use two question marks, such as `list??`, the Python code implementing the function will also be displayed.
The official documentation provides plenty of descriptions and examples that are beyond the textbook. It is encouraged to study the source code of the libraries to see examples of high-quality implementations of production code.