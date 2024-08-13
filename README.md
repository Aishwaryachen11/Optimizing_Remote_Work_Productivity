## **OPTIMIZING REMOTE WORK PRODUCTIVITY: INSIGHTS FROM POST-PANDEMIC WORKFORCE ANALYSIS**

### **1. Introduction:** 
The COVID-19 pandemic drastically altered the landscape of work, forcing many organizations to shift from traditional office settings to remote work environments. This sudden transition has prompted both employees and employers to reassess the effectiveness and productivity of remote work. To better understand these dynamics, a survey was conducted among 1,500 remote workers in New South Wales, Australia, across various industries. The survey aimed to capture the shifts in remote work experiences and attitudes during different stages of the pandemic and to explore the long-term implications of remote work.The focus of this analysis is to provide insights for organizations seeking to implement remote work policies that maximize productivity while aligning with the demand for virtual and hybrid work environments. By identifying the factors that most significantly impact worker productivity, organizational stakeholders can make informed decisions about who should be considered for remote work and the level of flexibility that should be offered.

### **2. About the Data:**
The dataset used for this analysis was accessed via Kaggle’s dataset library, which contained a post of the dataset sourced from Maven Analytics (https://www.kaggle.com/datasets/melodyyiphoiching/remote-working-survey). This data was collected by the New South Wales (NSW) Productivity Commission, a government task force in Australia tasked with identifying opportunities to boost productivity, through a series of “Remote Working Insight” surveys in 2020 and 2021. Each of the two surveys were completed through interviews with 1,500 employed workers in the New South Wales area who were identified as “remoteable” (defined as “having experience of remote working in their current job”) across a variety of industries and occupations. The reason for selecting this dataset because it provided insights into a wide range of variables related to remote work - including employee attitudes and preferences, organizational characteristics and policies, and breakdown of time allocated to tasks - as well as standard productivity measures. As described in the NSW summary, the survey questions asked participants about the following topics:
Their attitudes to remote working
The amount of time they spent working remotely
Their employers’ policies, practices, and attitudes
How they spent their time when working remotely
How barriers to remote working have changed
The barriers they faced to hybrid working
Their expectations for the future remote working *Source: 2021 Remote Working Survey Appendix A

**Preparing the Data for Analysis:**
Merging of the data sets Our first step in preparing the data for analysis was merging the datasets collected during 2020 and 2021 into one file with similar columns. 

**Cleaning the Data:**

Use this link to access with the notebook in Google Colab : [Open Colab Notebook](https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity)

Below is the summary of steps followed for data cleaning and merging:

**Transforming Column Headers:** Replaced spaces, question marks, and colons with underscores for consistency.
Added a "responder_id" column for unique identification across datasets.
Simplified column names to avoid issues in code execution.

**Resolving Null Values:** Printed the number of null values per column to assess their impact.
Replaced nulls in categorical columns with "No response" for pattern analysis.
Replaced nulls in numerical columns with the median to mitigate outliers.
Identifying and Replacing Outliers Used boxplots and print statements to detect outliers, particularly in open-ended responses.
Corrected values exceeding 24 hours by identifying and replacing extreme outliers.
Adjusted outliers by replacing values beyond three standard deviations with the median.

**Bucketing Categorical Variables:** Encoded categorical variables into numeric values for machine learning.
Grouped less frequent categories into "other" for simplicity.
Reframed response options from percentages and text to a standard scale based on workdays.

**Adding Engineered Features:** Created a "commute_time_difference" field to analyze changes in commute time.
Generated calculated fields to enhance analysis of time redistribution among remote workers.
Planned for additional features as the analysis evolves.

### **FEATURE IMPORTANCE ANALYSIS USING MACHINE LEARNING MODELS:**

Machine learning models like Random Forest and Easy Ensemble AdaBoost are implemented to help us determine which features were closest tied to worker productivity.

**Why Feature Importance is Performed:**
Feature importance analysis is a critical step in understanding the underlying factors that drive outcomes in predictive models. In this context, feature importance helps us identify which variables (features) have the greatest influence on the productivity of remote workers compared to their in-person office counterparts. By ranking these features in order of their impact, we can:

Prioritize Influential Factors: Focus on the variables that matter most, allowing organizations to address key areas that will have the greatest effect on productivity.

Tailor Remote Work Policies: Develop more effective and targeted remote work policies that consider the most significant factors, such as age, work-life balance, and commuting time.

Enhance Model Interpretability: Understanding feature importance makes the predictive model more interpretable, which is crucial for gaining stakeholder trust and making actionable recommendations.

Optimize Resource Allocation: By knowing which features are most important, organizations can allocate resources more effectively, focusing on the areas that will yield the highest returns in terms of employee productivity and satisfaction.


### **IMPLEMENTATION**


Implementing the ensemble algorithms, such as the Balanced Random Forest Classifier and Easy Ensemble AdaBoost classifier, to identify which features are most important in predicting productivity_remote_vs_office.

**Data Preparation:**

Label Encoding: The target variable productivity_remote_vs_office is encoded into numeric form for model training.
Feature Preparation: Features are selected from the dataset, excluding unnecessary columns. Categorical features are converted to dummy variables for compatibility with machine learning models.
Train-Test Split: The data is split into training and testing sets to allow for model evaluation on unseen data.

Reasons for Excluding Specific Columns:

birth_year:
This column represents the year of birth of the respondents. While the age of respondents could be an important feature, the raw birth year is often not as informative as the derived age. Additionally, including birth year directly might require additional feature engineering (e.g., calculating age) to be meaningful.
To avoid directly using an unprocessed year as a feature, it’s excluded. However, if age calculation was intended, it would have been included as a derived feature.

responder_id:
This is an identifier for each respondent, which is usually a unique value with no predictive power regarding the target variable. Including it in the model would not contribute to the model's predictive ability and could potentially introduce noise.
Purpose of This Step:
Feature Selection: By dropping these columns, the code ensures that only relevant features are used in training the machine learning models. The columns that are removed are either target variables, identifiers, or raw values that require further processing.
Model Integrity: It helps maintain the integrity of the model training process by ensuring that the model does not inadvertently use information that it should not have access to during training.

**Model Training:**
Train a Balanced Random Forest Classifier to handle any class imbalance in the target variable.
Train an Easy Ensemble AdaBoost classifier for robust feature importance identification.

**Model Evaluation:**
Predictions: Both models predict the target variable on the test dataset.
Classification Reports: The performance of each model is evaluated using classification reports, which provide metrics such as precision, recall, and F1-score for each class.
Algorithm Differentiation: A column is added to the classification report DataFrames to indicate which model produced each set of results.
Combined Reports: The reports from both classifiers are combined into a single DataFrame for easy comparison

Here are the results from the Random Forest and AdaBoost classifiers:

Classification Report - Random Forest
Overall Accuracy: 39%
Weighted F1-Score: 35%
Precision and Recall: The model has varying precision and recall across different classes, with better performance for the class representing "same productivity" compared to other classes.
Classification Report - AdaBoost
Overall Accuracy: 39%
Weighted F1-Score: 37%
Precision and Recall: Similar to the Random Forest, with slightly better performance in certain classes but still quite variable.
Observations:
Both models show moderate performance, which may be due to the complexity and variability in the target variable. Feature importance can help identify which factors are driving these predictions, even if the models themselves aren't highly accurate.

**Feature Importance Analysis:**
Extract and visualize feature importance from both models.
Here are the features sorted in descending order by their importance for each algorithm:

**Insights:**
**Balanced Random Forest Top Features:**

Age: The most important factor, indicating that different age groups may experience varying productivity levels when working remotely versus in the office.
Remote Hours Working: Significant time spent working remotely correlates with changes in productivity, suggesting that how employees manage their time remotely is crucial.
Remote Hours Personal Family Time: Balancing work with personal and family time is a key factor in productivity, highlighting the importance of work-life balance in remote work settings.
In-person Hours Commuting: Commuting time impacts productivity, possibly due to the stress and time lost during commutes, which is alleviated in remote work scenarios.
In-person Hours Personal Family Time: Just as with remote work, balancing personal and family time while working in-person is crucial for maintaining productivity.

**Easy Ensemble AdaBoost Top Features:**

Remote Hours Working: Similarly identified as a critical factor, emphasizing its role in determining productivity in both models.
Age: Again, age emerges as a critical factor, with older or younger workers potentially experiencing different challenges or advantages in remote work.
Remote Hours Personal Family Time: Maintaining a balance between work and family during remote hours is critical, affecting productivity levels.
In-person Hours Commuting: Reflects the impact of commuting on productivity, suggesting that reducing commute times can lead to better work performance.
In-person Hours Working: The time spent working in the office versus remotely also plays a crucial role in determining productivity.

**Recommendations:**
Personalized Remote Work Policies: Organizations should consider age as a factor when designing remote work policies. Different age groups may require different levels of support and flexibility to maximize productivity.

Focus on Work-Life Balance: Encourage and facilitate employees to maintain a healthy balance between work and personal/family time, especially during remote work. This can be achieved through flexible working hours, mental health support, and promoting a culture that values work-life balance.

Reduce Commuting Stress: For roles that require in-person work, consider measures to reduce commuting stress, such as flexible start times, remote work days, or providing transportation benefits.

Monitor and Optimize Remote Working Hours: Regularly review how remote working hours are utilized and provide guidance or tools to help employees manage their time effectively. This could include time management workshops, productivity tools, or regular check-ins to ensure employees are not overworking or struggling with time management.

**Conclusion:**
Both the Balanced Random Forest and Easy Ensemble AdaBoost algorithms highlight similar features as key drivers of productivity differences between remote and in-person work. Age, remote working hours, and the balance between work and personal time are consistently important. These insights suggest that while remote work can offer significant benefits, its success largely depends on how well employees manage their time and balance their work and personal responsibilities. Organizations should tailor their remote work policies to these factors to maximize productivity and employee satisfaction.
