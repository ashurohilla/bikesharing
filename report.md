Report: Predicting Bike Sharing Demand with AutoGluon Solution

By **Ashish Rohilla**

**Initial Training**

Upon attempting to submit the predictions, it was realized that some of the experiments produced negative prediction values. To comply with Kaggle's submission guidelines, all negative outputs from the respective predictors were replaced with 0.

Five different experiments were conducted:

Initial Raw Submission [Model: initial]
Added Features Submission (EDA + Feature Engineering) [Model: add_features]
Hyperparameter Optimization (HPO) - Initial Setting Submission
Hyperparameter Optimization (HPO) - Setting 1 Submission
The top-ranked model was the "add_features" model named WeightedEnsemble_L3. It achieved a validation RMSE score of 37.9800 and the best Kaggle score of 0.44798 on the test dataset. This model was developed by training on data obtained through exploratory data analysis (EDA) and feature engineering without using a hyperparameter optimization routine. While some models showed improved RMSE scores on the validation data after hyperparameter optimization, the "add_features" model performed the best on the unseen test dataset. Several models delivered competitive performance, and the selection was based on both RMSE (cross-validation) and Kaggle (test data) scores.

Note: The autogluon package considers RMSE scores in the Jupyter notebook to be negative for ranking purposes and expects them to be multiplied by '-1' to produce accurate RMSE scores. Therefore, values in the notebook may appear as negative RMSE values.

**Exploratory Data Analysis and Feature Creation**


The exploratory analysis revealed the following findings and additional features were added accordingly:

The "datetime" feature was parsed as a datetime feature to extract the hour information from the timestamp.
The independent features "season" and "weather" were initially read as integers. Since they are categorical variables, they were transformed into the category data type.
The year, month, day (dayofweek), and hour were extracted as distinct independent features from the datetime feature using feature extraction. After feature extraction, the datetime feature was dropped.
The "casual" and "registered" features showed a significant improvement in RMSE scores during cross-validation and were highly correlated with the target variable "count." However, since these features were only present in the train dataset and absent in the test data, they were ignored/dropped during model training.
A categorical feature called "day_type" was added based on the "holiday" and "workingday" features. It effectively segregated the categories into "weekday," "weekend," and "holiday."
The features "temp" (temperature in degree Celsius) and "atemp" ('feels like' temperature in degree Celsius) had a high positive correlation of 0.98. To reduce multicollinearity between independent variables, "atemp" was dropped from the train and test datasets.
Data visualization techniques were employed to gain insights from the features.
The model's performance improved by approximately 138% after adding the additional features. Converting certain categorical variables with integer data types into their true categorical data types and dropping correlated variables helped enhance the model's performance. Splitting the datetime feature into multiple independent features (year, month, day, and hour) and adding the "day_type" feature further improved the model's performance by enabling it to assess seasonality and historical patterns more effectively.

**Hyperparameter Tuning**

Hyperparameter tuning improved the model's performance compared to the initial submission. Three different configurations were used for hyperparameter optimization experiments. Although the hyperparameter-tuned models showed competitive performance, the "add_features" model outperformed them on the Kaggle (test) dataset.

**Observations:**

The autogluon package was used for training while considering the prescribed settings. However, the performance of the hyperparameter-optimized models was sub-optimal due to the limited options autogluon could explore when tuning hyperparameters with a fixed set of values provided by the user.
The "time_limit" and "presets" parameters were crucial during hyperparameter optimization using autogluon.
If the time limit for model building was too short, autogluon might fail to build any models for the prescribed set of hyperparameters.
Hyperparameter optimization with presets like "high_quality" (with auto_stack enabled) required high memory usage and was computationally intensive within the given time limit and available resources. Therefore, lighter and faster preset options like "medium_quality" and "optimized_for_deployment" were experimented with. The "optimized_for_deployment" preset was preferred for the hyperparameter optimization routine, as the other presets failed to create models using AutoGluon for the experimental configurations.
Balancing the exploration vs. exploitation dilemma was a major challenge while using AutoGluon with a predefined range of hyperparameters.