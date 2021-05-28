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

+++ {"deletable": true, "editable": true}

<p><font size="6"><b>Visualisation: Seaborn </b></font></p>

> *Data wrangling in Python*  
> *November, 2020*
>
> *© 2020, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: true
---
import pandas as pd
import matplotlib.pyplot as plt
```

+++ {"deletable": true, "editable": true}

# Seaborn

+++ {"deletable": true, "editable": true}

[Seaborn](https://seaborn.pydata.org/) is a Python data visualization library:

* Built on top of Matplotlib, but providing
    1. High level functions.
    2. Support for _tidy data_, which became famous due to the `ggplot2` R package.
    3. Attractive and informative statistical graphics out of the box.
* Interacts well with Pandas

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: true
---
import seaborn as sns
```

+++ {"deletable": true, "editable": true}

## Introduction

+++ {"deletable": true, "editable": true}

We will use the Titanic example data set:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: true
---
titanic = pd.read_csv('data/titanic.csv')
```

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
titanic.head()
```

+++ {"deletable": true, "editable": true}

Let's consider following question:
>*For each class at the Titanic, how many people survived and how many died?*

+++ {"deletable": true, "editable": true}

Hence, we should define the *size/count* of respectively the zeros (died) and ones (survived) groups of column `Survived`, also grouped by the `Pclass`. In Pandas terminology:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
survived_stat = titanic.groupby(["Pclass", "Survived"]).size().rename('count').reset_index()
survived_stat
# Remark: the `rename` syntax is to provide the count column a column name 
```

+++ {"deletable": true, "editable": true}

Providing this data in a bar chart with pure Pandas is still partly supported:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
survived_stat.plot(x='Survived', y='count', kind='bar')
## A possible other way of plotting this could be using groupby again:   
# survived_stat.groupby('Pclass').plot(x='Survived', y='count', kind='bar') # (try yourself by uncommenting)
```

+++ {"deletable": true, "editable": true}

but with mixed results.

+++ {"deletable": true, "editable": true}

__Seaborn__ provides another level of abstraction to visualize such *grouped* plots with different categories:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.catplot(data=survived_stat, 
            x="Survived", y="count", 
            col="Pclass", kind="bar")
```

+++ {"deletable": true, "editable": true}

Moreover, these `count` operations are embedded in Seaborn (similar to other 'Grammar of Graphics' packages such as ggplot in R and plotnine/altair in Python). We can do these operations directly on the original `titanic` data set in a single coding step:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.catplot(data=titanic, 
            x="Survived", 
            col="Pclass", kind="count")
```

+++ {"deletable": true, "editable": true}

Check <a href="#this_is_tidy">here</a> for a short recap about `tidy` data.

+++ {"deletable": true, "editable": true}

<div class="alert alert-info">

**Remember**

