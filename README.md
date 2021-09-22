# Description

The Building Information aGGregation, harmonisation and analytics (BIGG) platform is a EU-funded project to aims at demonstrating the application of big data technologies and data analytics techniques for the complete buildings life-cycle of more than 4000 buildings in 6 large-scale pilot test-beds. This repository contains the language-agnostic documentation of the R and Python libraries implemented on the framework of the BIGG AI toolbox.

# Related repositories
- [biggpy - Python library](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr - R package](https://github.com/BeeGroup-cimne/biggr#readme)

# Required data model for the input
biggr and biggpy requires the usage of harmonised buildings-energy-related datasets energy consumption, weather, thermal conditions data 

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

## <code>timeSeries</code>

* R:
> <code>data.frame</code> with two columns. The first one, named "time", defining the series' timestamps using POSIXct class and UTC timezone. The second column represents the value column and it can be of whatever class needed by the variable (<code>character, float, integer, factor</code>).

* Python:
> <code>pandas.Series</code> with indexed time in UTC timezone. The series type can be whatever is needed by the variable (<code>string, float, integer</code>) 


# Modules of the AI toolbox

## Data preparation
It contains the functions used for cleaning, gaps detection, ...
[Access to the data preparation module documentation](DataPreparation.md)

## Data transformation
Lorem ipsum .....
[Access to the data transformation module documentation](DataTransformation.md)

## Modelling
Lorem ipsum .....
[Access to the data transformation module documentation](Modelling.md)

## Cross validation
Lorem ipsum .....
[Access to the data transformation module documentation](CrossValidation.md)

## Model optimisation
Lorem ipsum .....
[Access to the data transformation module documentation](ModelOptimisation.md)

## Reinforcement Learning
Lorem ipsum .....
[Access to the data transformation module documentation](ReinforcementLearning.md)





