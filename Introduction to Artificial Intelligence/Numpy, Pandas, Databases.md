>[!todo]- (In-Slide) Announcements
>- Create a developer account on Twitter
>- Install Python connectors for MYSQL and MongoDB

# Numpy
<br>A powerful N-dimensional object of homogeneous type and sophisticated (broadcasting) functions.

```Python
#import library
import numpy as np

# creating a numpy array as a vector:
data = np.array([1, 2, 3])
# creating a numpy array as a matrix
data = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
# creating an array of zeros
data = np.zeros((2, 3), dtype = np.int)
# creating an array of ones
data = ones((2, 3), dtype = np.int)
# creating an n identity array
data = np.identity(4, dtype=int)
```
<br>Floating point arrays default to double precision.

```Python
a = np.array([0, 1.0, 2, 3])  
a.dtype  
# dtype(‘float’)  
a.nbytes  
#output is 32
```
<br>Numpy: Array Slicing

```Python
# indices: 0, 1, 2, 3, 4  
a = np.array([10, 11, 12, 13, 14])  
a[1:3]  
array([11, 12])  

# negative indices work also  
a[1:-2]  
array([11, 12])  
A[-4:3]

# omitted boundaries are assumed to the beginning (or end) of the list  

# grab first three elements  
a[:3]  
array([10, 11, 12])

# grab last two elements  
a[-2:]  
array([13, 14])  

# every other element  
a[::2]  
Array([10, 12, 14])

# slicing can be done with multidimensional arrays, 
a[2::2,1:4:2]
```
<br>Numpy: Array calculation methods  
- Rule 1: Operations between multiple array objects are first checked for proper shape match.  
- Rule 2: Mathematical operators (+, -, \*, /, exp, log, ..) apply element-wise, on the values.  
- Rule 3: Reduction operations(mean, std, skew, kurt, sum, prod, ...) apply to the whole array, unless an axis is specified
- Rule 4: Missing values propagate unless explicitly ignored (nanmean, nansum, ...)

>[!note]+
>Numpy arrays are the best data structure for training AI models, especially in this class.

# <br>Pandas
<br>A popular library for importing and managing datasets in python for Machine Learning.  
`import pandas as pd`
<br>It is used for data analysis, manipulation, and visualization. Pandas is used to build indexed arrays (1D) and Matrices (2D) where column and rows are named and are accessed via the names.

### Reading Data with Pandas

```Python
# use read_table() for data stored online  
df = pd.read_table('https://media.geeksforgeeks.org/wp-content/uploads/nba.csv', delim_whitespace=True)

# use read_csv() for data stored in a CSV file  
df = pd.read_csv(‘data.csv’, index_col = 0)

# use read_excel() for data stores in an excel file  
df = pd.read_excel(‘data.xlsx’, index_col = 0)

# exploring data with pandas  
df.head() # list first 5 columns
```

| Format   | Method, Function, Class |
| -------- | ----------------------- |
| txt, csv | read_table, read_csv    |
| pickle   | read_pickle             |
| HDFS     | read_hdfs, HDFStore     |
| SQL      | read_sql_table          |
| Excel    | read_excel              |
| R(exp)   | rpy.common.load_data    |
### Series and Data frames
<br>A Series is a one-dimensional array that can hold any data type (integer, float, string, Python object, etc.). Elements in the Series are labelled, and the labels are collectively called the index.

**Basic Methods to Create a Series**

```Python
s = pd.Series(data, index=index)  
s = pd.Series([‘Cary’, ‘Lynn’, ‘Sam’], index=[‘n1’,’n2’,’n3’])  

# from an ndarray  
from numpy.random import randn  
s = pd.Series(randn(5), index=[‘a’, ‘b’, ‘c’, ‘d’,‘e’])  

# from an dictionary  
d = {‘n1’: ‘Cary’, ‘n2’: ‘Lynn’, ‘n3’: ‘Sam’}  
s = pd.Series(d, name=‘People’)
```

**Series Attributes and Methods**

```Python
# series value
s.values

# series index
s.index
index = pd.Index([‘n1’, ‘n2’, ‘n3’], dtype=object)

# series name
s.name
s.name = ‘Important People'

# data type
s.dtype

# length
len(s)

# unique values
s.unique()
```
<br>**Series Indexing and Slicing**

```Python
s[‘n2’]

# slice elements
s[1:2]

# slice elements by index. Note: Inclusive of upper bound.
s[‘n2’: ‘n3’]

# indexing by label or integer position. Notation is unambiguous .  
series[integer]: Row position or indel label
l s = pd.Series([1,2,3], index=[3, 0, 2])  
>>> s[0]  
2  
>>> s[1]  
Keyerror

Use special attributes to select rows:  
● loc attribute is purely index label-based  
● iloc attribute is purely integer position based  
>>> s.iloc[0]  
1  
>>> s.loc[0]  
2  
>>> s.iloc[1]  
2  
>>> s.loc[1]  
KeyError: ‘the label [1] is not in the [index]’
```

A **DataFrame** is a two-dimensional labeled data structure, similar to a table or spreadsheet in Excel. It consists of rows and columns, where each column can have a different data type (e.g., integer, float, string) and is essentially a pandas Series.
Unlike Series, which require homogenous types, a DataFrame can have heterogeneity when it comes to the types stored across columns.
<br>Columns have labels, which makes it easier to work with the data. Rows also have labels; collectively, row labels are called ***index***.
<br>**Creating DataFrames**

```Python
# basic creation syntax
df = pd.DataFrame(data, index=labels, column=column_names) # general format
>>> data = [[32, ‘M’], [18, ‘F’] ,[26, ‘M’]]
>>> df = pd.DataFrame(data)

# using dictionaries to create columns
>>> d = {‘Age’: [32, 18, 26], ‘Gender’: [‘M’, ‘F’, ‘M’]}
>>> df = pd.DataFrame(d, index=[‘Cary’, ‘Lynn’, ‘Sam’], columns=[‘Gender’, ‘Age’])
```