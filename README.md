# Description

The Building Information aGGregation, harmonisation and analytics (BIGG) platform is a EU-funded project to aims at demonstrating the application of big data technologies and data analytics techniques for the complete buildings life-cycle of more than 4000 buildings in 6 large-scale pilot test-beds. This repository contains the language-agnostic documentation of the R and Python libraries implemented on the framework of the BIGG AI toolbox.

# Related repositories
- [biggpy - Python library](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr - R package](https://github.com/BeeGroup-cimne/biggr#readme)

# Required data model for the input
The functionalities implemented in biggr and biggpy requires the usage of harmonised datasets of the energy consumption, weather information, building characteristics and thermal conditions of buildings. This data model is presented in the WP4 of the BIGG project, and can be found in [this repository](www.google.com).

# Data types definition

## <code>string</code>
* R: <code>character</code> class
* Python: <code>string</code> type 

## <code>float</code>
* R: <code>float</code> class
* Python: <code>float</code> type 

## <code>integer</code>
* R: <code>integer</code> class
* Python: <code>int</code> type 

## <code>boolean</code>
* R: <code>logical</code> class
* Python: <code>bool</code> type 

## <code>date</code>
* R: <code>Date</code> class
* Python: <code>datetime.date</code> class (datetime library is needed)

## <code>datetime</code>
* R: <code>POSIXct</code> class in UTC timezone.
* Python: <code>datetime.datetime</code> class (datetime library is needed) in UTC timezone.

## <code>list</code>
* R: <code>c(<element1>,<element2>,...,<elementN>)</code> function, where all elements belong to character, float, integer, logical, Date or POSIXct classes.
* Python: <code>list</code><code>[<element1>,<element2>,...,<elementN>]</code>, where all elements belong to the types \
string, float, integer, or boolean, or to the datetime.date or datetime.datetime classes or whatever Python class.

## <code>dictionary</code>
* R: <code>list(<key1>:<value1>,<key2>:<value2>,...,<keyN>:<valueN>)</code>, where all keys belong to character class, and values can be float, integer, logical, Date, POSIXct, list or data.frame classes.
* Python: <code>dict</code><code>{<key1>:<value1>,<key2>:<value2>,...,<keyN>:<valueN>}</code>, where all keys belong to string, int or any non mutable type, and values can be
string, float, integer, boolean, list or whatever Python class.

## <code>generator</code>
Generators are iterators, but you can only iterate over them once. 
It’s because they do not store all the values in memory, they generate the values on the fly. 
Generators are best for calculating large sets of results (particularly calculations involving loops themselves) where 
you don’t want to allocate the memory for all results at the same time. 
You use them by iterating over them, either with a ‘for’ loop or by passing them to any function or construct that 
iterates. Most of the time generators are implemented as functions. However, they do not <code>return</code> a value, 
they <code>yield</code> it.

* Python: <code>generator</code> A generator expression in Python can be created with a one-liner expression like:

```python 
g = (x**2 for x in range(10))
print g.next()
```
or can be created by defining a python function that <code>yield</code> a result, like:
```python
def __gen(exp):
    for x in exp:
        yield x**2
g = __gen(iter(range(10)))
print g.next()
```
* R

## <code>timeSeries</code>

* R:
> <code>data.frame</code> with two columns. The first one, named "time", defining the series' initial timestamp using POSIXct class and UTC timezone. The second column represents the value column and it can be of whatever class needed by the variable (<code>character, float, integer, factor</code>).

* Python:
> <code>pandas.Series</code> class with indexed time in UTC timezone. The series type can be whatever is needed by the variable (<code>string, float, integer</code>) 

> <code>pandas.DataFrame</code> class with a DateTimeIndex and one column representing the values of the series. The series type can be whatever is needed by the variable (<code>string, float, integer</code>) 

* Note! In the case of non cumulative consumption or some weather feature time series, the time stamp of each element represents the initial time where the value applies. We assume that it does not change during that time step, until the next time series element.

## <code>matrix</code>
* R: <code>data.frame</code>
* Python: <code>pandas.DataFrame</code>

## <code>object</code>
* R: <code>whatever object</code> that can be serialised
* Python: <code>whatever object</code> that can be serialised

# Modules of the AI toolbox

## Data preparation
It contains the functions used for time stamps alignment, data validation, time gaps detection, outlier detection and missing data management

[Access to the data preparation module documentation](DataPreparation.md)

## Data transformation
It contains functions used for transforming cleaned datasets to valuable features for the usage in statistical and machine learning algorithms. This functions includes low pass filtering techniques, fourier decomposition, obtaining calendar features, multiple seasonalities profiling, weather features transformations, among others.

[Access to the data transformation module documentation](DataTransformation.md)

## Modelling
This module contains the functions used to train, optimise, predict and assess the statistical or machine learning models applied to energy consumption, thermal comfort and user behaviour in buildings.

[Access to the data transformation module documentation](Modelling.md)

## Reinforcement Learning
This is a very brief description of the functions implemented in this module.

[Access to the data transformation module documentation](ReinforcementLearning.md)





