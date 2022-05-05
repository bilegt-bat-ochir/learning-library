# Lab 4: Get Explainability for Forecast

## Introduction

Forecast will also give explainability for each of the target time series in the dataset. Explainability report includes both global and local level explanations. Explanations provides insights on the features that are influencing the forecast. Global explanation represents the general model behaviour - e.g., which features does the model consider important ? Local explanation tells the impact of each feature at a single time step level. Forecast provides local explanations for all the forecasts that it generates

In this session, we will discuss how to get global and local explanation for the best model chosen by forecast, inorder to understand the features that are influencing the forecast

Here is a case study on using the forecast api to get the global and local explanations

***Estimated Time***: 15 minutes

### Objectives
In this lab, you will:
* Learn how to generate global explanation
* Learn how to generate local explanation for all the time steps in the forecast forizon

### Prerequisites
* You have completed all the task in Lab 3

## Task 1: Get Global Explanation

1. Call the explanation API using below code as shown below:
    ```Python
    url = "https://forecasting----.------.------.---.oraclecloud.com/20220101/forecasts/{}/explanations/".format(create_forecast_id)

    payload={}
    headers = {}
    response = requests.request("GET", url, headers=headers, data=payload, auth=auth)
    get_forecast_explanations = json.loads(response.text)
    get_forecast_explanations
    ```

