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
* Python: <code>integer</code> type 

## <code>boolean</code>
* R: <code>logical</code> class
* Python: <code>boolean</code> type 

## <code>date</code>
* R: <code>Date</code> class
* Python: <code>datetime.date</code> class (datetime library is needed)

## <code>datetime</code>
* R: <code>POSIXct</code> class in UTC timezone.
* Python: <code>datetime.datetime</code> class (datetime library is needed) in UTC timezone.

## <code>list</code>
* R: <code>c(element1,element2,...,elementN)</code> function, where all elements belong to the same class: character, float, integer, logical, Date or POSIXct classes.
* Python: <code>[element1,element2,...,elementN]</code>, where all elements belong to the same type: string, float, integer, boolean, or whatever Python class.

## <code>dictionary</code>
* R: <code>list(key1=value1,key2=value2,...,keyN=valueN)</code>, where all keys belong to character class, and values can be float, integer, logical, Date, POSIXct, list or data.frame classes.
* Python: <code>{key1:value1,key2:value2,...,keyN:valueN}</code>, where all keys belong to string type, and values can be
string, float, integer, boolean, or whatever Python class.

## <code>timeSeries</code>

* R: <code>data.frame</code> with two columns. The first one, named "time", defining the series' initial timestamp using Date/POSIXct class and UTC timezone. The second column represents the value column and it can be of whatever class needed by the variable.

* Python: <code>pandas.Series</code> class with indexed time in UTC timezone (datetime/date class). The series type can be whatever is needed by the variable (pandas library is needed)

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





