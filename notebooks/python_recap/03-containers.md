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

---

> This notebook is based on material of the [*Python Scientific Lecture Notes*](https://scipy-lectures.github.io/), and the [*Software Carptentry: Programming with Python course*](https://swcarpentry.github.io/python-novice-gapminder).


## Containers


If I measure air pressure multiple times, doing calculations with a hundred variables called `pressure_001`, `pressure_002`, etc., would be slow. We need _container_ or _collection_ data types to combine multiple values in a single object.


### Lists


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

<div class="alert alert-warning">

__Warning__: 
    
Indexing in Python starts at 0 (as in C, C++ or Java), not at 1 (as in Fortran or Matlab)!
    
</div>


Lists are __mutable__ objects and can be modified. Lists’ values can be replaced by assigning to them. Use an index expression on the left of assignment to replace a value:

```python
a_list[1] = 3000
a_list
```

__Slicing__ obtaining sublists of regularly-spaced elements:

```python
another_list = ['first', 'second', 'third', 'fourth', 'fifth']
print(another_list[2:4])
print(another_list[3:])
print(another_list[:2])
print(another_list[::2])
```

<div class="alert alert-info">

__Info__: 
    
* `L[start:stop]` contains the elements with indices i so `start <= i < stop`
* i ranging from start to stop-1. Therefore, L[start:stop] has (stop-start) elements.
* Slicing syntax: `L[start:stop:stride]`
* all slicing parameters are optional
</div>


Warning, with views equal to each other, they point to the same point in memory. Changing one of them is also changing the other!!

```python
a = ['a',  'b']
b = a
b[0] = 1
print(a)
```

**List methods**:


You can list the available _methods_ in the namespace using the `TAB` key  or using the (built-in) `dir()`-function:

```python tags=[]
#dir(list)
```

```python tags=[]
a_third_list = ['red', 'blue', 'green', 'black', 'white']
```

```python
# Appending
a_third_list.append('pink')
a_third_list
```

```python
# Removes and returns the last element
a_third_list.pop()
a_third_list
```

```python
# Extends the list in-place
a_third_list.extend(['pink', 'purple'])
a_third_list
```

```python
# Reverse the list
a_third_list.reverse()
a_third_list
```

```python
# Remove the first occurence of an element
a_third_list.remove('white')
a_third_list
```

```python
# Sort list
a_third_list.sort()
a_third_list
```

------------

```python tags=[]
a_third_list = ['red', 'blue', 'green', 'black', 'white']
```

```python
# remove the last two elements
a_third_list = a_third_list[:-2]
a_third_list
```

### TODO - exercises


<div class="alert alert-success">

**EXERCISE**:

Mimick the functioning of the *reverse* command using the appropriate slicing command:

<details><summary>Hints</summary>

- The slicing syntax is `L[start:stop:stride]` and the stride can be a negative value
- To access from start till end, the `start` and `stop` can be empty.
    
</details>    
    
</div>

```python clear_cell=true
a_third_list[::-1]
```

------------


Concatenating lists is just the same as summing both lists:

```python
a_list = ['pink', 'orange']
a_concatenated_list = a_third_list + a_list
a_concatenated_list
```

<div class="alert alert alert-danger">
    <b>Note</b>: Why is the following not working?
</div>

```python
reverted = a_third_list.reverse()
## comment out the next lines to test the error:
#a_concatenated_list = a_third_list + reverted
#a_concatenated_list
```

The list itself is reversed and no output is returned, so reverted is None, which can not be added to a list

------------

```python
# Repeating lists
a_repeated_list = a_concatenated_list*10
print(a_repeated_list)
```

**List comprehensions**

List comprehensions are a very powerful functionality. It creates an in-list for-loop option, looping through all the elements of a list and doing an action on it, in a single, readable line.

```python
number_list = [1, 2, 3, 4]
[i**2 for i in number_list]
```

and with conditional options:

```python
[i**2 for i in number_list if i>1]
```

```python
[i**2 for i in number_list if i>1]
```

```python
# Let's try multiplying with two on a list of strings:
print([i*2 for i in a_repeated_list])
```

Cool, this works! let's check more about strings:


#### Strings


NOTE 2024 -> just short reference to list (slicing,...) and that it is immutable
https://swcarpentry.github.io/python-novice-gapminder/11-lists.html#character-strings-can-be-indexed-like-lists.


Different string syntaxes (simple, double or triple quotes)

```python
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

```python
## pay attention when using apostrophes! - test out the next two lines one at a time
#print('Hi, what's up?')
#print("Hi, what's up?")
```

The newline character is **\n**, and the tab character is **\t**.

```python
print('''Never gonna tell a lie and hurt you.
Never gonna give you up,\tnever gonna let you down
Never \ngonna\n run around and\t desert\t you''')
```

Strings are collections like lists. Hence they can be indexed and sliced, using the same syntax and rules.

```python
a_string = "hello"
print(a_string[0])
print(a_string[1:5])
print(a_string[-4:-1:2])
```

Accents and special characters can also be handled in Unicode strings (see http://docs.python.org/tutorial/introduction.html#unicode-strings).

```python
print(u'Hello\u0020World !')
```

A string is an immutable object and it is not possible to modify its contents. One may however create new strings from the original one.

```python
#a_string[3] = 'q'   # uncomment this cell
```

We won't introduce all methods on strings, but let's check the namespace and apply a few of them:

```python
#dir(str) # uncomment this cell
```

```python
another_string = "Strawberry-raspBerry pAstry package party"
another_string.lower().replace('r', 'l', 7)
```

String formatting to make the output as wanted can be done as follows:

```python
print('An integer: %i; a float: %f; another string: %s' % (1, 0.1, 'string'))
```

The [`format` string print](https://pyformat.info/) options in python 3 are able to interpret the conversions itself:

```python
print('An integer: {}; a float: {}; another string: {}'.format(1, 0.1, 'string'))
```

```python
n_dataset_number = 20
sFilename = 'processing_of_dataset_%d.txt' % n_dataset_number
print(sFilename)
```

<div class="alert alert alert-success">
    <b>Exercise</b>: With the `dir(list)` command, all the methods of the list type are printed. However, we're not interested in the hidden methods. Use a list comprehension to only print the non-hidden methods (methods with no starting or trailing '_'):
</div>

```python clear_cell=true
[el for el in dir(list) if not el[0]=='_']
```

<div class="alert alert alert-success">
    <b>Exercise</b>: Given the previous sentence `the quick brown fox jumps over the lazy dog`, split the sentence and put all the word-lengths in a list. 
</div>

```python
sentence = "the quick brown fox jumps over the lazy dog"
```

```python clear_cell=true
#split in words and get word lengths
[len(word) for word in sentence.split()]
```

------------


#### Dictionaries


A dictionary is basically an efficient table that **maps keys to values**. It is an **unordered** container

It can be used to conveniently store and retrieve values associated with a name

```python
# Always key : value combinations, datatypes can be mixed
hourly_wage = {'Jos':10, 'Frida': 9, 'Gaspard': '13', 23 : 3}
hourly_wage
```

```python
hourly_wage['Jos']
```

Adding an extra element:

```python
hourly_wage['Antoinette'] = 15
hourly_wage
```

You can get the keys and values separately:

```python
hourly_wage.keys()
```

```python
hourly_wage.values()
```

```python
hourly_wage.items() # all combinations in a list
```

```python
# ignore this loop for now, this will be explained later
for key, value in hourly_wage.items():
    print(key,' earns ', value, '€/hour')
```

<div class="alert alert alert-success">
    <b>Exercise</b> Put all keys of the `hourly_wage` dictionary in a list as strings.  If they are not yet a string, convert them:
</div>

```python
hourly_wage = {'Jos':10, 'Frida': 9, 'Gaspard': '13', 23 : 3}
```

```python clear_cell=true
str_key = []
for key in hourly_wage.keys():
    str_key.append(str(key))
str_key
```

----------------------------


#### Tuples


Tuples are basically immutable lists. The elements of a tuple are written between parentheses, or just separated by commas

```python
a_tuple = (2, 3, 'aa', [1, 2])
a_tuple
```

```python
a_second_tuple = 2, 3, 'aa', [1,2]
a_second_tuple
```

the key concept here is mutable vs. immutable
* mutable objects can be changed in place
* immutable objects cannot be modified once created
