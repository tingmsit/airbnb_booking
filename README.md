# Airbnb New Users Booking

[Airbnb](www.allstate.com) is one of the world’s largest marketplaces for unique, authentic places to stay and things to do, offering over 7 million accommodations and 50,000 handcrafted activities, all powered by local hosts. An economic empowerment engine, Airbnb has helped millions of hospitality entrepreneurs monetize their spaces and their passions while keeping the financial benefits of tourism in their own communities. With more than three quarters of a billion guest arrivals to date, and accessible in 62 languages across 220+ countries and regions, Airbnb promotes people-to-people connection, community and trust around the world.

### Objective
- Predict in which country a new user will make his or her first booking. Evaluation metric is NDCG (Normalized discounted cumulative gain) at k = 5. In other word, making a maximum of 5 predictions on the country of the first booking at the used id level

### Clients & Impacts:

- **Airbnb Leadership**: model can be used as a recommendation system for new users to encourage conversion with better targeted marketing.


- **Airbnb Clients**: more relevant recommendation to clients, which enable smoother user experience on finding the first destination.

### Data Source:
- [Kaggle](https://www.kaggle.com/c/airbnb-recruiting-new-user-bookings/data)
    - To obtain data, download from the link above, unzip and put all csv files into a folder called "data" OR using the script [HERE](https://github.com/sittingman/airbnb_booking/blob/master/0.obtain_data.ipynb)
- [Data Dictionary](https://github.com/sittingman/airbnb_booking/blob/master/data_dict.ipynb)

### Outline of Approach

   #### [Data Cleansing/Wrangling](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Understanding the shape of the datasets, check for missing value, and align on data types.
   - Findings: 
       - convert all date/time related columns into datetime
       - deal with missing age and through assigning new variables and binning
       - filling first_affiliate_tracked missing value with new category value "missing"no missing data. 
       - create new categorical as needed to summary sparse variables

   #### [Exploratory Analysis](https://github.com/sittingman/airbnb_booking/blob/master/1.data_expo.ipynb): 
   - Identify variables that may influence the likelihood of a new user making a booking
   - Findings: 
       - almost all variables had some influence on the likelihood of booking (under chi-square test)
       - drop affiliate provide as that is highly predictive by affiliate channel
       - for session data, we will assume the last record of the each user action is the last action for the session, and created binary variables based on content on action type and details

   
   #### Machine Learning: 
   - Strategy:
       - apply feature selection method to narrow down numbers of binary variables from action details and type. Use LassoCV, followed by Recursive Feature Elimination (RFE) using Logistics Regression and Random Forest as regressors.
       - combine features set as identify from exploratory analysis with the action details binary variables
       - compare base model performance across five type of classification models based on NDCG score. Pick top three models for hyperparameters tunning
       - apply deep learning method using Keras to identify potential performance improvement
    
   - Findings:
      - first test on one train test suggested that all models have ndgc scores around 0.8
      - once validated with 4 folds cross validation with roc_auc_ovr scoring, tree base models and light gbm appears to have the best results
      - Perform hyperparameters tunning using Bayesian optimization with hyperopt, train-set results are as follow:
          | Models | ndcg on train set |
          | --- |  --- |     
          |extra trees | 0.820 |
          | random forecast | 0.806 |
          | lightgbm| 0.824 | 
       - Perform deep learning using Keras, ndcg score is 0.826
          
### Performance Summary 

| Models | Kaggle ndcg |
| --- |  --- |     
|extra trees | 0.845 |
|extra trees (tuned) | 0.845 |
| random forecast | 0.842 |
| random forecast (tuned) | 0.842 |
| lightgbm| 0.825 | 
| lightgbm tuned | 0.825 | 
|Deep learning (keras) | 


### Recommendations/next steps



[Capstone Report]

[Presentation Slides]