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

# Python the basics: functions

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is largely based on material of the *Python Scientific Lecture Notes* (https://scipy-lectures.github.io/), adapted with some exercises.


## Functions


### Function definition


Function blocks must be indented as other control-flow blocks

```python run_control={"frozen": false, "read_only": false}
def the_answer_to_the_universe():
    print(42)

the_answer_to_the_universe()
```

**Note**: the syntax to define a function:

* the def keyword;
* is followed by the function’s name, then
* the arguments of the function are given between parentheses followed by a colon.
* the function body;
* and return object for optionally returning values.



### Return statement


Functions can *optionally* return values

```python run_control={"frozen": false, "read_only": false}
def calcAreaSquare(edge):
    return edge**2
calcAreaSquare(2.3)
```

### Parameters


Mandatory parameters (positional arguments)

```python run_control={"frozen": false, "read_only": false}
def double_it(x):
    return 2*x

double_it(3)
```

```python run_control={"frozen": false, "read_only": false} jupyter={"outputs_hidden": true}
#double_it()
```

Optional parameters (keyword or named arguments)

The order of the keyword arguments does not matter, but it is good practice to use the same ordering as the function's definition

*Keyword arguments* are a very convenient feature for defining functions with a variable number of arguments, especially when default values are to be used in most calls to the function.

```python run_control={"frozen": false, "read_only": false}
def double_it (x=1):
    return 2*x

print(double_it(3))
```

```python run_control={"frozen": false, "read_only": false}
print(double_it())
```

```python run_control={"frozen": false, "read_only": false}
def addition(int1=1, int2=1, int3=1):
    return int1 + 2*int2 + 3*int3

print(addition(int1=1, int2=1, int3=1))
```

```python run_control={"frozen": false, "read_only": false}
print(addition(int1=1, int3=1, int2=1)) # sequence of these named arguments do not matter
```

<div class="alert alert-danger">
    <b>NOTE</b>: <br><br>
  
Default values are evaluated when the function is defined, not when it is called. This can be problematic when using mutable types (e.g. dictionary or list) and modifying them in the function body, since the modifications will be persistent across invocations of the function.

</div>


Using an immutable type in a keyword argument:

```python run_control={"frozen": false, "read_only": false}
bigx = 10
def double_it(x=bigx):
    return x * 2
bigx = 1e9
double_it()
```

Using an mutable type in a keyword argument (and modifying it inside the function body)

```python run_control={"frozen": false, "read_only": false}
def add_to_dict(args={'a': 1, 'b': 2}):
    for i in args.keys():
        args[i] += 1
    print(args)
```

```python run_control={"frozen": false, "read_only": false}
add_to_dict
add_to_dict()
add_to_dict()
add_to_dict()
```

```python run_control={"frozen": false, "read_only": false}
#the {'a': 1, 'b': 2} was created in the memory on the moment that the definition was evaluated
```

```python run_control={"frozen": false, "read_only": false}
def add_to_dict(args=None):
    if not args:
        args = {'a': 1, 'b': 2}
        
    for i in args.keys():
        args[i] += 1
        
    print(args)
```

```python run_control={"frozen": false, "read_only": false}
add_to_dict
add_to_dict()
add_to_dict()
```

### Variable number of parameters



Special forms of parameters:

* *args: any number of positional arguments packed into a tuple
* **kwargs: any number of keyword arguments packed into a dictionary



```python run_control={"frozen": false, "read_only": false}
def variable_args(*args, **kwargs):
    print('args is', args)
    print('kwargs is', kwargs)

variable_args('one', 'two', x=1, y=2, z=3)
```

### Docstrings


Documentation about what the function does and its parameters. General convention:

```python run_control={"frozen": false, "read_only": false}
def funcname(params):
    """Concise one-line sentence describing the function.
    
    Extended summary which can contain multiple paragraphs.
    """
    # function body
    pass

funcname?
```

### Functions are objects



Functions are first-class objects, which means they can be:

* assigned to a variable
* an item in a list (or any collection)
* passed as an argument to another function.



```python run_control={"frozen": false, "read_only": false}
va = variable_args
va('three', x=1, y=2)
```

### Methods


Methods are functions attached to objects. You’ve seen these in our examples on lists, dictionaries, strings, etc...

Calling them can be done by dir(object):


-------------------------------

```python run_control={"frozen": false, "read_only": false}
dd = {'antea': 3, 'IMDC': 2, 'arcadis': 4, 'witteveen': 5, 'grontmij': 1, 'fluves': 6, 'marlinks': 7}
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Make a function of the exercise in the previous notebook: Given the dictionary `dd`, check if a key is already existing in the dictionary and raise an exception if the key already exist. Otherwise, return the dict with the element added.
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
def check_for_key(checkdict, key):
    """
    Function checks the presence of key in dictionary checkdict and returns an 
    exception if the key is already used in the dictionary
    
    """
    if key in checkdict.keys():
        raise Exception('Key already used in this dictionary')
```

```python run_control={"frozen": false, "read_only": false}
check_for_key(dd, 'deme')
```

```python run_control={"frozen": false, "read_only": false}
#check_for_key(dd, 'antea') # uncomment this line
```

-----------------------------


## Object oriented Programming


Wondering what OO is? A very nice introduction is given here: http://py.processing.org/tutorials/objects/


Python supports object-oriented programming (OOP). The goals of OOP are:

* to organize the code, and
* to re-use code in similar contexts.




Here is a small example: we create a Student class, which is an object gathering several custom functions (**methods**) and variables (**attributes**), we will be able to use:

```python run_control={"frozen": false, "read_only": false}
class Employee():  #object
    
    def __init__(self, name, wage=60.):
        """
        Employee class to save the amount of hours worked and related earnings
        """
        self.name = name
        self.wage = wage
        
        self.hours = 0.        
        
    def worked(self, hours):
        """add worked hours on a project
        """
        try:
            hours = float(hours)
        except:
            raise Exception("Hours not convertable to float!")
            
        self.hours += hours
        
    def calc_earnings(self):
        """
        Calculate earnings
        """
        return self.hours *self.wage
```

```python run_control={"frozen": false, "read_only": false}
bert = Employee('bert')
```

```python run_control={"frozen": false, "read_only": false}
bert.worked(10.)
bert.worked(20.)
bert.wage = 80.
```

```python run_control={"frozen": false, "read_only": false}
bert.calc_earnings()
```

```python run_control={"frozen": false, "read_only": false}
dir(Employee)
```

It is just the same as all the other objects we worked with!


---------------------------------------


<div class="alert alert-success">
    <b>EXERCISE</b>: Extend the class `Employee` with a projects attribute, which is a dictionary. Projects can be added by the method `new_project`. Hours are contributed to a specific project
</div>

```python clear_cell=true run_control={"frozen": false, "read_only": false}
class Employee():  #object
    
    def __init__(self, name, wage=60.):
        """
        Employee class to save the amount of hours worked and related earnings
        """
        self.name = name
        self.wage = wage
        self.projects = {}  

    def new_project(self, projectname):
        """
        """
        if projectname in self.projects:
            raise Exception("project already exist for", self.name)
        else:
            self.projects[projectname] = 0.
            
        
    def worked(self, hours, projectname):
        """add worked hours on a project
        """
        try:
            hours = float(hours)
        except:
            raise Exception("Hours not convertable to float!")

        if not projectname in self.projects:
            raise Exception("project non-existing for", self.name)
            
        self.projects[projectname] += hours
        
    def calc_earnings(self):
        """
        Calculate earnings
        """
        total_hours = 0
        for val in self.projects.values():
            total_hours += val
            
        return total_hours *self.wage
    
    def info(self):
        """
        get info
        """
        for proj, hour in self.projects.items():
            print(hour, 'worked on project', proj)
```

```python run_control={"frozen": false, "read_only": false}
bert = Employee('bert')
bert.new_project('vmm')
```

```python run_control={"frozen": false, "read_only": false}
bert.worked(10., 'vmm')
```

```python run_control={"frozen": false, "read_only": false}
bert.calc_earnings()
```

```python run_control={"frozen": false, "read_only": false}
bert.new_project('pwc')
```

```python run_control={"frozen": false, "read_only": false}
bert.info()
```

```python run_control={"frozen": false, "read_only": false}
bert.worked(3., 'pwc')
```

---------------------------------------
