---
jupytext:
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

# Control flow

> *© 2024, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), and the [*Software Carptentry: Programming with Python course*](https://swcarpentry.github.io/python-novice-gapminder).

+++

## Introduction

+++

When we have a collection of data, we want to: 

- do something for each of the elements in the collection (loop), e.g. "convert all elements to another unit"
- do somethig for all elements in a collection according to a certain condition, e.g. "report on all locations where the value is below a certain threshold"

```{code-cell} ipython3
water_quality = {
    "Demer": 9.5,
    "Nete": 8.4,
    "Leie": 6.6
}
```

```{code-cell} ipython3
:tags: []

report_location = "Nete"
f"The measured dissolved oxygen in {report_location} on March 18th 2024 was {water_quality[report_location]} mg/l."
```

```{code-cell} ipython3
for report_location in water_quality.keys():
    if water_quality[report_location] < 7:
        print(f"The measured dissolved oxygen in {report_location} on March 18th 2024 was {water_quality[report_location]} mg/l.")
```

## Loops

+++

A `for` loop tells Python to execute some statements once for each value in a list, a character string, or some other collection (anything that is 'iterable'): "for each element in this group, do these operations".

```{code-cell} ipython3
for oxygen in [9.5, 8.4, 6.6]:
    print(oxygen)
```

```{code-cell} ipython3
for char in "oxygen":
    print(char)
```

A for loop is made up of a 'collection', a 'loop variable', and a 'body':

- The collection, `[9.5, 8.4, 6.6]`, is what the loop is iterating through.
- The body, `print(oxygen)`, specifies what to do for each element in the collection. It can contain many statements (lines).
- The loop variable, `oxygen`, is what changes for each iteration of the loop (the 'current element').

The colon at the end of the first line signals the start of a block of statements. Python uses __indentation__ to show nesting: Any consistent indentation is legal, but almost everyone uses __four spaces__.

```{code-cell} ipython3
#for oxygen in [9.5, 8.4, 6.6]:
#print(oxygen)
```

<div class="alert alert-warning">

__Remember__: 
    
Indentation is always meaningful in Python.
    
</div>

+++

A loop variable can be called anything. It is a good idea to choose variable names that are meaningful, otherwise it would be more difficult to understand what the loop is doing.

```{code-cell} ipython3
for i in [9.5, 8.4, 6.6]:  # oxygen
    print(i/1000) # conversion to g/l
```

Python has a built-in function called `range` that generates a sequence of numbers:

```{code-cell} ipython3
for i in range(3):
    print(i)
```

The built-in function `enumerate` takes a iterable collection (e.g. a list) and generates a new sequence of the same length. Each element of the new sequence is a pair composed of the index (0, 1, 2,…) and the value from the original sequence:

```{code-cell} ipython3
for idx, do in enumerate([9.5, 8.4, 6.6]):
    print(idx, do)
```

__Looping over dictionary__

use `.items()` to loop over the elements of a dictionary:

```{code-cell} ipython3
water_quality = {
    "Demer": 9.5,
    "Nete": 8.4,
    "Leie": 6.6
}
```

```{code-cell} ipython3
for key, value in water_quality.items():
    print(f"Location: {key} has DO value: {value} mg/l")
```

or loop over the keys elements only:

```{code-cell} ipython3
for key in water_quality.keys():
    print('Location: {}'.format(key))
```

## Conditionals

+++

Use `if` statements to control whether or not a block of code is executed. The structure is similar to a `for` statement:

- First line opens with if and ends with a colon `:`
- Body containing one or more statements is indented (usually by 4 spaces)
- Use `else` to execute a block of code when an if condition is not true
- Use `elif` to specify additional tests (can be multiple)

Python steps through the branches of the conditional in order, testing each in turn. So __ordering matters__.

```{code-cell} ipython3
dissolved_oxygen = 11.
if dissolved_oxygen > 10:
    print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is high.")
elif dissolved_oxygen > 8:
    print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is moderate.")        
else:
    print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is low.")     
```

Conditionals are used a lot in combination with loops:

```{code-cell} ipython3
oxygen_levels = [11.65, 9.8, 6.3, 10.5, 7.7]
for dissolved_oxygen in oxygen_levels:
    if dissolved_oxygen > 10:
        print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is high.")
    elif dissolved_oxygen > 8:
        print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is moderate.")        
    else:
        print(f"The measured dissolved oxygen {dissolved_oxygen} mg/l is low.")        
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

```{code-cell} ipython3
dissolved_oxygen = 13.
dissolved_oxygen != 13
```

```{code-cell} ipython3
dissolved_oxygen = 13.
(dissolved_oxygen < 10) | (dissolved_oxygen > 12)  # Note the round brackets
```

The condition can also be triggered by a function, e.g. a method that returns a True/False:

```{code-cell} ipython3
"dissolved oxygen".islower()
```

`True` and `False` booleans are not the only values in Python that are true and false. In fact, any value can be used in an if or elif.:

```{code-cell} ipython3
if "":
    print("empty string")
if "word":
    print("word")
if []:
    print("empty list")
if [1, 2, 3]:
    print("non-empty list")
if 0:
    print("zero")
if 1:
    print("one")
```

## Exercises

+++

<div class="alert alert-success">

**EXERCISE**:


```python
word = 'oxygen'
for letter in word:
    print(letter)
```

How many times is the body of the loop executed?    

<details><summary>Hints</summary>
    
- The string "oxygen" is a collection of N individual characters over which the loop iterates.
    
</details>
   
</div>

```{code-cell} ipython3
:tags: [nbtutor-solution]

# using the 'accumulator pattern' to check the number of counts
acc = 0
for letter in 'oxygen':
    acc += 1  # the in-place operator
print(acc)    
```

<div class="alert alert-success">

**EXERCISE**:
    
A number of raw measurements of conductivity `conductivities = [3.3, 4.5, 6.6]` need to be adjusted by the calibration parameters according to $a+conductivity*r$ with $a$ and $r$ respectively 0.43 and 1.35.
    
- Loop through the conductivities list
- print for each of the elements the calibrated value using the calibration parameters $a$ and $r$     

<details><summary>Hints</summary>
    
    
</details>
   
</div>

```{code-cell} ipython3
conductivities = [3.3, 4.5, 6.6]
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

a = 0.43
r = 1.35
for conductivity in conductivities:
    print(a + conductivity*r )
```

<div class="alert alert-success">

**EXERCISE**:

A number of air pressure measurements `pressures_hPa = [1013, 950, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]` need to be processed. You want to enlist the indices (position) of the list with an air pressure below 1000 (hPa) and store these indices in a new variabe `indices`. 
    
- Create an empty list with variable name `indices`
- Loop through the `pressures_hPa` list
- If the air pressure is below 1000, _append_ the corresponding index/position to the `indices` list

Print the list `indices` (it should contain the values [1, 5, 6, 8])
    
<details><summary>Hints</summary>
    
- Rememeber `enumerate` to track the index while looping though a list in a for-loop?
- `append` is a list method that can be used to add an en element to a list __in-place__
        
</details>
   
</div>

```{code-cell} ipython3
pressures_hPa = [1013, 950, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

indices = []
for j, pressure in enumerate(pressures_hPa):
    if pressure < 1000:
        indices.append(j)
indices        
```

<div class="alert alert-success">

__EXERCISE__:


The optimal dissolved oxygen concentration for fish is between 5 to 20 mg/l. You received a number of real-time dissolved oxygen measurements:
   
```python
water_quality = {"Demer": 9.5, "Nete": 8.4, "Leie": 6.6, "Dender": 3, "Dijle": 21, "Ijzer": 4.4}
```    

You want to automatically raise an alert when conditions are not optimal:
    
- Loop through the dictionary
- For each element with bad conditions (consider the DO range of 5-20mg/l as optimal), print an alert: "Alert: Poor conditions measured at LOCATION with DO concetration of CONCENTRATION mg/l." (with LOCATION and CONCENTRATION filled in by the location (dict key) and the value (dict value).

<details><summary>Hints</summary>

- To loop through a dictionary, make sure to use the `items()` to be able to use the key (location) and the value (concentration)
- Multiple conditions can be combined using `|` or `&`. Make sure to add round brackets fo the order of execution is respected. 
- To include the variables in the print statement, check the [formatted string literals](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals)
    
</details>
   
</div>

```{code-cell} ipython3
water_quality = {"Demer": 9.5, "Nete": 8.4, "Leie": 6.6, "Dender": 3, "Dijle": 21, "Ijzer": 4.4}
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

for location, do in water_quality.items():
    if (do > 20) | (do < 5):
        print(f"Alert: Poor conditions measured at {location} with DO concetration of {do} mg/l.")
```

<div class="alert alert-success">

**EXERCISE**:
    
You have a directory of files on your computer with data files coming from from 2 different measurement devices: "Sigma900" and "Avalanche2000":
    
```python
file_names = ["sigma_001.txt", "sigma_004.txt", "ava_x01.txt", "ydoc3.txt", "sigma_002.txt", 
              "sigma_003.txt", "ava_x02.txt", "ava_x03.txt", "ava_x04.txt", "sigma_005.txt"]
```
   
You want to process the files with a specific function (data pipeline) based on the file naming:
    
- loop over the file names
- if the file name starts with `sigma`, print "Processing file FILE-NAME with sigma pipeline." (replacing FILE-NAME with the real file name)
- if the file name starts with `ava`, print "Processing file FILE-NAME with avalanche pipeline." (replacing FILE-NAME with the real file name)    
- otherwise, print "Unrecognized file FILE-NAME" (replacing FILE-NAME with the real file name)    

<details><summary>Hints</summary>

- Check the string-methods to find a way to test if a string `startswith` something
- You need if/elif and else to handle all options
- To include the file name in the print statement, check the [formatted string literals](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals)
    
</details>
   
</div>

```{code-cell} ipython3
file_names = ["sigma_001.txt", "sigma_004.txt", "ava_x01.txt", "ydoc3.txt", "sigma_002.txt", 
              "sigma_003.txt", "ava_x02.txt", "ava_x03.txt", "ava_x04.txt", "sigma_005.txt"]
```

```{code-cell} ipython3
:tags: [nbtutor-solution]

for file_name in file_names:
    if file_name.startswith("sigma"):
        print(f"Processing file {file_name} with sigma pipeline.")
    elif file_name.startswith("ava"):
        print(f"Processing file {file_name} with avalanche pipeline.")       
    else: 
        print(f"Unrecognized file {file_name}")
```

## Topics on loops

+++

### Loop over files in directory

+++

The [pathlib module](https://docs.python.org/3/library/pathlib.html) provides useful abstractions for file and path manipulation like returning the name of a file without the file extension. This is very useful when looping over files and directories.
    
To enlist the files in a folder, the `glob` method is available. For example, using a wild card `*`:

```{code-cell} ipython3
from pathlib import Path
```

```{code-cell} ipython3
for file_path in Path("../").glob("*"):
    if file_path.suffix == ".ipynb":
        print(file_path)
```

We can make this check on file type shorter by adjusting the search pattern:

```{code-cell} ipython3
from pathlib import Path

for file_path in Path("../").glob("*.ipynb"):
    print(file_path.suffix)
```

```{code-cell} ipython3
from pathlib import Path

for file_path in Path("../").glob("*"):
    if file_path.is_dir():
        print(file_path)
```

<div class="alert alert-info">

__Reminder__: 
    
Any Python object has certain characteristics - __attributes__, e.g. `file_path.suffix` which do not require round brackets. An object also provides a number of methods to act on the object - __methods__, e.g. `file_path.is_dir()` which require round brackets `()`.
    
</div>

+++

### List comprehensions

+++

List comprehensions are a powerful functionality. It creates an in-list for-loop option, looping through all the elements of a list and doing an action on it, in a single, readable line.

```{code-cell} ipython3
number_list = [1, 2, 3, 4]
[i**2 for i in number_list]
```

and with conditional options:

```{code-cell} ipython3
[i**2 for i in number_list if i > 2]
```

We can shorten some of the exercises using list comprehensions:

1. applying calibration parameters to conductivities

```{code-cell} ipython3
conductivities = [3.3, 4.5, 6.6]
for conductivity in conductivities:
    print(0.43 + conductivity*1.35 )
```

becomes...

```{code-cell} ipython3
[a + conductivity*r  for conductivity in conductivities]
```

1. Catching the indices of the pressures below 1000

```{code-cell} ipython3
pressures_hPa = [1013, 950, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]

indices = []
for j, pressure in enumerate(pressures_hPa):
    if pressure < 1000:
        indices.append(j)
indices        
```

becomes...

```{code-cell} ipython3
[j  for j, pressure in enumerate(pressures_hPa) if pressure < 1000]
```

### `while` loops

A `while` loop run until a given condition is fulfilled. The structure is similar to a `for` statement:

- First line opens with if and ends with a colon `:`
- Body containing one or more statements is indented (usually by 4 spaces)

```{code-cell} ipython3
level = 90
while level < 1000:
    print(f"inside loop - current level: {level}")
    level += level*2
print(f"While loop ended, current level: {level}")    
```

Use `break` to go out of a for/while loop:

```{code-cell} ipython3
level = 90
while level < 1000:
    print(f"inside loop - current level: {level}")

    if level == 270:
        print("Stop the iteration")
        break
    level += level*2

print(f"While loop ended, current level: {level}")    
```

Use `continue` to go directly to the next iteration of a loop:

```{code-cell} ipython3
level = 90
while level < 1000:
    print(f"inside loop - current level: {level}")

    if level == 270:
        print("Skip this iteration, adding one")
        level += 1
        continue
    level += level*2

print(f"While loop ended, current level: {level}")    
```
