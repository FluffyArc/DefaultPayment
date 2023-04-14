# **Default Payment**
## Problem Statement
In 2001, 2022, and 2005, Taiwan was one of the countries that experienced a debt crisis (Chang, 2022). One of the causes is the lax supervision from the bank regarding the unstructured reporting system. **Therefore, the default payment precaution by the customers should be avoided by predicting their status based on their historical transactions**

**Reference:**<br>
Chang, Chih-Hsiung.2022. Information Asymmetry and Card Debt Crisis in Taiwan. Bulletin of Applied Economics. vol 9, no 2: pg: 123-145

## Goals
To prevent the potential defaulters.

# Exploratory Data Analysis (EDA)
Based on the ```df.info()``` command, every feature had **no missing value**. And there are **21.000** data with the following description:

<table>
    <tr>
        <td>ID</td>
        <td>ID of each client</td>
    </tr>
    <tr>
        <td>LIMIT_BAL</td>
        <td>Amount of given credit in NT dollars (includes individual and family/supplementary credit)</td>
    </tr>
    <tr>
        <td>SEX</td>
        <td>Gender (1=male, 2=female)</td>
    </tr>
    <tr>
        <td>EDUCATION</td>
        <td>(1=graduate school, 2=university, 3=high school, 4=others, 5=unknown, 6=unknown)</td>
    </tr>
    <tr>
        <td>MARRIAGE</td>
        <td>Marital status (1=married, 2=single, 3=others)</td>
    </tr>
    <tr>
        <td>AGE</td>
        <td>Age in years</td>
    </tr>
    <tr>
        <td>PAY_0</td>
        <td>Repayment status in September, 2005 (-2=no consumption, -1=pay duly, 0=the use of revolving credit, 1=payment delay for one month, 2=payment delay for two months, … 8=payment delay for eight months, 9=payment delay for nine months and above)</td>
    </tr>
    <tr>
        <td>PAY_2</td>
        <td>Repayment status in August, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_3</td>
        <td>Repayment status in July, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_4</td>
        <td>Repayment status in June, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_5</td>
        <td>Repayment status in May, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_6</td>
        <td>Repayment status in April, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>BILL_AMT1</td>
        <td>Amount of bill statement in September, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT2</td>
        <td>Amount of bill statement in August, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT3</td>
        <td>Amount of bill statement in July, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT4</td>
        <td>Amount of bill statement in June, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT5</td>
        <td>Amount of bill statement in May, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT6</td>
        <td>Amount of bill statement in April, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT1</td>
        <td>Amount of previous payment in September, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT2</td>
        <td>Amount of previous payment in August, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT3</td>
        <td>Amount of previous payment in July, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT4</td>
        <td>Amount of previous payment in June, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT5</td>
        <td>Amount of previous payment in May, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT6</td>
        <td>Amount of previous payment in April, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>default.payment.next.month</td>
        <td>Default payment (1=yes, 0=no)</td>
    </tr>
</table>

Furthermore, it also discovered that all the categorical data had been encoded. The result of the ```df['Column'].value_counts()``` resulting the following image below:

Selain itu, dari proses EDA ini juga ditemukan bahwa semua data kategorik sudah di **encode**. Dengan menjalankan fungsi ```df['Column'].value_counts()``` didapatkan hasil sebagai berikut:<br>
<img width="173" alt="value_counts" src="https://user-images.githubusercontent.com/40890491/231958560-aa657e51-2d2b-4aac-90fd-439662deaa44.png">


As for the SEX feature, all the values are corresponding with the available category. However, there are some values on EDUCATION and MARRIAGE features that are not corresponding with the available category (Ex: 0 in MARRIAGE and EDUCATION), 

## Univariate Analysis
Univariate analysis was conducted by understanding all the features deeper. For all the numerical data, the understanding process was done using the boxplot with the following code ```sns.boxplot(y=df[account[i]], color='blue', orient='v')```

