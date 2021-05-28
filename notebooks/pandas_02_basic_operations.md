---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.1
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

<p><font size="6"><b> 02 - Pandas: Basic operations on Series and DataFrames</b></font></p>


> *Data wrangling in Python*  
> *November, 2020*
>
> *© 2020, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

```{code-cell} ipython3
import pandas as pd

import numpy as np
import matplotlib.pyplot as plt
```

```{code-cell} ipython3
# redefining the example DataFrame

countries = pd.DataFrame({'country': ['Belgium', 'France', 'Germany', 'Netherlands', 'United Kingdom'],
        'population': [11.3, 64.3, 81.3, 16.9, 64.9],
        'area': [30510, 671308, 357050, 41526, 244820],
        'capital': ['Brussels', 'Paris', 'Berlin', 'Amsterdam', 'London']})
```

```{code-cell} ipython3
countries.head()
```

# Elementwise-operations 

+++

The typical arithmetic (+, -, \*, /) and comparison (==, >, <, ...) operations work *element-wise*.

With as scalar:

```{code-cell} ipython3
population = countries['population']
population
```

```{code-cell} ipython3
population * 1000
```

```{code-cell} ipython3
population > 50
```

With two Series objects:

```{code-cell} ipython3
countries['population'] / countries['area']
```

+++ {"editable": true}

## Adding new columns

We can add a new column to a DataFrame with similar syntax as selecting a columns: create a new column by assigning the output to the DataFrame with a new column name in between the `[]`.

For example, to add the population density calculated above, we can do:

```{code-cell} ipython3
:editable: true

countries['population_density'] = countries['population'] / countries['area'] * 1e6
```

```{code-cell} ipython3
---
editable: true
jupyter:
  outputs_hidden: false
---
countries
```

# Aggregations (reductions)

+++

Pandas provides a large set of **summary** functions that operate on different kinds of pandas objects (DataFrames, Series, Index) and produce single value. When applied to a DataFrame, the result is returned as a pandas Series (one value for each column). 

+++

The average population number:

```{code-cell} ipython3
population.mean()
```

The minimum area:

```{code-cell} ipython3
countries['area'].min()
```

For dataframes, often only the numeric columns are included in the result:

```{code-cell} ipython3
countries.median()
```

# Application on a real dataset

+++

Reading in the titanic data set...

```{code-cell} ipython3
df = pd.read_csv("data/titanic.csv")
```

Quick exploration first...

```{code-cell} ipython3
df.head()
```

```{code-cell} ipython3
len(df)
```

The available metadata of the titanic data set provides the following information:

VARIABLE   |  DESCRIPTION
------ | --------
Survived       | Survival (0 = No; 1 = Yes)
Pclass         | Passenger Class (1 = 1st; 2 = 2nd; 3 = 3rd)
Name           | Name
Sex            | Sex
Age            | Age
SibSp          | Number of Siblings/Spouses Aboard
Parch          | Number of Parents/Children Aboard
Ticket         | Ticket Number
Fare           | Passenger Fare
Cabin          | Cabin
Embarked       | Port of Embarkation (C = Cherbourg; Q = Queenstown; S = Southampton)

+++

<div class="alert alert-success">
<b>EXERCISE</b>:

 <ul>
  <li>What is the average age of the passengers?</li>
</ul>

</div>

```{code-cell} ipython3
:clear_cell: true

df['Age'].mean()
```

<div class="alert alert-success">
<b>EXERCISE</b>:

 <ul>
  <li>Plot the age distribution of the titanic passengers</li>
</ul>
</div>

```{code-cell} ipython3
:clear_cell: true

df['Age'].hist() #bins=30, log=True
```

<div class="alert alert-success">
<b>EXERCISE</b>:

 <ul>
  <li>What is the survival rate? (the relative number of people that survived)</li>
</ul>
<br>

Note: the 'Survived' column indicates whether someone survived (1) or not (0).
</div>

```{code-cell} ipython3
:clear_cell: true

df['Survived'].sum() / len(df['Survived'])
```

```{code-cell} ipython3
:clear_cell: true

df['Survived'].mean()
```

<div class="alert alert-success">
<b>EXERCISE</b>:

 <ul>
  <li>What is the maximum Fare? And the median?</li>
</ul>
</div>

```{code-cell} ipython3
:clear_cell: true

df['Fare'].max()
```

```{code-cell} ipython3
:clear_cell: true

df['Fare'].median()
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Calculate the 75th percentile (`quantile`) of the Fare price (Tip: look in the docstring how to specify the percentile)</li>
</ul>
</div>

```{code-cell} ipython3
:clear_cell: true

df['Fare'].quantile(0.75)
```

<div class="alert alert-success">
<b>EXERCISE</b>:

 <ul>
  <li>Calculate the normalized Fares (normalized relative to its mean), and add this as a new column ('Fare_normalized') to the DataFrame.</li>
</ul>
</div>

```{code-cell} ipython3
:clear_cell: true

df['Fare'] / df['Fare'].mean()
```

```{code-cell} ipython3
:clear_cell: true

df['Fare_normalized'] = df['Fare'] / df['Fare'].mean()
df.head()
```

<div class="alert alert-success">
    
**EXERCISE**:

* Calculate the log of the Fares. Tip: check the `np.log` function.

</div>

```{code-cell} ipython3
:clear_cell: true

np.log(df['Fare'])
```

# Numpy -  multidimensional data arrays

+++

NumPy is the fundamental package for scientific computing with Python. It contains among other things:

* a powerful N-dimensional array/vector/matrix object
* sophisticated (broadcasting) functions
* function implementation in C/Fortran assuring good performance if vectorized
* tools for integrating C/C++ and Fortran code
* useful linear algebra, Fourier transform, and random number capabilities

Also known as *array oriented computing*. The recommended convention to import numpy is:

```{code-cell} ipython3
import numpy as np
```

## Speed

+++

Memory-efficient container that provides fast numerical operations:

```{code-cell} ipython3
L = range(1000)
%timeit [i**2 for i in L]
```

```{code-cell} ipython3
a = np.arange(1000)
%timeit a**2
```

## It's used by Pandas under the hood

+++

The columns of a DataFrame are internally stored using numpy arrays. We can also retrieve this data as numpy arrays, for example using the `to_numpy()` method:

```{code-cell} ipython3
arr = countries["population"].to_numpy()
arr
```

What we said above about element-wise operations and reductions works the same for numpy arrays:

```{code-cell} ipython3
arr + 10
```

```{code-cell} ipython3
arr.mean()
```

Numpy contains more numerical functions than pandas, for example to calculate the log:

```{code-cell} ipython3
np.log(arr)
```

Those functions can *also* be applied on pandas objects:

```{code-cell} ipython3
np.log(countries["population"])
```

<div class="alert alert-info">

__NumPy__ provides

* multi-dimensional, homogeneously typed arrays  (single data type!)

<br>

__Pandas__ provides

* 2D, heterogeneous data structure (multiple data types!)
* labeled (named) row and column index

</div>

+++

# Acknowledgement


> This notebook is partly based on material of Jake Vanderplas (https://github.com/jakevdp/OsloWorkshop2014).

---