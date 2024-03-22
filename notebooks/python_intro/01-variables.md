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

# Variables and assignment

> *© 2024, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), the [*Software Carptentry: Programming with Python gapminder course*](https://swcarpentry.github.io/python-novice-gapminder) and the [*Software Carptentry: Programming with Python inflammation course*](https://swcarpentry.github.io/python-novice-inflammation/).


## Introduction


Any Python interpreter can be used as a calculator:

```python
3 + 5 * 4
```

This is great but not very interesting. To do anything useful with data, we need to assign its value to a __variable__. In Python, we can assign a value to a variable, using the equals sign `=`. 

For example, we can track the air pressure by assigning the value 1010 to a variable `pressure_hPa`:

```python
pressure_hPa = 1010 # hPa
```

<div class="alert alert-info">

__Info__: 
    
Everything in a line of code following the ‘#’ symbol is a comment that is ignored by Python. Comments allow programmers to leave explanatory notes.
    
</div>


## Variables


In Python the `=` symbol assigns the value on the right to the name on the left. From now on, whenever we use `pressure_hPa`, Python will substitute the value we assigned to it.

In Python, variable names:
- can include letters, digits, and underscores (typically used to separate words in long variable names)
- cannot start with a digit
- are case sensitive.


Variables can be used in calculations:

```python
pressure_hPa * 100
```

And the output reassigned again to a new variable:

```python
pressure_Pa = pressure_hPa * 100
```

<div class="alert alert-info">

__Tip__: Use meaningful variable names. e.g. `pressure_Pa` is much easier to understand than `p`.
    
</div>


<div class="alert alert-success">

**EXERCISE**:

A person weights 65 kg. First assign this value to a variable name `weight_kg`. Next, calculate the weight in gram and assign the result of the calculation to a variable name `weight_g`.

<details><summary>Hints</summary>

- Make sure to use the variable name `weight_kg` in the calcualtion of the weight in gram. 

</details>    
    
</div>

```python tags=["nbtutor-solution"]
weight_kg = 65
weight_g = weight_kg * 1000
```

<!-- #region -->
<div class="alert alert-success">

**EXERCISE**:

What is the final value of position in the program below? 
    
```
initial = "left"
position = initial
initial = "right"
```
    
Try to predict the value without running the program, then check your prediction.

    
<details><summary>Hints</summary>

- The best way is to try out the statement and ask for the variable `position` in the end.
    
</details>    
    
</div>
<!-- #endregion -->

```python tags=["nbtutor-solution"]
initial = "left"
position = initial
initial = "right"
position
```

<div class="alert alert-success">

**EXERCISE**:

Sorting Out References: Python allows you to assign multiple values to multiple variables in one line by separating the variables and values with commas. What does the following program print out?
    
```
pressure, weight = 1010, 60.5    
print(weight)
```

<details><summary>Hints</summary>

- The reason this works is because Python is applying _unpacking_ here. It maps the two outcomes on the right hand side of the addignment to the two variable names available on the left hand side. 
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
pressure, weight = 1010, 60.5    
print(weight)  # prints 60.5
```

<div class="alert alert-success">

**EXERCISE**:

Try to print the variable `pressure_p` using the `print` function. Check the output and explain what is happening.

<details><summary>Hints</summary>

- To use a function in Python, call the function name and pass the arguments to the function within round brackets. The parentheses are important: if you leave them off, the function doesn’t actually run! 
- The last line of an error message is usually the most informative.
- Python is case-sensitive.
- A typo can lead to this kind of errors, so always have a check.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
# Variables must be created before they are used.
# print(pressure_p)
```

<div class="alert alert-warning">

__Remark__: 
    
Be aware that it is the order of execution of cells that is important in a Jupyter notebook and not the order in which cells appear.
    
</div>


__variables-as-sticky-notes__

A variable in Python is analogous to a sticky note with a name written on it: assigning a value to a variable is like putting that sticky note on a particular value. Assigning a value to one variable does not change values of other, seemingly related. 

```
weight_kg = 65
weight_lb = 2.2 * weight_kg
```

![](../../img/python-sticky-note-variables-02.svg)

The expression 2.2 * weight_kg is evaluated to 143.0, and then this value is assigned to the variable `weight_lb` (i.e. the sticky note weight_lb is placed on 143.0). At this point, each variable is 'stuck' to completely distinct and unrelated values.

If we now change `weight_kg`:

```
weight_kg = 100
```

![](../../img/python-sticky-note-variables-03.svg)

Since `weight_lb` doesn’t 'remember' where its value comes from, it is not updated when we change `weight_kg`.


## Types of data


Every value in a program has a specific type. Python knows various types of data. Common ones are:

- __integer__ numbers, represents positive or negative whole numbers like 3 or -512.
- __floating point__ numbers, represents real numbers like 3.14159 or -2.5.
- character __string__ (usually called "string", str)
- __boolean__, represens eaither True or False


<div class="alert alert-info">

__Tip__: Use the Python built-in function `type` to find the type of a value.
    
</div>


In the example above, variable `pressure_hPa` has an integer value of 1010:

```python
pressure_hPa = 1010 # hPa
type(pressure_hPa)
```

If we want to more precisely, we can use a floating point value by executing:

```python
pressure_hPa = 1010.33 # hPa
type(pressure_hPa)
```

To create a string, we add __single or double quotes__ around text. To identify and track measurement location throughout our study, we can assign each location a unique identifier by storing it in a string:

```python
location_1 = "001"
type(location_1)
```

```python
is_larger = pressure_hPa > 2000
type(is_larger)
```

Types control what operations (or methods) can be performed on a given value:

```python
5 - 3
```

```python
True + 1
```

```python
#"sample" + "_" - location_1
"sample" + "_" + location_1
```

A Python shell can replace your pocket calculator, with the basic arithmetic operations addition, substraction, division ... natively implemented
+, -, *, /, % (modulo) natively implemented

 *operation*| *python implementation* 
----------:| --------------------- 
 addition  | `+`
 substraction | `-`
 multiplication | `*`
 division | `/`
 modulo | `%`
 exponentiation | `**`

```python
print (7 * 3.)  # Can mix integers and floats freely in operations - Python 3 automatically converts integers to floats as needed.
print (2**10)   # exponentation
print (8 % 3)   # modulo
```

Mathematical rules on order of operations and usage of brackets do apply:

```python
2 + 3 * 10, (2 + 3) * 10
```

The `//` operator performs integer (whole-number) floor division, the `/` operator performs floating-point division, and the `%` (or modulo) operator calculates and returns the remainder from integer division:

```python
print(3/2)   # ! converted to floats
print(3/2.)
print(3.//2.)  #integer division
```

Where reasonable, `float()` will convert to a floating point number, and `int()` will convert to an integer:

```python
int(float('2.'))
```

```python
print(1 + int('2'))
print(1 + float('2.0'))
print(1 + int(2.0))
```

<div class="alert alert-success">

**EXERCISE**:

What data type of value is `21.55`? How can you find out?

<details><summary>Hints</summary>

- It is possible to find out by using the built-in function `type()`.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
type(21.55)
```

<div class="alert alert-success">

**EXERCISE**:

What data type of value is the result of `3.25 + 4`?

<details><summary>Hints</summary>

- Integers are automatically converted to floats as necessary.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
type(3.25 + 4)
```

<!-- #region -->
<div class="alert alert-success">

**EXERCISE**:

Which of the following will return the floating point number 2.0? 
    
```
first = 1.0
second = "1"
third = "1.1"
```

1. `first + float(second)`
1. `float(second) + float(third)`
1. `first + int(third)`
1. `first + int(float(third))`
1. `int(first) + int(float(third))`
1. `2.0 * second`
    
    

<details><summary>Hints</summary>

- There may be more than one right answer.
    
</details>    
    
</div>
<!-- #endregion -->

```python tags=["nbtutor-solution"]
first = 1.0
second = "1"
third = "1.1"
print(first + float(second))  # 1
print(first + int(float(third)))  # 4
```

<div class="alert alert-success">

**EXERCISE**:

When running `int("3.0")`, Python returns an error. How would you solve this to get an integer value starting from the string "3.0"?

<details><summary>Hints</summary>

- If you ask Python to perform two consecutive typecasts, you must convert it explicitly in code.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
int(float("3.0"))
```
