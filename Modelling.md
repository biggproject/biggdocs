[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# :card_file_box: Data Modelling / Cross Validation

For data partitioning we will use directly most of the splitter classes and methods already provided by the library
scikit-learn, like:
* [KFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html#sklearn.model_selection.KFold)
* [TimeSeriesSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html#sklearn.model_selection.TimeSeriesSplit)
* [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html#sklearn.model_selection.train_test_split)

For all the splitter classes that can be used in the cross-validation framework, please refer to:
[scikit-learn splitter-classes](https://scikit-learn.org/stable/modules/classes.html#splitter-classes)

# :card_file_box: Data Modelling / Model Assessment

For model assessment we will use most of the methods already provided by the library scikit-learn, like:

* [cross_validate](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html?highlight=cross%20validate#sklearn-model-selection-cross-validate)
: to evaluate an estimator (classifier or regressor) instance by cross-validation using multiple metrics.
* [cross_val_score](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html#sklearn.model_selection.cross_val_score)
: to evaluate an estimator (classifier or regressor) instance by cross-validation using a single metric.

The metrics to use are related to the specific estimator that has been chosen. Please refer to this list:
[sklearn-metrics](https://scikit-learn.org/stable/modules/classes.html?highlight=metrics#sklearn-metrics-metrics)


## :round_pushpin: evaluate_model_cv_with_tuning

### Description:
    
This function performs a nested cross-validation (double cross-validation), which includes an internal hyper-parameter 
tuning, to reduce the bias when combining the two tasks of model selection and generalization error estimation. However,
the purpose of this function is not to select the best model instance of a model family but instead to provide a less 
biased estimate of a tuned model’s performance on the dataset.

### Input arguments:
* _model_family_: <code>object</code>. object identifying a model family (a ML algorithm, e.g. 'SVC',
'DecisionTreeClassifier', etc.)
* _X_data_: <code>timeSeries</code>. X time series. Training vector of shape (_n_samples_, _n_features_), 
where _n_samples_ is the number of samples and _n_features_ is the number of features.
* _y_data_: <code>timeSeries</code>. Y time series. Target relative to X for classification or regression; 
None for unsupervised learning.
* _parameter_grid_: <code>dict</code>. Dictionary containing the set of parameters of the model family to explore.
* _scoring_: <code>string</code>. A string representing the scoring function to use.
* _cv_splitter_outer_: <code>Generator</code>. This parameter is a generator coming from a partitioning function of the 
library which yields couples of _k_ training sets and test sets indices, each couple representing one split. This 
splitter is related to the outer loop of cross-validation and generally has a _k_ lower than or equal to the inner.
The default value is 10.
* _cv_splitter_inner_: <code>Generator</code>. This parameter is a generator coming from a partitioning function of the 
library which yields couples of _k_ training sets and test sets indices, each couple representing one split. This 
splitter is related to the inner loop of cross-validation for the hyper-parameter tuning. The default value is 5.

### Return values:
* _mean_score_: <code>float</code>. Mean cross-validated score for all the model instances.
* _mean_std_: <code>float</code>. Standard deviation from the mean score.
* _cv_results_: <code>list</code> of <code>dict</code>. A list of dictionaries where each element represents the results
obtained on a specific model instance in terms of performance evaluation and selected hyper-parameters. Can be imported
into a DataFrame.

### Details:

This function will evaluate multiple model instances, i.e. several models of the same family instantiated with a 
specific set of hyper-parameters. However, the purpose of this procedure is not the selection of the best model (the 
function _GridSearchCV_ is in charge of this task) among all the instances but is instead to provide a 
less-biased evaluation of a subset of instances of a model family. The score obtained from the function 
_GridSearchCV_ is not a reasonable estimate of our testing error, when evaluating the performance of the 
best model, and it is shown to be too optimistic in practice. Indeed, in that case, we use the final scores to pick-up
the best model. It means that we used knowledge from the test set (i.e. test score) to decide our model’s 
training parameter and that score is not representative of the generalization performance of the model. 
While, in this case, the hyper-parameter tuning is less likely to overfit the dataset as 
it is only carried out on a subset of the dataset provided by the outer cross-validation procedure. 
This function will run the hyper-parameter tuning for each training set of the outer cross-validation split and 
evaluate the best selected model instance at that iteration using the test set of that split. 
If _k_ is the number of folds or iterations of the outer cross-validation loop, then this function will produce _k_
"surrogate models" each one with a specific hyper-parameter configuration and accuracy score. The statistical assumption
is that the _k_ outer surrogate models will be equivalent to the final model built by the _tune_hyper_parameter_.
The results of each evaluation carried out on the outer cross validation splits are then averaged into one single value
and the surrogate models are discarded.

Example:

<img src="figures/nested_cv.png" alt="nested_cv" width="500"> 


# :card_file_box: Data Modelling / Model Identification

For the hyper-parameter tuning phase, we will use directly the function 
[GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html#sklearn.model_selection.GridSearchCV).

For the models, we will use directly the ML algorithms defined in scikit-learn or in statsmodels or in another packages 
publicly available. For scikit-learn, please refer to:
* [Supervised learning models](https://scikit-learn.org/stable/supervised_learning.html)
* [Unsupervised learning models](https://scikit-learn.org/stable/unsupervised_learning.html)

## :round_pushpin: identify_best_model

### Description:
    
This function implement a complete generalized pipeline to find the best model among different model families, each one 
associated with a specific parameter grid, given a input time series and a scoring function. 

### Input arguments:
* _X_data_: <code>timeSeries</code>. X time series. Training vector of shape (_n_samples_, _n_features_), 
where _n_samples_ is the number of samples and _n_features_ is the number of features.
* _y_data_: <code>timeSeries</code>. Y time series. Target relative to X for classification or regression; 
None for unsupervised learning.
* _model_families_parameter_grid_: <code>dict</code>. Dictionary of key:values pairs, where the key is an object
identifying the _model_family_ (e.g. 'SVC', 'DecisionTreeClassifier', etc.) and the value is a dictionary identifying 
the parameter grid (subset of parameters to test) for that specific model family.
* _scoring_: <code>string</code>. A string representing the scoring function to use.
* _cv_splitter_outer_: <code>Generator</code>. This parameter is a generator coming from a partitioning function of the 
library which yields couples of _k_ training sets and test sets indices, each couple representing one split. This 
splitter is related to the outer loop of cross-validation and generally has a _k_ lower than or equal to the inner. d
* _cv_splitter_inner_: <code>Generator</code>. This parameter is a generator coming from a partitioning function of the 
library which yields couples of _k_ training sets and test sets indices, each couple representing one split. This 
splitter is related to the inner loop of cross-validation for the hyper-parameter tuning and for the final model tuning 
required by the model selection procedure.

### Return values:
* _best_model_instance_: <code>object</code>. Best model instance of the model families found by the exhaustive search 
and retrained on the whole dataset.
* _best_params_: <code>dict</code>. Dictionary with a key:value pair, where the key identifies the best model family and 
the value the best configuration, i.e. the best set of hyper-parameters.
* _mean_score_: <code>float</code>. Mean score for all the model instances produced by the nested cross validation 
procedure.
* _mean_std_: <code>float</code>. Standard deviation from the mean score.
* _cv_results_final_: <code>dict</code>. A dict with keys as column headers and values as columns representing the 
test score for each split, each parameter combination, the rank of each set of parameters and the mean test score and 
standard deviation. Can be imported into a DataFrame.
* _cv_results_evaluation_: <code>list</code> of <code>dict</code>. 
A list of dictionaries related to the results of the nested cross-validation procedure. Each element represents the 
results obtained on a specific model instance in terms of performance evaluation and selected hyper-parameters.
Can be imported into a DataFrame.

### Details:

This function implements a full generalized pipeline to select the best model among several model instances of different
model families. First, It will select the best model family for the given dataset (the family giving the best score)
using the same nested cross-validation procedure (_data_modelling_._evaluate_model_cv_with_tuning_), 
then it will run the _GridSearchCV_ function on the best model family with 
the related _parameter_grid_.

# :card_file_box: Data Modelling / Model Persistence and prediction

## :round_pushpin: serialize_model

### Description:
    
This function serializes and saves a model instance, with a given file format, 
to a specific path on the file system.

### Input arguments:
* _model_instance_: <code>object</code>.Model instance which has been already fitted on X data.
* _model_full_path_: <code>string</code>. Full path (not relative and with no file extensions) of the file
where the model should be saved. The extension will be added by the function based on the format argument.
* _format_: <code>string</code>. Format of the model to serialize and persist (tbd by the programming language or 
framework adopted, e.g. mlflow).

### Return values:
* _file_name_: <code>string</code>.String identifying the filename where the model will be stored.

### Details:

This procedure will save a fitted model instance on the file system, so that it can be persisted and then loaded
for future prediction on new _X_data_ using the complementary function _deserialize_and_predict_. The file format for
the serialization and the naming convention to use are specific of the programming language or framework adopted.

## :round_pushpin: deserialize_and_predict

### Description:
    
This function deserializes a model, inferring the file format from the file name, applies the model on the 
_X_data_ and returns the predicted values in the form of a time series.

### Input arguments:
* _model_full_path_: <code>string</code>. String identifying the full path (not relative and with the extension),
on the file system of the file from which the model should be loaded.
* _X_data_: <code>timeSeries</code>. X time series. Vector of predictors of shape (_n_samples_, _n_features_), 
where _n_samples_ is the number of samples and _n_features_ is the number of features or predictors.

### Return values:
* _y_data_: <code>timeSeries</code>. Y time series. Predicted target values.

### Details:

This function will load a model stored on the file system and specified with a full path (not relative) and predict, 
based on new _X_data_, the target values. The function will infer the format of the model from its file name and
select automatically the correct loader (see the function _serialize_model_) to deserialize it. The predicted values 
are provided as an output in the form of time series.

## :round_pushpin: test_stationarity_acf_pacf

### Description:
    
This function tests the stationarity and plot the autocorrelation and partial autocorrelation of the time series. 

### Input arguments:
* _data: <code>timeSeries</code> for which the stationarity as to be evaluated.
* _sample: <code>float</code>. Sample of the data that will be evaluated.
* _maxLag: <code>int</code>. Maximum lag which is included in test, default value of 12*(nobs/100)^{1/4} is used when None.

### Return values: 
* <code>plot</code> of the mean and variance of the sample with the p-value.
* <code>plot</code> of the autocorrelation and partial autocorrelation. 


### Details:

This function will test the stationarity by running Augmented Dickey-Fuller test wiht 95%
In statistics, the Dickey–Fuller test tests the null hypothesis that a unit root is present in an autoregressive model. 
    The alternative hypothesis is different depending on which version of the test is used but is usually stationarity or trend-stationarity. 
    - plotting mean and variance of a sample from data
    - plotting autocorrelation and partial autocorrelation
* p-value > 0.05: Fail to reject the null hypothesis (H0), the data has a unit root and is non-stationary.
* p-value <= 0.05: Reject the null hypothesis (H0), the data does not have a unit root and is stationary.

This function is used to verify stationarity so that suitable forecasting methods can be applied. 

Partial autocorrelation is a summary of the relationship between an observation in a time series with observations at prior time steps 
with the relationships of intervening observations removed.
The partial autocorrelation at lag k is the correlation that results after removing the effect of any correlations due to the terms at shorter lags.

The autocorrelation for an observation and an observation at a prior time step is comprised of both the direct correlation and indirect correlations. 
These indirect correlations are a linear function of the correlation of the observation, with observations at intervening time steps.

These correlations are used to define the parameters of the forecasting methods (lag). 


## :round_pushpin: split_train_test

### Description:
    
This function splits the time series into train and test datasets at any given data point. 

### Input arguments:
* _data_: <code>timeSeries</code> we want to split. 
* _test_: <code>float</code> (ex: 0.2) or <code>str</code>: index position (ex."yyyy-mm-dd", 1000). Test size 
* _plot_: <code>bool</code> to decide if the 2 new time series have to be plotted

### Return values: 
* _ts_train_: <code>timeSeries</code>. Train time series
* _ts_test_: <code>timeSeries</code>. Test time series 
* _Optional_<code>plot</code> of the train and test time series 

### Details:

This function takes a dataset and divides it into two subsets: the first one will be used to fit the model and the second one will be the used to evaluate the fit. 

## :round_pushpin: param_tuning_sarimax

### Description:
    
This function performs an exhaustive search on all the parameters of the parameter grid defined and identifies the best parameter set for a sarimax model, given a scoring function.

### Input arguments:
* _data_: <code>timeSeries</code> used to fit the sarimax estimator. This may either be a Pandas Series object or a numpy array. 
* _m_: <code>int</code>. The period for seasonal differencing, m refers to the number of periods in each season. For example, m is 4 for quarterly data, 12 for monthly data, or 1 for annual (non-seasonal) data. Default is 1. Note that if m == 1 (i.e., is non-seasonal), seasonal will be set to False.
* _information_criterion_: <code>str</code>. The information criterion used to select the best model. Possibilities are ‘aic’, ‘bic’, ‘hqic’, ‘oob’. Default is 'aic'. 
* _Optional_ _max_order_: <code>int</code>. Maximum value of p+q+P+Q. If the sum of p and q is >= max_order, a model will not be fit with those parameters, but will progress to the next combination. Default is 5.

### Return values: 
* _best_model_: <code>Object</code>. Best parameters for the sarimax model found by the exhaustive search. 

### Details:

This function automatically discovers the optimal order for an sarimax model. It seeks to identify the most optimal parameters. 
In order to find the best model, it optimizes for a given information_criterion, one of (‘aic’, ‘aicc’, ‘bic’, ‘hqic’, ‘oob’) and returns the sarimax model which minimizes the value.


## :round_pushpin: param_tuning_prophet

### Description:
    
This function performs a search on all the parameters of the parameter grid defined and identifies the best parameter set for a prophet model, given a MAPE scoring.

### Input arguments:
* _dtf_train_: <code>timeSeries</code>. Import in a DataFrame containing the train set and with the column containing the timestamp named "ds". Used to fit the prophet estimator.
* _p_: <code>int</code>. The number of periods you want to be forecasted. 
* _Optional_seasonality_mode_: <code>list of strings</code> containing the set of parameters to explore 'multiplicative' and/or 'additive'.
* _Optional_changepoint_prior_scale_: <code>list of floats</code> controling the flexibility of the changepoints.
* _Optional_holidays_prior_scale_: <code>list of floats</code> controling the flexibility of the holidays. 
* _Optional_n_changepoints_: <code>list of int</code> containing the maximum number of trend changepoints allowed when modeling the trend.


### Return values: 
* _optimals_: <code>Object</code>. Best parameters for the prophet model found by the exhaustive search. 

### Details:

This function seeks to identify the most optimal parameters. In order to find the best model, it optimizes for MAPE criterion and returns the prophet model which minimizes the value.

## :round_pushpin: fit_sarimax

### Description:
    
This function trains and fits a SARIMAX model 

### Input arguments:
* _ts_train_: <code>timeSeries</code> used to train the model. 
* _order_: <code>tuple</code>. ARIMA(p,d,q) --> p: lag order (AR), d: 
                  degree of differencing (to remove trend), q: order 
                  of moving average (MA). 
* _seasonal_order_: <code>tuple</code>. (P,D,Q,s) --> s: number of 
                  observations per seasonal (ex. 7 for weekly 
                  seasonality with daily data, 12 for yearly 
                  seasonality with monthly data).              
* _exog_train_: <code>timeSeries</code> containing the exogeneous variables.  

### Return values: 
* _model_: <code>Object</code> holding the model. 
* _dtf_train_:  <code>timeSeries</code>. Imported in a DataFrame, contains the fitted values.  

### Details:

A seasonal autoregressive integrated moving average (SARIMA) model is one step different from an ARIMA model based on the concept of seasonal trends:

The AR from ARIMA stands for autoregressive and refers to using lagged values of our target variable to make our prediction. For example, we might use today’s, yesterday’s, and the day before yesterday’s sales numbers to forecast tomorrow’s sales. That would be an AR(3) model as it uses 3 lagged values to make its prediction.

The I stands for integrated. It means that instead of taking the raw target values, we are differencing them. For example, our sales prediction model would try to forecast tomorrow’s change in sales (i.e. tomorrow’s sales minus today’s sales) rather than just tomorrow’s sales. The reason we need this is that many time series exhibit a trend, making the raw values non-stationary. Taking the difference makes our Y variable more stationary.

The MA stands for moving average. A moving average model takes the lagged prediction errors as inputs. It’s not a directly observable parameter unlike the others (and it’s not fixed as it changes along with the model’s other parameters). At a high level, feeding the model’s errors back to itself serves to push it somewhat towards the correct value (the actual Y values).

The S in SARIMA stands for seasonality: consistently cyclical and easily predictable, which means that we should look past the cyclicality (in other words adjust for it).

SARIMAX extends on this framework just by adding the capability to handle exogenous variables.
              
              
## :round_pushpin: test_sarimax

### Description:
    
This function gets the prediction of the sarimax model. 

### Input arguments:
* _ts_train_: <code>timeSeries</code> used to train the model. 
* _ts_test_: <code>timeSeries</code> used to test the model.  
* _exog_test_: <code>timeSeries</code> containing the exogeneous variables. 
* _p_: <code>int</code> the number of periods to be forecasted. 
* _model_: <code>Object</code> holding model from the fit_sarimax function. 
 
### Return values: 
* _dtf_test_: <code>timeSeries</code> containing the true values and the forecasted ones. 

### Details:
This function will make the prediction using the model previously created. 
              

## :round_pushpin: fit_prophet

### Description:
    
This function trains and fits a PROPHET model 

### Input arguments:
* _ts_train_: <code>timeSeries</code>. Imported into a DataFrame with columns 'ds' (dates), 
             'y' (values), 'cap' (capacity if growth="logistic") [requirement of the Prophet model], 
             other additional regressor. 
* _lst_exog_: <code>list</code> of exogeneous variables. 
* _freq_: <code>str</code>. Frequency can be "D" for daily, "M" for monthly, "Y" annual, ...
### Return values: 
* _model_: <code>Object</code> holding the model. 

### Details:
Prophet makes use of a decomposable time series model with three main model components: trend, seasonality, and holidays.

They are combined in the following equation:
y(t) = g(t) + s(t) + h(t) + e(t)

g(t): trend models non-periodic changes; linear or logistic.
s(t): seasonality represents periodic changes; i.e. weekly, monthly, yearly.
h(t): ties in effects of holidays; on potentially irregular schedules ≥ 1 day(s).
The error term e(t) represents any idiosyncratic changes which are not accommodated by the model; later we will make the parametric assumption that e(t) is normally distributed.

Similar to a generalized additive model, with time as a regressor, Prophet fits several linear and non-linear functions of time as components.
Prophet is framing the forecasting problem as a curve-fitting exercise rather than looking explicitly at the time based dependence of each observation.
              
## :round_pushpin: test_prophet

### Description:
    
This function gets the prediction of the prophet model. 

### Input arguments:
* _model_: <code>Object</code> holding the model trained in the fit_prophet function. 
* _period_: <code>int</code>. Number of periods to be predicted.
* _freq_: <code>str</code>. Frequency can be "D" for daily, "M" for monthly, "Y" for annual, ... 
* _dtf_test: <code>timeSeries</code> imported in a DataFrame 

### Return values: 
* _dtf_forecast_: <code>timeSeries</code> imported as a DataFrame, containing the real and forecasted values. 

### Details:
This function will make the prediction using the model previously created. 


## :round_pushpin: evaluate_forecast 

### Description:
    
This function calculates evaluation metrics for the prediction. 

### Input arguments:
* _dtf_: <code>timeSeries</code>. Imported into a DataFrame with columns raw values, fitted training values, predicted test values.  
* _title_: <code>str</code>. Title of the plot. 
* _plot_: <code>bool</code> to visualize on a plot the result. Default is True. 

### Return values: 
* _dtf_eval_: <code>timeSeries</code>. Imported into a DataFrame with raw ts and forecast.
* _plot_ of the real value and the forecast. With it, the value of the metrics. 

### Details:
This function will calculate several metrics (mae, mse, rmse for example) to be able to compare the results of the forecast models. 

## :round_pushpin: schedule_optimizer

### Description:
    
This function gives alternative schedules for households to answer to the flexibility request. 

### Input arguments:
* _X_forecast_: <code>timeSeries</code>. Forecast of X household energy consumption. 
* _flex_request_: <code>tuple</code>. Time window and amount of energy asked in the flexibility request. 
* _X_user_pref_: <code>dict</code> containing the X user preferences.
* _dynamic_tariff_: <code>timeSeries</code>. Price of energy per hour. 
* _dynamic_renew_energy_: <code>timeSeries</code>. Quantity of available renewable energy per hour. 

### Return values: 
* _Y_schedule_: <code>dict</code> A dict containing the alternative schedule for each Y households. 

### Details:
This function will analyze which households have to adapt their consumption to answer to the flexibility request based on the forecast and taking into account their preferences. Alternative schedules are created for these identified households considering the dynamic tariff and the amount of renewable energy available.



