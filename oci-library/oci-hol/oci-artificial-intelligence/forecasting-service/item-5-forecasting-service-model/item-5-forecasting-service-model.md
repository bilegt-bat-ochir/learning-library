# Lab 3: Train Model and Forecast

## Introduction

In this session, we will show you how to use create and get forecast APIs. 

*Estimated Time*: 30 minutes

### Objectives

In this lab, you will:
- Learn to use forecast API 
- Learn to get forecasts and predictions intervals for the forecast horizon

### Prerequisites
- A Free tier or paid tenancy account in OCI
- You have completed all the tasks in Lab 2 
- Download the sample python [notebook](files/ForecastingAPI_LiveLabs_SampleNotebook.ipynb) : We will be using for explaining on how to use Forecasting Service APIs

## Task 1: Create a Forecast model

Creating a model requires 3 actions to kick off training the forecasting model.

  * Pass the inline data in the payload of the forecast train settings
  * Set other training parameters as show in below code snippet
  * Create Forecast API Call using the /forecasts url

  We pre-define some the parameters of the payload based on the example input data (the one we uploaded in previous lab session)

    ```Python 
    date_col = 'date'
    target_col = 'sales'
    id_col = 'item_id'
    data_frequency = 'DAY'
    forecast_frequency = 'DAY'
    forecast_horizon  = 14
    forecast_name = "LiveLabs Inline Forecasting Service API "
    ```

In the example below we show how to create the payload for calling create forecast API. 
- "compartmentId": is same as tenancy id(refer Task 5 : Create Project ID in Lab 2)
- "projectId": the one you get after creating a project (refer Task 5 : Create Project ID in Lab 2)
- "targetVariables": name of the column in primary data having the target values
- models: models selected for training. Here we are showing some the models implemented in our service.Our AutoML service selects the best model out of all the models selected for training. 
- "forecastHorizon": number of future timesteps for which to forecast 
- "tsColName": name of the timestamp column  
- "dataFrequency": 'MINUTE','HOUR', 'DAY', 'WEEK', 'MONTH' or 'YEAR'  and custom frequency depending on frequency of input data
- "forecastFrequency": 'HOUR', 'DAY', 'WEEK', 'MONTH' or 'YEAR' and custom frequency depending on forecast frequency required . For custom frequency : If input dataFrequency multiplier is more than 1, then the forecast frequency should be also at the same base frequency as the input. Eg.  If dataFrequency : 2HOURS  , then forecastFrequency: 24HOURS if you want forecastFrequency to be a DAY level
- "isDataGrouped": True if data is grouped or having additional data. False if using only one series with no additional data
- "columnData": inline data (Please refer Task 4: Inline Data preparation in Lab 2)
- "columnSchema": provide column name and data type for each column in the data source
- "additionalDataSource": column schema for additional data to be provided if using additional data.This field should be removed if there is no additional data
- "models" : We can use any of the available algorithms are univariate and multivariate methods. 

   - Univariate :"SMA","DMA","HWSM","HWSA","SES","DES","SA","SM","UAM","UHM","HWSMDAMPED","HWSADAMPED", "PROPHET", "ARIMA"
   
   - Multivariate : "PROBRNN","APOLLONET","EFE"               

    ```Python
    %%time

    url = "https://forecasting.aiservice.us-phoenix-1.oci.oraclecloud.com/20220101/forecasts"

    payload = simplejson.dumps(
    {
    "displayName": forecast_name,
    "compartmentId": compartment_id,
    "projectId": project_id,
    "forecastCreationDetails": {
        "forecastHorizon": forecast_horizon,
        "confidenceInterval": "CI_5_95",
        "errorMeasure": "RMSE",
        "forecastTechnique": "ROCV",
        "forecastFrequency": forecast_frequency,
        "isForecastExplanationRequired": True,
        "modelDetails": {
            "models": [
                    "SMA",
                    "DMA",
                    "HWSM",
                    "HWSA",
                    "SES",
                    "DES",
                    "PROPHET"
            ]
        },
        "dataSourceDetails": {
            "type": "INLINE",
            "primaryDataSource": {
                "columnData": prim_load,
                "isDataGrouped": True,
                "tsColName": date_col,
                "tsColFormat": "yyyy-MM-dd HH:mm:ss",
                "dataFrequency": data_frequency,
                "columnSchema": [
                        {
                            "columnName": id_col,
                            "dataType": "STRING"
                        },
                        {
                            "columnName": date_col,
                            "dataType": "DATE"
                        },
                        {
                            "columnName": target_col,
                            "dataType": "INT"
                        }
                    ]
            },
            "additionalDataSource": {
                "columnData": add_load,
                "isDataGrouped": True,
                "tsColName": date_col,
                "tsColFormat": "yyyy-MM-dd HH:mm:ss",
                "dataFrequency": data_frequency,
                "columnSchema": [
                        {
                            "columnName": id_col,
                            "dataType": "STRING"
                        },
                        {
                            "columnName": "date",
                            "dataType": "DATE"
                        },
                        {
                            "columnName": "onpromotion",
                            "dataType": "INT"
                        }
                    ]
            }
              
        },
        "targetVariables": [
            target_col
        ]
    }
    }
        , ignore_nan=True
    )

    headers = {
      'Content-Type': 'application/json'
    }
          
    response = requests.request("POST", url, headers=headers, data=payload, auth=auth)
    ```

  We store the response and get forecast id 
    ```Python
    create_forecast_response = json.loads(response.text)
    create_forecast_response
    ```
   
