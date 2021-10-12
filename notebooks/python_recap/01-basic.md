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

# Python the basics: datatypes

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is largely based on material of the *Python Scientific Lecture Notes* (https://scipy-lectures.github.io/), adapted with some exercises.


## Importing packages


Importing packages is always the first thing you do in python, since it offers the functionalities to work with.

Different options are available:

* <span style="color:green">import <i>package-name</i></span>  <p> importing all functionalities as such
* <span style="color:green">from <i>package-name</i> import <i>specific function</i></span>  <p> importing a specific function or subset of the package
* <span style="color:green">from <i>package-name</i> import *  </span>   <p> importing all definitions and actions of the package (sometimes better than option 1)
* <span style="color:green">import <i>package-name</i> as <i>short-package-name</i></span>    <p> Very good way to keep a good insight in where you use what package


import all functionalities as such

```python run_control={"frozen": false, "read_only": false}
# Two general packages
import os
import sys
```

## Basic python datatypes


### Numerical types


Python supports the following numerical, scalar types:
* integer
* floats
* complex
* boolean

```python run_control={"frozen": false, "read_only": false}
an_integer = 3
print(type(an_integer))
```

```python run_control={"frozen": false, "read_only": false}
an_integer
```

```python run_control={"frozen": false, "read_only": false}
# type casting: converting the integer to a float type
float(an_integer)
```

```python run_control={"frozen": false, "read_only": false}
a_float = 0.2
type(a_float)
```

```python run_control={"frozen": false, "read_only": false}
a_complex = 1.5 + 0.5j
# get the real or imaginary part of the complex number by using the functions
# real and imag on the variable
print(type(a_complex), a_complex.real, a_complex.imag)
```

```python run_control={"frozen": false, "read_only": false}
a_boolean = (3 > 4)
a_boolean
```

A Python shell can therefore replace your pocket calculator, with the basic arithmetic operations addition, substraction, division ... are natively implemented
+, -, *, /, % (modulo) natively implemented

 *operation*| *python implementation* 
----------:| --------------------- 
 addition  | `+`
 substraction | `-`
 multiplication | `*`
 division | `/`
 modulo | `%`
 exponentiation | `**`

```python run_control={"frozen": false, "read_only": false}
print (7 * 3.)
print (2**10)
print (8 % 3)
```

**Attention !**

```python run_control={"frozen": false, "read_only": false}
print(3/2)
print(3/2.)
print(3.//2.)  #integer division
```

### Containers


#### Lists


A list is an ordered collection of objects, that may have different types. The list container supports slicing, appending, sorting ...

Indexing starts at 0 (as in C, C++ or Java), not at 1 (as in Fortran or Matlab)!


```python run_control={"frozen": false, "read_only": false}
a_list = [2.,'aa', 0.2]
a_list
```

```python run_control={"frozen": false, "read_only": false}
# accessing individual object in the list
a_list[1]
```

```python run_control={"frozen": false, "read_only": false}
# negative indices are used to count from the back
a_list[-1]
```

**Slicing**: obtaining sublists of regularly-spaced elements

```python run_control={"frozen": false, "read_only": false}
another_list = ['first', 'second', 'third', 'fourth', 'fifth']
print(another_list[3:])
print(another_list[:2])
print(another_list[::2])
```

* Note that L[start:stop] contains the elements with indices i such as start<= i < stop 
* (i ranging from start to stop-1). Therefore, L[start:stop] has (stop-start) elements.
* Slicing syntax: L[start:stop:stride]
* all slicing parameters are optional


Lists are *mutable* objects and can be modified

```python run_control={"frozen": false, "read_only": false}
another_list[3] = 'newFourth'
print(another_list)
another_list[1:3] = ['newSecond', 'newThird']
print(another_list)
```

Warning, with views equal to each other, they point to the same point in memory. Changing one of them is also changing the other!!

```python run_control={"frozen": false, "read_only": false}
a = ['a',  'b']
b = a
b[0] = 1
print(a)
```

**List methods**:


You can always list the available methods in the namespace by using the dir()-command:

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
#dir(list)
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
a_third_list = ['red', 'blue', 'green', 'black', 'white']
```

```python run_control={"frozen": false, "read_only": false}
# Appending
a_third_list.append('pink')
a_third_list
```

```python run_control={"frozen": false, "read_only": false}
# Removes and returns the last element
a_third_list.pop()
a_third_list
```

```python run_control={"frozen": false, "read_only": false}
# Extends the list in-place
a_third_list.extend(['pink', 'purple'])
a_third_list
```

```python run_control={"frozen": false, "read_only": false}
# Reverse the list
a_third_list.reverse()
a_third_list
```

```python run_control={"frozen": false, "read_only": false}
# Remove the first occurence of an element
a_third_list.remove('white')
a_third_list
```

```python run_control={"frozen": false, "read_only": false}
# Sort list
a_third_list.sort()
a_third_list
```

------------


<div class="alert alert-success">
    <b>EXERCISE</b>: What happens if you put two question marks behind the command?
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
a_third_list.count?
```

```python clear_cell=true run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
a_third_list.index?
```

------------

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
a_third_list = ['red', 'blue', 'green', 'black', 'white']
```

```python run_control={"frozen": false, "read_only": false}
# remove the last two elements
a_third_list = a_third_list[:-2]
a_third_list
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Mimick the functioning of the *reverse* command using the appropriate slicing command:
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
a_third_list[::-1]
```

------------


Concatenating lists is just the same as summing both lists:

```python run_control={"frozen": false, "read_only": false}
a_list = ['pink', 'orange']
a_concatenated_list = a_third_list + a_list
a_concatenated_list
```

<div class="alert alert alert-danger">
    <b>Note</b>: Why is the following not working?
</div>

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
reverted = a_third_list.reverse()
## comment out the next lines to test the error:
#a_concatenated_list = a_third_list + reverted
#a_concatenated_list
```

The list itself is reversed and no output is returned, so reverted is None, which can not be added to a list

------------

```python run_control={"frozen": false, "read_only": false}
# Repeating lists
a_repeated_list = a_concatenated_list*10
print(a_repeated_list)
```

**List comprehensions**

List comprehensions are a very powerful functionality. It creates an in-list for-loop option, looping through all the elements of a list and doing an action on it, in a single, readable line.

```python run_control={"frozen": false, "read_only": false}
number_list = [1, 2, 3, 4]
[i**2 for i in number_list]
```

and with conditional options:

```python run_control={"frozen": false, "read_only": false}
[i**2 for i in number_list if i>1]
```

```python run_control={"frozen": false, "read_only": false}
[i**2 for i in number_list if i>1]
```

```python run_control={"frozen": false, "read_only": false}
# Let's try multiplying with two on a list of strings:
print([i*2 for i in a_repeated_list])
```

Cool, this works! let's check more about strings:


#### Strings


Different string syntaxes (simple, double or triple quotes)

```python run_control={"frozen": false, "read_only": false}
s = 'Never gonna give you up'
print(s)
s = "never gonna let you down"
print(s)
s = '''Never gonna run around 
    and desert you'''         
print(s)
s = """Never gonna make you cry, 
    never gonna say goodbye"""
print(s)
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
## pay attention when using apostrophes! - test out the next two lines one at a time
#print('Hi, what's up?')
#print("Hi, what's up?")
```

The newline character is **\n**, and the tab character is **\t**.

```python run_control={"frozen": false, "read_only": false}
print('''Never gonna tell a lie and hurt you.
Never gonna give you up,\tnever gonna let you down
Never \ngonna\n run around and\t desert\t you''')
```

Strings are collections like lists. Hence they can be indexed and sliced, using the same syntax and rules.

```python run_control={"frozen": false, "read_only": false}
a_string = "hello"
print(a_string[0])
print(a_string[1:5])
print(a_string[-4:-1:2])
```

Accents and special characters can also be handled in Unicode strings (see http://docs.python.org/tutorial/introduction.html#unicode-strings).

```python run_control={"frozen": false, "read_only": false}
print(u'Hello\u0020World !')
```

A string is an immutable object and it is not possible to modify its contents. One may however create new strings from the original one.

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
#a_string[3] = 'q'   # uncomment this cell
```

We won't introduce all methods on strings, but let's check the namespace and apply a few of them:

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
#dir(str) # uncomment this cell
```

```python run_control={"frozen": false, "read_only": false}
another_string = "Strawberry-raspBerry pAstry package party"
another_string.lower().replace('r', 'l', 7)
```

String formatting to make the output as wanted can be done as follows:

```python run_control={"frozen": false, "read_only": false}
print('An integer: %i; a float: %f; another string: %s' % (1, 0.1, 'string'))
```

The [`format` string print](https://pyformat.info/) options in python 3 are able to interpret the conversions itself:

```python run_control={"frozen": false, "read_only": false}
print('An integer: {}; a float: {}; another string: {}'.format(1, 0.1, 'string'))
```

```python run_control={"frozen": false, "read_only": false}
n_dataset_number = 20
sFilename = 'processing_of_dataset_%d.txt' % n_dataset_number
print(sFilename)
```

<div class="alert alert alert-success">
    <b>Exercise</b>: With the `dir(list)` command, all the methods of the list type are printed. However, we're not interested in the hidden methods. Use a list comprehension to only print the non-hidden methods (methods with no starting or trailing '_'):
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
[el for el in dir(list) if not el[0]=='_']
```

<div class="alert alert alert-success">
    <b>Exercise</b>: Given the previous sentence `the quick brown fox jumps over the lazy dog`, split the sentence and put all the word-lengths in a list. 
</div>

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
sentence = "the quick brown fox jumps over the lazy dog"
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
#split in words and get word lengths
[len(word) for word in sentence.split()]
```

------------


#### Dictionaries


A dictionary is basically an efficient table that **maps keys to values**. It is an **unordered** container

It can be used to conveniently store and retrieve values associated with a name

```python run_control={"frozen": false, "read_only": false}
# Always key : value combinations, datatypes can be mixed
hourly_wage = {'Jos':10, 'Frida': 9, 'Gaspard': '13', 23 : 3}
hourly_wage
```

```python run_control={"frozen": false, "read_only": false}
hourly_wage['Jos']
```

Adding an extra element:

```python run_control={"frozen": false, "read_only": false}
hourly_wage['Antoinette'] = 15
hourly_wage
```

You can get the keys and values separately:

```python run_control={"frozen": false, "read_only": false}
hourly_wage.keys()
```

```python run_control={"frozen": false, "read_only": false}
hourly_wage.values()
```

```python run_control={"frozen": false, "read_only": false}
hourly_wage.items() # all combinations in a list
```

```python run_control={"frozen": false, "read_only": false}
# ignore this loop for now, this will be explained later
for key, value in hourly_wage.items():
    print(key,' earns ', value, '€/hour')
```

<div class="alert alert alert-success">
    <b>Exercise</b> Put all keys of the `hourly_wage` dictionary in a list as strings.  If they are not yet a string, convert them:
</div>

```python jupyter={"outputs_hidden": true}
hourly_wage = {'Jos':10, 'Frida': 9, 'Gaspard': '13', 23 : 3}
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
str_key = []
for key in hourly_wage.keys():
    str_key.append(str(key))
str_key
```

----------------------------


#### Tuples


Tuples are basically immutable lists. The elements of a tuple are written between parentheses, or just separated by commas

```python run_control={"frozen": false, "read_only": false}
a_tuple = (2, 3, 'aa', [1, 2])
a_tuple
```

```python run_control={"frozen": false, "read_only": false}
a_second_tuple = 2, 3, 'aa', [1,2]
a_second_tuple
```

the key concept here is mutable vs. immutable
* mutable objects can be changed in place
* immutable objects cannot be modified once created
