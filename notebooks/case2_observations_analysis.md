---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.13.8
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

<p><font size="6"><b> CASE - Observation data - analysis</b></font></p>

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

```{code-cell} ipython3
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

plt.style.use('seaborn-whitegrid')
```

## 1. Reading in the enriched observations data

+++

<div class="alert alert-success">

**EXERCISE**

- Read in the `survey_data_completed.csv` file and save the resulting `DataFrame` as variable `survey_data_processed` (if you did not complete the previous notebook, a version of the csv file is available in the `data` folder).
- Interpret the 'eventDate' column directly as python `datetime` objects and make sure the 'occurrenceID' column is used as the index of the resulting DataFrame (both can be done at once when reading the csv file using parameters of the `read_csv` function)
- Inspect the first five rows of the DataFrame and the data types of each of the data columns. Verify that the 'eventDate' indeed has a datetime data type.

<details><summary>Hints</summary>

- All read functions in Pandas start with `pd.read_...`.
- To check the documentation of a function, use the keystroke combination of SHIFT + TAB when the cursor is on the function.
- Remember `.head()` and `.info()`?

</details>

</div>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_processed = pd.read_csv("data/survey_data_completed.csv",
                                    parse_dates=['eventDate'], index_col="occurrenceID")
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_processed.head()
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_processed.info()
```

## 2. Tackle missing values (NaN) and duplicate values

+++

See [pandas_07_missing_values.ipynb](pandas_07_missing_values.ipynb) for an overview of functionality to work with missing values.

+++

<div class="alert alert-success">

**EXERCISE**

How many records in the data set have no information about the `species`? Use the `isna()` method to find out.

<details><summary>Hints</summary>

- Do NOT use `survey_data_processed['species'] == np.nan`, but use the available method `isna()` to check if a value is NaN
- The result of an (element-wise) condition returns a set of True/False values, corresponding to 1/0 values. The amount of True values is equal to the sum.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_processed['species'].isna().sum()
```

<div class="alert alert-success">

**EXERCISE**

How many duplicate records are present in the dataset? Use the method `duplicated()` to check if a row is a duplicate.

<details><summary>Hints</summary>

- The result of an (element-wise) condition returns a set of True/False values, corresponding to 1/0 values. The amount of True values is equal to the sum.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_processed.duplicated().sum()
```

<div class="alert alert-success">

**EXERCISE**

- Select all duplicate data by filtering the `observations` data and assign the result to a new variable `duplicate_observations`. The `duplicated()` method provides a `keep` argument define which duplicates (if any) to mark.
- Sort the `duplicate_observations` data on both the columns `eventDate` and `verbatimLocality` and show the first 9 records.

<details><summary>Hints</summary>

- Check the documentation of the `duplicated` method to find out which value the argument `keep` requires to select all duplicate data.
- `sort_values()` can work with a single columns name as well as a list of names.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

duplicate_observations = survey_data_processed[survey_data_processed.duplicated(keep=False)]
duplicate_observations.sort_values(["eventDate", "verbatimLocality"]).head(9)
```

<div class="alert alert-success">

**EXERCISE**

- Exclude the duplicate values (i.e. keep the first occurrence while removing the other ones) from the `observations` data set and save the result as `survey_data_unique`. Use the `drop duplicates()` method from Pandas.
- How many observations are still left in the data set?

<details><summary>Hints</summary>

- `keep=First` is the default option for `drop_duplicates`
- The number of rows in a DataFrame is equal to the `len`gth

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data_unique = survey_data_processed.drop_duplicates()
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

len(survey_data_unique)
```

<div class="alert alert-success">

**EXERCISE**

Use the `dropna()` method to find out:

- For how many observations (rows) we have all the information available (i.e. no NaN values in any of the columns)?
- For how many observations (rows) we do have the `species` data available ?

<details><summary>Hints</summary>

- `dropna` by default removes by default all rows for which _any_ of the columns contains a `NaN` value.
- To specify which specific columns to check, use the `subset` argument

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

len(survey_data_unique.dropna()), len(survey_data_unique.dropna(subset=['species']))
```

<div class="alert alert-success">

**EXERCISE**

Filter the  `survey_data_unique` data and select only those records that do not have a `species` while having information on the `sex`. Store the result as variable `not_identified`.

