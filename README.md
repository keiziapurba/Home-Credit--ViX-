# Home Credit Indonesia (Virtual Internship Experience Rakamin)

## Background
- Many people struggle to get loans due to insufficient or non-existent credit histories. And, unfortunately, this population is often taken advantage of by untrustworthy lenders.
- Home Credit strives to broaden financial inclusion for the unbanked population by providing a positive and safe borrowing experience. In order to make sure this underserved population has a positive loan experience, Home Credit makes use of a variety of alternative data--including telco and transactional information--to predict their clients' repayment abilities.

### Problem Statement
Building a model to predict how capable each applicant is of repaying a loan, so that loan sanctioning is done only for the applicants who are likely to repay the loan.

## Evaluation Matrix
- **Area Under Curve (AUC)**: An ROC curve is the most commonly used way to visualize the performance of a binary classifier, and AUC is (arguably) the best way to summarize its performance in a single number. 
- **Confusion Matrix**: To get an overview of complete predictions.
---

## Data Overview
<img width="679" alt="Screen Shot 2022-04-22 at 22 10 25" src="https://user-images.githubusercontent.com/91368463/164742901-57ad20e0-c3fd-45ff-bda4-70775517b193.png">

1. application_{train|test}.csv
- This is the main table, broken into two files for Train (with TARGET) and Test (without TARGET).
- Static data for all applications. One row represents one loan in our data sample.

2. bureau.csv
- All client's previous credits provided by other financial institutions that were reported to Credit Bureau (for clients who have a loan in our sample).
- For every loan in our sample, there are as many rows as number of credits the client had in Credit Bureau before the application date. 

