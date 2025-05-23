# Description of the input data:

The primary data for this project, accessed from the DBRepo (train, validation, and test subsets), is a sensor dataset intended for machine failure prediction. Each instance in the dataset represents a machine, and the data includes a variety of sensor readings and a binary target variable ('fail', where 1 indicates a failure and 0 indicates no failure). These datasets are loaded into Pandas DataFrames for analysis, with the following key columns:

•	footfall: The count of passing entities near the machine.

•	tempMode: The machine's temperature mode.

•	AQ: Air quality index in the vicinity.

•	USS: Proximity measurements from an ultrasonic sensor.

•	CS: Electrical current usage of the machine.

•	VOC: Level of volatile organic compounds detected.

•	RP: Rotational speed of machine parts (RPM).

•	IP: Pressure input to the machine.

•	Temperature: The machine's operating temperature.

•	fail: The binary target variable indicating machine failure.


# Machine Learning Model Training and Evaluation Pipeline

[![DOI](https://zenodo.org/badge/971428215.svg)](https://doi.org/10.5281/zenodo.15270002)


This Python script performs a machine learning pipeline for classifying a binary outcome ('fail') based on sensor data. It includes data loading, exploration, model training (Logistic Regression, KNN, Random Forest), evaluation, result saving, and integration with a remote data repository (DBRepo).

The already splitted data (train-validation-test) was retrieved from the DBRepo: https://test.dbrepo.tuwien.ac.at/database/7f109eb4-a337-422e-9b22-f43b7f2a4cf6/subset
Identifieres of each subset:

Train: https://handle.test.datacite.org/10.82556/d81q-jc28

Validation: https://handle.test.datacite.org/10.82556/azm3-kr44

Test: https://handle.test.datacite.org/10.82556/mmbk-7097

Full Dataset: https://handle.test.datacite.org/10.82556/exht-bk06

Output available at: https://test.researchdata.tuwien.ac.at/records/j3pt9-shg53

Output Identifier: https://handle.test.datacite.org/10.70124/j3pt9-shg53

## Environment Variables

The script requires the following environment variables to be set:

- `DBREPO_USER`: DBRepo username.
- `DBREPO_PASSWORD`: DBRepo password.
- `DBREPO_TOKEN`: TUWRD Test Instance API access token.

So in order to reproduce the experiment, one needs both a DBRepo user as well as a TUWRD user. After the successfull creation of the TUWRD user, one needs to generate an access token (Settings -> Profile -> Applications -> Personal Acess Token)

## Dependencies

The necessary libraries are listed in `requirements.txt`. Please install them using `pip install -r requirements.txt`.

## Usage

1.  **Install Dependencies:** Install the required libraries using `pip install -r requirements.txt`. Ensure the `dbrepo` library is also installed and configured.
2.  **Set Environment Variables:** Set the `DBREPO_USER`, `DBREPO_PASSWORD`, and `DBREPO_TOKEN` environment variables.
3.  **Run the Script:** Execute the Python script (The notebook was created with Anaconda, but any other Jupyter Environment would suffice).

The script will perform the machine learning pipeline, save results locally, and upload them to the DBRepo.
## 0. Imports

This section imports necessary libraries. A complete list of dependencies can be found in `requirements.txt`.

## 1. Data Exploration

This section loads the dataset from the DBRepo using the provided identifier.

- It retrieves the data using the `client.get_identifier_data()` function.
- The 'id' column is dropped.
- All columns except 'fail' are converted to numeric types.

The subsequent code likely performs further data exploration and visualization to understand the dataset.

## 2. Model Training and Evaluation

This section prepares the data for model training and then trains and evaluates three different classification models.

### 2.1. Data Splitting

- Features (X) and the target variable (y) are defined.
- The dataset is split into training and testing sets, maintaining the proportion of the target variable in both sets.

### 2.2. Model Initialization and Hyperparameter Tuning (GridSearchCV)

For each model (Logistic Regression, KNN, Random Forest):

- A base model is initialized.
- A parameter grid defines hyperparameters to be tested.
- `GridSearchCV` is used with stratified K-Fold cross-validation to find the best hyperparameter combinations.
- The best model is stored.
- The best parameters are printed.

### 2.3. Model Evaluation on Test Set

For each trained model:

- Predictions are made on the test set.
- Evaluation metrics (accuracy, precision, recall, F1-score) are calculated and stored.
- A confusion matrix is generated and visualized, saved as a PNG file.

### 2.4. Cross-Validation (Alternative Evaluation)

- Stratified 5-fold cross-validation is performed on the training data to get a more robust performance estimate. Mean accuracy scores are printed.

## 3. Results and Model Ranking

This section compares model performance and saves the results.

- Evaluation metrics for each model are printed.
- A Pandas DataFrame stores and compares the metrics.
- Models are ranked based on their F1-score.
- The ranking is printed and saved to a text file.
- Best results for each model are saved to individual text files.

## 4. Save Winning Model

- The model with the highest F1-score is identified as the 'winning_model'.
- The trained winning model is saved to a pickle file.
- The name of the winning model is saved to a text file.

## 5. Upload Results to TUWRD

This section uploads the generated result files and the trained winning model to the DBRepo.

- The base URL for the API is defined.
- The access token is retrieved from an environment variable.
- A dictionary of files to upload is created.
- Each file is uploaded to the Repo using the `client.upload_file()` function, associated with the specified identifier.
- A success message is printed.
- More details on the API calls can be found in the official documentation: https://researchdata.tuwien.ac.at/tuw/about/api