<details><summary>Hints</summary>

- To combine logical operators element-wise in Pandas, use the `&` operator.
- Pandas provides both a `isna()` and a `notna()` method to check the existence of `NaN` values.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

mask = survey_data_unique['species'].isna() & survey_data_unique['sex'].notna()
not_identified = survey_data_unique[mask]
```

```{code-cell} ipython3
not_identified.head()
```

__NOTE!__

The `DataFrame` we will use in the further analyses contains species information:

```{code-cell} ipython3
survey_data = survey_data_unique.dropna(subset=['species']).copy()
survey_data['name'] = survey_data['genus'] + ' ' + survey_data['species']
```

<div class="alert alert-info">

**INFO**

For biodiversity studies, absence values (knowing that something is not present) are useful as well to normalize the observations, but this is out of scope for these exercises.
</div>

+++

## 3. Select subsets of the data

```{code-cell} ipython3
survey_data['taxa'].value_counts()
#survey_data.groupby('taxa').size()
```

<div class="alert alert-success">

**EXERCISE**

- Select the observations for which the `taxa` is equal to 'Rabbit', 'Bird' or 'Reptile'. Assign the result to a variable `non_rodent_species`. Use the `isin` method for the selection.

<details><summary>Hints</summary>

- You do not have to combine three different conditions, but use the `isin` operator with a list of names.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

non_rodent_species = survey_data[survey_data['taxa'].isin(['Rabbit', 'Bird', 'Reptile'])]
non_rodent_species.head()
```

```{code-cell} ipython3
len(non_rodent_species)
```

<div class="alert alert-success">

**EXERCISE**

Select the observations for which the `name` starts with the characters 'r' (make sure it does not matter if a capital character is used in the 'taxa' name). Call the resulting variable `r_species`.

<details><summary>Hints</summary>

- Remember the `.str.` construction to provide all kind of string functionalities? You can combine multiple of these after each other.
- If the presence of capital letters should not matter, make everything lowercase first before comparing (`.lower()`)

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

r_species = survey_data[survey_data['name'].str.lower().str.startswith('r')]
r_species.head()
```

```{code-cell} ipython3
len(r_species)
```

```{code-cell} ipython3
r_species["name"].value_counts()
```

<div class="alert alert-success">

**EXERCISE**

Select the observations that are not Birds. Call the resulting variable <code>non_bird_species</code>.

<details><summary>Hints</summary>

- Logical operators like `==`, `!=`, `>`,... can still be used.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

non_bird_species = survey_data[survey_data['taxa'] != 'Bird']
non_bird_species.head()
```

```{code-cell} ipython3
len(non_bird_species)
```

<div class="alert alert-success">

**EXERCISE**

Select the __Bird__ (taxa is Bird) observations from 1985-01 till 1989-12 using the `eventDate` column. Call the resulting variable `birds_85_89`.

<details><summary>Hints</summary>

