# Build a ML Pipeline for Short term Rental Prices in NYC


The second project for the [ML DevOps Engineer Nanodegree](https://www.udacity.com/course/machine-learning-dev-ops-engineer-nanodegree--nd0821) by Udacity.

## Description

This project, part of Unit 3 on Building a Reproducible Model Workflow, involves developing a full end-to-end machine learning pipeline to predict Airbnb rental prices. The goal is to create a reusable and efficient solution.

## Prerequisites

Python and Jupyter Notebook are required.

## Dependencies
- mlflow


## Installation



```bash
pip install mlflow
```

## Usage

Building a reproducible ML pipeline will require different components which will be needed to be contained in there own environment. The following image shows the pipeline contained within weights and biases. You can check the pipeline at W&B [here](https://wandb.ai/ibrahimsherif/nyc_airbnb/overview?workspace=user-ibrahimsherif)

![Pipeline](/images/pipeline_graph_view.PNG)

The pipeline shows each component with input and output artifacts for each component.
- ```data_get```: Upload the data from local path to W&B
- ```eda```: A notebook which contains EDA for the dataset
- ```data_clean```: Clean the dataset and handle outliers
- ```data_tests```: Performs data validation
- ```data_split```: Splits the dataset to trainval and test
- ```train_random_forest```: Builds and trains a pipeline which includes handling of missing data, some feature engineering, modeling and generates scoring results.
- ```test_model```: Evaluates the saved pipeline on the test data and generates scoring results.

 Build the pipeline.
```bash
cd ./Build-a-ML-Pipeline-for-Short-term-Rental-Prices-in-NYC
mlflow run . 
``` 

Run EDA step which will open a jupyter notebook
```bash
cd ./Build-a-ML-Pipeline-for-Short-term-Rental-Prices-in-NYC/componenets/EDA
mlflow run . 
```

Run evaluation step
```bash
mlflow run . -P hydra_options="main.execute_steps=test_model"
```

Run a specific component
```bash
mlflow run . -P hydra_options="main.execute_steps=train_random_forest"
```

Run a sweep of different hyperparameters to train the model. ```hydra/launcher=joblib``` enables parallel training.
```bash
mlflow run . -P hydra_options="-m hydra/launcher=joblib main.execute_steps=train_random_forest pipeline.model.random_forest.max_features=0.1,0.33,0.5,0.75,1 pipeline.tfidf.max_features=10,15,30"
```

Run the pipeline directly from github using a different sample of data.
```bash
mlflow run https://github.com/ibrahim-sheriff/Build-a-ML-Pipeline-for-Short-term-Rental-Prices-in-NYC \
            -v [RELEASE_VERSION] \
            -P hydra_options="data.sample='sample2.csv'"
```
