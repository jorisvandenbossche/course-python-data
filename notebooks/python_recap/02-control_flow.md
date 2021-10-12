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

# Python the basics: control flows

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is largely based on material of the *Python Scientific Lecture Notes* (https://scipy-lectures.github.io/), adapted with some exercises.



## Control flow


### if / elif / else


**Synthax**
> if (boolean expression):

>     do this if true 

> elif (boolean expression):

>     do this if this is true and if condition is false

> else:

>     in all other cases

**Blocks are delimited by indentation**

Be careful to respect the indentation depth. The Ipython shell automatically increases the indentation depth after a column : sign; to decrease the indentation depth, go four spaces to the left with the Backspace key. Press the Enter key twice to leave the logical block.

<!-- #raw -->
> syntax:

> if (boolean expression):

>     do this if true 
<!-- #endraw -->

```python run_control={"frozen": false, "read_only": false}
an_int = 2
if an_int == 1:
    print(1)
elif an_int == 2:
     print(2)
else:
    print('A lot')
```

### for / range


iterating can be over indices (ints) or over values

```python run_control={"frozen": false, "read_only": false}
for index in range(4):
    print(index)

for word in ('cool', 'powerful', 'readable'):
    # remember string formatting?
    print('Python is {}!'.format(word))
```

### while


Typical C-style while loop

```python run_control={"frozen": false, "read_only": false}
z = 1 + 1j
# Mandelbrot problem
while abs(z) < 100:
    z = z**2 + 1
print(z)
```

### break / continue


*break* out of enclosing for/while loop

```python run_control={"frozen": false, "read_only": false}
z = 1 + 1j
# Mandelbrot problem
while abs(z) < 100:
    # breaks if imaginary part is negative
    if z.imag < 0:
        break
    z = z**2 + 1
print(z)
```

*continue* the next iteration of a loop

```python run_control={"frozen": false, "read_only": false}
aList = [1, 0, 2, 4]
print('printing the inverse of the integers, excluding division by zero')
for element in aList:
    if element == 0:
        continue
    print(1. / element)
```

### conditional expressions

<!-- #region -->
> if < OBJECT > :


Evaluates to False:

* any number equal to zero (0, 0.0, 0+0j)
* an empty container (list, tuple, set, dictionary, ...)
* False, None

Evaluates to True:

* everything else


<!-- #endregion -->

> a == b

Tests equality, with logics

```python run_control={"frozen": false, "read_only": false}
1 == 1
```

> a is b:

Tests identity: both sides are the **same object**

```python run_control={"frozen": false, "read_only": false}
print(1 is 1.)
a = 1
b = 1 
print (a is b)
```

> a in b:

For any collection b: b contains a

If b is a dictionary, this tests that a is a key of b.

```python run_control={"frozen": false, "read_only": false}
a_list = [1, 2, 3]
2 in a_list
```

```python run_control={"frozen": false, "read_only": false}
'ae' in a_list
```

```python run_control={"frozen": false, "read_only": false}
3. in a_list
```

```python run_control={"frozen": false, "read_only": false}
a_dict = {'theKey' : 'theValue'}
'theKey' in a_dict
```

```python run_control={"frozen": false, "read_only": false}
'theValue' in a_dict
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
dd = {'antea': 3, 'IMDC': 2, 'arcadis': 4, 'witteveen': 5, 'grontmij': 1}
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Return the key of an item in the dictionary `dd` if the value is provided (assume that the user is always providing a value that is part of the dictionary and all values only occur once). Make sure the returned key is formatted as capitals and use `value=3` to test:
</div>

```python jupyter={"outputs_hidden": true}
value = 2
```

```python clear_cell=true run_control={"frozen": false, "read_only": false}
# return the name of the company given a certain value between 1 and 5:
for k in dd:
    if dd[k] == value:
        print(k.upper())
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Given the dictionary `dd`, check if a key is already existing in the dictionary and print a message `already in dictionary`. Use 'antea' as string to check for.
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
if 'antea' in dd.keys():
    print('already in dictionary')
```

------------------------


## Advanced iteration


#### Iterate over any sequence


You can iterate over any sequence (string, list, keys in a dictionary, lines in a file, ...)

```python run_control={"frozen": false, "read_only": false}
vowels = 'aeiouy'
for i in 'powerful':
    if i in vowels:
        print(i)
```

```python run_control={"frozen": false, "read_only": false}
message = "Hello how are you?"
message.split() # returns a list
for word in message.split():
    print(word)    
```

Few languages (in particular, languages for scientific computing) allow to loop over anything but integers/indices. With Python it is possible to loop exactly over the objects of interest without bothering with indices you often don’t care about. This feature can often be used to make code more readable.


#### Keeping track of enumeration number


Common task is to iterate over a sequence while keeping track of the item number.
* Could use while loop with a counter as above. Or a for loop:

```python run_control={"frozen": false, "read_only": false}
words = ('cool', 'powerful', 'readable')
for i in range(0, len(words)):
    print(i, words[i])
```

* But, Python provides enumerate keyword for this:

```python run_control={"frozen": false, "read_only": false}
for index, item in enumerate(words):
    print(index, item)
```

Or combine elements of two containers:

```python run_control={"frozen": false, "read_only": false}
tea = 'tea'
for word, let in zip(words, tea):
    print(word, let)
```

This can be extended with more iterating options (check the itertools! http://pymotw.com/2/itertools/ !)


#### Looping over dictionary


use **items()**

```python run_control={"frozen": false, "read_only": false}
a_dict = {'One': 1, 'Two':2, 'Three':3}
for key, val in a_dict.items():
    print('Key: {} has value: {}'.format(key, val))
```

```python run_control={"frozen": false, "read_only": false}
for key in a_dict.keys():
    print('Key: {}'.format(key))
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Write code that accepts a sentence and calculate the number of letters and digits.<br>
 
Suppose the following input is supplied to the program:<br><br>

    "hello world! 123"<br><br>
    
Then, the output should be:<br>

    LETTERS 10<br>
    DIGITS 3<br><br>
    
and store the number of letters and digits in a dictionary d={"DIGITS": #digits, "LETTERS": #letters}
</div>


```python clear_cell=true run_control={"frozen": false, "read_only": false}
sentence = "hello world! 123"
d = {"DIGITS": 0, "LETTERS": 0}
for char in sentence:
    if char.isdigit():
        d["DIGITS"] += 1
    elif char.isalpha():
        d["LETTERS"] += 1
    else:
        pass
print("LETTERS", d["LETTERS"])
print("DIGITS", d["DIGITS"])
```

---------------------


## Exception handling


Exceptions are raised by errors in Python:

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
# 1/0 # uncomment this cell
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
## uncomment the following lines in this cell
#d = {1:1, 2:2}  
#d[3]
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
## uncomment the following lines in this cell
#l = [1, 2, 3]
#l.foobar
```

As you can see, there are **different types of exceptions** for different errors.


### raise Exception

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
k = 'a_string'   # test yourself with integer value, e.g. 3
if not isinstance(k, str):
    raise Exception('Provide a string as input!')
```

You can also be more precise about the type of error, eg. TypeError. A list of default provided exceptions is given here: https://docs.python.org/2/library/exceptions.html


    


### try/except: catching exceptions


> try:

>     do something

> except:

>     do this if the other was raising an error


Different exceptions can be combined, raising different type of errors:

```python run_control={"frozen": false, "read_only": false}
k = 'f6'
import sys
try:
    i = int(k)
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexpected error:", sys.exc_info()[0])
    raise
    
```
