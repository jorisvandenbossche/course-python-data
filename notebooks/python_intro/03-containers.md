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

# Containers and slicing

> *© 2024, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

------

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), the [*Software Carptentry: Programming with Python gapminder course*](https://swcarpentry.github.io/python-novice-gapminder) and the [*Software Carptentry: Programming with Python inflammation course*](https://swcarpentry.github.io/python-novice-inflammation/).


## Introduction


If I measure air pressure multiple times, doing calculations with a hundred variables called `pressure_001`, `pressure_002`, etc., would be slow and inconvenient. We need _container_ or _collection_ data types to combine multiple values in a single object.


## Lists


We can use a list to store many values together:

- Contained within square brackets `[...]`.
- Values separated by commas `,`.


```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
```

```python
len(pressures_hPa)  # len is a built-in function in Python (does not require an import)
```

A list is an ordered collection of objects, that may have different types. 

```python
a_list = [2.,'aa', 0.2]
a_list
```

Use an item’s index to fetch it from a list  and have access to individual object in the list:

```python
a_list[1]
```

```python
a_list[-1]  # negative indices are used to count from the back
```

Use `[]` on its own to represent a list that doesn’t contain any values. 

```python
type([]), len([])
```

<div class="alert alert-warning">

__Warning__: 
    
Indexing in Python starts at 0 (as in C, C++ or Java), not at 1 (as in Fortran or Matlab)!
    
</div>


Lists are __mutable__ objects and can be modified. Lists’ __values can be replaced by assigning to them__. Use an index expression on the left of assignment to replace a value:

```python
a_list[1] = 3000
a_list
```

__Slicing:__ 

Obtaining sublists of regularly-spaced elements by using the indexes (either contiguous or non-contiguous with a step size):

```python
another_list = ['first', 'second', 'third', 'fourth', 'fifth']
print(another_list[2:4])
print(another_list[3:])
print(another_list[:2])
print(another_list[::2])
print(another_list[:-2])
```

```python
print(another_list[-4:-2])
```

Python reports an `IndexError` if we attempt to access a value that doesn’t exist:

```python
#another_list[20]
```

<div class="alert alert-info">

__Info__: 

* Slicing syntax: `L[start:stop:step]`
* `L[start:stop]` contains the elements with indices i so `start <= i < stop`
* i ranging from start to stop-1. Therefore, L[start:stop] has (stop-start) elements.
* All slicing parameters are optional
* All slicing parameters can be negative (counting backwards)

</div> 


When assigning a list to a new variable name, both names are linked to the same data in memory on your computer (cfr. two sticky notes specifiying the same). When changing one of them, the other will also be adjusted:

```python
a = ['a',  'b']
b = a
b[0] = 1
print(a)
```

By making an explicit `copy()`, both names are linking to independent data: 

```python
a = ['a',  'b']
b = a.copy()
b[0] = 1
print(a)
```

<div class="alert alert-warning">

__Warning__: 
    
With two variables being identical, they point to the same point in memory. Changing one of them is also changing the other!
    
</div>


**List methods**:


You can enlist the available _methods_ of a `list` using the `TAB` key  or using the (built-in) `dir()`-function:

```python
#dir(list)
```

```python
a_third_list = ['red', 'blue', 'green', 'black', 'white']
```

Som examples of methods:

```python
# Appending in-place
a_third_list.append('pink')
a_third_list
```

```python
# Removes and returns the last element in-place
a_third_list.pop()
a_third_list
```

```python
# Extends the list in-place
a_third_list.extend(['pink', 'purple'])
a_third_list
```

```python
a_third_list.extend(['pink', 'purple'])
```

<div class="alert alert-info">

__Remember__: 
    
 The methods of a list update the list __in-place__, so we do not reassign the output to a variable name.
    
</div> 


Concatenating lists is the same as summing both lists:

```python
a_list = ['pink', 'orange']
a_concatenated_list = a_third_list + a_list
a_concatenated_list
```

The multiplication of a list with a value N will repeat the same list N times:

```python
a_repeated_list = a_concatenated_list * 4
print(a_repeated_list)
```

### Exercises

```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
```

<div class="alert alert-success">

**EXERCISE**:

Change the 3rd element of the `pressures_hPa` list to the value 1111.

<details><summary>Hints</summary>

- Selecting elements require square brackets `[]`
- Python starts counting at 0, so the third element has index 2
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
pressures_hPa[2] = 1111
pressures_hPa
```

<!-- #region -->
<div class="alert alert-success">

**EXERCISE**:

Insert an additional measured pressure value of `1212` on position 5 of the list (making the list one element larger).

<details><summary>Hints</summary>

- Explore the available list _methods_ by using the TAB key after the dot: `pressures_hPa.` + TAB. Which method would be useful to _insert_ a new element?
- Remember that list methods do adjust the list in-place.

    
</details>    
    
</div>
<!-- #endregion -->

```python tags=["nbtutor-solution"]
pressures_hPa.insert(4, 1212)
pressures_hPa
```

<div class="alert alert-success">

**EXERCISE**:

Select the last 3 elements of the list `pressures_hPa`.

<details><summary>Hints</summary>

- You can count elements backwards from the end by using a negative index.
- The syntax to select elements is `L[start:stop:step]`, with each of them optional.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
pressures_hPa[-3:]
```

<div class="alert alert-success">

**EXERCISE**:

Select all of the even-numbered items from the variable `pressures_hPa`.

<details><summary>Hints</summary>

- Start with element 1 (which is the second element, since indexing starts at 0), go on until the end (since no end is given), and uses a step size of 2 (i.e., selects every second element).
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
pressures_hPa[1::2]
```

<div class="alert alert-success">

**EXERCISE**:

Explain the difference between `sorted(pressures_hPa)` and `pressures_hPa.sort()`.

<details><summary>Hints</summary>

- `.sort()` is a list method whereas the `sorted` is a standard Python function  
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
# Returns a sorted copy of the list (the original list `pressures_hPa` remains unchanged)
print(sorted(pressures_hPa))
# The list methos `sort` sorts the list in-place and does not return anything on itself
print(pressures_hPa.sort())
print(pressures_hPa)
```

<!-- #region -->
<div class="alert alert-success">

**EXERCISE**:

Why is the following not working?
    
```python
a_third_list = ['red', 'blue', 'green', 'black', 'white']
reverted = a_third_list.reverse()
a_concatenated_list = a_third_list + reverted    
```
    
- Try out the code snippet yourself and clarify why this raises an error. 
- Fix the error so the `a_third_list` is concatenated with the reverse of `a_third_list`

<details><summary>Hints</summary>

- A list method updated the list in-place.
- To create a copy instead of a view on the data, use the `copy()` method.    
    
</details>    
    
</div>
<!-- #endregion -->

```python tags=["nbtutor-solution"]
a_third_list = ['red', 'blue', 'green', 'black', 'white']
a_third_list_reversed = a_third_list.copy()
a_third_list_reversed.reverse()
a_concatenated_list = a_third_list + a_third_list_reversed
a_concatenated_list
```

------------


## Strings


A character string is a 'container' of individual characters. Different string syntaxes do exist: simple, double or triple quotes:

```python
lyric = 'Never gonna give you up'
print(lyric)

lyric = "never gonna let you down"
print(lyric)

lyric = '''Never gonna run around 
    and desert you'''         
print(lyric)

lyric = """Never gonna make you cry, 
    never gonna say goodbye"""
print(lyric)
```

```python
## pay attention when using apostrophes! - test out the next two lines one at a time
#print('Hi, what's up?')
#print("Hi, what's up?")
```

<div class="alert alert-info">

__Remember__: 
    
 Single `'` and double `"` quotes are the same and can be interchanged. See the [official Python style guidelines](https://peps.python.org/pep-0008/#string-quotes).
    
</div> 


The newline character is **\n**, and the tab character is **\t**.

```python
print("Never gonna tell a lie and hurt you. Never gonna give you up,\tnever gonna let you down Never \ngonna\n run around and\t desert\t you")
```

Strings are a container just like lists. Hence they can be __indexed and sliced__, using the same syntax and rules:

```python
a_string = "Python course"
print(a_string[0])
print(a_string[1:5])
print(a_string[-4:-1:2])
```

A string is __an immutable object__ and it is not possible to modify its contents. Immutable obects (e.g. strings) can’t be changed after creation, mutable objects (e.g. lists) can be modified in place after creation.

```python
#a_string[3] = 'q'   # uncomment this cell
```

String objects also provide a number of __methods__. One can explore the namespace using `dir()` or using the TAB-key:

```python
#dir(str) # uncomment this cell
```

```python
another_string = "Strawberry-raspBerry pAstry package party"
another_string.lower().replace("r", "s", 7)
```

__String formatting__

[Formatted String Literals](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals) are a powerful way to include the value of Python expressions inside a string:

- prefixing the string with `f`
- write Python expressions as `{ expression }`

```python
an_integer = 1
a_float = 2.0
a_string = "Python course"

f"An integer: {an_integer}; a float: {a_float}; another string: {a_string}"
```

Let's say we have multiple data files in a directory on our computer and we want to access a specific file using a configuration parameter:

```python
dataset_id = 20
file_name= f"processing_of_dataset_{dataset_id}.txt"
file_name
```

------------


## Dictionaries


Besides air pressure, I also measure temperature and I'd like to keep the measurements together. 

```python
pressures_hPa = 1013
temperature_degree = 23
```

Storing them all in a single list would make is hard to work with. I want a _container_ or _collection_ data type that maps a name to values. 

We can use a dictionary that **maps keys to values**:

- Contained within curly brackets `{...}`.
- Each element is a combination of a key and a value separated by a colon: `key: value`.
- Elements separated by commas `,`.

```python
measurement = {"pressure_hPa": pressures_hPa,
               "temperature_degree": temperature_degree
              }
measurement
```

A dictionary can be used to store and retrieve values associated with a name. To retrieve the value, use square brackets `[]` with the key:

```python
measurement["pressure_hPa"]
```

New key-value combinations can be added by providing a new key

```python
measurement['location'] = "Ostend"
measurement
```

Assigning a new value to an existing key will overwrite the original value:

```python
measurement['location'] = "Ghent"
measurement
```

You can access the keys and values separately or the items as a list-alike:

```python
list(measurement.keys()), list(measurement.values()), list(measurement.items())
```

----------------------------


## Tuples


Tuples are __immutable lists__. The elements of a tuple are written between parentheses, or just separated by commas

```python
a_tuple = (2, 3, 'aa', [1, 2])
a_tuple
```

```python
a_second_tuple = 2, 3, 'aa', [1,2]
a_second_tuple
```

<div class="alert alert-info">

__Remember__: 
    
mutable vs. immutable:

- Immutable objects cannot be modified once created. When we want to change the value of a string we can only replace the old with a completely new.
- Mutable objects can be changed in place. We can change individual elements, append new elements, or reorder the whole. If you want variables with mutable values to be independent, you must make a copy of the value when you assign it.
    
</div> 




### Exercises


<div class="alert alert-success">

**EXERCISE**:

Assign the string "abracadabra" to a variable `my_spell`. Convert the string variable `my_spell` to uppercase using the appropriate string method.

<details><summary>Hints</summary>

- Use the TAB-key to explore the availabal methods of a string.
- A method need to be called using round brackets (like any other function).
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
my_spell = "abracadabra"
my_spell.upper()
```

<div class="alert alert-success">

**EXERCISE**:

Assign the string "abracadabra" to a variable `my_spell`. Select all of the even-numbered characters from the variable `my_spell`.

<details><summary>Hints</summary>

- Strings are a container of characters and can be sliced just like lists.
    
</details>    
    
</div>

```python tags=["nbtutor-solution"]
my_spell = "abracadabra"
my_spell[1::2]
```

<div class="alert alert-success">

**EXERCISE**:

You did a set of water quality measurements of Dissolved oxygen (mg/l) in different rivers on 18 March 2024. The measurements are combined in a dictionary with variable name `water_quality`.  
    
- Demer: 9.5 mg/l
- Nete: 8.4 mg/l
- Leie: 6.6 mg/l
    
To automate reporting, you want to automate the print statement of the measured dissolved oxygen for a given location:
    
- Define a variable `report_location` which represents one of the measurement locations, e.g. "Nete"
- Use the variable `report_location` to automate the output of the sentence "The measured dissolved oxygen in REPORT-lOCATION on March 3rd 2024 was REPORTED-VALUE mg/l.'. For the "Nete", this would be 'The measured dissolved oxygen in Nete on March 18th 2024 was 8.4 mg/l.'
    
Changing the variable `report_location` to "Demer" should provide the correct output statement for the "Demer".

<details><summary>Hints</summary>

- This requires a combination of a literal string (prefix a string with an f) and the selecting of an element from a dictionary (using square brackets `[]`).    
- Note the different usage of the curly braces `{}`: First to create a dictionary and second to contain the python statements in the literal string.
</details>

</div>

```python
water_quality = {
    "Demer": 9.5,
    "Nete": 8.4,
    "Leie": 6.6
}
```

```python tags=["nbtutor-solution"]
report_location = "Nete"
f"The measured dissolved oxygen in {report_location} on March 18th 2024 was {water_quality[report_location]} mg/l."
```

## For data analysis, a table would be more appropriate... Pandas

```python
import pandas as pd
```

```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
temperature_degree = [23, 20, 17, 8, 12, 5, 16, 22, -2, 16]
locations = ['Ghent - Sterre', 'Ghent - Coupure', 'Ghent - Blandijn', 
             'Ghent - Korenlei', 'Ghent - Kouter', 'Ghent - Coupure',
             'Antwerp - Groenplaats', 'Brussels- Grand place', 
             'Antwerp - Justitipaleis', 'Brussels - Tour & taxis']

measurements = pd.DataFrame({'pressure_hPa': pressures_hPa,
                'temperature_degree': temperature_degree,
                'location': locations})
measurements
```

The Pandas package provides important container types for data manipulation: Tables aka DataFrames.

```python

```
