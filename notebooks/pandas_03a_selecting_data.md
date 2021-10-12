---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

<!-- #region -->
<p><font size="6"><b>03 - Pandas: Indexing and selecting data - part I</b></font></p>


> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---
<!-- #endregion -->

```python
import pandas as pd
```

```python
# redefining the example DataFrame

data = {'country': ['Belgium', 'France', 'Germany', 'Netherlands', 'United Kingdom'],
        'population': [11.3, 64.3, 81.3, 16.9, 64.9],
        'area': [30510, 671308, 357050, 41526, 244820],
        'capital': ['Brussels', 'Paris', 'Berlin', 'Amsterdam', 'London']}
countries = pd.DataFrame(data)
countries
```

# Subsetting data


## Subset variables (columns)


For a DataFrame, basic indexing selects the columns (cfr. the dictionaries of pure python)

Selecting a **single column**:

```python
countries['area'] # single []
```

Remember that the same syntax can also be used to *add* a new columns: `df['new'] = ...`.

We can also select **multiple columns** by passing a list of column names into `[]`:

```python
countries[['area', 'population']] # double [[]]
```

## Subset observations (rows)


Using `[]`, slicing or boolean indexing accesses the **rows**:


### Slicing

```python
countries[0:4]
```

### Boolean indexing (filtering)


Often, you want to select rows based on a certain condition. This can be done with *'boolean indexing'* (like a where clause in SQL) and comparable to numpy. 

The indexer (or boolean mask) should be 1-dimensional and the same length as the thing being indexed.

```python
countries['area'] > 100000
```

```python
countries[countries['area'] > 100000]
```

```python
countries[countries['population'] > 50]
```

An overview of the possible comparison operations:

Operator   |  Description
------ | --------
==       | Equal
!=       | Not equal
\>       | Greater than
\>=       | Greater than or equal
\<       | Lesser than
<=       | Lesser than or equal

and to combine multiple conditions:

Operator   |  Description
------ | --------
&       | And (`cond1 & cond2`)
\|       | Or (`cond1 \| cond2`)


<div class="alert alert-info" style="font-size:120%">
<b>REMEMBER</b>: <br><br>

So as a summary, `[]` provides the following convenience shortcuts:

* **Series**: selecting a **label**: `s[label]`
* **DataFrame**: selecting a single or multiple **columns**:`df['col']` or `df[['col1', 'col2']]`
* **DataFrame**: slicing or filtering the **rows**: `df['row_label1':'row_label2']` or `df[mask]`

</div>


## Some other useful methods: `isin` and `string` methods


The `isin` method of Series is very useful to select rows that may contain certain values:

```python
s = countries['capital']
```

```python
s.isin?
```

```python
s.isin(['Berlin', 'London'])
```

This can then be used to filter the dataframe with boolean indexing:

```python
countries[countries['capital'].isin(['Berlin', 'London'])]
```

Let's say we want to select all data for which the capital starts with a 'B'. In Python, when having a string, we could use the `startswith` method:

```python
string = 'Berlin'
```

```python
string.startswith('B')
```

In pandas, these are available on a Series through the `str` namespace:

```python
countries['capital'].str.startswith('B')
```

For an overview of all string methods, see: https://pandas.pydata.org/pandas-docs/stable/reference/series.html#string-handling


# Exercises using the Titanic dataset

```python
df = pd.read_csv("data/titanic.csv")
```

```python
df.head()
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Select all rows for male passengers and calculate the mean age of those passengers. Do the same for the female passengers.</li>
</ul>
</div>

```python clear_cell=true
males = df[df['Sex'] == 'male']
```

```python clear_cell=true
males['Age'].mean()
```

```python clear_cell=true
df[df['Sex'] == 'female']['Age'].mean()
```

We will later see an easier way to calculate both averages at the same time with groupby.


<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>How many passengers older than 70 were on the Titanic?</li>
</ul>
</div>

```python clear_cell=true
len(df[df['Age'] > 70])
```

```python clear_cell=true
(df['Age'] > 70).sum()
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Select the passengers that are between 30 and 40 years old?</li>
</ul>
</div>

```python clear_cell=true
df[(df['Age'] > 30) & (df['Age'] <= 40)]
```

<div class="alert alert-success">

<b>EXERCISE</b>:

Split the 'Name' column on the `,` extract the first part (the surname), and add this as new column 'Surname'.

