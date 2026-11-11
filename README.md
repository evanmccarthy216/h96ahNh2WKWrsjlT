# h96ahNh2WKWrsjlT
**Customer Satisfaction Prediction**

Predicting whether a customer is happy or unhappy based on their answers to a short delivery survey, and finding which survey questions actually matter.

**Overview**
A delivery company surveyed a group of customers with six questions, each answered on a scale of 1 (less) to 5 (more). Each customer was also labeled as happy (1) or unhappy (0).

**The goals of this project was to:**
Build a model that predicts customer happiness from survey answers
Find the smallest set of questions that still predicts happiness well
Recommend which questions could be removed from future surveys
Data

126 survey responses, no missing values.

Column	Description
Y	Target: 0 = unhappy, 1 = happy
X1	My order was delivered on time
X2	Contents of my order was as I expected
X3	I ordered everything I wanted to order
X4	I paid a good price for my order
X5	I am satisfied with my courier
X6	The app makes ordering easy for me

The classes are fairly balanced: 69 happy (54.8%) and 57 unhappy (45.2%).

**Approach**
Exploratory analysis. I looked at the happy rate at each answer level for every question, plus a correlation heatmap. X1 and X5 showed the clearest relationship with happiness, X3 and X6 showed a moderate one, and X2 and X4 showed little to none.

Evaluation method. With only 126 rows, a single train/test split is unreliable (one customer changes the test accuracy by about 4 points). Every model was evaluated with repeated stratified 5-fold cross-validation (5 folds x 10 repeats = 50 test scores), and I report the mean and standard deviation.

Model comparison. I tested a majority-class baseline, logistic regression, decision tree, random forest, gradient boosting, SVM, naive Bayes, and KNN on all six questions.

Feature selection. Since there are only six questions, I tested all 63 possible combinations with three models instead of guessing which to drop.

Tuning. I tuned decision trees, random forests, and KNN on the best feature sets with grid search.

**Results**
Model	Features	CV accuracy
Baseline (always guess happy)	none	54.8%
Best model using all features (Random Forest)	all 6	58.4% ± 8.1%
Decision Tree (depth 2)	X1, X5	66.6% ± 8.0%
Decision Tree (depth 3)	X1, X5	67.9% ± 8.8%
KNN (K = 7, Manhattan distance)	X1, X3, X5, X6	68.3% ± 9.4%

**Key findings:**
Fewer questions gave better results. Removing the weak questions raised accuracy by about 10 points. KNN went from 56% with all six questions to 68% after dropping X2 and X4.
X1 (on-time delivery) is the most important question. It appeared in all 15 of the top-performing feature subsets.
Two very different models (decision tree and KNN) landed at about 67 to 68%, which suggests that is close to the limit of what this dataset supports.
About the 73% target

Accuracy on individual test folds ranged from 50% to 84%, and about 32% of folds scored 73% or higher. A model evaluated on one lucky split could easily report 73%+, but the 50-fold average of about 67 to 68% is a more honest estimate of how the model will perform on new data.

**Final model**
I chose a depth-2 decision tree using X1 and X5. It performs within noise of the more complex models and reduces to one simple rule:

A customer is predicted happy if they rated on-time delivery 5 and courier satisfaction 4 or higher.

This makes the model easy for an operations team to understand and act on without any code.

**Survey Recommendations**
Keep X1 and X5. They carry most of the predictive power.
Remove X2 (contents as expected). It showed no relationship with happiness or with any other question.
Remove X4 (price). It had almost no relationship with happiness.
X3 and X6 are optional. They added small, inconsistent gains. They could be kept for operational insight but aren't needed for prediction.
Business Takeaway

Customer happiness depends mostly on the delivery experience. Customers are happy when their order arrives fully on time and the courier interaction goes well. Price and order contents matter much less. Improving delivery timeliness and courier quality is likely the most effective way to raise satisfaction.

**Next Steps**
Collect more survey responses. The small sample is the biggest limit on how reliably any model can be evaluated.
Use nested cross-validation to get an unbiased estimate of the full feature selection and tuning process.

**How to Run**
The notebook was built in Google Colab. The easiest way to run it is to open it in Colab and run all cells. The first cell downloads the dataset automatically.

**Tools**
Python, pandas, NumPy, scikit-learn, Matplotlib, seaborn, Google Colab