2. Sample Json ouput

    The JSON format is also straight-forward, it contains a key `globalFeatureImportance` listing all the influencing features and their feature importance scores - raw and normalized scores

    ```Json
    {'dataSourceType': 'INLINE',
    'freeformTags': None,
    'definedTags': None,
    'systemTags': None,
    'explanations': [{'targetColumn': '13_BEVERAGES',
      'bestModel': 'PROPHET',
      'bestHyperParameters': {'changepoint_range': '0.8800000000000001',
        'seasonality_mode': 'additive',
        'changepoint_prior_scale': '0.21544346900318823',
        'seasonality_prior_scale': '2.4051320102374683',
        'holidays_prior_scale': '0.46415888336127775'},
      'hyperparameterSearchMethod': 'OPTUNA',
      'bestModelSelectionMetric': 'RMSE',
      'globalFeatureImportance': {'influencingFeatures': {'onpromotion': {'normalizedScore': 0.6431381,
          'rawScore': 159.37825},
        'trend': {'normalizedScore': 0.041080225, 'rawScore': 10.180231},
        'AgeFeature': {'normalizedScore': 0.055123076, 'rawScore': 13.660238},
        'se@7': {'normalizedScore': 0.26065862, 'rawScore': 64.5947}}},
      'localFeatureImportance': {'forecastHorizon': 14,
        'influencingFeatures': [{'onpromotion': {'normalizedScore': 0.046111915,
          'rawScore': 163.4498},
          'trend': {'normalizedScore': 0.0058014663, 'rawScore': 20.564068},
          'AgeFeature': {'normalizedScore': -0.007784638, 'rawScore': -27.593681},
          'se@7': {'normalizedScore': 0.011428176, 'rawScore': 40.508686}},
        {'onpromotion': {'normalizedScore': -0.026147036, 'rawScore': -92.68163},
          'trend': {'normalizedScore': 0.005916347, 'rawScore': 20.971275},
          'AgeFeature': {'normalizedScore': -0.007938789, 'rawScore': -28.14009},
          'se@7': {'normalizedScore': -0.011160823, 'rawScore': -39.561016}},
        {'onpromotion': {'normalizedScore': -0.044880837, 'rawScore': -159.08607},
          'trend': {'normalizedScore': 0.0060312278, 'rawScore': 21.378485},
          'AgeFeature': {'normalizedScore': -0.008092941, 'rawScore': -28.6865},
          'se@7': {'normalizedScore': -0.030622493, 'rawScore': -108.54549}},
        {'onpromotion': {'normalizedScore': 0.067521974, 'rawScore': 239.34059},
          'trend': {'normalizedScore': 0.0061461083, 'rawScore': 21.785694},
          'AgeFeature': {'normalizedScore': -0.008247092, 'rawScore': -29.23291},
          'se@7': {'normalizedScore': -0.023299698, 'rawScore': -82.58887}},
        {'onpromotion': {'normalizedScore': 0.016673084, 'rawScore': 59.099957},
          'trend': {'normalizedScore': 0.006260989, 'rawScore': 22.192904},
          'AgeFeature': {'normalizedScore': -0.008401243, 'rawScore': -29.77932},
          'se@7': {'normalizedScore': 0.0043223756, 'rawScore': 15.321233}},
        {'onpromotion': {'normalizedScore': -0.018118262, 'rawScore': -64.22258},
          'trend': {'normalizedScore': 0.0063758693, 'rawScore': 22.600113},
          'AgeFeature': {'normalizedScore': -0.008555395, 'rawScore': -30.32573},
          'se@7': {'normalizedScore': 0.022873763, 'rawScore': 81.07909}},
        {'onpromotion': {'normalizedScore': -0.06629089, 'rawScore': -234.97687},
          'trend': {'normalizedScore': 0.00649075, 'rawScore': 23.007322},
          'AgeFeature': {'normalizedScore': -0.008709546, 'rawScore': -30.872139},
          'se@7': {'normalizedScore': 0.023169866, 'rawScore': 82.12866}},
        {'onpromotion': {'normalizedScore': -0.010089491, 'rawScore': -35.763535},
          'trend': {'normalizedScore': 0.0066056303, 'rawScore': 23.414532},
          'AgeFeature': {'normalizedScore': -0.008863697, 'rawScore': -31.418549},
          'se@7': {'normalizedScore': 0.011428176, 'rawScore': 40.508686}},
        {'onpromotion': {'normalizedScore': -0.058262125, 'rawScore': -206.51782},
          'trend': {'normalizedScore': 0.006720511, 'rawScore': 23.821741},
          'AgeFeature': {'normalizedScore': -0.009017848, 'rawScore': -31.964958},
          'se@7': {'normalizedScore': -0.011160823, 'rawScore': -39.561016}},
        {'onpromotion': {'normalizedScore': -0.06093838, 'rawScore': -216.00417},
          'trend': {'normalizedScore': 0.0068353913, 'rawScore': 24.22895},
          'AgeFeature': {'normalizedScore': -0.009172, 'rawScore': -32.511368},
          'se@7': {'normalizedScore': -0.030622493, 'rawScore': -108.54549}},
        {'onpromotion': {'normalizedScore': 0.022025598, 'rawScore': 78.072655},
          'trend': {'normalizedScore': 0.006950272, 'rawScore': 24.63616},
          'AgeFeature': {'normalizedScore': -0.009326151, 'rawScore': -33.057777},
          'se@7': {'normalizedScore': -0.023299698, 'rawScore': -82.58887}},
        {'onpromotion': {'normalizedScore': -0.03952832, 'rawScore': -140.11337},
          'trend': {'normalizedScore': 0.0070651523, 'rawScore': 25.04337},
          'AgeFeature': {'normalizedScore': -0.009480302, 'rawScore': -33.604187},
          'se@7': {'normalizedScore': 0.0043223756, 'rawScore': 15.321233}},
        {'onpromotion': {'normalizedScore': -0.0127657475,
          'rawScore': -45.249886},
          'trend': {'normalizedScore': 0.007180033, 'rawScore': 25.450579},
          'AgeFeature': {'normalizedScore': -0.009634453, 'rawScore': -34.150597},
          'se@7': {'normalizedScore': 0.022873763, 'rawScore': 81.07909}},
        {'onpromotion': {'normalizedScore': -0.04220458, 'rawScore': -149.59973},
          'trend': {'normalizedScore': 0.0072949133, 'rawScore': 25.857788},
          'AgeFeature': {'normalizedScore': -0.009788604, 'rawScore': -34.697006},
          'se@7': {'normalizedScore': 0.023169866, 'rawScore': 82.12866}}]}}]}
    ```
