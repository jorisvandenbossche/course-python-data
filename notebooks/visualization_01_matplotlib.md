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

<p><font size="6"><b>Visualisation: Matplotlib</b></font></p>


> *Data wrangling in Python*  
> *November, 2020*
>
> *© 2020, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

+++

# Matplotlib

+++

[Matplotlib](http://matplotlib.org/) is a Python package used widely throughout the scientific Python community to produce high quality 2D publication graphics. It transparently supports a wide range of output formats including PNG (and other raster formats), PostScript/EPS, PDF and SVG and has interfaces for all of the major desktop GUI (graphical user interface) toolkits. It is a great package with lots of options.

However, matplotlib is...

> The 800-pound gorilla — and like most 800-pound gorillas, this one should probably be avoided unless you genuinely need its power, e.g., to make a **custom plot** or produce a **publication-ready** graphic.

> (As we’ll see, when it comes to statistical visualization, the preferred tack might be: “do as much as you easily can in your convenience layer of choice [nvdr e.g. directly from Pandas, or with seaborn], and then use matplotlib for the rest.”)

(quote used from [this](https://dansaber.wordpress.com/2016/10/02/a-dramatic-tour-through-pythons-data-visualization-landscape-including-ggplot-and-altair/) blogpost)

And that's we mostly did, just use the `.plot` function of Pandas. So, why do we learn matplotlib? Well, for the *...then use matplotlib for the rest.*; at some point, somehow!

Matplotlib comes with a convenience sub-package called ``pyplot`` which, for consistency with the wider matplotlib community, should always be imported as ``plt``:

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt
```

## - dry stuff - The matplotlib `Figure`, `axes` and `axis`

At the heart of **every** plot is the figure object. The "Figure" object is the top level concept which can be drawn to one of the many output formats, or simply just to screen. Any object which can be drawn in this way is known as an "Artist" in matplotlib.

Lets create our first artist using pyplot, and then show it:

```{code-cell} ipython3
fig = plt.figure()
plt.show()
```

On its own, drawing the figure artist is uninteresting and will result in an empty piece of paper (that's why we didn't see anything above).

By far the most useful artist in matplotlib is the **Axes** artist. The Axes artist represents the "data space" of a typical plot, a rectangular axes (the most common, but not always the case, e.g. polar plots) will have 2 (confusingly named) **Axis** artists with tick labels and tick marks.

There is no limit on the number of Axes artists which can exist on a Figure artist. Let's go ahead and create a figure with a single Axes artist, and show it using pyplot:

```{code-cell} ipython3
ax = plt.axes()
```

Matplotlib's ``pyplot`` module makes the process of creating graphics easier by allowing us to skip some of the tedious Artist construction. For example, we did not need to manually create the Figure artist with ``plt.figure`` because it was implicit that we needed a figure when we created the Axes artist.

Under the hood matplotlib still had to create a Figure artist, its just we didn't need to capture it into a variable. We can access the created object with the "state" functions found in pyplot called **``gcf``** and **``gca``**.

+++

## - essential stuff - `pyplot` versus Object based

+++

Some example data:

```{code-cell} ipython3
x = np.linspace(0, 5, 10)
y = x ** 2
```

Observe the following difference:

+++

**1. pyplot style: plt...** (you will see this a lot for code online!)

```{code-cell} ipython3
plt.plot(x, y, '-')
```

**2. creating objects**

```{code-cell} ipython3
fig, ax = plt.subplots()
ax.plot(x, y, '-')
```

Although a little bit more code is involved, the advantage is that we now have **full control** of where the plot axes are placed, and we can easily add more than one axis to the figure:

```{code-cell} ipython3
fig, ax1 = plt.subplots()
ax1.plot(x, y, '-')
ax1.set_ylabel('y')

ax2 = fig.add_axes([0.2, 0.5, 0.4, 0.3]) # inset axes
ax2.set_xlabel('x')
ax2.plot(x, y*2, 'r-')
```

<div class="alert alert-info" style="font-size:18px">

<b>REMEMBER</b>:

 <ul>
  <li>Use the <b>object oriented</b> power of Matplotlib!</li>
  <li>Get yourself used to writing <code>fig, ax = plt.subplots()</code></li>
</ul>
</div>

```{code-cell} ipython3
fig, ax = plt.subplots()
ax.plot(x, y, '-')
# ...
```

## An small cheat-sheet reference for some common elements

```{code-cell} ipython3
x = np.linspace(-1, 0, 100)

fig, ax  = plt.subplots(figsize=(10, 7))

# Adjust the created axes so that its topmost extent is 0.8 of the figure.
fig.subplots_adjust(top=0.9)

ax.plot(x, x**2, color='0.4', label='power 2')
ax.plot(x, x**3, color='0.8', linestyle='--', label='power 3')

ax.vlines(x=-0.75, ymin=0., ymax=0.8, color='0.4', linestyle='-.') 
ax.axhline(y=0.1, color='0.4', linestyle='-.')
ax.fill_between(x=[-1, 1.1], y1=[0.65], y2=[0.75], color='0.85')

fig.suptitle('Figure title', fontsize=18, 
             fontweight='bold')
ax.set_title('Axes title', fontsize=16)

ax.set_xlabel('The X axis')
ax.set_ylabel('The Y axis $y=f(x)$', fontsize=16)

ax.set_xlim(-1.0, 1.1)
ax.set_ylim(-0.1, 1.)

ax.text(0.5, 0.2, 'Text centered at (0.5, 0.2)\nin data coordinates.',
        horizontalalignment='center', fontsize=14)

ax.text(0.5, 0.5, 'Text centered at (0.5, 0.5)\nin Figure coordinates.',
        horizontalalignment='center', fontsize=14, 
        transform=ax.transAxes, color='grey')

ax.legend(loc='upper right', frameon=True, ncol=2, fontsize=14)
```

For more information on legend positioning, check [this post](http://stackoverflow.com/questions/4700614/how-to-put-the-legend-out-of-the-plot) on stackoverflow!

+++

## Quick introduction to style

+++

Whereas Matplotlib makes it possible to change every detail in a plot and [create your own style](https://matplotlib.org/tutorials/introductory/customizing.html), there are a number of pre-defined styles provided by Matplotlib:

```{code-cell} ipython3
plt.style.available
```

```{code-cell} ipython3
x = np.linspace(0, 10)

with plt.style.context('ggplot'):  # 'seaborn', ggplot', 'bmh', 'grayscale', 'seaborn-whitegrid', 'seaborn-muted'
    fig, ax = plt.subplots()
    ax.plot(x, np.sin(x) + x + np.random.randn(50))
    ax.plot(x, np.sin(x) + 0.5 * x + np.random.randn(50))
    ax.plot(x, np.sin(x) + 2 * x + np.random.randn(50))
```

Just pick **your favorite style**... which will be active for the remainder of the notebook:

```{code-cell} ipython3
plt.style.use('seaborn-whitegrid')
```

or go all the way and define your own custom style, see the [official documentation](https://matplotlib.org/3.1.1/tutorials/introductory/customizing.html) or [this tutorial](https://colcarroll.github.io/yourplotlib/#/).

+++

<div class="alert alert-info">

<b>REMEMBER</b>:

 <ul>
  <li>If you just want <b>quickly a good-looking plot</b>, use one of the available styles (<code>plt.style.use('...')</code>)</li>
  <li>Still, the object-oriented setup makes it possible to change every detail!</li>
</ul>
</div>

+++

## Interaction with Pandas

+++

What we have been doing while plotting with Pandas:

```{code-cell} ipython3
import pandas as pd
```

```{code-cell} ipython3
flowdata = pd.read_csv('data/vmm_flowdata.csv', 
                       index_col='Time', 
                       parse_dates=True)
```

```{code-cell} ipython3
flowdata.plot()
```

The `plot()` method in Pandas uses Matplotlib under the hood and adds some convenience (e.g. legend) to it.

+++

### Pandas versus matplotlib

+++

#### Comparison 1: single plot

```{code-cell} ipython3
flowdata.plot(figsize=(16, 6)) # shift tab this!
```

Making this with matplotlib...

```{code-cell} ipython3
fig, ax = plt.subplots(figsize=(16, 6))
ax.plot(flowdata)
ax.legend(["L06_347", "LS06_347", "LS06_348"])
```

is also fine, both options would work.

+++

#### Comparison 2: with subplots

```{code-cell} ipython3
axs = flowdata.plot(subplots=True, sharex=True,
                    figsize=(16, 8), colormap='viridis', # Dark2
                    fontsize=15, rot=0)
```

Mimicking this in matplotlib (just as a reference):

```{code-cell} ipython3
from matplotlib import cm
import matplotlib.dates as mdates

colors = [cm.viridis(x) for x in np.linspace(0.0, 1.0, len(flowdata.columns))] # list comprehension to set up the colors

fig, axs = plt.subplots(3, 1, figsize=(16, 8))

for ax, col, station in zip(axs, colors, flowdata.columns):
    ax.plot(flowdata.index, flowdata[station], label=station, color=col)
    ax.legend()
    if not ax.is_last_row():
        ax.xaxis.set_ticklabels([])
        ax.xaxis.set_major_locator(mdates.YearLocator())
    else:
        ax.xaxis.set_major_locator(mdates.YearLocator())
        ax.xaxis.set_major_formatter(mdates.DateFormatter('%Y'))
        ax.set_xlabel('Time')
    ax.tick_params(labelsize=15)
```

Is already a bit harder ;-). Using different subplots (aka facetting) to split up the data is useful in data exploration and is by default provided by Pandas. 

+++

### Best of both worlds!

```{code-cell} ipython3
fig, ax = plt.subplots() #prepare a matplotlib figure

flowdata.plot(ax=ax) # use pandas for the plotting
```

```{code-cell} ipython3
fig, ax = plt.subplots(figsize=(15, 5)) #prepare a matplotlib figure

flowdata.plot(ax=ax) # use pandas for the plotting

# Provide further adaptations with matplotlib:
ax.set_xlabel("")
ax.grid(which="major", linewidth='0.5', color='0.9')
fig.suptitle('Flow station time series', fontsize=15)
```

It is a useful pattern: 1/ Prepare with Matplotlib, 2/ Plot using Pandas and 3/ further adjust specific elements with Matplotlib

```{code-cell} ipython3
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(16, 6)) #provide with matplotlib 2 axis

flowdata[["L06_347", "LS06_347"]].plot(ax=ax1) # plot the two timeseries of the same location on the first plot
flowdata["LS06_348"].plot(ax=ax2, color='0.2') # plot the other station on the second plot

# further adapt with matplotlib
ax1.set_ylabel("L06_347")
ax2.set_ylabel("LS06_348")
ax2.legend()
```

<div class="alert alert-info">

 <b>Remember</b>: 

 <ul>
  <li>You can do anything with matplotlib, but at a cost... <a href="http://stackoverflow.com/questions/tagged/matplotlib">stackoverflow</a></li>
      
  <li>The preformatting of Pandas provides mostly enough flexibility for quick analysis and draft reporting.</li>
</ul>
<br>

If you take the time to make your perfect/spot-on/greatest-ever matplotlib-figure: Make it a <b>reusable function</b>!

</div>

+++

<div class="alert alert-info" style="font-size:18px">

 <b>Remember</b>: 

`fig.savefig()` to save your Figure object!

</div>

+++

# Need more matplotlib inspiration? 

+++

For more in-depth material:
* http://www.labri.fr/perso/nrougier/teaching/matplotlib/
* notebooks in matplotlib section: http://nbviewer.jupyter.org/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/Index.ipynb#4.-Visualization-with-Matplotlib
* main reference: [matplotlib homepage](http://matplotlib.org/)

+++

<div class="alert alert-info" style="font-size:18px">

**Galleries!**

Galleries are great to get inspiration, see the plot you want, and check the code how it is created:
    
* [matplotlib gallery](http://matplotlib.org/gallery.html") is an important resource to start from
* [seaborn gallery](https://seaborn.pydata.org/examples/index.html)
* The Python Graph Gallery (https://python-graph-gallery.com/)

</div>