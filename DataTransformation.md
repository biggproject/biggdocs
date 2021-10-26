[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Data Transformation / Profiling

## :round_pushpin: clustering_dlc

### Description:
Cluster similar daily load curves based on the load curves itself, calendar variables and outdoor temperature

### Input arguments:
* _consumption_: <code>timeSeries</code> containing the total energy consumption of a building.
* _temperature_: <code>timeSeries</code> containing the outdoor temperature of the related building.
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
* _dailyClassification_: <code>timeSeries</code> in daily frequency, containing the classification of each daily load curve.
* _absoluteLoadCurvesCentroids_: <code>matrix</code> with row names as the identifier of the cluster, and column names as the day hours. This matrix is filled with the hourly average consumption of each cluster detected.
* _clusteringCentroids_: <code>matrix</code> with row names as the identifier of the cluster, and column names as the input variables used by the clustering algorithm. This matrix is filled with the average values of each input variable and cluster.
* _classificationModel_: <code>object</code> containing a simple classification model to predict a load curve based on calendar features. 
* _opts_: <code>dictionary</code>
  * _normSpecs_: <code>matrix</code> of aggregations used in the Z-score normalisation of the clustering inputs. The different types of aggregations (mean, median, std, ...) are specified as row names, and the different input features are related with column names.
  * _loadCurveTransformation_: <code>object</code>
  * _inputVars_: <code>object</code>
  * _nDayParts_: <code>object</code>

### Details:
Spectral clustering is used as the technique to infer the unknown daily load curves patterns. 
In addition, a classification model to predict a load curve based on calendar features.
The minimum frequency allowed of the arguments _consumption_ and _temperature_ to cluster daily load curves is hourly ('H').

## :round_pushpin: classification_dlc

### Description:
Classify daily load curves based on the outputs of a clustering and a new set of data

### Input arguments:
* _consumption_: <code>timeSeries</code> containing the total energy consumption of a building.
* _temperature_: <code>timeSeries</code> containing the outdoor temperature of the related building.
* _localTimeZone_: <code>string</code> specifying the local time zone related to the building in analysis. The format of this time zones are defined by the IANA Time Zone Database (https://www.iana.org/time-zones).
* _clustering_: <code>clustering_dlc() output</code>
* _methodPriority_: <code>list of strings</code> Possible values:
  * _absoluteLoadCurvesCentroids_: Based on the absolute consumption load curve
  * _clusteringCentroids_: Based on the inputs considered in the clustering procedure. Applying the same transformations done during that process.
  * _classificationModel_: Based on a classification model of the calendar features.

### Return values:
* _dailyClassification_: <code>timeSeries</code>

### Details:
The minimum frequency allowed of the arguments _consumption_ and _temperature_ to cluster daily load curves is hourly ('H').

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
This function computes the first-order low pass filter for smoothing a time series 

### Input arguments:
* _data_: <code>timeSeries</code> to filter
* _smoothingTimeScaleParameter_: <code>float</code> of the smoothing time scale parameter. It corresponds to the so-called alpha parameter of low pass filter. It ranges from 0 to 1.

### Return values:

### Details:
This function can be used in different cases:
* Consumption time series, it helps removing artificial fluctuaction
* Outdoor temperature time series, it helps to linearise the relation between consumption and outdoor temperature, as it simplifies the modelling of the thermal inertia of the building
* Wind speed time series, it helps to smooth the wind speed data
* Solar radiation time series, it helps to linearise the relation between consumption and solar radiation, as it simplifies the modelling of the solar gains of the building

## :round_pushpin: get_lpf_smoothing_time_scale

### Description:
Physical transformation of the smoothing time scale parameter to consider no affectance to the output variable after a known number of hours

### Input arguments:
* _data_: <code>timeSeries</code> to filter
* _timeConstantInHours_: <code>float</code>. It means the number of hours that are affected by a certain change in the input time series (_data_ argument). For instance, when outdoor temperature is considered, this time constant in hours correspond to the thermal inertia of the building.

### Return values:
* _smoothingTimeScaleParameter_: <code>float</code> of the smoothing time scale parameter. It corresponds to the so-called alpha parameter of low pass filter. It ranges from 0 to 1.

### Details:
The time step of the _data_ time series should be detected using the detect_time_step() function and then, using this information and the time constant in hours, the smoothingTimeScaleParameter to be used in the lpf_ts() function is obtained. This scope of this function is to translate this smoothing factor to a more understandable physics-based coefficient.


# :card_file_box: Data Transformation / Calendar

## :round_pushpin: calendar_components

### Description:
Decompose the time in date, day of the year, day of the week, day of the weekend, working day, non-working day, season, month, hour, minute, ...

### Input arguments:
* _data_: <code>list of dates/datetimes</code> containing the time stamps to be decomposed. The datetimes time zone must be UTC, and in the case of dates, they must be related to local time. 
* _localTimeZone_: <code>string</code> specifying the local time zone related to the building in analysis. The format of this time zones are defined by the IANA Time Zone Database (https://www.iana.org/time-zones). This argument is optional, by default no transformation to local time zone is done.

### Return values:
* _features_: <code>matrix</code> containing the different calendar components (columns) related to each timestamp (rows).

### Details:
It is important that the transformation is done considering the local time zone. Normally, the features generated by this function will be used as model inputs that tend to modelise the user behaviour and certain output variable (e.g. electricity consumption). Therefore, the user occupancy and behaviour is related with their activities in local time.


# :card_file_box: Data Transformation / Fourier Series

## :round_pushpin: fs_components

### Description:
Obtain the components of the Fourier Series, in sine-cosine form. It is useful for linearising the relationship of a seasonal input time series (e.g. solar azimuth, solar elevation, calendar features, ...) to some output (energy consumption, indoor temperatures, ...). It basically decomposes a cyclic time series into a set of sine-cosine components that are used as inputs for the modelling of some output, each of the components linearly depends to the output.

### Input arguments:
* _data_: <code>timeSeries</code> to be transformed. It has to contain a cyclic behaviour (e.g. hour of the day, day of the week, solar azimuth, day of the year, ...). 
* _mask_: <code>timeSeries</code> containing the elements of the _data_ time series that should be accounted for the transformation. The timestamps set to false will consider 0's for all their related sine-cosine components. By default, all elements of the time series are considered.
* _nHarmonics_: <code>integer</code> that defines the number of harmonics considered in the Fourier Series. A high number allows to model more precisely the relation, but it considerably increase the cost of computation. The number of harmonics is related with the number of features in the output matrix ((2*_nHarmonics)+1)

### Return values:
* _features_: <code>matrix</code> containing the sine-cosine components of the Fourier Series.

### Details:
For more information about Fourier Series, see https://en.wikipedia.org/wiki/Fourier_series



