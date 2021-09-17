[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# Data Preparation / Timestamps alignment

## detect_time_step

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