## Task 2: Get forecast and Prediction Intervals
- Take the forecast ID from response above and create a *Get forecast API* call using below code snippet
- Once the results are produced, the  'lifecycleState' changes to 'ACTIVE'. If it is 'CREATING', then you need to re-run the code below after sometimes.
- The ```forecastResult``` key in the below ```get_forecast_response``` gives us the forecast for the given horizon  
- We also get Prediction Intervals from ```predictionInterval``` key in the response

    It can take sometime to create the forecast depending on the models selected and the size of the input data.  For the dataset we have taken, it should take around 10-15 minutes to give results. Till then you can take a break or focus on other task.

    ```Python
    create_forecast_id = create_forecast_response['id']
    create_forecast_id
    url = "https://forecasting-------------------------.oci.oraclecloud.com/20220101/forecasts/{}".format(create_forecast_id)

    payload={}
    headers = {}

    response = requests.request("GET", url, headers=headers, data=payload, auth=auth)
    get_forecast_response = json.loads(response.text)
    get_forecast_response
    ```  
    Forecast Response below :
    ```json
        {'description': None,
    'id': 'ocid1.aiforecast.oc1.phx.amaaaaaasxs7gpyaaeoiniiomm6b3wyl4tao5epvjh5qcxcjmuw765l6r35a',
    'responseType': None,
    'compartmentId': 'ocid1.tenancy.oc1..aaaaaaaadany3y6wdh3u3jcodcmm42ehsdno525pzyavtjbpy72eyxcu5f7q',
    'projectId': 'ocid1.aiforecastproject.oc1.phx.amaaaaaasxs7gpya64347fdyocgxvqvxkt5qxr5rr2axxlovvdzwavhks4bq',
    'displayName': 'Stat_Prophet Inline Forecasting Service API BETA V2 ',
    'createdBy': None,
    'timeCreated': '2022-04-27T09:14:15.209Z',
    'timeUpdated': '2022-04-27T09:16:03.221Z',
    'lifecyleDetails': None,
    'lifecycleState': 'ACTIVE',
    'failureMessage': None,
    'forecastCreationDetails': None,
    'forecastResult': {'dataSourceType': 'INLINE',
      'forecastHorizon': 14,
      'forecast': [{'targetColumn': '13_BEVERAGES',
        'dates': ['2017-07-29 00:00:00',
        '2017-07-30 00:00:00',
        '2017-07-31 00:00:00',
        '2017-08-01 00:00:00',
        '2017-08-02 00:00:00',
        '2017-08-03 00:00:00',
        '2017-08-04 00:00:00',
        '2017-08-05 00:00:00',
        '2017-08-06 00:00:00',
        '2017-08-07 00:00:00',
        '2017-08-08 00:00:00',
        '2017-08-09 00:00:00',
        '2017-08-10 00:00:00',
        '2017-08-11 00:00:00'],
        'forecast': [1794.423,
        1458.0826,
        1322.5546,
        1746.7986,
        1664.3289,
        1606.625,
        1436.7811,
        1594.2352,
        1343.2721,
        1264.662,
        1584.5563,
        1464.1411,
        1624.6233,
        1521.1838],
        'predictionInterval': [{'upper': 2310.6255, 'lower': 1274.0431},
        {'upper': 1970.2235, 'lower': 992.85834},
        {'upper': 1826.6882, 'lower': 816.79974},
        {'upper': 2230.6548, 'lower': 1218.3107},
        {'upper': 2158.7866, 'lower': 1162.2372},
        {'upper': 2111.322, 'lower': 1099.0563},
        {'upper': 1940.0271, 'lower': 941.1704},
        {'upper': 2074.8967, 'lower': 1097.0505},
        {'upper': 1815.2491, 'lower': 847.81165},
        {'upper': 1740.884, 'lower': 755.6372},
        {'upper': 2069.1372, 'lower': 1137.5074},
        {'upper': 1964.7842, 'lower': 963.93646},
        {'upper': 2153.4753, 'lower': 1094.3989},
        {'upper': 2050.892, 'lower': 995.7819}]}],
      'metrics': {'trainingMetrics': {'numberOfFeatures': 1,
        'totalDataPoints': 1670,
        'generationTime': '108 seconds'},
      'targetColumns': [{'targetColumn': '13_BEVERAGES',
        'bestModel': 'ProphetModel',
        'errorMeasureValue': 272.46014,
        'errorMeasureName': 'RMSE',
        'numberOfMethodsFitted': 9,
        'seasonality': 7,
        'seasonalityMode': 'ADDITIVE',
        'modelValidationScheme': 'ROCV',
        'preprocessingUsed': {'aggregation': 'NONE',
          'outlierDetected': 14,
          'missingValuesImputed': 0,
          'transformationApplied': 'NONE'}}]}},
    'freeformTags': None,
    'definedTags': None,
    'systemTags': None}
    ```
