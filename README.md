# Capstone project

** Author: Bogdan Gavril **

## Executive summary

In the world of hardware design where schedules are extremely tight it is very important to determine the complexity of a project and to estimate the engineering time required for completion. In particular, PCB Design projects have a wide range of complexity and the design time varies significantly from project to project. Determining the complexity of a project and the design time have a huge impact on scheduling and on utilizing the engineering resources. Usually these determinations are made by the engineers themselves or by their managers, but many times there is a significant subjective component which makes these estimations to vary from person to person. 
This project proposes the use of Machine Learning methods to make these predictions using existing project data. The outcome consists of Machine Learning models that can be used to predict the design duration and to classify the project as High or Low complexity. 

The first part addresses the classification of the PCB Design projects complexity, using two labels: High and Low. Using he available data, the best classification model has an accuracy score of 0.76.
The second part addresses the prediction of the design duration. With the available data, the best model has an accuracy score of 0.42 which by itself is low and the analysis shows the reasons believed to be the cause of this limited accuracy. 
The analysis also found that the data fails to capture the changes that happen during a design and which can change either the complexity or the design duration or both. For example, a design considered of low complexity and estimated to take 30 days to complete can undergo changes along the way which make it more complex and increase the duration with 20 additional days. The data captures the actual duration of 50 days, but does not include and quantify the changes, thus a 50 days duration is recorded for a set of features which initially indicated a 30 days duration. 

The brief will include details and also recommendations for the next steps needed to improve the models and the accuracy. 

## Classification

### Rationale
Usually a project complexity is estimated by the engineers or the managers who are directly involved in the design, based on objective and subjective factors such as personal experience. A tool which uses only projects' features and eliminates the subjective factors, and which consistently provides good classifications, would considerably improve the resource planning and projects' assignment. The tool can also learn along and the data can improve as a result of the feature importance results.
The classification of a project based on physical and descriptive characteristics of the project can improve the management understanding of the tasks and can indicate the type of resources needed to be used. For example a low complexity project can be designed by a more junior engineer, while the very complex ones require some more experience. A classification tool can help team leaders to assign projects more accurately.

### Research Questions
The main question here is:
1. Can I use the available data to build a classifier tool that can help predicting the complexity of a project?

There are a few additional questions that this analysis can answer.

2. What data can be ignored, or dropped from the analysis?
3. What are the top features that influence the classification?
4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?

#### The overall objective is to find the best classification model that can tell whether a project is of high or low complexity. 


### Data Sources
I will use PCB design projects data collected from various places and periods of time. 
The dataset contains 345 entries and 18 features. The dataset is complete with no missing values and has both numerical and categorical data.
The features include physical characteristics of the boards designed, plus a number of descriptive characteristics.
Since the classification is needed before the design starts when only a part of the features are available some of the features from the dataset will be discarded.
The dataset header:

![dataset header](/images/df_head.png)

Here is the description of the dataset features: 

![dataset features](/images/df_feat.png)


### Methodology
I am using the following steps in my analysis:

1. Data understanding
- analyzing the data and observe the nature of the features, such as numerical or categorical, the relation between the various features, the missing values; this step also includes observing the outliers and the quality of the data in general

2. Engineer the features
- involves imputing missing values, scaling features, encoding categorical variables, removing outliers, etc.
- encoding is done using One Hot Encoder for categorical features and Standard Scaler for the numerical features
 
3. Prepare the data for use in the models
- involves creating the train and the test set by splitting the dataset; the models are trained on the train set and used to make predictions on the test set
4. Create and train several classification models using the train set
- K-Nearest Neighbors, Logistic Regression, Decision Trees, and Support Vector Classification
- K-Nearest Neighbors is a data classification method for estimating the likelihood that a data point will become a member of one group or another based on what group the data points nearest to it belong
- Logistic Regression is a statistical model that uses a logistic function to model a binary-dependent variable
- Decision Tree Classifier is a supervised learning algorithm. In this algorithm, data are continuously split into smaller parts until it reaches its class.
- Support Vector Classification is a supervised machine learning model that uses classification algorithms for two-group classification problems. SVC works by mapping data points to a high-dimensional space and then finding the optimal hyperplane that divides the data into two classes

