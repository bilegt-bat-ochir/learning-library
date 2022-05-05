# Forecasting Service

## Introduction

Forecasting is a common data science problem across several industry domains. Companies try to forecast future key business metrics such as demand, sales, revenue, inventory etc. that aid in effective planning and goal setting. A few forecasting use cases are ATM cash management & credit card offers in finance and banking industry, sales based promotions in retail industry, effective transportation planning & demand forecasting in supply chain and logistic industry.   

OCI forecasting service uses state-of-the-art machine learning & deep learning algorithms to deliver high quality and reliable forecasts with AutoML capability such as automatic data pre-processing, developer-focused AI explainability. 

The Forecasting Service at OCI (Oracle Cloud Infrastructure) will create customized Machine Learning models which are trained on the data uploaded by users. Users will be able submit historical data as inline data and data assets (created with data upload in object storage) via forecasting end-points and get forecast for the desired horizon. In addition to providing forecasts, this service also focuses on explaining predictions from the model by leveraging state-of-the-art model explainability techniques.
 
The *OCI Forecasting Service* is a fully managed, serverless and multi-tenant service and is accessible over public *REST APIs* by authenticated users via Cloud Console (Data Science Notebooks and Console available). OCI CLI and SDK will be added in upcoming releases.

This workshop contains five lab sessions for users to get familiar with data requirements and the full cycle of building the forecasting model and generate forecasts. At the end of the workshop, users will be able to understand how the data should be prepared and leverage the APIs for model training and getting forecast along with explainability. 

*Estimated Workshop Time*: 120 minutes (5 lab sessions)

### Objectives:

* Understand a high level overview of the OCI Forecasting Service
* Understand the full cycle/workflow of services provided in the OCI Forecasting Service
* Learn to use REST API to interact with Forecasting Service
* Learn basic data analysis preprocessing techniques to prepare data for model training
* Hands-on activities to experience the whole pipeline of machine learning model development from training to forecasting

### Prerequisites:
* An Oracle Free Tier, or Paid Cloud Account
* Grant proper permission for user to use the Forecasting Service
* Additional pre-requisites (cloud services) are mentioned per lab
* Familiar with services on Oracle Cloud Infrastructure (OCI), such as Data Science Notebooks, Object Storage
* Familiar with machine learning, data processing, statistics is desirable, but not required
* Familiar with Python/Java programming is strongly recommended (Optional for API integration)
* Familiar with editing tools (vim, nano) or shell environments (cmd, bash, etc) (Optional for API integration)

## Forecasting Service Concepts
* Data Science Notebook Session: Learn how to to set up a [Data Science Notebook Session](https://docs.oracle.com/en-us/iaas/data-science/using/use-notebook-sessions.htm)
* Using OCI Forecasting Service Console 
* Project: Projects are collaborative workspaces for organizing data assets, models, and forecasting portals.
* Data Assets: An abstracted data format to contain primary, additional and meta information of the actual data source for model training.
* Model: The ML model that was trained by forecasting algorithms and can forecast using univariate/multivariate time-series data along with meta data. A few parameters with default values are exposed so that user can choose to select.
* Forecasting: Once a model is trained successfully, it is automatically deployed into the cloud environment. Users can send new data to the API to get forecasting results.

## Forecasting Service Process

At a high level, here are the process of completing a full cycle of using forecasting service.

1. Create a Data Science Notebook Session: Data Science notebook sessions provide us with Jupyter Notebook for interactive coding environment for building and training models.
2. Create a project. A project is used to include and organize different assets, models and private endpoints for data connection in the same workspace.
3. Create a data asset. Data asset is an abstracted data representation for a data source.Currently it supports inline data generated from  csv files uploaded in the notebook session folder. 
4. Train a model. After specifying a data asset and the training parameters, you can train a forecasting  model.It will take 5-10 minutes or longer depending on the data size and number of models to be trained as parameter. Once a model is trained successfully, it is deployed automatically with an endpoint ready to generate forecast.
5. Forecasting - We call the GET Forecast API to get the forecast
6. Using OCI forecasting service using OCI console

Note that one project can have multiple data assets and multiple models.

You may now proceed to the next lab

## Acknowledgements
* **Authors**
    * Ravijeet Kumar - Senior Data Scientist - Oracle AI Services
    * Anku Pandey - Data Scientist - Oracle AI Services
    * Sirisha Chodisetty - Senior Data Scientist - Oracle AI Services
    * Sharmily Sidhartha - Principal Technical Program Manager - Oracle AI Services
    * Last Updated By/Date: Ravijeet Kumar, 29th-April 2022