* Get the first value of the 'Name' column.
* Split this string (check the `split()` method of a string) and get the first element of the resulting list.
* Write the previous step as a function, and 'apply' this function to each element of the 'Name' column (check the `apply()` method of a Series).

</div>

```python clear_cell=true
name = df['Name'][0]
name
```

```python clear_cell=true
name.split(",")
```

```python clear_cell=true
name.split(",")[0]
```

```python clear_cell=true
def get_surname(name):
    return name.split(",")[0]
```

```python clear_cell=true
df['Name'].apply(get_surname)
```

```python clear_cell=true
df['Surname'] = df['Name'].apply(get_surname)
```

```python clear_cell=true
# alternative using an "inline" lambda function
df['Surname'] = df['Name'].apply(lambda x: x.split(',')[0])
```

```python clear_cell=true
# alternative solution with pandas' string methods
df['Surname'] = df['Name'].str.split(",").str.get(0)
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Select all passenger that have a surname starting with 'Williams'.</li>
</ul>
</div>

```python clear_cell=true
df[df['Surname'].str.startswith('Williams')]
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Select all rows for the passengers with a surname of more than 15 characters.</li>
</ul>
    
</div>

```python clear_cell=true
df[df['Surname'].str.len() > 15]
```

# [OPTIONAL] more exercises


For the quick ones among you, here are some more exercises with some larger dataframe with film data. These exercises are based on the [PyCon tutorial of Brandon Rhodes](https://github.com/brandon-rhodes/pycon-pandas-tutorial/) (so all credit to him!) and the datasets he prepared for that. You can download these data from here: [`titles.csv`](https://drive.google.com/open?id=0B3G70MlBnCgKajNMa1pfSzN6Q3M) and [`cast.csv`](https://drive.google.com/open?id=0B3G70MlBnCgKal9UYTJSR2ZhSW8) and put them in the `/notebooks/data` folder.

```python
cast = pd.read_csv('data/cast.csv')
cast.head()
```

```python
titles = pd.read_csv('data/titles.csv')
titles.head()
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>How many movies are listed in the titles dataframe?</li>
</ul>
    
</div>

```python clear_cell=true
len(titles)
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>What are the earliest two films listed in the titles dataframe?</li>
</ul>
</div>

```python clear_cell=true
titles.sort_values('year').head(2)
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>How many movies have the title "Hamlet"?</li>
</ul>
</div>

```python clear_cell=true
len(titles[titles['title'] == 'Hamlet'])
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>List all of the "Treasure Island" movies from earliest to most recent.</li>
</ul>
</div>

```python clear_cell=true
titles[titles['title'] == 'Treasure Island'].sort_values('year')
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>How many movies were made from 1950 through 1959?</li>
</ul>
</div>

```python clear_cell=true
len(titles[(titles['year'] >= 1950) & (titles['year'] <= 1959)])
```

```python clear_cell=true
len(titles[titles['year'] // 10 == 195])
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>How many roles in the movie "Inception" are NOT ranked by an "n" value?</li>
</ul>
</div>

```python clear_cell=true
inception = cast[cast['title'] == 'Inception']
```

```python clear_cell=true
len(inception[inception['n'].isna()])
```

```python clear_cell=true
inception['n'].isna().sum()
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>But how many roles in the movie "Inception" did receive an "n" value?</li>
</ul>
</div>

```python clear_cell=true
len(inception[inception['n'].notna()])
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>Display the cast of the "Titanic" (the most famous 1997 one) in their correct "n"-value order, ignoring roles that did not earn a numeric "n" value.</li>
</ul>
</div>

```python clear_cell=true
titanic = cast[(cast['title'] == 'Titanic') & (cast['year'] == 1997)]
titanic = titanic[titanic['n'].notna()]
titanic.sort_values('n')
```

<div class="alert alert-success">

<b>EXERCISE</b>:

 <ul>
  <li>List the supporting roles (having n=2) played by Brad Pitt in the 1990s, in order by year.</li>
</ul>
</div>

```python clear_cell=true
brad = cast[cast['name'] == 'Brad Pitt']
brad = brad[brad['year'] // 10 == 199]
brad = brad[brad['n'] == 2]
brad.sort_values('year')
```

<!-- #region -->
# Acknowledgement


> The optional exercises are based on the [PyCon tutorial of Brandon Rhodes](https://github.com/brandon-rhodes/pycon-pandas-tutorial/) (so all credit to him!) and the datasets he prepared for that.

---
<!-- #endregion -->
