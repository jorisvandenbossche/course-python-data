---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.14.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

<p><font size="6"><b>Jupyter notebook introduction </b></font></p>

> *© 2024, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

-----

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), the [*Software Carptentry: Programming with Python gapminder course*](https://swcarpentry.github.io/python-novice-gapminder) and the [*Software Carptentry: Programming with Python inflammation course*](https://swcarpentry.github.io/python-novice-inflammation/).

+++

# Introduction

+++

To run Python, we are going to use Jupyter Notebooks via JupyterLab for this course. Jupyter notebooks are common in data science and visualization and serve as a convenient common-denominator experience for running Python code interactively. Jupyter notebooks let us __execute and view the results of our Python code interactively__ within the notebook.

__JupyterLab__ is an application server with a web user interface from [Project Jupyter](https://jupyter.org/) that enables one to work with documents and activities such as __Jupyter notebooks__ (a notebook is a file stored on your computer as a JSON file).

+++

<div class="alert alert-info">

__INFO__

Jupyter Notebook files have the file extension `.ipynb` to distinguish them from plain-text files.
    
</div>

+++

Let's have a check at the GUI provided by Juypyterlab. More detailed information on JupyterLab can be found in the [JupyterLab user interface documentation](https://jupyterlab.readthedocs.io/en/stable/user/interface.html) as a reference.

+++

# Notebook cell types

Jupyter mixes code and text in different types of blocks, called cells. We often use the term “code” to mean “the source code of software written in a language such as Python”. A “code cell” in a Notebook is a cell that contains software; a “text cell” is one that contains ordinary prose written for human beings.

+++

## Code: Python

```{code-cell} ipython3
# Code cell, then we are using python
print('Hello Course')
```

```{code-cell} ipython3
DS = 10
print(DS + 5)
```

Writing code is what you will do most during this course!

+++

## Text: Markdown

+++

Text cells, using Markdown syntax. With the syntax, you can make text **bold** or *italic*, amongst many other things...

+++

* list
* with
* items

+++

Including images:

![images](https://upload.wikimedia.org/wikipedia/commons/c/c3/Python-logo-notext.svg)

and links [Link to interesting resources](https://www.youtube.com/watch?v=z9Uz1icjwrM)

+++

Mathematical formulas can also be incorporated (LaTeX it is...)
$$\frac{dBZV}{dt}=BZV_{in} - k_1 .BZV$$
$$\frac{dOZ}{dt}=k_2 .(OZ_{sat}-OZ) - k_1 .BZV$$

+++

Or tables:

course | points
 --- | --- 
 Math | 8
 Chemistry | 4

+++

Code can also be incorporated, but than just to illustrate:

+++

```python
BOT = 12
print(BOT)
```

+++

Get more background on the markdown syntax: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

+++

<div class="alert alert-success">

**EXERCISE**:

What is displayed when a Python cell in a notebook that contains several calculations is executed? 
    
- Create a new code cell
- Add the following content to the cell    
    ```
    7 * 3
    2 + 1
    ```
- Execute the cell and check the output
    

<details><summary>Hints</summary>

- To add a new cell, you can use the `+`-icon button in the notebook menu
- To execute a cell, you can use the play-icon button in the notebook menu of use SHIFT + ENTER

</details>    
    
</div>

```{code-cell} ipython3
:tags: [nbtutor-solution]

# Jupyter returns the output of the last calculation.
7 * 3
2 + 1
```

<div class="alert alert-success">

**EXERCISE**:

What happens if you write some Python in a code cell and then you switch it to a Markdown cell? 
    
- Create a new code cell
- Add the following content to the cell    
    ```
    x = 6 * 7 + 12
    print(x)
    ```
- Execute the cell and check the output 
- Change the cell to a 'Markdown' cell type    
    
<details><summary>Hints</summary>

- To switch a cell type, us the dropdown menu with cell types in the notebook menu

</details>    
    
</div>

+++ {"tags": ["nbtutor-solution"]}

x = 6 * 7 + 12
print(x)

+++

## Execution: SHIFT + ENTER

To run a cell: <strike>push the start triangle in the menu or</strike> type **SHIFT + ENTER**

![](../../img/shiftenter.jpg)

+++

# When in trouble

+++

<div class="alert alert-danger">

__NOTE__: 
    
When you're stuck, or your notebook crashes: 
    
- first try <code>Kernel</code> > <code>Interrupt</code> -> your cell should stop running
- if no succes -> <code>Kernel</code> > <code>Restart</code> -> restart your notebook
    
</div>

+++

<div class="alert alert-info">

__Kernels?__

The JupyterLab docs define kernels as “separate processes started by the server that runs your code in different programming languages and environments.” When we open a Jupyter Notebook, that starts a kernel - a process - that is going to run the code. In this lesson, we’ll be using the Jupyter ipython kernel which lets us run Python 3 code interactively.

Using other Jupyter kernels for other programming languages would let us write and execute code in other programming languages in the same JupyterLab interface, like R, Java, Julia, Ruby, JavaScript, Fortran, etc.
</div>

```{code-cell} ipython3

```
