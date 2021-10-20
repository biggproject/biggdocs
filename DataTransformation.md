[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Data Transformation / Profiling

## :round_pushpin: clustering_dlc

### Description:
Cluster similar daily load curves based on the load curves itself, calendar variables and outdoor temperature

### Input arguments:
* _consumption_: <code>timeSeries</code> containing the total energy consumption of a building. Minimum time step: hourly.
* _temperature_: <code>timeSeries</code> containing the outdoor temperature of the related building. Minimum time step: hourly.
* _localTimeZone_: <code>string</code> specifying the local time zone related to the building in analysis. The format of this time zones are defined by the IANA Time Zone Database (https://www.iana.org/time-zones).
* _kMax_: <code>integer</code> defining the maximum number of allowed groups in the clustering proceed.
* _inputVars_: <code>list of strings</code>. Select a number of features as an input of the clustering.
  * _loadCurve_: use the transformed version of the consumption daily load curve (based on _loadCurveTransformation_ and _nDayParts_ arguments)
  * _dailyTemperature_: use the average daily outdoor temperature
  * _dailyConsumption_: use the total daily consumption
  * _daysOfTheWeek_: use a discrete value to represent the days of the week
  * _daysWeekend_: use a boolean representing whether is weekend or not
  * _holidays_: use a boolean representing whether is holiday or not
* _loadCurveTransformation_: <code>string</code> that defines the transformation procedure over the consumption load curves. Possible values are:
  * _relative_: All daily load curves are relative to their total daily consumption. It is the default mode.
  * _absolute_: The absolute consumption is used to define each daily load curves.
* _nDayParts_: <code>integer</code> defining the parts of day used to. Possible values: 24, 12, 8, 6, 4, 3, 2.


### Return values:
* _dailyClassification_: <code>timeSeries</code>
* _absoluteLoadCurvesCentroids_: <code>matrix</code>
* _clusteringCentroids_: <code>matrix</code>
* _clusteringModel_: <code>object</code>
* _opts_: <code>dictionary</code>
  * _normSpecs_: 
  * _loadCurveTransformation_: 
  * _inputVars_:
  * _nDayParts_:

### Details:


## :round_pushpin: classification_dlc

### Description:
Classify daily load curves based on the outputs of a clustering and a new set of data

### Input arguments:
* _consumption_: <code>timeSeries</code> containing the total energy consumption of a building. Minimum time step: hourly.
* _temperature_: <code>timeSeries</code> containing the outdoor temperature of the related building. Minimum time step: hourly.
* _localTimeZone_: <code>string</code> specifying the local time zone related to the building in analysis. The format of this time zones are defined by the IANA Time Zone Database (https://www.iana.org/time-zones).
* _clustering_: <code>clustering_dlc() output</code>

### Return values:
* _dailyClassification_: <code>timeSeries</code>

### Details:


## :round_pushpin: weekly_profile_detection

### Description:
    
The function returns the weekly profile of the input time series.

### Input arguments:
* _data_: <code>timeSeries</code>. Input time series whose weekly profile has to be detected.
* _holidays_: <code>timeSeries</code>. Time series of holidays in the date range of the input time series. 

### Return values: 
* _data_: <code>timeSeries</code>. Time series representing the weekly profile, e.g. weekly electricity consumption
pattern of the input time series at hourly frequency. 

### Details:
The function takes as input a time-indexed series of a measurement of _instantaneous_ type and derives the weekly
pattern from the data. Holidays are provided as an input time series to the function and are excluded from the 
calculation, together with outliers. To be consistent with the input data, the resulting time series 
will be temporarily aligned with the last week of the input series. 
The function uses _data_preparation_._detect_time_step_ to infer the frequency of the input data and convert it to 
hourly data with _data_preparation_._align_time_grid_.
The minimum frequency allowed to compute the weekly profile is hourly ('H').

## :round_pushpin: yearly_profile_detection

### Description:
    
The function returns the yearly profile of the input time series.

### Input arguments:
* _data_: <code>timeSeries</code>. Input time series whose yearly profile has to be detected.
* _holidays_: <code>timeSeries</code>. Time series of holidays in the date range of the input time series. 

### Return values: 
* _data_: <code>timeSeries</code>. Time series representing the yearly profile, e.g. yearly electricity consumption
pattern of the input time series at daily frequency.

### Details:
The function takes as input a time-indexed series of a measurement of _instantaneous_ type and derives the yearly
pattern from the data. Holidays are provided as an input time series to the function and are excluded from the 
calculation, together with outliers. To be consistent with the input data, the resulting time series 
will be temporarily aligned with the last year of the input series. 
The function uses _data_preparation_._detect_time_step_ to infer the frequency of the input data and convert it to 
daily data with _data_preparation_._align_time_grid_.
The lowest frequency allowed to be able to compute the weekly profile is daily ('D').

## :round_pushpin: trend_estimation

### Description:

v2

### Input arguments:

### Return values:

### Details:


# :card_file_box: Data Transformation / Low Pass Filter

## :round_pushpin: lpf_ts

### Description:
First-order low pass filter (smoothing)
Use cases:
- with consumption data, it helps removing artificial fluctuaction
- with outdoor temperature data, it helps to linearise the relation between consumption and outdoor temperature, as it simplifies the modelling of the thermal inertia of the building

### Input arguments:
* _data_: 
* _smoothingTimeScaleParameter_: 

### Return values:

### Details:


## :round_pushpin: get_lpf_smoothing_time_scale

### Description:
Physical transformation of the smoothing time scale parameter
Use cases:
- for outdoor temperature, timeConstantInHours means the thermal inertial of the envelope of the building in hours

### Input arguments:
timestepsPerHour, timeConstantInHours

### Return values:

### Details:


# :card_file_box: Data Transformation / Calendar

## :round_pushpin: calendar_components

### Description:
Decompose the time in date, day of the year, day of the week, day of the weekend, working day, non-working day, season, month, hour, minute, ...

### Input arguments:
time array

### Return values:

### Details:


# :card_file_box: Data Transformation / Fourier Series

## :round_pushpin: fs_components

### Description:
Decompose a cyclic time series (e.g. solar azimuth, solar elevation, calendar features, ...) into the components of sin(x), cos(x) depending the number of harmonics provided

### Input arguments:
Time series, minCycle, maxCycle, nHarmonics

### Return values:

### Details:



