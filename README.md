Capstone project

** Author: Bogdan Gavril **

### Executive summary

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

### Research Question
The main question here is:
1. Can I use the available data to build a classifier tool that can help predicting the complexity of a project?

There are a few additional questions that this analysis can answer.

2. What data can be ignored, or dropped from the analysis?
3. What are the top features that influence the classification?
4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?

The overall objective is to find the best classification model that can tell whether a project is of high or low complexity. 


### Data Sources
I will use PCB design projects data collected from various places and periods of time. 
The dataset contains 345 entries and 19 features. The dataset is complete with no missing values and has both numerical and categorical data.
The features include physical characteristics of the boards designed, plus a number of descriptive characteristics.
Since the classification is needed before the design starts when only a part of the features are available some of the features from the dataset will be discarded.
The dataset header:
![dataset header](/images/df_head.png)

Here is the description of the dataset features: 

![dataset features](/images/df_feat.png)


### Methodology
I will use the following steps in my analysis:
1. Data understanding
2. Engineer the features
3. Prepare the data for use in the models - cleanup, encoding, train/test split
4. Create and train several classification models: K-Nearest Neighbors, Logistic Regression, Decision Trees, and Support Vector Machines.
5. Use Grid Search to find the best parameters for these models.
6. Tune the models using the best parameters
7. Choose the best model

### Results
The results were surprising at first, but diving deeper into the problem, this is what I determined. 
Even if the labels in this dataset are reasonable balanced, the models can classify with a somewhat limited precision and with room for improvements. 
The dataset does not have too many entries which limits the training of the models. Also, the determination of the complexity as captured in the dataset was definitely biased, being based on estimation made by engineers or managers, based on subjective factors.
The dataset and the models cannot capture the unexpected factors which affect the difficulty of a design along the way. A design can start as low complexity based on the initial and change at some point to a highly complex one, in which case the user may update the label, even if the features say otherwise, thus biasing the data. The data I am using fails to capture these variations.
Nevertheless, the classification model can be used as an indicator or an estimation, useful for the business purpose of assigning resources.
AUC and Precision have been used as model performance criteria. All models performed better than the baseline which has a score of 0.5. 
The best models are DecisionTree and SVC, with tuned parameters.

This is the confusion matrix for the Decision Tree classifier using the test set. Out of the 69 test samples the model classifies correctly 15 'Low' labels and 38 'High' labels and wrongly classifies 11 'Low and 5 'High' labels.

![conf matrix dtr](/images/cs_dtr_confm.png)

And a more visual representation of the classification: 
![visual dtr](/images/cs_dtr_viz.png)

This is the confusion matrix for the SVC classifier using the test set. Out of the 69 test samples the model classifies correctly 21 'Low' labels and 27 'High' labels and wrongly classifies 5 'Low and 16 'High' labels.

![conf matrix dtr](/images/cs_svc_confm.png)

And a more visual representation of the classification: 
![visual dtr](/images/cs_svc_viz.png)

Also here are useful findings related to the features that can be used as a general guide for project managers:

The complexity depends on the project version
![count version](/images/cs_cnt_ver.png)

The more components are on a board, the more complex the design is
![count comp](/images/cs_cnt_comp.png)

The complexity increases with the number of layers
![count layers](/images/cs_cnt_ly.png)

The type of board influences the complexity
![count type](/images/cs_cnt_typ.png)

### Answering the research question

Can I use the available data to build a classifier tool that can help predicting the complexity of a project?
- Yes, the data is reasonable but fails to capture the changes during a project which can affect the complexity of a design

What data can be ignored, or dropped from the analysis?
- Ignore all the features which in practice are not available before the project is finalized

What are the top features that influence the classification?
- 'comp', 'scope', 'type and 'line' have higher importance than others

Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?

- with a precision of 0.76 and AUC of 0.77 the model using Decision Tree is reasonable, but there is room for improvement 
- The first thing missing is more data and some of the next steps are shown below
- Overall, through this work, I can provide a reasonable model for classifying projects based on a given set of known characteristics

### Next steps
What can be done to improve the models:
- identify and include additional features which can be collected before the project starts
- collect much more data
- explore all the available hyperparameters for each model and use larger ranges for the parameters values
- explore other ways to encode the data
- use even fewer features, limit for example to 'comp', 'scope' and 'type' and explore the results
What to advise the organization?
- collect more data, historical or as becomes available and reach out to similar organizations for data
- use the classification model with the knowledge that it has a limited precision and can be used in conjunction with human input
How to use this models in practice?
- integrate the models in a practical classification application with an easy to use GUI

### Outline of project

- [Link to notebook 1](Capstone-BG-Classification.ipynb)



