When modeling, it is helpful to keep track of physical properties of a system. For this, we can use the State object, defined in the ModSim library.
<br>An example of initializing a State object with its "member variables":

```Python
bikeshare = State(olin=10, wellesley=2)
bikeshare.olin       # accesses the olin property, output should be 10
bikeshare.wellesley  # accesses the wellesley property, output should be 2
```
<br>The ModSim library provides a function called `show` that displays a `State` object as a table.

```Python
show(bikeshare)
```

### `flip` and Randomization
<br>The ModSim library provides a function called `flip` that generates random sequences of boolean values `True` and `False`. When you call it, you provide a probability between 0 and 1.

```Python
np.random.seed(20) # this line sets the generator so the results are the same every time we run it

if flip(0.5):
	print('heads')
```


