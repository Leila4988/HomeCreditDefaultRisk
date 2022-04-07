# HomeCreditDefaultRisk
Pipeline of solutions for home credit default competition on Kaggle.

* Author: [FTNW](https://from-new-world.blogspot.com/)
* [Competition site](https://www.kaggle.com/c/home-credit-default-risk)

## BUSINESS UNDERSTANDING
HomeCredit is an international non-bank financial institution to provide loans for customers. They want to lease the knowledge of outstanding people all over the world to produce one model, which predicts whether an applicant will pay back the loan. 

This competition is a typical Risk Control problem, based on four aspects of metrices: 

* information about the person: age, gender, degree, etc.
* information about the loan: type, time period, amount, etc.
* the willingness for repaying(financial habits): history of repaying loans, history of consuming, history of applying loans, history of repaying delays, etc.
* the ability for repaying(financial information): salary, property, liabilities, etc. 

Some similar and useful resources can be found in this [discussion](https://www.kaggle.com/competitions/home-credit-default-risk/discussion/63032). 

## DATA UNDERSTANDING
Data for this competition are tabular data stored in .csv files, consisting of seven .csv files. Detailed introduction of data can be found in the [Data](https://www.kaggle.com/competitions/home-credit-default-risk/data) module of this competition.

## DATA PREPARATION
Pipeline of operations on training and testing data. 

##### Exploratory data analysis

TODO

### Data Preprocessing
##### Data cleaning (usually done by data providers in kaggle)
1. remove columns with a single and repetitive value
2. remove rows with duplicate data
3. columns with very few(relative to the amount of whole dataset) unique values or with low variance -> if useless remove else try encode as categorical values
4. transform data format: transform date columns from negetive numbers to normal date format
5. transform unreasonable data to null

##### Missing values
| data file | percent of columns |
| ------ | ------ | 
| application_train.csv | 44.6% columns with above 20% missing values | 
| bureau.csv | 35% columns with above 5% missing values |
| bureau_balance.csv | 0 missing values |
| POS\_CASH_balance.csv | 25% columns with missing values |
| credit\_card_balance.csv | 39.1% columns with missing values |
| installments_payments.csv | 25% columns with missing value |
| previous_application.csv | 37.8% columns with above 20% missing values |

Those files except application\_train.csv represent different aspects of users' history information, which will be processed further and aggregated with the application\_train.csv. Therefore we will leave those missing values right now, and handle them after aggregation. 

##### Outliers
There is no standard definition or range for outliers and it costs a lot of time to check every feature to find out outliers. Meanwhile, XGBoost and LightGBM models used in this solution are not based on distances and not sensitive to outliers, therefore this solution skips the processing of outliers. 

### Feature Engineering
There are mainly three kinds of methods to generate new features:

* automatic and violent feature engineering via feature dictionary: aggregate operations, count, unique, statistical methods, etc
* feature engineering based on domain knowledge
* feature engineering based on feature importance: focus on features with relative higher importance and do more hands-on feature engineering, such as rnn to extract time-series and nlp for text features, etc

##### basic operations
1. encode categorical features: one-hot encoder or label encoder
2. data scaling: normalization or standardization (not necessary for non-distance based models)

##### bureau_balance.csv
aggregate mean for categorical features and {min, max, size} for numerical features on every SK\_ID_BUREAU

##### bureau.csv 
1. left join with aggregated bureau_balance.csv;
2. aggregate mean for categorical features and statistical metrices for numerical features on every SK\_ID_CURR within most recent 3,5,10 records to exploit the recent info;
3. calculate weighted mean(use the time period as the weight) of aggregated data 

##### POS\_CASH_balance.csv
aggregate mean for categorical features and {min, max, size} for numerical features on every SK\_ID_CURR

##### credit\_card_balance.csv
aggregate mean for categorical features and {min, max, size} for numerical features on every SK\_ID_CURR

##### installments_payments.csv
1. create some simple features with interaction between two exising features and True/False split
2. ggregate mean for categorical features and {min, max, size} for numerical features on every SK\_ID_CURR

##### previous_application.csv
1. split the whole dataset into three parts for aggregation: the whole dataset, dataset with approved applications, dataset with refused applications
2. aggregate mean for categorical features and statistical metrices for numerical features on every SK\_ID_CURR within most recent 3,5,10 records to exploit the recent info;
3. calculate weighted mean(use the time period as the weight) of aggregated data

##### application_train.csv
1. encode categorical features
2. create some simple features with division between two features 
3. left join with above processed dataset

## MODELING AND EVALUATION
### Algorithm
##### LightGBM


### Evaluation
##### Loss function and evaluation metric

##### Cross validation


### Fine-tune



## DEPLOYMENT