## Prediction

### Rationale
Usually the design time is estimated by the engineers or managers who are directly involved in the design, based on objective and subjective factors such as personal experience. A tool which uses only projects' features and eliminates the subjective factors, and which consistently provides good estimations, would considerably improve the resource planning and projects' scheduling. Plus the tool can be used by anybody, such as non-technical teams, project managers, etc. The tool can also learn along and the data can improve as a result of the feature importance results.

### Research Question
The main question here is: 
1. Can I use the available data to build a classifier tool that can help predicting the complexity of a project?

There are a few additional questions that this analysis can answer.

2. What data can be ignored, or dropped from the analysis?
3. What are the top features that influence the prediction?
4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?

### Data Sources
I will use PCB design projects data collected from various places and periods of time. 
The dataset contains 345 entries and 19 features. The dataset is complete with no missing values and has both numerical and categorical data.
The features include physical characteristics of the boards designed, plus a number of descriptive characteristics.
Since the classification is needed before the design starts when only a part of the features are available some of the features from the dataset will be discarded.
The dataset header:
![dataset header](/images/df_head.png)

Here is the description of the dataset features: 

![dataset features](/images/df_feat.png)


### Methodology
I will use the following steps in my analysis:
1. Data understanding
2. Engineer the features
3. Prepare the data for use in the models - cleanup, encoding, train/test split
4. Create and train several classification models: Linear Regression, Ridge, Lasso, Transformed Target Regressor with Ridge, Transformed Target Regressor with Random Forest Regressor, Transformed Target Regressor with Lasso
5. Use Grid Search to find the best parameters for these models.
6. Tune the models using the best parameters
7. Extract the feature importance 
7. Choose the best model and use a dataset reduced to the most important features

### Results
1. The models can predict, but with very low score, even if the MAE is better than the baseline
2. The dataset does not have too many entries which limits the training of the models.
3. Much more data is needed to allow for a good training of the models
4. The dataset cannot capture the unexpected factors which affect a design along the way. The data has been entered at the end of a project, thus the duration was the real duration as registered at the end of the design. What is important here is that the dataset cannot capture the real events along a project, such as changes and blockers, which delay the design and adds to the overall duration. A design which normally takes 30 days, can be in fact much longer, 60 days or more, due to continuous changes and difficult technical requirements. So, even if the features recorded in the dataset indicate a theoretical duration, in reality the duration can differ vastly.
5. The quality of the data might not be the best, since it was collected with no purpose of this kind in mind.
6. The prediction model can be used as a loose indicator, a pure estimation. It can be useful for the business purpose of scheduling, since the result is theoretically correct. In this particular business case, low accuracy can be accepted.
7. Score and Median Absolute Error (MAE) have been used for evaluating the models. All models performed better than the baseline, but the score is very low. In itself, this can be accepted if the accuracy is not a requirement, such as in this case.
8. The best model is TransformedTargetRegressor with regressor = RandomForestRegressor, with tuned parameters.
9. The features which influence the design duration are: 'scope','line','complexity','dbl','ver'

This is a comparison table of the models and grid search:

![comparison](/images/p_comp_tbl.png)

This is a visualization of the predictions of all the grid models. It shows the actual test data versus the predictions on 36 samples from the dataset. 
![visual all](/images/p_viz_all.png)

The final model visualization and the R2 plot:
![visual final](/images/p_viz_final.png)

![R2](/images/p_r2.png)

Also here are useful findings related to how the features are related to the median project duration:

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
- Ignore all the features which in practice are not available before the project is done
3. What are the top features that influence the duration of a project?
- 'scope','line','complexity','dbl','ver' have the highest importance
4. Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?
- No, the model is not accurate at all. 
- What is missing is much more data to train the models
- I must find a way to capture the missing 'change' and 'subjective' features which directly affect the duration. 'change' should capture a numeric value which represents the amount or percentage of the changes happening along the design. 'subjective' should capture factors such as the teams, the time of the year, vacations or team changes, changes in priorities, etc.

### Next steps
What can be done to improve the models:
- identify and include additional features which can be collected before the project starts
- collect much more data
- explore all the available hyperparameters for each model and use larger ranges for the parameters values
- explore other ways to encode the data
- use even fewer features
What to advise the organization?
- collect more data, historical or as becomes available and reach out to similar organizations for data
- use the prediction model with the knowledge that it has a very low precision and can be used as an indicator and in conjunction with human input
How to use this models in practice?
- integrate the model in a practical prediction application with an easy to use GUI


#### Outline of project


- [Link to notebook 2](Capstone-BG.ipynb)



#### Contact and Further Information
bgavril@me.com


