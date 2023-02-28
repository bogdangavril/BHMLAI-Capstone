# BHMLAI-Capstone

The link to the notebooks: https://github.com/bogdangavril/BHMLAI-Capstone.git

Classification: https://github.com/bogdangavril/BHMLAI-Capstone.git
Prediction: https://github.com/bogdangavril/BHMLAI-Capstone.git

Datasets in folder 'data'.


Data understanding: The dataset represents PCB design projects data collected from various places and periods of time. 
The dataset contains 345 entries and 19 features. The dataset is complete with no missing values and has both numerical and categorical data.
The features include physical characteristics of the boards designed, plus a number of descriptive characteristics.

Classification

Business understanding:

Usually a project complexity is estimated by the engineers or the managers who are directly involved in the design, based on objective and subjective factors such as personal experience. A tool which uses only projects' features and eliminates the subjective factors, and which consistently provides good classifications, would considerably improve the resource planning and projects' assignment. The tool can also learn along and the data can improve as a result of the feature importance results.

The classification of a project based on physical and descriptive characteristics of the project can improve the management understanding of the tasks and can indicate the type of resources needed to be used. For example a low complexity project can be designed by a more junior engineer, while the very complex ones require some more experience. A classification tool can help team leaders to assign projects more accurately.

Important observation
The classification is needed and happens before the design starts when only a part of the features are available. Thus, some of the features will be discarded.

Conclusions
Even if the labels in this dataset are reasonable blanced, the models can classify with a limited precision and with room for improvement
The dataset does not have too many entries which limits the training of the models. Also, the determination of the complexity as captured in the dataset was definitely biased, being based on estimation made by engineers or managers, based on subjective factors.
More data collected, plus a classifications made based on project characteristics, will improve the dataset in the future, giving a better training to the models.
The dataset and the models cannot capture the unexpected factors which affect the difficulty of a design along the way. A design can start as low complexity based on the initial and change at some point to a highly complex one, in which case the user may update the label, even if the features say otherwise, thus biasing the data.
The classification model can be used as an indicator or a pure estimation, useful for the business purpose of assigning resources.
AUC and Precision have been used as model performance criteria. All models performed better than the baseline.
The best models are DecisionTree and SVC, with tuned parameters.
Answers to the initial data task:
Can I use the available data to build a classifier tool that can help predicting the complexity of a project?
- Yes, the data is reasonable
What data can be ignored, or dropped from the analysis?
- Ignore all the features which in practice are not available before the project is done
Do I need to clean up the data and how do I prepare the data?
- Data was clean, preparation included dropping unused columns and encode the data
What are the top features that influence the classification?
- 'comp', 'scope', 'type and 'line' have higher importance
Is the model accurate enough? What is missing, how can I improve the data set and the analysis as next steps?
- with a precision of 0.76 and AUC of 0.77 the model using Decision Tree is reasonable, but there is room for improvement. The first thing missing is more data and some of the next steps are shown below.
Overall, through this work, I can provide a reasonable model for classifying projects based on a given set of known characteristics.

Next steps
What can be done to improve the models:
- identify and include additional features which can be collected before the project starts
- collect much more data
- explore all the available hyperparameters for each model and use larger ranges for the parameters values
- explore other ways to encode the data
- use even fewer features, limit for example to 'comp', 'scope' and 'type' and explore the results
What to advise the organization?
- collect more data, hystorical or as becomes available and reach out to similar organizations for data
- use the classification model with the knowledge that it has a limited precision and can be used in conjunction with human input
How to use this models in practice?
- integrate the models in a practical classification application with an easy to use GUI

Prediction

Business understanding
Usually the design time is estimated by the engineers or managers who are directly involved in the design, based on objective and subjective factors such as personal experience. A tool which uses only projects' features and eliminates the subjective factors, and which consistently provides good estimations, would considerably improve the resource planning and projects' scheduling. Plus the tool can be used by anybody, such as non-technical teams, project managers, etc. The tool can also learn along and the data can improve as a result of the feature importance results.

Conclusions
1. The models can predict, but with very low score, even if the MAE is better than the baseline
2. The dataset does not have too many entries which limits the training of the models.
3. Much more data is needed to allow for a good training of the models
The most important conclusions:
4. The dataset cannot capture the unexpected factors which affect a design along the way. The data has been entered at the end of a project, thus the duration was the real duration as registered at the end of the design. What is important here is that the dtataset cannot capture the real events along a project, such as changes and blockers, which delay the design and adds to the overall duration. A design which normally takes 30 days, can be in fact much longer, 60 days or more, due to continuous changes and difficult technical requirements. So, even if the features recorded in the dataset indicate a theoretical duration, in reality the duration can differ vastly.
5. The quality of the data might not be the best, since it was collected with no purpose of this kind in mind.
6. The prediction model can be used as a loose indicator, a pure estimation. It can be useful for the business purpose of scheduling, since the result is theoretically correct. In this particular business case, low accuracy can be accepted.
7. Score and MAE have been used for evaluating the models. All models performed better than the baseline, but the score is very low. In itself, this can be accepted if the accuracy is not a requirement, such as in this case.  
8. The best model is TransformedTargetRegressor with regressor = RandomForestRegressor, with tuned parameters.

### Next steps
1. What can be done to improve the models:
* identify and include additional features which can be collected before the project starts
* collect much more data 
* explore all the available hyperparameters for each model and use larger ranges for the parameters values
* explore other ways to encode the data
* use even fewer features
2. What to advise the organization?
* collect more data, historical or as becomes available and reach out to similar organizations for data
* use the prediction model with the knowledge that it has a very low precision and can be used as an indicator and in conjunction with human input
3. How to use this models in practice?
* integrate the model in a practical prediction application with an easy to use GUI
