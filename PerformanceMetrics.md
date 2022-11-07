[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Performance Metrics
This module contains additional performance metrics that are not implemented in the standard libraries but are needed to assess the performance
of ML models in the BIGG use cases.

## :round_pushpin: mean_bias_error

### Description:
    
The Mean Bias Error (MBE) captures the bias of the model when predicting. According to the ASHRAE guidelines, a positive value implies a positive bias 
and an underestimation of the output variable, while a negative bias implies an overestimation. For example, if we are predicting the electricity 
consumption, a positive value of the MBE would mean that the model under-predicts the measured consumptions.
However the MBE suffers from cancellation errors, i.e. the sum of positive and negative values could reduce the value of MBE.

### Input arguments:
* _y_true_: <code>timeSeries</code>. Timeseries of the true values of the target variable $y$.
* _y_pred_: <code>timeSeries</code>. Timeseries of the predicted values of the target variable $\hat{y}$.
* _normalised_: <code>boolean</code>. Normalise by the mean if True.
* _number_of_parameters_: <code>integer</code>. Number of adjustable model parameters. It is used to
        compute the degrees of freedom of the estimate.

### Return values: 
* _loss_: <code>float</code>. A non-negative floating point value (the best value is 0.0), or an
        array of floating point values, one for each individual target in case of multiple outputs.
        

### Details:
The MBE should be ideally as close to zero as possible. This would mean that the model is neither underestimating or overestimating the consumptions.
However, 
According to ASHRAE guidelines, the formula with `normalised=False` is:
$$\frac 1n \sum_{i=1}^n (y_i - \hat{y_i})$$
If `normalised=True`, the normalised version of the MBE, called NMBE, can be expressed as a percentage:
$$\frac{1}{\overline{m}} \sum_{i=1}^n \frac{(y_i - \hat{y_i})}{n-p} \cdot 100$$
where $n$ is the number of samples, $p$ is the number of adjustable model parameters, while $\overline{m}$ is the mean of the measured values of the 
target.
On the other side, the IPMVP protocol subtracts the predicted values from the measured ones, thus inverting the considerations about under-predicting 
and over-predicting. 

## :round_pushpin: cv_rmse

### Description:
    
The Coefficient of Variation of the Root Mean Squared Error (CVRMSE) is a normalized version of the RMSE. It measures the variability of the errors 
between measured and simulated values. It does not suffer from cancellation errors because of the square operator but, on the other side,
it punishes larger errors.

### Input arguments:
* _y_true_: <code>timeSeries</code>. Timeseries of the true values of the target variable $y$.
* _y_pred_: <code>timeSeries</code>. Timeseries of the predicted values of the target variable $\hat{y}$.
* _number_of_parameters_: <code>integer</code>. Number of adjustable model parameters. It is used to
        compute the degrees of freedom of the estimate.

### Return values: 
* _loss_: <code>float</code>. A non-negative floating point value (the best value is 0.0), or an
        array of floating point values, one for each individual target in case of multiple outputs.
        

### Details:
The CVRMSE should be ideally as close to zero as possible and can be calculated with the following formula:
$$\frac{1}{\overline{m}}\sqrt{\frac{1}{n-p}\sum_{i=1}^{n}(y_{i} - \hat{y_i})^{2}} \cdot 100$$
where $n$ is the number of samples, $p$ is the number of adjustable model parameters, while $\overline{m}$ is the mean of the measured values of the 
target.