- No hints, you can do this! (with the help of some `<=` and `&`, and don't forget the put brackets around each comparison that you combine)


</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

birds_85_89 = survey_data[(survey_data["eventDate"] >= "1985-01-01")
                          & (survey_data["eventDate"] <= "1989-12-31 23:59")
                          & (survey_data['taxa'] == 'Bird')]
birds_85_89.head()
```

Alternative solution:

```{code-cell} ipython3
:tags: [nbtutor-solution]

# alternative solution
birds_85_89 = survey_data[(survey_data["eventDate"].dt.year >= 1985)
                          & (survey_data["eventDate"].dt.year <= 1989)
                          & (survey_data['taxa'] == 'Bird')]
birds_85_89.head()
```

<div class="alert alert-success">

**EXERCISE**

- Drop the observations for which no 'weight' (`wgt` column) information is available.
- On the filtered data, compare the median weight for each of the species (use the `name` column)
- Sort the output from high to low median weight (i.e. descending)

__Note__ You can do this all in a single line statement, but don't have to do it as such!

<details><summary>Hints</summary>

- You will need `dropna`, `groupby`, `median` and `sort_values`.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

# Multiple lines
obs_with_weight = survey_data.dropna(subset=["wgt"])
median_weight = obs_with_weight.groupby(['name'])["wgt"].median()
median_weight.sort_values(ascending=False)
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

# Single line statement
(survey_data
     .dropna(subset=["wgt"])
     .groupby(['name'])["wgt"]
     .median()
     .sort_values(ascending=False)
)
```

## 4. Species abundance

+++

<div class="alert alert-success">

**EXERCISE**

Which 8 species (use the `name` column to identify the different species) have been observed most over the entire data set?

<details><summary>Hints</summary>

- Pandas provide a function to combine sorting and showing the first n records, see [here](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.nlargest.html)...

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data.groupby("name").size().nlargest(8)
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data['name'].value_counts()[:8]
```

<div class="alert alert-success">

**EXERCISE**

- What is the number of different species (`name`) in each of the `verbatimLocality` plots? Use the `nunique` method. Assign the output to a new variable `n_species_per_plot`.
- Define a Matplotlib `Figure` (`fig`) and `Axes` (`ax`) to prepare a plot. Make an horizontal bar chart using Pandas `plot` function linked to the just created Matplotlib `ax`. Each bar represents the `species per plot/verbatimLocality`. Change the y-label to 'Plot number'.

<details><summary>Hints</summary>

- _...in each of the..._ should provide a hint to use `groupby` for this exercise. The `nunique` is the aggregation function for each of the groups.
- `fig, ax = plt.subplots()` prepares a Matplotlib Figure and Axes.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

n_species_per_plot = survey_data.groupby(["verbatimLocality"])["name"].nunique()

fig, ax = plt.subplots(figsize=(6, 6))
n_species_per_plot.plot(kind="barh", ax=ax, color="lightblue")
ax.set_ylabel("plot number")

# Alternative option:
# inspired on the pivot table we already had:
# species_per_plot = survey_data.reset_index().pivot_table(
#     index="name", columns="verbatimLocality", values="occurrenceID", aggfunc='count')
# n_species_per_plot = species_per_plot.count()
```

<div class="alert alert-success">

**EXERCISE**

- What is the number of plots (`verbatimLocality`) each of the species (`name`) have been observed in? Assign the output to a new variable `n_plots_per_species`. Sort the counts from low to high.
- Make an horizontal bar chart using Pandas `plot` function to show the number of plots each of the species was found (using the `n_plots_per_species` variable).

<details><summary>Hints</summary>

- Use the previous exercise to solve this one.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

n_plots_per_species = survey_data.groupby(["name"])["verbatimLocality"].nunique().sort_values()

fig, ax = plt.subplots(figsize=(8, 8))
n_plots_per_species.plot(kind="barh", ax=ax, color='0.4')
ax.set_xlabel("Number of plots");
ax.set_ylabel("");
```

<div class="alert alert-success">

**EXERCISE**

- Starting from the `survey_data`, calculate the amount of males and females present in each of the plots (`verbatimLocality`). The result should return the counts for each of the combinations of `sex` and `verbatimLocality`. Assign to a new variable `n_plot_sex` and ensure the counts are in a column named "count".
- Use `pivot` to convert the `n_plot_sex` DataFrame to a new DataFrame with the `verbatimLocality` as index and `male`/`female` as column names. Assign to a new variable `pivoted`.

<details><summary>Hints</summary>

- _...for each of the combinations..._ `groupby` can also be used with multiple columns at the same time.
- If a `groupby` operation gives a Series as result, you can give that Series a name with the `.rename(..)` method.
- `reset_index()` is useful function to convert multiple indices into columns again.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

n_plot_sex = survey_data.groupby(["sex", "verbatimLocality"]).size().rename("count").reset_index()
n_plot_sex.head()
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

pivoted = n_plot_sex.pivot(columns="sex", index="verbatimLocality", values="count")
```

```{code-cell} ipython3
pivoted.head()
```

To check, we can use the variable `pivoted` to plot the result:

```{code-cell} ipython3
pivoted.plot(kind='bar', figsize=(12, 6), rot=0)
```

<div class="alert alert-success">

**EXERCISE**

Recreate the previous plot with the `catplot` function from the Seaborn library starting from `n_plot_sex`.

<details><summary>Hints</summary>

- Check the `kind` argument of the `catplot` function to figure out to specify you want a barplot with given x and y values.
- To link a column to different colors, use the `hue` argument
- Using `height` and `aspect`, the figure size can be optimized.


</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

sns.catplot(data=n_plot_sex, x="verbatimLocality", y="count",
            hue="sex", kind="bar", height=3, aspect=3)
```

<div class="alert alert-success">

**EXERCISE**

Recreate the previous plot with the `catplot` function from the Seaborn library directly starting from `survey_data`.

<details><summary>Hints</summary>

- Check the `kind`argument of the `catplot` function to find out how to use counts to define the bars instead of a `y` value.
- To link a column to different colors, use the `hue` argument


</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

sns.catplot(data=survey_data, x="verbatimLocality",
            hue="sex", kind="count", height=3, aspect=3)
```

<div class="alert alert-success">

**EXERCISE**

- Make a summary table with the number of records of each of the species in each of the plots (also called `verbatimLocality`). Each of the species `name`s is a row index and each of the `verbatimLocality` plots is a column name.
- Using the Seaborn <a href="http://seaborn.pydata.org/generated/seaborn.heatmap.html">documentation</a> to make a heatmap.

<details><summary>Hints</summary>

- Make sure to pass the correct columns to respectively the `index`, `columns`, `values` and `aggfunc` parameters of the `pivot_table` function. You can use the `datasetName` to count the number of observations for each name/locality combination (when counting rows, the exact column doesn't matter).

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

species_per_plot = survey_data.pivot_table(index="name",
                                           columns="verbatimLocality",
                                           values="datasetName",
                                           aggfunc='count')

# alternative ways to calculate this
#species_per_plot = survey_data.groupby(['name', 'verbatimLocality']).size().unstack(level=-1)
#pecies_per_plot = pd.crosstab(survey_data['name'], survey_data['verbatimLocality'])
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

fig, ax = plt.subplots(figsize=(8,8))
sns.heatmap(species_per_plot, ax=ax, cmap='Greens')
```

## 5. Observations over time

+++

<div class="alert alert-success">

**EXERCISE**

Make a plot visualizing the evolution of the number of observations for each of the individual __years__ (i.e. annual counts) using the `resample` method.

<details><summary>Hints</summary>

- You want to `resample` the data using the `eventDate` column to create annual counts. If the index is not a datetime-index, you can use the `on=` keyword to specify which datetime column to use.
- `resample` needs an aggregation function on how to combine the values within a single 'group' (in this case data within a year). In this example, we want to know the `size` of each group, i.e. the number of records within each year.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

survey_data.resample('A', on='eventDate').size().plot()
```

To evaluate the intensity or number of occurrences during different time spans, a heatmap is an interesting representation.

+++

<div class="alert alert-success">

**EXERCISE**

- Create a table, called `heatmap_prep`, based on the `survey_data` DataFrame with the row index the individual years, in the column the months of the year (1-> 12) and as values of the table, the counts for each of these year/month combinations.
- Using the seaborn <a href="http://seaborn.pydata.org/generated/seaborn.heatmap.html">documentation</a>, make a heatmap starting from the `heatmap_prep` variable.

<details><summary>Hints</summary>

- The `.dt` accessor can be used to get the `year`, `month`,... from a `datetime` column
- Use `pivot_table` and provide the years to `index` and the months to `columns`. Do not forget to `count` the number for each combination (`aggfunc`).
- Seaborn has an `heatmap` function which requires a short-form DataFrame, comparable to giving each element in a table a color value.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

heatmap_prep = survey_data.pivot_table(index=survey_data['eventDate'].dt.year,
                                       columns=survey_data['eventDate'].dt.month,
                                       values='species', aggfunc='count')
fig, ax = plt.subplots(figsize=(10, 8))
ax = sns.heatmap(heatmap_prep, cmap='Reds')
```

Remark that we started from a `tidy` data format (also called *long* format) and converted to *short* format with in the row index the years, in the column the months and the counts for each of these year/month combinations as values.

+++

## (OPTIONAL SECTION) 6. Evolution of species during monitoring period

+++

*In this section, all plots can be made with the embedded Pandas plot function, unless specificly asked*

+++

<div class="alert alert-success">

**EXERCISE**

Plot using Pandas `plot` function the number of records for `Dipodomys merriami` for each month of the year (January (1) -> December (12)), aggregated over all years.

<details><summary>Hints</summary>

- _...for each month of..._ requires `groupby`.
- `resample` is not useful here, as we do not want to change the time-interval, but look at month of the year (over all years)

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

merriami = survey_data[survey_data["name"] == "Dipodomys merriami"]
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

fig, ax = plt.subplots()
merriami.groupby(merriami['eventDate'].dt.month).size().plot(kind="barh", ax=ax)
ax.set_xlabel("number of occurrences");
ax.set_ylabel("Month of the year");
```

<div class="alert alert-success">

**EXERCISE**

Plot, for the species 'Dipodomys merriami', 'Dipodomys ordii', 'Reithrodontomys megalotis' and 'Chaetodipus baileyi', the monthly number of records as a function of time during the monitoring period. Plot each of the individual species in a separate subplot and provide them all with the same y-axis scale

<details><summary>Hints</summary>

- `isin` is useful to select from within a list of elements.
- `groupby` AND `resample` need to be combined. We do want to change the time-interval to represent data as a function of time (`resample`) and we want to do this _for each name/species_ (`groupby`). The order matters!
- `unstack` is a Pandas function a bit similar to `pivot`. Check the [unstack documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.unstack.html) as it might be helpful for this exercise.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

subsetspecies = survey_data[survey_data["name"].isin(['Dipodomys merriami', 'Dipodomys ordii',
                                                      'Reithrodontomys megalotis', 'Chaetodipus baileyi'])]
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

month_evolution = subsetspecies.groupby("name").resample('M', on='eventDate').size()
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

species_evolution = month_evolution.unstack(level=0)
axs = species_evolution.plot(subplots=True, figsize=(14, 8), sharey=True)
```

<div class="alert alert-success">

**EXERCISE**

Recreate the same plot as in the previous exercise using Seaborn `relplot` functon with the `month_evolution` variable.
    
<details><summary>Hints</summary>

- We want to have the `counts` as a function of `eventDate`, so link these columns to y and x respectively.
- To create subplots in Seaborn, the usage of _facetting_ (splitting data sets to multiple facets) is used by linking a column name to the `row`/`col` parameter. 
- Using `height` and `aspect`, the figure size can be optimized.
    
</details>

+++

Uncomment the next cell (calculates `month_evolution`, the intermediate result of the previous excercise):

```{code-cell} ipython3
:tags: [nbtutor-solution]

# Given as solution..
subsetspecies = survey_data[survey_data["name"].isin(['Dipodomys merriami', 'Dipodomys ordii',
                                                      'Reithrodontomys megalotis', 'Chaetodipus baileyi'])]
month_evolution = subsetspecies.groupby("name").resample('M', on='eventDate').size().rename("counts")
month_evolution = month_evolution.reset_index()
month_evolution.head()
```

Plotting with seaborn:

```{code-cell} ipython3
:tags: [nbtutor-solution]

sns.relplot(data=month_evolution, x='eventDate', y="counts",
            row="name", kind="line", hue="name", height=2, aspect=5)
```

<div class="alert alert-success">

**EXERCISE**

Plot the annual amount of occurrences for each of the 'taxa' as a function of time using Seaborn. Plot each taxa in a separate subplot and do not share the y-axis among the facets.

<details><summary>Hints</summary>

- Combine `resample` and `groupby`!
- Check out the previous exercise for the plot function.
- Pass the `sharey=False` to the `facet_kws` argument as a dictionary.

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

year_evolution = survey_data.groupby("taxa").resample('A', on='eventDate').size()
year_evolution.name = "counts"
year_evolution = year_evolution.reset_index()
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

sns.relplot(data=year_evolution, x='eventDate', y="counts",
            col="taxa", col_wrap=2, kind="line", height=2, aspect=5,
            facet_kws={"sharey": False})
```

<div class="alert alert-success">

**EXERCISE**

The observations where taken by volunteers. You wonder on which day of the week the most observations where done. Calculate for each day of the week (`dayofweek`) the number of observations and make a bar plot.

<details><summary>Hints</summary>

- Did you know the Python standard Library has a module `calendar` which contains names of week days, month names,...?

</details>

```{code-cell} ipython3
:tags: [nbtutor-solution]

fig, ax = plt.subplots()
survey_data.groupby(survey_data["eventDate"].dt.dayofweek).size().plot(kind='barh', color='#66b266', ax=ax)
import calendar
xticks = ax.set_yticklabels(calendar.day_name)
```

Nice work!
