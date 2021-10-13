[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Data Preparation / Time Stamps Alignment

## :round_pushpin: detect_time_step

### Description:
    
The function infers, i.e. automatically deduce from the input data, the minimum time step (frequency) that can be used
for the input time series.

### Input arguments:
* _data_: <code>timeSeries</code>. Input time series whose time step has to be detected.
* _maxMissingTimeSteps_: <code>int</code>. Optional tolerance threshold (percentage) specifying the maximum number 
of missing values in the time series. 

### Return values: 
* _timeStep_: <code>string</code>. A string in ISO 8601 format representing the time step (e.g. "15T","1H", "3M", "1D"
,...).

If no frequency can be detected, the function will return None.

### Details:
The function takes as input a time-indexed series and optionally the type of the input series and returns the time step
that can be associated with it, represented with a string alias formatted according to the ISO 8601.

The supported string frequency aliases are:

|Alias|Description|
|:----------:|:-------------:|
|S|secondly frequency|
|T, min|minutely frequency|
|H|hourly frequency|
|D|daily frequency|
|W|weekly frequency|
|MS|month start frequency|
|M|month end frequency|
|AS, YS|year start frequency|
|A, Y|year end frequency|

If the frequency is not uniform, because of missing values for example, the function will still try to infer the 
frequency and return the best guess, i.e. the most frequent time step.

The function handles explicit missing values, i.e. samples having NaN as value or implicit missing values, i.e.
samples that are missing in the series given a detected minimum frequency. 

If _maxMissingTimeSteps_ is not provided, the 
function will return the minimum detected frequency regardless of the number of missing values. If it is provided, in case of 
non-uniform time series, the function will return the best frequency to use, i.e. the minimum frequency of the series
with a number of missing time steps lower than or equal to this argument value.

## :round_pushpin: align_time_grid

### Description:
    
The function aligns the frequency of the input time series with the output frequency given as an argument using the 
specified aggregation function. 
### Input arguments:
* _data_: <code>timeSeries</code>. The time series that has to be aligned with an output time step,
i.e. with a specific frequency. If the _measurementReadingType_ of the series is not _instantaneous_, the data must 
be converted first using the function _clean_ts_integrate_.
* _outputTimeStep_: <code>string</code>. The frequency used to resample the input time series for the alignment.
It must be a string in ISO 8601 format representing the time step (e.g. "15T","1H", "M", "D",...).
* _aggregationFunction_: <code>enumeration</code>. The aggregation function to use when resampling the series. Possible
values are:
  * _avg_: average value in a time period.
  * _sum_: sum of the values in a time period.
  * _min_: minimum value in a time period.
  * _max_: maximum value in a time period.

### Return values: 
* _data_: <code>timeSeries</code>. The time series resampled with the specified period and aggregation function.

### Details:
The function takes as input a time-indexed series with an output time step (frequency) and an aggregation function
and returns a downsampled series using the specified arguments. This function must be used only after the time series is
converted to the _instantaneous_ type of measurement with the function _clean_ts_integrate_. 

## :round_pushpin: clean_ts_integrate

### Description:
    
The function converts a _cumulative_ (counter) or _onChange_ (delta) measurement to instantaneous.
### Input arguments:
* _data_: <code>timeSeries</code>. The cumulative or on-change time series that has to be converted to instantaneous.
An _instantaneous_ measurement is a gauge metric, in which the value can increase or decrease and measures a specific
instant in time.
* _measurementReadingType_: <code>enumeration</code>. An argument used to define the measurement reading type. 
Possible values:
  * _onChange_: representing a delta metric, in which the value measures the change since it was last recorded.
  * _cumulative_: representing a cumulative metric, in which the value can only increase over time or be reset to zero
on restart.

### Return values: 
* _data_: <code>timeSeries</code>. The _cumulative_ or _onChange_ time series with the measurements converted to the 
_instantaneous_ type.

### Details:
The function takes as input a time-indexed series with measurements of type _cumulative_ or _onChange_ and returns a 
time series converted to the _instantaneous_ type. The input time series is transformed to an _instantaneous_ metric
based on the _measurementReadingType_. For example, if the _measurementReadingType_ is _cumulative_, the counter 
rollover (reset) must be taken into account in the transformation process.


# :card_file_box: Data Preparation / Outlier Detection


## :round_pushpin: clean_ts_min_max_outliers

### Description:
Detect elements of the time series outside a minimum and maximum range.
 
### Arguments:
* _data_: <code>timeSeries</code> An argument containing the time series from which the outliers need to be detected.
* _min_: <code>float</code> The minimum value allowed for each element of the time series.
* _max_: <code>float</code> The maximum value allowed for each element of the time series.
* _minSeries_: <code>timeSeries</code>. An optional argument defining a time series with minimum allowed values.
* _maxSeries_: <code>timeSeries</code>. An optional argument defining a time series with maximum allowed values.

### Details:
This function detects elements of a time series outside the allowed range in which you know the data should be. In the case of energy consumption, it should be a positive value and not exceeding the total capacity permitted. Sometimes, this value is not easy to define.

Additionally, with the minSeries and maxSeries arguments, this ranges can be set differently along the period. When using this feature, the time step of both _minSeries_ and _maxSeries_ need to be resampled to the original frequency of the _data_ time series, applying forward fill if is needed (Remember the time stamps of each time series element always represent the begining of the time step).

### Value: 
This function returns a logical <code>timeSeries</code> object using the original time period and frequency, only assigning True values when an element should be considered as an outlier.


## :round_pushpin: clean_ts_znorm_outliers

### Description:
Detect elements of the time series out of the z-normalization transformation threshold

### Arguments:
* _data_: <code>timeSeries</code> An argument containing the time series from which the outliers need to be detected.
 znormThreshold, znorm_window

### Details:

### Value: 
This function returns a logical <code>timeSeries</code> object using the original time period and frequency, only assigning True values when an element should be considered as an outlier.


## :round_pushpin: clean_ts_lm_calendar_outliers

### Description:
Detect elements of the time series out of the XX% confidence of a linear model based on calendar variables (month, weekday, hour)

### Arguments:
* _data_: <code>timeSeries</code> An argument containing the time series from which the outliers need to be detected.
 lm_quantileThreshold, holidays calendar

### Details:

### Value: 
This function returns a logical <code>timeSeries</code> object using the original time period and frequency, only assigning True values when an element should be considered as an outlier.


## :round_pushpin: plot_outliers

### Description:
v2

### Arguments:
Time series, outliers logical time series, cleaned time series, showPlot

### Details:
Plot the data cleaning process of a time series to expose the outliers

### Value: 
This function returns a plot (svg, pdf, html)


## :round_pushpin: detect_static_min_max_outliers

### Description:
Detect element outside the min max range (To be discussed _ seems not to be used today)

### Arguments:
Value, min, max

### Details:

### Value: 
This function returns a 


## :round_pushpin: detect_static_allowed_values

### Description:
Detect if the element fullfill the regular expression or it exists in the list of possible values

### Arguments:
Value, regexp, possibleValues

### Details:

### Value: 
This function returns a 

