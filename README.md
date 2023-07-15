# Project Title

Classifier Comparsion for Portugese Bank Marketing Data

## Description

The dataset is from a Portuguese banking institution and it contains a collection of the results from multiple marketing campaigns conducted to drive customer to signup for a deposit program. We conducted analysis and noted the observation on the performance of the classifiers (k-nearest neighbors, logistic regression, decision trees, and support vector machines).


### Process

* Data is reviewed and checked for duplicats and NaN values
* Perform Feature Engineering:	
	* Review Data Type and Shape
	* Check for dataset inbalance on for target
	*  Mapping Target Output to Numeric Value of 1 and 0	
	* Category Transformer for Catagory and Numberic data using One-hot encoder as Preprocessing
	* Modify data field to data that have relationship to Target based on hypothesis
	* Drop Irrvalent Features
	* Build Pipeline and Framework for each model to train and measure fit time

### Obersevation

Based on the target of the data, denoted as y, it represents whether a customer signup for the service. There are only 2 classes: Yes (Signup) or No (Didn't signup). This is a binary classification problem. Four models are developed and compared to evaluate which one is the best to predict whether a customer will sign up for the service.

Imbalanced Dataset

Based on the the count of the target value, the dataset is considered as imbalanced. Upsampling or downsampling techiques can be applied to the data such as SMOTE to augement the minority class.

Evaluation

* Following parameters are calcuated:
- Trainig Score
- Testing Score
- Accuracy
- Precision
- Recall
- f1
- Fit Time (Avg)
- ROC Curve

By reviewing the score of the 4 models without hypertuning to compare these models with default parameters.

On Score, it is meant to return the mean accuracy on the given test data and labels. Logistic Regression has the highest score value. However on the average fit time, Decision Tree provide the fastest execution of the fitting process.

                 model  test score  training score  average fit time
0  Logistic Regression    0.894620        0.904551          0.055512
1                  SVM    0.892410        0.909924          0.183259
2                  kNN    0.882830        0.938369          0.032278
3        Decision Tree    0.865881        1.000000          0.021088

On the Accuracy, Rcall and Precision, since the problem is imbalanced, it is prefered to use f1 score to evaluate the dataset instead of accuracy. The reason is F1 score also known as F-measure or balanced F-score is an error metric which measures model performance by calculating the harmonic mean of precision and recall for the minority positive class. F1 score can be interpreted as a measure of overall model performance from 0 to 1, where 1 is the best. To be more specific, F1 score can be interpreted as the model’s balanced ability to both capture positive cases (recall) and be accurate with the cases it does capture (precision).

                 model  Accuracy  Precision    Recall        f1
0  Logistic Regression  0.894620   0.646341  0.317365  0.425703
1                  SVM  0.892410   0.666667  0.251497  0.365217
2                  kNN  0.882830   0.532258  0.395210  0.453608
3        Decision Tree  0.866618   0.458333  0.461078  0.459701

Based on the calculation, Decision Tree has the highest f1 value at 0.459701. Referring to the scale of 0 to 1, It is below 0.5, which tell us that the model is not good.

Lastly, ROC Curve is calculate to visualize the seneitivity and specificity of the model. The more that the ROC curve hugs the top left corner of the plot, the better the model does at classifying the data into categories. Logistic Regression has the best performing model based on ROC. 

roc_auc_score for Logistic Reg:  0.8721067658349329
roc_auc_score for Decision Tree: 0.9021276391554703
roc_auc_score for SVM:           0.8750515834932822
roc_auc_score for kNN:           0.9150323896353166

Explanation on Approach related to feature engineering. It is recommended to perform 2 model first on Personal and Marketing data, this will give you optimization on the user demographics and how the campaign impact one's decison on term deposit. The second model can layer in the macro economic data to see how external factor outside of marketing campaign impact their decision.

Base Data:
 	1) Personal Data + Business Data + Marketing Data
Extended Data:
	2) Personal Data + Business Data + Marketing Data + Economic Data

Top Features by Weight for Base
					feature	coef	abs_coef
19	cat__contact_unknown		-0.661074	0.661074
10	cat__loan_yes			-0.437374	0.437374
9	cat__loan_no			0.436722	0.436722
1	num__campaign			-0.395385	0.395385
17	cat__contact_cellular		0.366603	0.366603
15	cat__education_university	0.321913	0.321913
18	cat__contact_telephone		0.293819	0.293819
8	cat__housing_yes		-0.280314	0.280314
7	cat__housing_no			0.279662	0.279662
16	cat__education_unknown		-0.250098	0.250098

Top Features by Weight for Extended
				feature	coef	abs_coef
5	num__emp.var.rate		-1.095149	1.095149
6	num__cons.price.idx		0.704451	0.704451
28	cat__contact_telephone		-0.531053	0.531053
23	cat__education_illiterate	0.386014	0.386014
27	cat__contact_cellular		0.353962	0.353962
2	num__pdays			-0.345941	0.345941
30	cat__day_of_week_mon		-0.217874	0.217874
7	num__cons.conf.idx		0.213368	0.213368
21	cat__education_basic.9y		-0.205894	0.205894
17	cat__default_unknown		-0.176847	0.17684

It is interesting to see that with economic data and a larger dataset. Marco economic data such as CPI and employment rate become the top 2 coefficient. Also, it shows that Monday perform poorly in terms of conversion for marketing campaign. In the base model, personal info such as laon and housing influences the marketing campaign outcome where customer with no loand and no housing tend to sign up for term deposit. This make senses as those customer likely to have more cash as oppose to paying for motrage or loan payment. There are also commonity across 2 models such as cellular outreach are perferred method. 

Also # of campaigns to the customer will have lower probability of a customer signup. This may be due to marketing fatigue as this suggest company may have to experiment with new channels.
Marketing data is mandatory as it is core to the model. 

Reference:

Here are the explanation of the key coefficient used in the model. 

Personal
   1 - age (numeric)
Business
   5 - default: has credit in default? (categorical: "no","yes","unknown")
   6 - housing: has housing loan? (categorical: "no","yes","unknown")
   7 - loan: has personal loan? (categorical: "no","yes","unknown")
Marketing
   8 - contact: contact communication type (categorical: "cellular","telephone") 
   9 - month: last contact month of year (categorical: "jan", "feb", "mar", ..., "nov", "dec")
  10 - day_of_week: last contact day of the week (categorical: "mon","tue","wed","thu","fri")
  13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)
  15 - poutcome: outcome of the previous marketing campaign (categorical: "failure","nonexistent","success")
   # social and economic context attributes
Economic
  16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)
  17 - cons.price.idx: consumer price index - monthly indicator (numeric)     
  18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)     
  19 - euribor3m: euribor 3 month rate - daily indicator (numeric)
