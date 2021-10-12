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

# Python the basics: Reusing code

> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---

> This notebook is largely based on material of the *Python Scientific Lecture Notes* (https://scipy-lectures.github.io/), adapted with some exercises.



## Introduction

For now, we have typed all instructions in the interpreter. For longer sets of instructions we need to change track and write the code in text files (using a text editor or Spyder), that we will call either scripts or modules.

* Sets of instructions that are called several times should be written inside **functions** for better code reusability.
* Functions (or other bits of code) that are called from several scripts should be written inside a **module**, so that only the module is imported in the different scripts (do not copy-and-paste your functions in the different scripts!).



<!-- #region -->
## Scripts


Let us first write a *script*, that is a file with a sequence of instructions that are executed each time the script is called. Instructions may be e.g. copied-and-pasted from the interpreter (but take care to respect indentation rules!).

The extension for Python files is ``.py``. Write or copy-and-paste the following lines in a file called test.py
<!-- #endregion -->

```python run_control={"frozen": false, "read_only": false}
%%file test.py

message = "Hello how are you?"
for word in message.split():
    print(word)
```

Let us now execute the script interactively, that is inside the Ipython interpreter. This is maybe the most common use of scripts in scientific computing.

```python run_control={"frozen": false, "read_only": false}
%run test.py
```

```python run_control={"frozen": false, "read_only": false}
message
```

The script has been executed. Moreover the variables defined in the script (such as ``message``) are now available inside the interpreter’s namespace.

Other interpreters also offer the possibility to execute scripts (e.g., ``execfile`` in the plain Python interpreter, etc.).

It is also possible to execute this script as a *standalone program*, by executing the script inside a shell terminal (Linux/Mac console or cmd Windows console). For example, if we are in the same directory as the test.py file, we can execute this in a console:

```python run_control={"frozen": false, "read_only": false}
!python test.py
```

## Creating modules

If we want to write larger and better organized programs (compared to simple scripts), where some objects are defined, (variables, functions, classes) and that we want to reuse several times, we have to create our own *modules*.

Let us create a module demo contained in the file ``demo.py``:

```python run_control={"frozen": false, "read_only": false}
%%file demo.py

"A demo module."

def print_b():
    "Prints b."
    print('b')

def print_a():
    "Prints a."
    print('a')


c = 2
d = 2
```

In this file, we defined two functions ``print_a`` and ``print_b``. Suppose we want to call the ``print_a`` function from the interpreter. We could execute the file as a script, but since we just want to have access to the function ``print_a``, we are rather going to **import it as a module**. The syntax is as follows.

```python run_control={"frozen": false, "read_only": false}
import demo
```

```python run_control={"frozen": false, "read_only": false}
demo.print_a()
```

```python run_control={"frozen": false, "read_only": false}
demo.print_b()
```

## `__main__` function and executing scripts

```python run_control={"frozen": false, "read_only": false}
%%file demo2.py

"A second demo module with a main function."

def print_a():
    "Prints a."
    print('a')

def print_b():
    "Prints b."
    print('b')
    
if __name__ == '__main__':
    print_a()
```

```python run_control={"frozen": false, "read_only": false}
!python demo2.py
```

```python run_control={"frozen": false, "read_only": false}
import demo2
demo2.print_b()
```

Standalone scripts may also take command-line arguments:

```python run_control={"frozen": false, "read_only": false}
%%file demo3.py

import sys

if __name__ == '__main__':
    print(sys.argv)
```

```python run_control={"frozen": false, "read_only": false}
!python demo3.py arg1 arg2
```