Using below code, we can save the forecast as tabular data in a csv file with prediction intervals.

```Python
df_forecasts = pd.DataFrame({'forecast_dates':[],'upper':[],'lower':[],'forecast':[], 'series_id':[]})
for i in range(len(get_forecast_response['forecastResult']['forecast'])):

    group = get_forecast_response['forecastResult']['forecast'][i]['targetColumn']
    point_forecast = get_forecast_response['forecastResult']['forecast'][i]['forecast']
    pred_intervals = pd.DataFrame(get_forecast_response['forecastResult']
                              ['forecast'][i]['predictionInterval'],dtype=float)
    out = pred_intervals
    out['forecast'] = point_forecast
    forecast_dates = pd.DataFrame({'forecast_dates':get_forecast_response['forecastResult']['forecast'][i]['dates']})
    forecasts = pd.concat([forecast_dates,out],axis=1)
    forecasts['series_id'] = group
    df_forecasts = df_forecasts.append(forecasts, ignore_index = False)
file_name = 'forecast_demo.csv'
df_forecasts.to_csv(file_name, index = None)
df_forecasts        
```
The forecast.csv will be saved in the same folder as the notebook file.

![](images/lab3_task2.png " ")


## Task 3: Get Training Metrics report from the response

* We can also retrieve training metrics report from the response which shows the best model selected using AutoML based on performance on selected error metric. Eg. RMSE in this example. 
* We also get seasonality, outlier detected and other preprocessing methods applied to the series


    ```Python
    get_forecast_response['forecastResult']['metrics']['targetColumns']
    ```

    ```Json
    [{'targetColumn': '13_BEVERAGES',
      'bestModel': 'ProphetModel',
      'errorMeasureValue': 272.46014,
      'errorMeasureName': 'RMSE',
      'numberOfMethodsFitted': 9,
      'seasonality': 7,
      'seasonalityMode': 'ADDITIVE',
      'modelValidationScheme': 'ROCV',
      'preprocessingUsed': {'aggregation': 'NONE',
      'outlierDetected': 14,
      'missingValuesImputed': 0,
      'transformationApplied': 'NONE'}}]
    ```

**Congratulations on completing this lab!**

You may now proceed to the next lab

## Acknowledgements

* **Authors**
    * Ravijeet Kumar - Senior Data Scientist - Oracle AI Services
    * Anku Pandey - Data Scientist - Oracle AI Services
    * Sirisha Chodisetty - Senior Data Scientist - Oracle AI Services
    * Sharmily Sidhartha - Principal Technical Program Manager - Oracle AI Services
    * Last Updated By/Date: Ravijeet Kumar, 29th-April 2022
