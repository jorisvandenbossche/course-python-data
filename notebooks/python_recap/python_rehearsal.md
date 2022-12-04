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
    display_name: Python [conda env:DS-python]
    language: python
    name: conda-env-DS-python-py
---

<!-- #region -->
<p><font size="6"><b> Python rehearsal</b></font></p>


> *© 2021, Joris Van den Bossche and Stijn Van Hoey  (<mailto:jorisvandenbossche@gmail.com>, <mailto:stijnvanhoey@gmail.com>). Licensed under [CC BY 4.0 Creative Commons](http://creativecommons.org/licenses/by/4.0/)*

---
<!-- #endregion -->

## I measure air pressure

```python
pressure_hPa = 1010 # hPa
```

<div class="alert alert-info">
    <b>REMEMBER</b>: Use meaningful variable names
</div>


I'm measuring at sea level, what would be the air pressure of this measured value on other altitudes?


## I'm curious what the equivalent pressure would be on other alitudes...


The **barometric formula**, sometimes called the exponential atmosphere or isothermal atmosphere, is a formula used to model how the **pressure** (or density) of the air **changes with altitude**. The pressure drops approximately by 11.3 Pa per meter in first 1000 meters above sea level.

$$P=P_0 \cdot \exp \left[\frac{-g \cdot M \cdot h}{R \cdot T}\right]$$

see https://www.math24.net/barometric-formula/ or https://en.wikipedia.org/wiki/Atmospheric_pressure


where:
* $T$ = standard temperature, 288.15 (K)
* $R$ = universal gas constant, 8.3144598, (J/mol/K)
* $g$ = gravitational acceleration, 9.81 (m/s$^2$)
* $M$ = molar mass of Earth's air, 0.02896 (kg/mol)

and:
* $P_0$ = sea level pressure (hPa)
* $h$ = height above sea level (m)


Let's implement this...


To calculate the formula, I need the exponential operator. Pure Python provide a number of mathematical functions, e.g. https://docs.python.org/3.7/library/math.html#math.exp within the `math` library

```python
import math
```

```python
# ...modules and libraries...
```

<div class="alert alert-danger">
    <b>DON'T</b>: <code>from os import *</code>. Just don't!
</div>

```python
standard_temperature = 288.15
gas_constant = 8.31446
gravit_acc = 9.81
molar_mass_earth = 0.02896
```

<div class="alert alert-success">
    <b>EXERCISE</b>: 
        <ul>
          <li>Calculate the equivalent air pressure at the altitude of 2500 m above sea level for our measured value of <code>pressure_hPa</code> (1010 hPa)</li>
    </ul> 
</div>

```python clear_cell=true
height = 2500
pressure_hPa * math.exp(-gravit_acc * molar_mass_earth* height/(gas_constant*standard_temperature))
```

```python
# ...function/definition for barometric_formula... 
```

```python clear_cell=true
def barometric_formula(pressure_sea_level, height=2500):
    """Apply barometric formula
    
    Apply the barometric formula to calculate the air pressure on a given height
    
    Parameters
    ----------
    pressure_sea_level : float
        pressure, measured as sea level
    height : float
        height above sea level (m)
    
    Notes
    ------
    see https://www.math24.net/barometric-formula/ or 
    https://en.wikipedia.org/wiki/Atmospheric_pressure
    """
    standard_temperature = 288.15
    gas_constant = 8.3144598
    gravit_acc = 9.81
    molar_mass_earth = 0.02896
    
    pressure_altitude = pressure_sea_level * math.exp(-gravit_acc * molar_mass_earth* height/(gas_constant*standard_temperature))
    return pressure_altitude
```

```python
barometric_formula(pressure_hPa, 2000)
```

```python
barometric_formula(pressure_hPa)
```

```python
# ...formula not valid above 11000m... 
# barometric_formula(pressure_hPa, 12000)
```

```python clear_cell=true
def barometric_formula(pressure_sea_level, height=2500):
    """Apply barometric formula
    
    Apply the barometric formula to calculate the air pressure on a given height
    
    Parameters
    ----------
    pressure_sea_level : float
        pressure, measured as sea level
    height : float
        height above sea level (m)
    
    Notes
    ------
    see https://www.math24.net/barometric-formula/ or 
    https://en.wikipedia.org/wiki/Atmospheric_pressure
    """
    if height > 11000:
        raise Exception("Barometric formula only valid for heights lower than 11000m above sea level")
    
    standard_temperature = 288.15
    gas_constant = 8.3144598
    gravit_acc = 9.81
    molar_mass_earth = 0.02896
    
    pressure_altitude = pressure_sea_level * math.exp(-gravit_acc * molar_mass_earth* height/(gas_constant*standard_temperature))
    return pressure_altitude
```

```python
# ...combining logical statements... 
```

```python
height > 11000 or pressure_hPa < 9000
```

```python
# ...load function from file...
```

Instead of having the functions in a notebook, importing the function from a file can be done as importing a function from an installed package. Save the function `barometric_formula` in a file called `barometric_formula.py` and add the required import statement `import math` on top of the file. Next, run the following cell:

```python clear_cell=false
from barometric_formula import barometric_formula
```

<div class="alert alert-info">
    <b>REMEMBER</b>: 
     <ul>
      <li>Write functions to prevent copy-pasting of code and maximize reuse</li>
      <li>Add documentation to functions for your future self</li>
      <li>Named arguments provide default values</li>
      <li>Import functions from a file just as other modules</li>
    </ul> 
    
</div>


## I measure air pressure multiple times


We can store these in a Python [list](https://docs.python.org/3/tutorial/introduction.html#lists):

```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
```

```python
# ...check methods of lists... append vs insert
```

<div class="alert alert-warning">
    <b>Notice</b>: 
        <ul>
          <li>A list is a general container, so can exist of mixed data types as well.</li>
    </ul> 
</div>

```python
# ...list is a container...
```

### I want to apply my function to each of these measurements


I want to calculate the barometric formula **for** each of these measured values.

```python
# ...for loop... dummy example
```

<div class="alert alert-success">
    <b>EXERCISE</b>: 
        <ul>
          <li>Write a <code>for</code> loop that prints the adjusted value for altitude 3000m for each of the pressures in <code>pressures_hPa</code> </li>
    </ul> 
</div>

```python clear_cell=true
for pressure in pressures_hPa:
    print(barometric_formula(pressure, 3000))
```

```python
# ...list comprehensions...
```

<div class="alert alert-success">
    <b>EXERCISE</b>: 
        <ul>
          <li>Write a <code>for</code> loop as a list comprehension to calculate the adjusted value for altitude 3000m for each of the pressures in <code>pressures_hPa</code> and store these values in a new variable <code>pressures_hPa_adjusted</code></li>
    </ul> 
</div>

```python clear_cell=true
pressures_hPa_adjusted = [barometric_formula(pressure, 3000) for pressure in pressures_hPa]
pressures_hPa_adjusted
```

### The power of numpy

```python
import numpy as np
```

```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
```

```python
np_pressures_hPa = np.array([1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001])
```

```python
# ...slicing/subselecting is similar...
```

```python
print(np_pressures_hPa[0], pressures_hPa[0])
```

```python

```

<div class="alert alert-info">
    <b>REMEMBER</b>: 
    <ul>
        <li> <code>[]</code> for accessing elements
        <li> <code>[start:end:step]</code>
    </ul>
</div>

```python
# ...original function using numpy array instead of list... do both
```

```python clear_cell=true
np_pressures_hPa * math.exp(-gravit_acc * molar_mass_earth* height/(gas_constant*standard_temperature))
```

<div class="alert alert-info">
    <b>REMEMBER</b>: The operations do work on all elements of the array at the same time, you don't need a <strike>`for` loop<strike>
</div>


It is also a matter of **calculation speed**:

```python
lots_of_pressures = np.random.uniform(990, 1040, 1000)
```

```python
%timeit [barometric_formula(pressure, 3000) for pressure in list(lots_of_pressures)]
```

```python
%timeit lots_of_pressures * np.exp(-gravit_acc * molar_mass_earth* height/(gas_constant*standard_temperature))
```

<div class="alert alert-info">
    <b>REMEMBER</b>: for calculations, numpy outperforms python
</div>


### Boolean indexing and filtering (!)

```python
np_pressures_hPa
```

```python
np_pressures_hPa > 1000
```

You can use this as a filter to select elements of an array:

```python
boolean_mask = np_pressures_hPa > 1000
np_pressures_hPa[boolean_mask]
```

or, also to change the values in the array corresponding to these conditions:

```python
boolean_mask = np_pressures_hPa < 900
np_pressures_hPa[boolean_mask] = 900
np_pressures_hPa
```

**Intermezzo:** Exercises boolean indexing:

```python
AR = np.random.randint(0, 20, 15)
AR
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Count the number of values in AR that are larger than 10 
    
_Tip:_ You can count with True = 1 and False = 0 and take the sum of these values
</div>

```python clear_cell=true
sum(AR > 10)
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Change all even numbers of `AR` into zero-values.
</div>

```python clear_cell=true
AR[AR%2 == 0] = 0
AR
```

<div class="alert alert-success">
    <b>EXERCISE</b>: Change all even positions assuming position index starts at 1 (positions 1, 2, 3...) of matrix AR into the value 30
</div>

```python clear_cell=true
AR[1::2] = 30
AR
```

<div class="alert alert-success">

**EXERCISE**
    
Select all values above the 75th percentile of the following array AR2 ad take the square root of these values

<details><summary>Hints</summary>

- Numpy provides a function `percentile` to calculate a given percentile

</details>

</div>

```python clear_cell=false
AR2 = np.random.random(10)
AR2
```

```python clear_cell=true
np.sqrt(AR2[AR2 > np.percentile(AR2, 75)])
```

<div class="alert alert-success">

**EXERCISE**
    
Convert all values -99 of the array AR3 into Nan-values 

<details><summary>Hints</summary>

- Nan values can be provided in float arrays as `np.nan` and that numpy provides a specialized function to compare float values, i.e. `np.isclose()`

</details>

</div>

```python
AR3 = np.array([-99., 2., 3., 6., 8, -99., 7., 5., 6., -99.])
```

```python clear_cell=true
AR3[np.isclose(AR3, -99)] = np.nan
AR3
```

## I also have measurement locations

```python
location = 'Ghent - Sterre'
```

```python
# ...check methods of strings... split, upper,...
```

```python

```

```python
locations = ['Ghent - Sterre', 'Ghent - Coupure', 'Ghent - Blandijn', 
             'Ghent - Korenlei', 'Ghent - Kouter', 'Ghent - Coupure',
             'Antwerp - Groenplaats', 'Brussels- Grand place', 
             'Antwerp - Justitipaleis', 'Brussels - Tour & taxis']
```

<div class="alert alert alert-success">
    <b>EXERCISE</b>: Use a list comprehension to convert all locations to lower case.
    
_Tip:_ check the available methods of lists by writing: `location.` + TAB button
</div>

```python clear_cell=true
[location.lower() for location in locations]
```

## I also measure temperature

```python
pressures_hPa = [1013, 1003, 1010, 1020, 1032, 993, 989, 1018, 889, 1001]
temperature_degree = [23, 20, 17, 8, 12, 5, 16, 22, -2, 16]
locations = ['Ghent - Sterre', 'Ghent - Coupure', 'Ghent - Blandijn', 
             'Ghent - Korenlei', 'Ghent - Kouter', 'Ghent - Coupure',
             'Antwerp - Groenplaats', 'Brussels- Grand place', 
             'Antwerp - Justitipaleis', 'Brussels - Tour & taxis']
```

Python [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) are a convenient way to store multiple types of data together, to not have too much different variables:

```python
measurement = {}
measurement['pressure_hPa'] = 1010
measurement['temperature'] = 23
```

```python
measurement
```

```python
# ...select on name, iterate over keys or items...
```

```python

```

```python
measurements = {'pressure_hPa': pressures_hPa,
                'temperature_degree': temperature_degree,
                'location': locations}
```

```python
measurements
```

__But__: I want to apply my barometric function to measurements taken in Ghent when the temperature was below 10 degrees...

```python
for idx, pressure in enumerate(measurements['pressure_hPa']):
    if measurements['location'][idx].startswith("Ghent") and \
        measurements['temperature_degree'][idx]< 10:
        print(barometric_formula(pressure, 3000))
        
```

## when a table would be more appropriate... Pandas!

```python
import pandas as pd
```

```python
measurements = pd.DataFrame(measurements)
measurements
```

```python
barometric_formula(measurements[(measurements["location"].str.contains("Ghent")) & 
                  (measurements["temperature_degree"] < 10)]["pressure_hPa"], 3000)
```

<div class="alert alert-info">
    <b>REMEMBER</b>: We can combine the speed of numpy with the convenience of dictionaries and much more!
</div>
