---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.13.0
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

<p><font size="6"><b>Visualisation: Seaborn </b></font></p>


> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

```{code-cell} ipython3
:tags: []

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

# Seaborn

+++

[Seaborn](https://seaborn.pydata.org/) is a Python data visualization library:

* Built on top of Matplotlib, but providing
    1. High level functions.
    2. Support for _tidy data_, which became famous due to the `ggplot2` R package.
    3. Attractive and informative statistical graphics out of the box.
* Interacts well with Pandas

```{code-cell} ipython3
:tags: []

import seaborn as sns
```

## Introduction

+++

We will use the Titanic example data set:

```{code-cell} ipython3
:tags: []

titanic = pd.read_csv('data/titanic.csv')
```

```{code-cell} ipython3
titanic.head()
```

Let's consider following question:
>*For each class at the Titanic and each gender, what was the average age?*

+++

Hence, we should define the *mean* of the male and female groups of column `Survived` in combination with the groups of the `Pclass` column. In Pandas terminology:

```{code-cell} ipython3
survived_stat = titanic.groupby(["Pclass", "Sex"])["Age"].mean().reset_index()
survived_stat
```

Providing this data in a bar chart with pure Pandas is still partly supported:

```{code-cell} ipython3
survived_stat.plot(x='Pclass', y='Age', kind='bar')
## A possible other way of plotting this could be using groupby again:   
#survived_stat.groupby('Pclass').plot(x='Sex', y='Age', kind='bar') # (try yourself by uncommenting)
```

but with mixed results.

+++

__Seaborn__ provides another level of abstraction to visualize such *grouped* plots with different categories:

```{code-cell} ipython3
sns.catplot(data=survived_stat, 
            x="Sex", y="Age", 
            col="Pclass", kind="bar")
```

Check <a href="#this_is_tidy">here</a> for a short recap about `tidy` data.

+++

<div class="alert alert-info">

**Remember**

- Seaborn is especially suitbale for these so-called <a href="http://vita.had.co.nz/papers/tidy-data.pdf">tidy</a> dataframe representations.
- The [Seaborn tutorial](https://seaborn.pydata.org/tutorial/data_structure.html#long-form-vs-wide-form-data) provides a very good introduction to tidy (also called _long-form_) data. 
- You can use __Pandas column names__ as input for the visualisation functions of Seaborn.

</div>

+++

## Interaction with Matplotlib

+++

Seaborn builds on top of Matplotlib/Pandas, adding an additional layer of convenience. 

Topic-wise, Seaborn provides three main modules, i.e. type of plots:

- __relational__: understanding how variables in a dataset relate to each other
- __distribution__: specialize in representing the distribution of datapoints
- __categorical__: visualize a relationship involving categorical data (i.e. plot something _for each category_)

The organization looks like this:

+++

![](../img/seaborn_overview_modules.png)

+++

We first check out the top commands of each of the types of plots: `relplot`, `displot`, `catplot`, each returning a Matplotlib `Figure`:

+++

### Figure level functions

+++

Let's start from: _What is the relation between Age and Fare?_

```{code-cell} ipython3
# A relation between variables in a Pandas DataFrame -> `relplot`
sns.relplot(data=titanic, x="Age", y="Fare")
```

Extend to: _Is the relation between Age and Fare different for people how survived?_

```{code-cell} ipython3
sns.relplot(data=titanic, x="Age", y="Fare",
            hue="Survived")
```

Extend to: _Is the relation between Age and Fare different for people how survived and/or the gender of the passengers?_

```{code-cell} ipython3
age_fare = sns.relplot(data=titanic, x="Age", y="Fare",
                       hue="Survived",
                       col="Sex")
```

The function returns a Seaborn `FacetGrid`, which is related to a Matplotlib `Figure`:

```{code-cell} ipython3
type(age_fare), type(age_fare.fig)
```

As we are dealing here with 2 subplots, the `FacetGrid` consists of two Matplotlib `Axes`:

```{code-cell} ipython3
age_fare.axes, type(age_fare.axes.flatten()[0])
```

Hence, we can still apply all the power of Matplotlib, but start from the convenience of Seaborn.

+++

<div class="alert alert-info">

**Remember**

The `Figure` level Seaborn functions:    
    
- Support __faceting__ by data variables (split up in subplots using a categorical variable)
- Return a Matplotlib `Figure`, hence the output can NOT be part of a larger Matplotlib Figure

</div>

+++ {"tags": []}

### Axes level functions

+++

In 'technical' terms, when working with Seaborn functions, it is important to understand which level they operate, as `Axes-level` or `Figure-level`: 

- __axes-level__ functions plot data onto a single `matplotlib.pyplot.Axes` object and return the `Axes`
- __figure-level__ functions return a Seaborn object, `FacetGrid`, which is a `matplotlib.pyplot.Figure`

Remember the Matplotlib `Figure`, `axes` and `axis` anatomy explained in [visualization_01_matplotlib](visualization_01_matplotlib.ipynb)? 

Each plot module has a single `Figure`-level function (top command in the scheme), which offers a unitary interface to its various `Axes`-level functions (.

+++

We can ask the same question: _Is the relation between Age and Fare different for people how survived?_

```{code-cell} ipython3
scatter_out = sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived")
```

```{code-cell} ipython3
type(scatter_out)
```

But we can't use the `col`/`row` options for facetting:

```{code-cell} ipython3
:tags: []

# sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived", col="Sex")  # uncomment to check the output
```

We can use these functions to create custom combinations of plots:

```{code-cell} ipython3
fig, (ax0, ax1) = plt.subplots(1, 2, figsize=(10, 6))
sns.scatterplot(data=titanic, x="Age", y="Fare", hue="Survived", ax=ax0)
sns.violinplot(data=titanic, x="Survived", y="Fare", ax=ax1)  # boxplot, stripplot,.. as alternative to represent distribution per category
```

__Note!__ Check the similarity with the _best of both worlds_ approach:

1. Prepare with Matplotlib
2. Plot using Seaborn 
3. Further adjust specific elements with Matplotlib if needed

+++

<div class="alert alert-info">

**Remember**

The `Axes` level Seaborn functions:    
    
- Do NOT support faceting by data variables 
- Return a Matplotlib `Axes`, hence the output can be used in combination with other Matplotlib `Axes` in the same `Figure`

</div>

+++

### Summary statistics

+++

Aggregations such as `count`, `mean` are embedded in Seaborn (similar to other 'Grammar of Graphics' packages such as ggplot in R and plotnine/altair in Python). We can do these operations directly on the original `titanic` data set in a single coding step:

```{code-cell} ipython3
sns.catplot(data=titanic, x="Survived", col="Pclass", 
            kind="count")
```

To use another statistical function to apply on each of the groups, use the `estimator`:

```{code-cell} ipython3
sns.catplot(data=titanic, x="Sex", y="Age", col="Pclass", kind="bar", 
            estimator=np.mean)
```

## (OPTIONAL) exercises

+++

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

+++

## (OPTIONAL) exercises

+++

<div class="alert alert-success">

**EXERCISE**

Make a violin plot showing the Age distribution for each Sex in each of the Pclass categories:

- Use a different color for the Sex.
- Use the Pclass to make a plot for each of the classes along the x-axis
- Check the behavior of the split argument and apply it to compare male/female.
- Use the sns.despine function to remove the boundaries around the plot.
    
<details><summary>Hints</summary>

- Have a look at https://seaborn.pydata.org/examples/grouped_violinplots.html for inspiration.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

sns.displot(data=titanic, x="Age", row="Sex", aspect=3, height=2)
```

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
:tags: [nbtutor-solution]

# Figure based
sns.catplot(data=titanic, x="Pclass", y="Age", 
            hue="Sex", split=True,
            palette="Set2", kind="violin")
sns.despine(left=True)
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

# Axes based
sns.violinplot(data=titanic, x="Pclass", y="Age", 
               hue="Sex", split=True,
               palette="Set2")
sns.despine(left=True)
```

## Some more Seaborn functionalities to remember

+++

Whereas the `relplot`, `catplot` and `displot` represent the main components of the Seaborn library, more useful functions are available. You can check the [gallery](https://seaborn.pydata.org/examples/index.html) yourself, but let's introduce a few rof them:

+++

__jointplot()__ and __pairplot()__

`jointplot()` and `pairplot()` are Figure-level functions and create figures with specific subplots by default:

```{code-cell} ipython3
# joined distribution plot
sns.jointplot(data=titanic, x="Fare", y="Age", 
              hue="Sex", kind="scatter") # kde
```

```{code-cell} ipython3
sns.pairplot(data=titanic[["Age", "Fare", "Sex"]], hue="Sex")  # Also called scattermatrix plot
```

__heatmap()__

+++

Plot rectangular data as a color-encoded matrix.

```{code-cell} ipython3
titanic_age_summary = titanic.pivot_table(columns="Pclass", index="Sex", 
                                          values="Age", aggfunc="mean")
titanic_age_summary
```

```{code-cell} ipython3
sns.heatmap(titanic_age_summary, cmap="Reds")
```

__lmplot() regressions__

+++

`Figure` level function to generate a regression model fit across a FacetGrid:

```{code-cell} ipython3
g = sns.lmplot(
    data=titanic, x="Age", y="Fare", 
    hue="Survived", col="Survived",  # hue="Pclass"
)
```

# Need more Seaborn inspiration?

+++

<div class="alert alert-info" style="font-size:18px">

__Remember__

[Seaborn gallery](https://seaborn.pydata.org/examples/index.html) and package [documentation](https://seaborn.pydata.org/index.html)

</div>

+++

<a id='this_is_tidy'></a>

+++

# Recap: what is `tidy`?

+++

If you're wondering what *tidy* data representations are, you can read the scientific paper by Hadley Wickham, http://vita.had.co.nz/papers/tidy-data.pdf. 

Here, we just introduce the main principle very briefly:

+++

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

+++

This is sometimes also referred as *short* versus *long* format for a specific variable... Seaborn (and other grammar of graphics libraries) work better on `tidy` (long format) data, as it better supports `groupby`-like transactions!

+++

<div class="alert alert-info" style="font-size:16px">

**Remember:**

A tidy data set is setup as follows:
 
- Each <code>variable</code> forms a <b>column</b> and contains <code>values</code>
- Each <code>observation</code> forms a <b>row</b>
- Each type of <code>observational unit</code> forms a <b>table</b>.

</div>
