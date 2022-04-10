# Churn Prediction with SAS: Telecom Customer Churn Analysis for Effective Targeting

![image](https://user-images.githubusercontent.com/98763622/162605097-85865028-3445-4f46-b43a-c6776c688811.png)

Original Analysis Report: https://medium.com/@zy145/churn-prediction-with-sas-telecom-customer-churn-analysis-for-effective-targeting-82008611a930
## Introduction
Telecom companies are facing fierce competition because of the homogeneity of services provided and the relatively low cost of switching. Research has shown that the average monthly churn rate among the top 4 wireless carriers in the US is 18.9% -21.2%. The primary purpose of this project is to understand the variation in churn decisions for different segments with clustering and find the profitable customer to retain with logistic regression, to help telecom companies to formulate retention strategies more effectively.

## Data Manipulation
The dataset we used for this project was collected by Faraz Rahman on Github. This dataset has a total of 7,043 observations and 21 variables. Among them, Churn is our target variable. It is defined as whether customers left within the last month. It also recorded services that each customer has signed up for such as phone, internet, etc. Besides, customer account information( tenure, contract, payment method, etc.) and demographic information about customers(gender, age range, etc.) have also been recorded.

We first dropped records with Null values. We encoded 7 binary variables and created dummy variables for 11 multinomial variables. For our modeling, we conducted log transformation for the three continuous variables for our prediction model and standardized them by scaling for our clustering analysis. They can make our data more “normal” so that the analysis results could become more valid.

## EDA
![image](https://user-images.githubusercontent.com/98763622/162605109-1da6a600-0b4f-4d80-b1de-7d75e6f12d64.png)
Exhibit 1: Baseline Churn rate with SAS

To better analyze the impact of each variable on churn, we first visualized the data(see Exhibit 1 in Appendix). We started by looking at the main case of the churn column. It tells us around 26% of customers left the platform within the last month.

Then we visualized the categorical data with respect to churn. In this case, the churn percent is almost equal in the case of gender, MultipleLines and PhoneService. The percent of churn is higher in the case of SeniorCitizen. Customers with Partners and Dependents have lower churn rate. Churn rate is much higher in the case of Fiber Optic InternetServices. Customers who do not have services like No OnlineSecurity, OnlineBackup, and TechSupport have left the platform in the past month. A larger percentage of Customers with monthly subscriptions have left when compared to Customers with one or two-year contracts. Churn percent is higher in customers having paperless billing options. Customers who have ElectronicCheck PaymentMethod tend to leave the platform more when compared to other options. From the results of the continuous variables, customers who have churned, have relatively low tenure, high monthly charges, and low total charges.

## Clustering Analysis
Customer segmentation is essential for a company to understand which segment to target. We used k-means clustering to form the segment. We observed the difference in churn rate among clusters and selected the one that would show a distinctly churn rate. In terms of variable selection, we used monthly charges, total charges, tenure, senior citizen, partner, and dependents in our model, as we want to understand both the customer value and demographics of different churn groups. We tried k = 2 to 6 and k = 3 gave us the best grouping in terms of separating churn rate. The mean features of the clusters are given below.

![image](https://user-images.githubusercontent.com/98763622/162605120-8499e7a6-953a-4b0f-a501-d6849f9b1c14.png)
Features of the clusters

* Cluster 1 — Young single switcher with low value: the monthly charge, total charge, and tenure for this cluster are low, likely because they are not with the company for a long time and they are on low-tier plans. They are young and single with a moderate churn rate.
* Cluster 2 — Affluent loyal family with high value: this cluster stays with the company for a long time, with a high monthly charge and moderate total charge.
* Cluster 3 — Over-priced seniors: these clusters’ monthly charges are higher than total charges in proportion, implying they are now over-charged. They are in general senior and live only with their partners.

## Modeling
* Model 1Model 2: Model 3: log_odds(churn=1) ~ demographic_data + account_data + subscription_data log_odds(churn=1) ~ demographic_data + account_data : log_odds(churn=1) ~ demographic_data

We used logistic regression to predict the churn rate of customers. We chose logistic regression rather than linear regression because our dependent Y value is binary (0 or 1).

### In-sample evaluation

![image](https://user-images.githubusercontent.com/98763622/162605148-bb72d33d-c060-45c3-8fc6-2bcc9c82d728.png)
Exhibit2: In sample ROC score using SAS

With logistic regression, we built 3 models and compared one another. For the first model, we used only customer demographic data to predict churn rate and AUC(see Exhibit 2 in Appendix) was 63% which seems not good, and “-2logL” did not have much difference with the Null model. Therefore, we added customer account data when building model 2 and it was meaningful. AUC increased to 82% and other measurements such as “-2logL ‘’ and “Likelihood Ratio” improved a lot as well.

Finally, in model 3 we also included customer subscription data (what customer has signed up for) when building the model, and model 3 got the best performance. Model 3 got 84% in AUC and also -2logL ‘’ and “Likelihood Ratio” improved when compared to model 2(using customer demographic data and customer account data).

### Out-of-sample evaluation

![image](https://user-images.githubusercontent.com/98763622/162605172-e8d7efbd-7761-4780-9588-4c235f426612.png)
Exhibit 3 — Confusion Matrix of Out of Sample Evaluation

We also used out-of-sample evaluation to compare models. We separated our data to train set(0.7) and test set(0.3) to perform out-of-sample evaluation. For the threshold, we used 0.5 to do a general evaluation. The result(see Exhibit 3 in Appendix) is interesting, model 1(model with only customer demographic data) predicts every customer will not churn. Precision(=TP/TP+FN) was 0. It means that model 1 is useless and similar to the Null model. In contrast, Model 3 (model made with demographic, account, and subscription data) got a precision score of 68% and accuracy was 89%. Here, we could prove that model 3 makes a good performance out of sample as well.

![image](https://user-images.githubusercontent.com/98763622/162605182-97e42045-6e75-4a63-90ae-54a9449c584b.png)

### Evaluation Results
From supervised analysis using logistic regression, we learned that we need to use customer account data and subscription data to predict churn rate well. With basic customer demographic data, we could not predict churn rate.

## Customer Segmentation

![image](https://user-images.githubusercontent.com/98763622/162605235-df665344-e934-4772-8a3d-7d8fa4cb212f.png)
Exhibit 4 — Mean Features of The Clusters

For segmentation, we looked at other mean characteristics of these clusters (see Exhibit 4 in Appendix) to see what we can do for each segment.

* Young single switcher with low values: nurture and increase the customer value. Their proportion of using fiber optic, online security ad-streaming TV is low, so we would suggest targeted promotions to increase their spending on these services.
* Loyal young families: for this cluster, no action is needed. We can keep existing policies and monitor their changes.
* Over-priced seniors: provide premium service to retain. For example, as their usage of technical support is low but considering their age, some dedicated customer support channels for the seniors could be developed to help them make the best out of the service provided.

## Logistic Regression

For logistic regression, based on the process of coming down to our best model, we can conclude that the model performance increases with the variables input. In the long term, we suggest the company build up data collection procedures to enable accurate insights from the model. Based on the results of our final models, we spot some issues that can be addressed to decrease the churn rate. For senior citizens, the beta for senior citizens is positive, indicating an increase in age corresponds to an increase in churn rate. This might happen when the senior dies. To maintain sustainable business growth, we would suggest the company acquire a younger segment and nurture its customer values. For contracts, the beta for contract_two_year is more negative than that for contract_one_year, indicating a longer contract corresponds to a lower churn rate. We suggest the company repackage the service plans to longer contract periods. For technical support, the beta for techsupport_yes is positive, indicating using technical support increases the odds of churn. Enhancing the technical support will benefit the churn-saving in the long run. Measures such as training for technical support staff and increasing the flexibility of technical support through video diagnostic can be applied.

Regarding data limitation, firstly, we don’t know which company or which market this data is collected from, but customer and service characteristics vary a lot from company to company, and this would create problems for generalization. Second, we prefer more information on geographic location, as service rollout of telecom is highly dependent on locations.As for limitations for methodology, for clustering, we could try more combinations of variables. For logistic regression, we could apply regularization to eliminate variables, and correlation doesn’t imply causation so causal relationship needs more investigation.