*The above explanation shows features contributing towards the model output/prediction from the base value. The base value is nothing but the average model output computed over most recent 100 time steps in the training data. If the dataset size is less than 100, then the base value is computed over the whole dataset. The features which have positive feature importance scores, push the prediction higher and the features that have negative feature importance scores, push the prediction lower. The feature importance scores are raw scores and those features with high in magnitude influence the prediction most and the sign of the scores indicates whether the influence is positive or negative.*

3. Plotting the global feature importance 

    Here is a simple function to plot the global feature importance from the above json output.

    ```Python
    import plotly.express as px
    import plotly.graph_objects as go
    def plot_global_feature_importance(get_forecast_explanations, time_step):
        df_imps = pd.DataFrame()
        global_feature_importance = get_forecast_explanations['explanations'][0]['globalFeatureImportance']['influencingFeatures']
        df_imps['Feature_Importance'] = local_feature_importance.values()
        df_imps["Feature_Importance"] = df_imps["Feature_Importance"].apply(lambda x:x["normalizedScore"])
        feature_names = local_feature_importance.keys()
        df_imps['Features'] = feature_names

        title = "Global Feature Importance for Timestep " + str(time_step)
        fig = px.bar(df_imps, y="Features", x='Feature_Importance', title=title)
        fig.update_traces(marker_color='lightsalmon')
        fig.show()

    plot_global_feature_importance(get_forecast_explanations, time_step)
    ```

  ### Sample Global feature importance plot

  ![Global Feature Importance ](images/lab4-task1-global-feature-importance.png)

## Task 2: Get Local Explanation

  To get local explanation, there is no seperate api call required. The api call for get explanation will fetch both global and local explanations.
  The key `localFeatureImportance` in the json output contains all the influencing features and their feature importance scores for all the time steps in the forecast horizon

1. Plotting the local feature importance 

    Here is a simple function to plot the local feature importance from the above json output.

    ```Python
    import plotly.express as px
    import plotly.graph_objects as go
    import numpy as np

    def plot_local_feature_importance(get_forecast_explanations, time_step):
        df_imps = pd.DataFrame()
        local_feature_importance = get_forecast_explanations['explanations'][0]['localFeatureImportance']['influencingFeatures'][time_step]
        df_imps['Feature_Importance'] = local_feature_importance.values()
        df_imps["Feature_Importance"] = df_imps["Feature_Importance"].apply(lambda x:x["normalizedScore"])
        feature_names = local_feature_importance.keys()
        df_imps['Features'] = feature_names

        title = "Local Feature Importance for Timestep " + str(time_step)
        fig = px.bar(df_imps, y="Features", x='Feature_Importance', title=title)
        fig.update_traces(marker_color='lightsalmon')
        fig.show()

    time_step = 1 

    plot_local_feature_importance(get_forecast_explanations, time_step)
    ```

    ### Sample Local feature importance plot for step 1 forecast

    ![Local Feature Importance for step 1 forecast](images/lab4-task2-local-feature-importance.png)

    Similarly, by changing the time step, you can get the local feature importance for that corresponding forecast.

**Congratulations on completing this lab!**

You may now proceed to the next lab


## Acknowledgements
* **Authors**
    * Ravijeet Kumar - Senior Data Scientist - Oracle AI Services
    * Anku Pandey - Data Scientist - Oracle AI Services
    * Sirisha Chodisetty - Senior Data Scientist - Oracle AI Services
    * Sharmily Sidhartha - Principal Technical Program Manager - Oracle AI Services
    * Last Updated By/Date: Ravijeet Kumar, 29th-April 2022
