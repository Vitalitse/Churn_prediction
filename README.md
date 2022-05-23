# Churn_prediction
In this project we are building a classification model to predict customers churn.
We will evaluate the results using ROC-AUC.

## Steps in the project:
1. Preprocessing and EDA of the data, follwing by merging all tables into the main DataFrame. As we have different number of users under different services (internet and phone), after merging the tables had NA's for users who have no additional services - replace this Yes/No NA's with 0 and Internet service NA's with category of 'No service' - Done.


2. Feature engineering: Seniority of the customer (Feb 1, 2020 - Start date), Year, month, week, average charge (Total charge/seniority) - Done. Seniority was removed as it was correlated with total charges.


3. Data encoding: encoded categorical features using OHE to create dummy variables which were used in Logistic regression and other models. Categorical variables: Type,PaymentMethod and Internet service. We droped the first category in each feature to avoid multicolinieriy - Done.


4. Check correlation between variables to avoid multicolinearity - Done, found high corr between seniority and total charges, seniority was removed.


5. Normalize the data - we will normalize the data (mean=0, sd=1) as we intend to use models which are sensetive to scaling such as SVM - Done, but it was done using pipeline and GridSearchCV after splitting the data to avoid data leakege.


6. Split the data: We created new target feature, 1 if customer left and 0 otherwise. We droped the EndDate column to avoid data leakedge and use the target columns as the target and all others as features. We droped the userID column which didn't add any new information and there is no need to learn anything about specific customer. We droped the BeginDate columns as date can't be used in this format. We then splited the data into train (80%) and test (20%). The train set will be used with Cross Validation to exemine the results on validation set - Done.


7. Imbalanced data - we will exemine the reuslt using tuning hyperparameters for balanced weights or upsampling method -Done, we used upsample in the pipeline to avoid data leakege in cross validation.


8. Build a function that will get a model, set of param and run GridSearch with CV. The models we will use: Logistic regression as a baseline, SVM, Random forest, XGBoost and LightGBM - Done, also added a pipeline as an input to use upsampling and scaling for the train set.
9. Evaluation of the result: We will exemine confision matrix to understnd the results and compatre the results of AUC ROC of train and validation set and the reuslt to our goal, to understand if we need to improve the model or add regularization or add feautres. In addition we will look at feature imprtance and SHAP in order to understand what are the most important feautres - Done, SHAP didn't work properly, we used only feature importance.

## Results:

This problem was solved best using LightGBM model. Our target was to get ROC AUC for test set higher than 88%

Best parameters used in the model found by GridSearchCV: {'modellearning_rate': 0.1, 'modelmax_depth': 20, 'model__n_estimators': 200}

Results:
Train ROC AUC: 91%
Test ROC AUC: 94%

F1 score for test set 89%