- Seaborn is especially suitbale for these so-called <a href="http://vita.had.co.nz/papers/tidy-data.pdf">tidy</a> dataframe representations.
- The [Seaborn tutorial](https://seaborn.pydata.org/tutorial/data_structure.html#long-form-vs-wide-form-data) provides a very good introduction to tidy (also called _long-form_) data. 
- You can use __Pandas column names__ as input for the visualisation functions of Seaborn.

</div>

+++ {"deletable": true, "editable": true}

## Interaction with Matplotlib

+++ {"deletable": true, "editable": true}

Seaborn builds on top of Matplotlib/Pandas, adding an additional layer of convenience. 

Topic-wise, Seaborn provides three main modules, i.e. type of plots:

- __relational__: understanding how variables in a dataset relate to each other
- __distribution__: specialize in representing the distribution of datapoints
- __categorical__: visualize a relationship involving categorical data (i.e. plot something _for each category_)

In 'technical' terms, when working with Seaborn functions, it is important to understand which level they operate, as `axes-level` or `figure-level`: 

- __axes-level__ functions plot data onto a single `matplotlib.pyplot.Axes` object and return the `Axes`
- __figure-level__ functions return a Seaborn object, `FacetGrid`, which is a `matplotlib.pyplot.Figure`

Remember the Matplotlib `Figure`, `axes` and `axis` anatomy explained in [visualization_01_matplotlib](visualization_01_matplotlib.ipynb)? 

Each plot module has a single `Figure`-level function, which offers a unitary interface to its various `Axes`-level functions. The organization looks like this:

+++ {"deletable": true, "editable": true}

![](../img/seaborn_overview_modules.png)

+++ {"deletable": true, "editable": true}

### Figure level functions

+++ {"deletable": true, "editable": true}

Let's start from: _What is the relation between Age and Fare?_

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
# A relation between variables in a Pandas DataFrame -> `relplot`
sns.relplot(data=titanic, x="Age", y="Fare")
```

+++ {"deletable": true, "editable": true}

Extend to: _Is the relation between Age and Fare different for people how survived?_

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.relplot(data=titanic, x="Age", y="Fare",
            hue="Survived")
```

+++ {"deletable": true, "editable": true}

Extend to: _Is the relation between Age and Fare different for people how survived and/or the gender of the passengers?_

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
age_fare = sns.relplot(data=titanic, x="Age", y="Fare",
                       hue="Survived",
                       col="Sex")
```

+++ {"deletable": true, "editable": true}

The function returns a Seaborn `FacetGrid`, which is related to a Matplotlib `Figure`:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
type(age_fare), type(age_fare.fig)
```

+++ {"deletable": true, "editable": true}

As we are dealing here with 2 subplots, the `FacetGrid` consists of two Matplotlib `Axes`:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
age_fare.axes, type(age_fare.axes.flatten()[0])
```

+++ {"deletable": true, "editable": true}

Hence, we can still apply all the power of Matplotlib, but start from the convenience of Seaborn.

+++ {"deletable": true, "editable": true}

<div class="alert alert-info">

**Remember**

The `Figure` level Seaborn functions:    
    
- Support __faceting__ by data variables (split up in subplots using a categorical variable)
- Return a Matplotlib `Figure`, hence the output can NOT be part of a larger Matplotlib Figure

</div>

+++ {"deletable": true, "editable": true}

### Axes level functions

+++ {"deletable": true, "editable": true}

We can ask the same question: _Is the relation between Age and Fare different for people how survived?_

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
scatter_out = sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived")
```

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
type(scatter_out)
```

+++ {"deletable": true, "editable": true}

But we can't use the `col`/`row` options for facetting:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: true
---
# sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived", col="Sex")  # uncomment to check the output
```

+++ {"deletable": true, "editable": true}

We can use these functions to create custom combinations of plots:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
fig, (ax0, ax1) = plt.subplots(1, 2, figsize=(10, 6))
sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived", ax=ax0)
sns.violinplot(data=titanic, x="Survived", y="Fare", ax=ax1)  # boxplot, stripplot,.. as alternative to represent distribution per category
```

+++ {"deletable": true, "editable": true}

__Note!__ Check the similarity with the _best of both worlds_ approach:

1. Prepare with Matplotlib
2. Plot using Seaborn 
3. Further adjust specific elements with Matplotlib if needed

+++ {"deletable": true, "editable": true}

<div class="alert alert-info">

**Remember**

The `Axes` level Seaborn functions:    
    
- Do NOT support faceting by data variables 
- Return a Matplotlib `Axes`, hence the output can be used in combination with other Matplotlib `Axes` in the same `Figure`

</div>

+++ {"deletable": true, "editable": true}

## (OPTIONAL) exercises

+++ {"deletable": true, "editable": true}

<div class="alert alert-success">

**EXERCISE**

- Make a histogram of the age, split up in two subplots by the `Sex` of the passengers.
- Put both subplots underneath each other. 
- Use the `height` and `aspect` arguments of the plot function to adjust the size of the figure.
    
<details><summary>Hints</summary>

- When interested in a histogram, i.e. the distribution of data, use the `displot` module
- A split into subplots is requested using a variable of the DataFrame (facetting), so use the `Figure`-level function instead of the `Axes` level functions.
- Link a column name to the `row` argument for splitting into subplots row-wise.

</details>

```{code-cell} ipython3
---
clear_cell: true
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.displot(data=titanic, x="Age", row="Sex", aspect=3, height=2)
```

+++ {"deletable": true, "editable": true}

<div class="alert alert-success">

**EXERCISE**

Make a violin plot showing the `Age` distribution for each `Sex` in each of the `Pclass` categories:
    
- Use a different color for the `Age`.
- Use the `Pclass` to make a plot for each of the classes along the `x-axis`
- Check the behavior of the `split` argument and apply it to compare male/female.
- Use the `sns.despine` function to remove the boundaries around the plot.    
    
<details><summary>Hints</summary>

- Have a look at https://seaborn.pydata.org/examples/grouped_violinplots.html for inspiration.

</details>

```{code-cell} ipython3
---
clear_cell: true
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
# Figure based
sns.catplot(data=titanic, x="Pclass", y="Age", 
            hue="Sex", split=True,
            palette="Set2", kind="violin")
sns.despine(left=True)
```

```{code-cell} ipython3
---
clear_cell: true
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
# Axes based
sns.violinplot(data=titanic, x="Pclass", y="Age", 
               hue="Sex", split=True,
               palette="Set2")
sns.despine(left=True)
```

+++ {"deletable": true, "editable": true}

## Some more Seaborn functionalities to remember

+++ {"deletable": true, "editable": true}

Whereas the `relplot`, `catplot` and `displot` represent the main components of the Seaborn library, more interesting functions are available. You can check the [gallery](https://seaborn.pydata.org/examples/index.html) yourself, but let's introduce a few rof them:

+++ {"deletable": true, "editable": true}

__jointplot()__ and __pairplot()__

`jointplot()` and `pairplot()` are Figure-level functions and create figures with specific subplots by default:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
# joined distribution plot
sns.jointplot(data=titanic, x="Fare", y="Age", 
              hue="Sex", kind="scatter") # kde
```

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.pairplot(data=titanic[["Age", "Fare", "Sex"]], hue="Sex")  # Also called scattermatrix plot
```

+++ {"deletable": true, "editable": true}

__heatmap()__

+++ {"deletable": true, "editable": true}

Plot rectangular data as a color-encoded matrix.

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
titanic_age_summary = titanic.pivot_table(columns="Pclass", index="Sex", 
                                          values="Age", aggfunc="mean")
titanic_age_summary
```

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
sns.heatmap(titanic_age_summary, cmap="Reds")
```

+++ {"deletable": true, "editable": true}

__lmplot() regressions__

+++ {"deletable": true, "editable": true}

`Figure` level function to generate a regression model fit across a FacetGrid:

```{code-cell} ipython3
---
deletable: true
editable: true
jupyter:
  outputs_hidden: false
---
g = sns.lmplot(
    data=titanic, x="Age", y="Fare", 
    hue="Survived", col="Survived",  # hue="Pclass"
)
```

+++ {"deletable": true, "editable": true}

# Need more Seaborn inspiration? 

+++ {"deletable": true, "editable": true}

<div class="alert alert-info" style="font-size:18px">

__Remember__

[Seaborn gallery](https://seaborn.pydata.org/examples/index.html) and package [documentation](https://seaborn.pydata.org/index.html)

</div>

+++ {"deletable": true, "editable": true}

<a id='this_is_tidy'></a>

+++ {"deletable": true, "editable": true}

# Recap: what is `tidy`?

+++ {"deletable": true, "editable": true}

If you're wondering what *tidy* data representations are, you can read the scientific paper by Hadley Wickham, http://vita.had.co.nz/papers/tidy-data.pdf. 

Here, we just introduce the main principle very briefly:

+++ {"deletable": true, "editable": true}

Compare:

#### un-tidy
        
| WWTP | Treatment A | Treatment B |
|:------|-------------|-------------|
| Destelbergen | 8.  | 6.3 |
| Landegem | 7.5  | 5.2 |
| Dendermonde | 8.3  | 6.2 |
| Eeklo | 6.5  | 7.2 |

*versus*

#### tidy

| WWTP | Treatment | pH |
|:------|:-------------:|:-------------:|
| Destelbergen | A  | 8. |
| Landegem | A  | 7.5 |
| Dendermonde | A  | 8.3 |
| Eeklo | A  | 6.5 |
| Destelbergen | B  | 6.3 |
| Landegem | B  | 5.2 |
| Dendermonde | B  | 6.2 |
| Eeklo | B  | 7.2 |

+++ {"deletable": true, "editable": true}

This is sometimes also referred as *short* versus *long* format for a specific variable... Seaborn (and other grammar of graphics libraries) work better on `tidy` (long format) data, as it better supports `groupby`-like transactions!

+++ {"deletable": true, "editable": true}

<div class="alert alert-info" style="font-size:16px">

**Remember:**

A tidy data set is setup as follows:
 
- Each <code>variable</code> forms a <b>column</b> and contains <code>values</code>
- Each <code>observation</code> forms a <b>row</b>
- Each type of <code>observational unit</code> forms a <b>table</b>.

</div>