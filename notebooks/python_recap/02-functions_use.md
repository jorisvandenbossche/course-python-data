---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.1
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Using Functions

> *© 2024, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), and the [*Software Carptentry: Programming with Python course*](https://swcarpentry.github.io/python-novice-gapminder).


## Introduction


We have used some functions already — now let’s take a closer look.

- To use a function, pass the arguments to the function within round brackets. 
- A value passed into a function is called an 'argument' (or also 'parameter').
- The parentheses are important: if you leave them off, the function doesn’t actually run.

For example, the functions `int()`, `str()`, and `float()` create a new value from an existing one with an updated data type. If the function doesn’t have a useful result to `return`, it usually returns the special value `None`. `None` is a Python object that stands in anytime there is no value.

```python
print('course')
```

```python
my_variable = print('course')
print(my_variable, type(my_variable))
```

__Note__ `print` takes zero or more arguments.


## Functions


Python has a number of [builtin-functions](https://docs.python.org/3/library/functions.html) that are always available. An example is the `max` function:

```python
max(1, 2, 3)
```

```python
max("a", "A")  # “larger” and “smaller” use (0-9, A-Z, a-z) to compare letters.
```

Functions may only work for certain (combinations of) arguments:

```python
max(1, 'a')
```

Functions may have default values for some arguments

```python
round(3.712), round(3.111), round(3.712, 2)  # By default, rounds to zero decimal places.
```

Functions have `arguments` and/or `keyword arguments`:

```python
round(3.712, ndigits=2)
```

<div class="alert alert-info">

__Tip__
    
To know how to use a function, check the help:
    
- use the built-in function `help()`
- add a question mark after the function/method name (without the brackets), `?round`
- use the SHIFT + TAB combination when the cursor is at the statement
    
</div>

```python
help(round)
```

```python
round?
```

## Methods


Functions attached to objects are called __methods__. Methods have parentheses like functions, but come after the variable (i.e. methods that you can apply on the object):

```python
some_str = "course"
some_str.upper()
```

If compatible, you can chain multiple methods

```python
print(some_str.isupper())
print(some_str.upper())
print(some_str.upper().isupper())
```

Methods can also have arguments and keyword arguments (with defaults) and checking the help is also similar.

```python
some_str.replace?
```

## Libraries/packages


> _Most of the power of a programming language is in its libraries_

- A library or package is a collection of files (called `modules`) that contains functions for use by other programs.
- The Python standard library is an extensive suite of modules that comes with Python itself.
- Many additional libraries are available from [PyPI (the Python Package Index)](https://pypi.org/).



Importing packages/libraries is always the first thing you do in Python, since it offers the functionalities to work with. It is like putting your equipment on the table to start working with it:

- Use import to load a library module into a program’s memory.
- Then refer to things from the module as `module_name.thing_name`.
   Python uses . to mean “part of”.
   
Let's import `math`, one of the modules in the standard Python library:

```python
import math
```

```python
math.exp(math.pi)
```

<div class="alert alert-info">

__Tip__
    
To know which functionalities a module (or an object) provides use the `<TAB>` key after the `.`
    
</div>


Different options are available:

* <span style="color:green">import <i>package-name</i></span>  <p> importing all functionalities as such
* <span style="color:green">from <i>package-name</i> import <i>specific function</i></span>  <p> importing a specific function, class or subset of the package
* <span style="color:green">import <i>package-name</i> as <i>short-package-name</i></span>    <p> Very good way to keep a good insight in where you use what package

```python
import math
from math import exp
import math as m
```

```python
math.sin(1)
```

<div class="alert alert-success">

**EXERCISE**:

What function from the `math` module  takes as input a real number `x`, and gives as output the greatest integer less than or equal to `x`? Apply the function to the number 1.7.

<details><summary>Hints</summary>

- Use the `math.` + `TAB`  button to explore the functions provided by the math library.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
math.floor(1.7)
```

<div class="alert alert-success">

**EXERCISE**:
    
During an experimental campaign, all experiments got a unique label. You need to code a small program that returns `True` when the label of the experiment ends with the number `2` and `False` otherwise. 
    
- Create a variable `experiment_label` with the value "Lab1_C_2"
- Search for a _method_ that you can apply on the variable that checks if a string ends with a certain character (or character sequence)

<details><summary>Hints</summary>

- A _method_ is like a function, but is attached to an object and comes after the variable. 
- Strings do have multiple methods available, so do check the available methods by using the `experiment_label.` + `TAB` key.
- You want to check if a certain string _ends with_ certain character
- Make sure to use the character "2" and not the integer 2 to check with to avoid a `TypeError`
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
experiment_label = "Lab1_C_2"
experiment_label.endswith("2")
```

<!-- #region -->
<div class="alert alert-success">

**EXERCISE**:

When a colleague of yours types `help(random)`, Python reports an error:
    
```python
NameError: name 'random' is not defined
```

What has your colleague forgotten to do?

<details><summary>Hints</summary>

- A module need to be imported before it can be used
    
</details>    
    
</div>
<!-- #endregion -->

```python tags=["nbtutor-solution"]
import random
#help(random)
```

<div class="alert alert-success">

**EXERCISE**:

You want to simulate a dice using Python code. Each time you run the code, a random number need to be returned between 1 and 6 (inclusive).     
    
Check in the Python standard library documentation of the [random module](https://docs.python.org/3/library/random.html) for a function that you can use to create your 'virtual dice'.  

<details><summary>Hints</summary>

- The section on ['functions for integers'](https://docs.python.org/3/library/random.html#functions-for-integers) should be helpful.
- Make sure the function is also able to reutrn the number 6.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
import random

random.randint(1, 6)
```

```python tags=["nbtutor-solution"]
# alternative using randrange
random.randrange(1, 7)
```

<!-- #region tags=[] -->
<div class="alert alert-success">

**EXERCISE**:

Explain in simple terms the order of operations in the following program: when does the addition happen, when does the subtraction happen, when is each function called, etc
    
```python
radiance = 1.0
max(2.1, 2.0 + min(radiance, 1.1 * radiance - 0.5))
```

What is the final returned value?

<details><summary>Hints</summary>

- Functions can be nested and will be executed from inside to the outside.
- Mathematical rules on order of operations and brackets do apply.
    
</details>    
    
</div>


<!-- #endregion -->

<!-- #region tags=["nbtutor-solution"] -->
```
Order of operations:
- 1.1 * radiance = 1.1
- 1.1 - 0.5 = 0.6
- min(radiance, 0.6) = 0.6
- 2.0 + 0.6 = 2.6
- max(2.1, 2.6) = 2.6

At the end, result = 2.6
```
<!-- #endregion -->

<div class="alert alert-success">

**EXERCISE**:

I'm measuring air pressure at sea level, what would be the equivalent air pressure of this measured value on other altitudes?
    
The **barometric formula**, sometimes called the exponential atmosphere or isothermal atmosphere, is a formula used to model how the **pressure** (or density) of the air **changes with altitude**. The pressure drops approximately by 11.3 Pa per meter in first 1000 meters above sea level.

$$P=P_0 \cdot \exp \left[\frac{-g \cdot M \cdot h}{R \cdot T}\right]$$
    
where:
* $T$ = standard temperature, 288.15 (K)
* $R$ = universal gas constant, 8.3144598, (J/mol/K)
* $g$ = gravitational acceleration, 9.81 (m/s$^2$)
* $M$ = molar mass of Earth's air, 0.02896 (kg/mol)

and:
* $P_0$ = sea level pressure (hPa)
* $h$ = height above sea level (m)    
    
The measured air pressure at sea level is 1010 hPa. Calculate the corresponding air pressure on 2500m above sea level.   

<details><summary>Hints</summary>

- 

FYI, see https://www.math24.net/barometric-formula/ or https://en.wikipedia.org/wiki/Atmospheric_pressure    
    
</details>    
    
</div>

```python tags=[]
standard_temperature = 288.15
gas_constant = 8.31446
gravit_acc = 9.81
molar_mass_earth = 0.02896
```

```python tags=["nbtutor-solution"]
pressure_hPa = 1010
height = 2500

pressure_hPa * math.exp(-gravit_acc * molar_mass_earth * height/(gas_constant * standard_temperature))
```

```python

```