3. bureau_balance.csv
- Monthly balances of previous credits in Credit Bureau.
- This table has one row for each month of history of every previous credit reported to Credit Bureau – i.e the table has (#loans in sample * # of relative previous credits * # of months where we have some history observable for the previous credits) rows.

4. POS_CASH_balance.csv
- Monthly balance snapshots of previous POS (point of sales) and cash loans that the applicant had with Home Credit.
- This table has one row for each month of history of every previous credit in Home Credit (consumer credit and cash loans) related to loans in our sample – i.e. the table has (#loans in sample * # of relative previous credits * # of months in which we have some history observable for the previous credits) rows.

5. credit_card_balance.csv
- Monthly balance snapshots of previous credit cards that the applicant has with Home Credit.
- This table has one row for each month of history of every previous credit in Home Credit (consumer credit and cash loans) related to loans in our sample – i.e. the table has (#loans in sample * # of relative previous credit cards * # of months where we have some history observable for the previous credit card) rows.

6. previous_application.csv
- All previous applications for Home Credit loans of clients who have loans in our sample.
- There is one row for each previous application related to loans in our data sample.

7. installments_payments.csv
- Repayment history for the previously disbursed credits in Home Credit related to the loans in our sample.
- There is a) one row for every payment that was made plus b) one row each for missed payment.
- One row is equivalent to one payment of one installment OR one installment corresponding to one payment of one previous Home Credit credit related to loans in our sample.

8. HomeCredit_columns_description.csv
---

# Data Analysis

## Data Exploration
- **Missing values**: 100 variables had missing values more than 15%.
- **Duplicated rows**: Customers had duplicate applications, with multiple accounts & credits.
- **Many attributes**: 122 variables in application table. Total 505 variables in joined table. 
- **Missing key features**: missing counts in features
- **Outliers**: approximately 5% of the dataset came with outliers.
- **Imbalanced Class**: 8.07% values for TARGET category 1, and 91.9% for category 0. 

## Data Cleaning
- Removed columns with missing values exceeding more than 15% .
- The duplicate rows were automatically eliminated  by grouping the applications ids.
- Duplicate accounts were aggregated by averaging method.
- Missing values were imputed with the most frequent values
- Outliers exceeding 2xStandardDev were removed

# Data Visualization and Insights

<img width="784" alt="Screen Shot 2022-04-22 at 22 22 45" src="https://user-images.githubusercontent.com/91368463/164745209-60df67a6-7e20-4f0b-b5b0-1f7f76a87ecd.png">

- From the distribution of Target variable, one thing that we can quickly notice is the Data Imbalance. There are only 8.07% of the total loans that had actually been Defaulted. This means that Defaulters is the minority class.
- On the other hand, there are 91.9% loans which were not Defaulted. Thus, Non-Defaulters will be our majority class.
- The Defaulters have been assigned a Target variable of 1 and Non-Defaulters have been assigned Target Variable 0.
- There are very few people who actually default, and they tend to show some sort of different behaviour. Thus in such cases of Fraud, Default and Anamoly Detection, we need to focus on outliers too, and we cannot remove them, as they could be the differentiating factor between Defaulter and Non-Defaulter.


<img width="828" alt="Screen Shot 2022-04-22 at 22 25 35" src="https://user-images.githubusercontent.com/91368463/164745685-cfcaf9b8-6dcf-46e9-8e83-45aa9376b4ae.png">

- About 71% of people have had their education only till Secondary/Secondary Special, along with 24.34% clients having done Higher Education. This suggests that most of the clients/borrowers don't have a high education level.
- From the second plot, we see that the people who have had their studies till only Lower Secondary have the highest Defaulting Characterists, with Secondary and Incomplete higher having similar defaulting tendencies.
- The group of people with Higher Education have comparably lower defaulting tendency, which is logical too. Also, people with Academic Degree show the least Defaulting Rate. However, the Academic Degree group are very few in numbers, so it might not be very useful.


<img width="830" alt="Screen Shot 2022-04-22 at 22 27 18" src="https://user-images.githubusercontent.com/91368463/164745938-9666fc4f-1cf6-40d3-a151-0f8f5c0e89b6.png">

- Among the applicants, the most common type of Occupation is Laborers contributing to close to 26% applications. The next most frequent occupation is Sales Staff, followed by Core Staff and Managers.
- The Defaulting Rate for Low-Skill Laborers is the highest among all the occupation types (~17.5%). This is followed by Drivers, Waiters, Security Staff, Laborers, Cooking Staff, etc. All the jobs are low-level jobs. This shows that low-level Jobs people tend to have higher default rate.
- The lowest Defaulting Rate are among Accountants, Core Staff, Managers, High skill tech staff, HR staff, etc. which are from medium to high level jobs. Thus it can be concluded that Low-level job workers tend to have a higher defaulting tendency compared to medium-high level jobs.


<img width="831" alt="Screen Shot 2022-04-22 at 22 29 24" src="https://user-images.githubusercontent.com/91368463/164746378-d783016d-adec-4e8c-b5ad-0a2d9673beb5.png">

- From the first plot we see that most of the applicants work in Organizations of Type 'Business Entity Type 3', 'XNA' or 'Self Employed'. The Organization Type 'XNA' could probably denote unclassified Organization TYpe.
- From the second plot, we notice that the applicants belonging to 'Transport: type 3' have the highest defaulting tendency as compared to the rest. They are followed by organizations of types: 'Industry: type 13', 'Industry: type 8', 'Restaurant', 'Construction', etc.
- The organizations which show lowest default rates are 'Trade: type 4', 'Industry: type 12', etc.
---

# Machine Learning Implementation and Evaluation

## Feature Engineering
- New features were created as per our estimate of impact of default-rate. ie. number inquiry counts, etc
- Categorical values were converted to dummy variables. 
- Imbalanced classes were balanced using “Upsampling”  method.

## Feature Selection
Handled using Feature Selection method BorutaPy which uses RandomForest as an estimator. Out of 505, only 95 top ranking features were selected

## Modeling
1. **Logistic Regression**
- Algorithm used for binary classification. This Machine Learning classification algorithm is used to predict the probability of a categorical dependent variable.
- Easy to implement and efficient to train
- Helps in Maximum likelihood estimation when used as a performance baseline 
- Highly Interpretable
- Usually performs better on binary classification

<img width="578" alt="Screen Shot 2022-04-22 at 22 54 47" src="https://user-images.githubusercontent.com/91368463/164750787-e6cf975e-dbee-489d-a638-43dd55fdfd4f.png">


2. **Random Forest**
- Ensemble learning method for classification
- Overcomes Overfitting issues
- Reduces variance and improves accuracy
- Works well with Categorical Variables
- Usually performs better on complex datasets, reduce overfitting across data values.

<img width="678" alt="Screen Shot 2022-04-22 at 22 53 49" src="https://user-images.githubusercontent.com/91368463/164750613-9e55f1f3-9534-4870-9117-d62c935d3616.png">


3. **Voting Classifier (Random Forest)**
Chosen to balance the high performance of Random Forest and to cover up the issue with data upsampling method.

<img width="465" alt="Screen Shot 2022-04-22 at 22 52 19" src="https://user-images.githubusercontent.com/91368463/164750395-05c38124-b6b0-4d10-993f-8c65486666f2.png">


###Confusion Matrix

<img width="429" alt="Screen Shot 2022-04-22 at 22 43 56" src="https://user-images.githubusercontent.com/91368463/164749443-d343e39c-7f37-4f01-a4b6-833b4f7fae30.png">


### ROC Curve

<img width="497" alt="Screen Shot 2022-04-22 at 22 44 21" src="https://user-images.githubusercontent.com/91368463/164749488-bc1f6f72-dee7-404a-b2bb-ee8049cef9f3.png">

### Top 20 Features Importance

<img width="379" alt="Screen Shot 2022-04-22 at 22 45 29" src="https://user-images.githubusercontent.com/91368463/164749703-3f3d88c1-8878-4f2c-af4e-c695a67c397f.png">

---

# Business Recommendation
Based on the data, it would be more effective  to approve applicants with the following characteristics:
- **Higher Values** For: External Source Scores 1/2/3, Client’s Current Employment Duration and Phone Number Change Recency
- **Lower Values** For: Client’s Previous Application Refusal Rate and Credit Bureau Inquiries in a Year
- Who are **Highly Educated** and who provide the **correct Contact/Permanent Address**
- **Occupation Type** In: Accountants, Managers and High Skill Tech & Core Staff
- **Work Organization Type Not** In: Self Employed and Business Entity Types 2/3

