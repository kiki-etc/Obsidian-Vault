To represent units, we'll use a library called Pint (<https://pint.readthedocs.io/>). To use it, we have to import a function called `UnitRegistry` and call it like this:

```Python
from pint import UnitRegistry
units = UnitRegistry()
```

Units can then be assigned to variables for cleaner code.

```Python
meter  = units.meter
second = units.second
```

If an object is assigned a value and an accompanying unit, the result is a *quantity* with two parts (magnitude and units), which can be accessed independently.

```Python
a = 9.8 * meter / second**2 # storing acceleration due to gravity with the units
a.magnitude                 # should have '9' as the output
a.units                     # meter/second^2 as the output

```

Values can be converted from one set of units to another.

```Python
v = 100 * meter / second

mile = units.mile
hour = units.hour

v.to(mile/hour)     # 'to' function is used to convert from predefined units
```