5. Evaluate the models
- the methods of evaluation are the score and ROC/AUC curve. ROC curve (receiver operating characteristic curve) is a graph showing the performance of a classification model at all classification thresholds and shows two parameters: true positive rate and false positive rate. AUC is the area under the ROC curve and ranges in value from 0 to 1. The closer to 1 is AUC the better the classification model is.
- the Confusion Matrix is another way to evaluate the performance of a model; it shows the correct and the false predictions in a matrix.

6. Use Grid Search to find the best parameters for these models.
- Grid Search is a cross-validation technique for finding the optimal parameter values from a given set of parameters in a grid. Each of the classification models can accept a number of parameters which affect the performance of the model.

7. Tune the models using the best parameters extracted from the grid search

8. Choose the best tuned model and explore the feature importance. 
- this is a technique that calculates a score for all the input features and simply represents the importance of each feature. This way it can be determined which features contribute the most to the performance of the model.


### Results

1. The best two models are:
#### DecisionTreeClassifier and SVC, with tuned parameters.

#### Decision Tree Classifier, score = 0.77 and AUC = 0.76. 
Here are the confusion matrix and the ROC curve. Out of the 69 test samples the model classifies correctly 15 'Low' labels and 38 'High' labels and wrongly classifies 11 'Low and 5 'High' labels.

![conf matrix dtr](/images/cs_conf_roc_dtr.png)

And this another visualization of the predictions versus the real test data:

![visual dtr](/images/cs_dtr_viz.png)

#### Support Vector Classifier, score = 0.69 and AUC = 0.77.
Here are the confusion matrix and the ROC curve. Out of the 69 test samples the model classifies correctly 21 'Low' labels and 27 'High' labels and wrongly classifies 5 'Low and 16 'High' labels.

![conf matrix dtr](/images/cs_conf_roc_svc.png)

And this another visualization of the predictions versus the real test data:

![visual dtr](/images/cs_svc_viz.png)

5. Feature importance plot shows that
- 'comp', 'scope', 'line' and 'type' have the largest influence on the Decision Tree classifier.

![visual dtr](/images/cs_feat_dtr.png)

- 'dbl', 'line', 'type' and 'category' have the largest influence over the Support Vector Classifier.

![visual dtr](/images/cs_feat_svc.png)
 
2. The labels in this dataset are reasonable balanced and the models can classify with reasonable accuracy. 
3. The dataset does not have too many entries which limits the training of the models. Also, the determination of the complexity as captured in the dataset was definitely biased, being based on estimation made by engineers or managers, based on subjective factors.
4. The dataset and the models cannot capture the unexpected factors which affect the difficulty of a design along the way. A design can start as low complexity based on the initial and change at some point to a highly complex one, in which case the user may update the label, even if the features say otherwise, thus biasing the data. The data I am using fails to capture these variations. Nevertheless, the classification model can be used as an indicator or an estimation, useful for the business purpose of assigning resources.
6. Here are other useful findings related to the features that can be used as a general guide for project managers:

The complexity depends on the project version

![count version](/images/cs_cnt_ver.png)

The more components are on a board, the more complex the design is

![count comp](/images/cs_cnt_comp.png)

The complexity increases with the number of layers

![count layers](/images/cs_cnt_ly.png)

The type of board influences the complexity

![count type](/images/cs_cnt_typ.png)

### Answering the research question

1. Can I use the available data to build a classifier tool that can help predicting the complexity of a project?
- Yes, the data is reasonable but fails to capture the changes during a project which can affect the complexity of a design

2. What data can be ignored, or dropped from the analysis?
- Ignore all the features which in practice are not available before the project is finalized: 'viano', 'pins', 'sq', 'dens', 'netno', 'level', 'duration'

3. What are the top features that influence the classification?
- 'comp', 'scope', 'type', 'line', 'dbl' and 'category' have higher importance than others

4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?
- with a precision of 0.76 and AUC of 0.77 the model using Decision Tree is good, but there is room for improvement 
- The first thing missing is more data and some of the next steps are shown below
- Overall, through this work, I can provide a reasonable model for classifying projects based on a given set of known characteristics

