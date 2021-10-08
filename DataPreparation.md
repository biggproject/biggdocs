[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Data Preparation / Time Stamps Alignment

## :round_pushpin: detect_time_step

### Description:
    
Detect minimum time step that can be used with the time series.

Treat calendar features separately (e.g. holidays are daily but must still allow the timeStep to be lower than days)

### Arguments:
* _data_: <code>timeSeries</code>. An argument containing the time series from which the time step has to be detected.
* _measurementReadingType_: <code>enumeration</code>. An optional argument to define the measurement reading type. Possible values:
"onChange"(default) to consider on change time series, "cumulative" to consider cumulative time series, "instantaneous" to consider instantaneous time series.

### Details:
Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 

Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi.

### Value: 
This function returns a string in ISO format with the time step (e.g. "15M","H", "5M", "1m",...)


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