![box_plot](https://user-images.githubusercontent.com/40890491/225930017-7601be80-887f-4501-9830-606b3445679b.png)

There are outliers indications for all the **BILL_AMT and PAY_AMT** features. The Z-score calculation will be used in the next section (Data Preprocessing) to remove all the outliers. In addition, the distribution of all numerical features can be seen in the following figure (using code ```sns.kdeplot(x=df[account[i]], color='green')```).

![kde_plot](https://user-images.githubusercontent.com/40890491/225931736-fa2aafc7-4e5d-46e1-8e32-d04255ccd1c2.png)

Based on the distribution plot above, it can be seen that **there is no feature with normal distribution on it**.<br>

As for the categorical features, using the ```sns.countplot(x=df[categoricals_demography[i]], data=df, color='green')``` code will help understanding the dataset

![count_plot](https://user-images.githubusercontent.com/40890491/225932589-a1d6ac27-1907-4292-bece-32176c22b080.png)
![count_plot_2](https://user-images.githubusercontent.com/40890491/225932759-286bbff4-645b-469d-b935-9000506eb87b.png)
![count_plot_3](https://user-images.githubusercontent.com/40890491/225932920-13ebccc3-afcb-4d66-b5fc-9d68b608a264.png)
![count_plot_4](https://user-images.githubusercontent.com/40890491/225932927-386b5201-681a-460c-9f11-c59074f2242e.png)

There are some insights retrieved from the barplot above:
<li>Most of the customers are Female and less than 30 years old</li>
<li>Most of the customers are coming from the undergraduate background of education</li>
<li>The marital status from most of the customers is single</li>
<li>Based on the historical data given, most of the customers already paid their bills duly</li>

<br>
The same thing also applied for the <b>PAY_N</b> features by running the Hal yang sama juga dilakukan untuk fitur <i>categorical</i> <b>PAY_N</b> dengan menggunakan code berikut ```sns.countplot(x=df[categoricals_payment[i]], color='green', orient='h')``` code

![count_plot_pay](https://user-images.githubusercontent.com/40890491/225933844-fbf86d5a-3c64-4d57-998d-dbb185c12ec9.png)

Based on the PAY_N barplot, the PAY_N features have a skewed distribution, and most of the customers use the revolving credit (code 0)


## Multivariate Analysis

The heatmap visualization is used to identify the correlation between **feature to feature** and **feature to target**

![heat_map](https://user-images.githubusercontent.com/40890491/225934997-aa414673-2e93-4a84-b6f0-1f279e8c8f54.png)

The target feature (default_payment_next_month) positively correlates with all the PAY_N features. **It means the longer the customers delay their payment, the more likely they will default for the next month**

On the other hand, the target feature negatively correlates with the LIMIT_BAL feature. **It means the higher the customer's limit, the more unlikely they will default for the next month**

# Data Preprocessing
## Handling Duplicate Data

After running the ```df['ID'].duplicated().sum()``` code, there is **no duplicate data found** on the dataset.

## One-hot Encoding
One-hot encoding is performed to a categorical feature with no order on it such as the **EDUCATION, MARRIAGE, and SEX** feature by using the ```pd.get_dummies()``` function.

## Feature Extraction
The feature extraction was performed to extract some new features based on the existing features with a high correlation with the target to boost the model's performance. There are **three new features** created in this step:
1. ```is_default_ever```, provide the information whether the customer was recorded default ever.
2. ```is_never_default```, is the opposite of ```is_default_ever```. It gives the information of whether the customers have no default record.
3. ```is_default_in_last_three_months```, tells whether the customer has defaulted in the latest three months consecutively.

## Data Transformation
Using the ```StandardScaler``` function, we transform all the features so that each has the same range of values.

## Data Split (80/20)
Using the library of ```train_test_split```, we split the data into two parts, **Train data (80%) and Test data (20%)**

## Handling Outliers
As mentioned above, the Z-Score method is used to remove all the outliers because the model that will be used is not robust to the outliers' value.

## Handling Imbalance Class
Because we have an imbalance target, a method such as **SMOTE** is required to increase the number of samples (oversampling).

# Data Model Analysis
The purpose of this step is to determine which of the Classification model is the best for this problem. We evaluate four models with the good score of **recall and accuracy**
![download](https://user-images.githubusercontent.com/40890491/231978041-dd8c344d-210b-41a4-bd38-0b6f6933f208.png)

From DMA Charts above, it can be seen that **even though Logistic Regression recall's score is below Naive Bayes, the accuracy of Logistic Regression is still better**. Therefore, the Logistic Regression will be used in this case.

![download (1)](https://user-images.githubusercontent.com/40890491/231978339-b70ca678-6420-4b10-8738-cd1f405b7609.png)

The precision-recall curve above tells that by giving the model threshold of 0.5, the **Logistic Regression Model is still giving the best output of recall** compared to the Naive Bayes and Random Forest model.

![download(2)](https://user-images.githubusercontent.com/40890491/231978543-2361a3b2-fa45-4522-b6f9-07aba0c917b9.png)

Specifically for Logistic Regression, by giving the threshold of 0.5, **the model can give a score above 0.6 (60%) with the accuracy score of 0.71 (71%)**

# Fitting Model - Logistic Regression
First of all, we try to fit the model by using ```LogisticRegression()``` function without any hyperparameters.

![lr_without_hyperparameters](https://user-images.githubusercontent.com/40890491/231979060-7a717398-57ef-49e6-b218-6e2698bee4f8.png)

The confusion matrix above describes using the Logistic Regression without any hyperparameters resulting in a **recall score of 68%**and the **accuracy score of 71%** with a threshold of 0.5. It means the model can identify 68% correct positive value from the actual observations.

By giving the model some hyperparameters, even though the accuracy **slightly decreases to 0.70**,  the recall score is also **increased slightly to 0.70**, which provides a good score compared to using any hyperparameters (the result can be seen in the following figur below).

![lr_with_hyperparameters](https://user-images.githubusercontent.com/40890491/231979638-2499bc4f-c64f-4861-a218-f034b2a5caf9.png)

# Gain and Lift Analysis
By using the library from ```kds```, we evaluate the model's performance by looking at the Gain and Lift charts. The code ```kds.metrics.report(y_test, prob_glm[:,1])``` is used to generate the desire report.

![gain_lift_report](https://user-images.githubusercontent.com/40890491/231980252-a27a90f9-f538-4387-ad13-8283d9681832.png)

**Gain Analysis**<br>
Based on the Gain Plot, the model can predict 70% of customers with a high probability of becoming defaulters at 40% of the total customers on the whole observation. <br>

**Lift Analysis**<br>
The Lift Plot above informs that by targeting 40% of customers with a high probability of becoming defaulters, the model can predict 1.75 times better than selecting 40% of random customers. <br>

**Conclusion**<br>
As for both **Gain** and **Lift** plots already above the random line, business-wise, the model can well predict the potential defaulters by using fewer resources compared to predicting defaulters randomly.