### Next steps
What can be done to improve the models:
- Identify and include additional features which can be collected before the project starts
- Collect much more data
- Explore all the available hyperparameters for each model and use larger ranges for the parameters values
- Explore other ways to encode the data
- Use even fewer features, limit for example to 'comp', 'scope' and 'type' and explore the results
What to advise the organization?
- Collect more data, historical or as becomes available and reach out to similar organizations for data
- Collect new data with the purpose of improving the Machine Learning prediction models. Use the new data to retrain the model and evaluate the performance
- Use the classification model in conjunction with human input
How to use this models in practice?
- Integrate the models in a practical classification application with an easy to use GUI

### Outline of project

- [Link to notebook 1](Capstone-BG-Classification.ipynb)



## Prediction

### Rationale
Usually the design time is estimated by the engineers or managers who are directly involved in the design, based on objective and subjective factors such as personal experience. A tool which uses only projects' features and eliminates the subjective factors, and which consistently provides good estimations, would considerably improve the resource planning and projects' scheduling. Plus the tool can be used by anybody, such as non-technical teams, project managers, etc. The tool can also learn along and the data can improve as a result of the feature importance results.

### Research Questions
The main question here is: 
1. Can I use the available data to build a classifier tool that can help predicting the complexity of a project?

There are a few additional questions that this analysis can answer.

2. What data can be ignored, or dropped from the analysis?
3. What are the top features that influence the prediction?
4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?

#### The overall objective is to find the best prediction model that can tell the duration of a new project.

### Data Sources
I will use PCB design projects data collected from various places and periods of time. 
The dataset contains 345 entries and 18 features. The dataset is complete with no missing values and has both numerical and categorical data.
The features include physical characteristics of the boards designed, plus a number of descriptive characteristics.
Since the classification is needed before the design starts when only a part of the features are available some of the features from the dataset will be discarded.

The dataset header:

![dataset header](/images/df_head.png)

Here is the description of the dataset features: 

![dataset features](/images/df_feat.png)


### Methodology
I am using the following steps in my analysis:

1. Data understanding
- analyzing the data and observe the nature of the features, such as numerical or categorical, the relation between the various features, the missing values; this step also includes observing the outliers and the quality of the data in general

2. Engineer the features
- involves imputing missing values, scaling features, encoding categorical variables, removing outliers, etc.
- encoding is done using One Hot Encoder and Binary Encoder for categorical features and Standard Scaler for the numerical features
 
3. Prepare the data for use in the models
- involves creating the train and the test set by splitting the dataset; the models are trained on the train set and then used to make predictions using the test set

4. Create and train several regression models using the train set.
- Linear Regression, Ridge, Lasso, Transformed Target Regressor with Ridge, Transformed Target Regressor with Random Forest Regressor, Transformed Target Regressor with Lasso
- Linear Regression is a model of the relationship between two variables, fit to a linear equation
- Ridge regression is a method of estimating the coefficients of multiple-regression models in scenarios where the independent variables are highly correlated
- Lasso (least absolute shrinkage and selection operator) is a regression analysis method that performs both variable selection and regularization in order to enhance the prediction accuracy and interpretability of the resulting model
- Transformed Target Regressor is a meta-estimator to regress on a transformed target. It can use different regressors, such as Ridge and Lasso
- Random Forest Regressor is a meta estimator that fits a number of classifying decision trees on various sub-samples of the dataset and uses averaging to improve the predictive accuracy and control over-fitting


5. Evaluate the models
- the method of evaluation is Median Absolute Error which is the median of all absolute differences between the target and the prediction
- the second method is the accuracy score of the model; the closer to 1 the score is, the better the model, but the accuracy needs to be evaluated in conjunction with the median absolute error. For applications where the accuracy is not critical, Median Absolute Error should be evaluated as the main criteria
- use the test set and determine the median absolute error and the score for each model
5. Use Grid Search to find the best parameters for these models.
- Grid Search is a cross-validation technique for finding the optimal parameter values from a given set of parameters in a grid. Each of the regression models can accept a number of parameters which can improve the performance. Grid Search finds these parameters to minimize the loss (MAE) and/or improve the score. 
6. Tune the models using the best parameters
- extract the best parameters from the grid search and tune the models
7. Extract the feature importance from the best performing tuned model
- this is a technique that calculates a score for all the input features and simply represents the importance of each feature. This way it can be determined which features contribute the most to the performance of the model.
8. Choose the best model and use a dataset reduced to the most important features
- this step is an attempt to retrain the best model on a reduced set of features extracted from the feature importance and analyze the score and the loss on the test set

### Results

1. After fitting all the models and running the Grid Search to find the optimal parameters, the results show that the best model is 
#### TransformedTargetRegressor with regressor RandomForestRegressor, with tuned parameters.

This is a comparison table of all the models and Grid Searches: 

![comparison](/images/p_comp_tbl.png)

This is a visualization of the predictions of all the grid models. It shows the actual test data versus the predictions on 36 samples from the test set. 

![visual all](/images/p_viz_all.png)

2. The most important features extracted using feature importance are 'scope','line','complexity','dbl','ver'

visual all](/images/p_feat_imp.png)

3. The final model TransformedTargetRegressor with regressor RandomForestRegressor, with tuned parameters, has been trained on a dataset reduced to the most important features from above.
- low accuracy score of 0.33
- Median Absolute Error of 12.07 on the test set which is much better better than the baseline which is 23.52

The final model visualization and the R2 plot:

![visual final](/images/p_viz_final.png)

![R2](/images/p_r2.png)
 
4. The reason for the limited accuracy score is the following: the dataset cannot capture the unexpected factors which affect a design along the way. The data has been entered at the end of a project, thus the duration was the real duration as registered at the end of the design, but what is important here is that the dataset cannot capture the real events along a project, such as changes and blockers, which delay the design and adds to the overall duration. A design which normally takes 30 days, can be in fact much longer, 60 days or more, due to continuous changes and difficult added technical requirements. In practice, for two projects with similar features, the features are recorded as such in the dataset and should be consistent with a certain similar estimated duration, but in reality the duration can differ vastly due to the mentioned irregularities. 
5. The quality of the data might not be the best, since it was collected with no purpose of this kind in mind. Some of the categorical and numeric features can be estimations and not exact values. 
6. The prediction model can be rather used as a loose indicator, an estimation. It can be useful for the business purpose of scheduling, since in this particular business case, low accuracy can be accepted.
7. Here are other useful findings related to how the features are related to the median project duration:

Board version has influence over the project duration. Prototype designs take the longest

![version](/images/p_version.png)

Projects take longer when there are new designs or major modifications of previous versions

![scope](/images/p_scope.png)

Production boards take more time than the flex boards and platform boards

![type](/images/p_type.png)

### Answering the research question

1. Can I use the available data to build a design duration prediction tool that can help the management to plan the schedules better?
- Yes, if the accuracy is not a requirement and the results can be used as indicators based on the initial project characteristics

2. What data can be ignored, or dropped from the analysis?
- Ignore all the features which in practice are not available before the project is done: 'viano', 'pins', 'sq', 'dens', 'netno', 'level'

3. What are the top features that influence the duration of a project?
- 'scope','line','complexity','dbl','ver' have the highest importance

4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?
- No, the model is not very accurate at only 0.33
- The Median Absolute Error has an acceptable value of 12.07, good comparing to the baseline, which makes the model useful
- What is missing can be more data to train the models
- I must find a way to capture the missing 'change' and 'subjective' features which directly affect the duration. 'change' should capture a numeric value which represents the amount or percentage of the changes happening along the design. 'subjective' should capture factors such as the teams, the time of the year, vacations or team changes, changes in priorities, etc.

### Next steps
What can be done to improve the models:
- Identify and include additional features which can be collected before the project starts
- Collect much more data
- Explore all the available hyperparameters for each model and use larger ranges for the parameters values to be used in the Grid Search
- Explore other ways to engineer the data
- Use even fewer features and explore the results
What to advise the organization?
- Collect more data, historical or as becomes available and reach out to similar organizations for data
- Collect new data with the purpose of improving the Machine Learning prediction models. Use the new data to retrain the model and evaluate the performance
- Use the prediction model with the knowledge that it has a very low precision and can be used as an indicator and in conjunction with human input
How to use this models in practice?
- Integrate the model in a practical prediction application with an easy to use GUI


#### Outline of project


- [Link to notebook 2](Capstone-BG.ipynb)



#### Contact and Further Information
bgavril@me.com


