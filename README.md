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

Use this link to access with the notebook in Google Colab : [Open Colab Notebook](https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Merging_%26_Data_Cleaning.ipynb)

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

Use this link to access with the notebook in Google Colab : [Open Colab Notebook](https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Merging_%26_Data_Cleaning.ipynb)

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

**Classification Report - Random Forest:**
Overall Accuracy: 32%
Weighted F1-Score: 33.68%
Precision and Recall: The model exhibits varying precision and recall across different classes, with notably better performance for the class representing "same productivity" (Class 3). However, the precision and recall for other classes, such as those representing lower or higher productivity levels, are considerably lower, indicating that the model struggles to differentiate between these classes effectively.

**Classification Report - AdaBoost:**
Overall Accuracy: 32%
Weighted F1-Score: 33.83%
Precision and Recall: Similar to the Random Forest, the AdaBoost model shows variability in precision and recall across different classes. While it performs slightly better in some cases compared to the Random Forest, particularly for the class representing "same productivity" (Class 3), it still faces challenges in accurately predicting the other classes.

**Observations:**
Both models demonstrate moderate performance, which could be attributed to the complexity and variability inherent in the target variable. The relatively low accuracy and F1-scores suggest that the models may have difficulty distinguishing between different productivity levels.
Despite these challenges, the models can still offer valuable insights, particularly when combined with feature importance analysis. Identifying the most influential factors driving the predictions could help improve model performance and provide actionable recommendations for optimizing remote work productivity policies.

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

### **Exploratory Data Analysis**

Use this link to access with the notebook in Google Colab : [Open Colab Notebook](https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Exploratory_Data_Analysis_.ipynb)

**Distribution of Respondents by Productivity (Pie Chart):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Distribution%20of%20Repondents%20by%20Productivity.png" alt="Description" width="400"/>

Insight: The majority of respondents (37.2%) feel "much more productive" when working remotely, followed by 25.4% who report "same productivity." Only a small fraction (4.9%) feel "much less productive."
Recommendation: Organizations should promote remote work as it appears to enhance productivity for a significant portion of the workforce. However, it's also important to identify and support those who struggle with remote work, offering them resources or the option to work in-person if needed.

**Best Hybrid Configurations:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Best%20Hybird%20Configuration.png" alt="Description" width="400"/>

Insight: The bar chart displays the average hours worked in-person, remotely, and the total working hours, categorized by the number of actual remote days. Employees working 0-1 remote days have the highest total working hours, indicating a potential overwork scenario when they primarily work in-person. However, as remote days increase, total working hours decrease slightly, suggesting a better work-life balance.
Recommendation: Organizations should consider promoting hybrid work arrangements with 2-3 remote days per week. This setup seems to offer a balance between in-person and remote work while maintaining sustainable total working hours. Additionally, monitoring work hours for those with fewer remote days can prevent overwork and burnout.

**AGE AND GENDER ANALYSIS**

**Respondents by Age Group and Gender:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Best%20Hybird%20Configuration.png" alt="Description" width="400"/>

Insight: The stacked bar chart shows the gender distribution across different age groups. Across most age groups, there is a higher percentage of male respondents compared to female respondents. The gender gap is narrower in the younger age groups (20-35), with a more balanced distribution, but it widens significantly in the older age groups (50-65), where male respondents dominate.
Recommendation: Organizations should consider implementing targeted gender diversity initiatives, particularly focusing on the older age groups where there is a noticeable gender imbalance. Additionally, mentoring programs could be beneficial for younger females to support career progression and leadership opportunities.

**Household Distribution (Bar Chart):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Household%20Distribution.png" alt="Description" width="400"/>

Insight: The chart shows that most respondents live in households with children, particularly "couple with dependent children" and "couple with no dependent children," followed by single-person households.
Recommendation: Remote work policies should consider household composition, offering flexibility for parents who may need to manage childcare alongside work. Additionally, single-person households might benefit from social interaction initiatives to prevent feelings of isolation during remote work.

**Generation Distribution:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Generation.png" alt="Description" width="400"/>

Insight: The pie chart reveals that nearly half of the respondents (48.2%) are from Generation X, followed by Millennials at 30.3%, Boomers+ at 19.9%, and a small percentage (1.6%) from Gen Z. This distribution indicates that the workforce is primarily composed of Gen X and Millennials, with fewer younger workers.
Recommendation: Organizations should tailor their engagement strategies to the needs and preferences of Gen X and Millennials, who make up the majority of the workforce. However, it is also important to develop recruitment and retention strategies to attract younger talent from Gen Z, ensuring a balanced generational mix and fostering innovation.

**PRODUCTIVITY ANALYSIS**

**Productivity by Org Preparedness:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Productivity%20by%20Org%20Prepardness.png" alt="Description" width="400"/>

Insight: The heatmap shows a strong correlation between organizational preparedness for remote work and productivity. Employees who perceive their organization as well-prepared ("strongly agree" or "somewhat agree") are more likely to report higher productivity levels. Conversely, those in less prepared organizations tend to report lower productivity.
Recommendation: Organizations should focus on enhancing their readiness for remote work by investing in necessary infrastructure, training, and support. Regular assessments of organizational preparedness can help identify areas for improvement, ensuring that employees feel supported and productive in remote environments.

**Productivity by Ease of Collaboration:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Productivity%20by%20Ease%20of%20Collaboration.png" alt="Description" width="400"/>

Insight: The chart highlights the relationship between the ease of collaboration and productivity levels. Respondents who find remote work collaboration easy tend to report higher productivity, with a significant number indicating "much more productive" or "same productivity." In contrast, those who find collaboration difficult are more likely to report lower productivity.
Recommendation: Investing in robust collaboration tools and training can significantly enhance productivity in remote work settings. Organizations should prioritize making collaboration easy, whether through better communication platforms, regular team meetings, or collaboration training, to boost overall productivity.

**Productivity by Seniority (Bar Chart):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Prodctivity%20by%20Seniority.png" alt="Description" width="400"/>

Insight: Employees with more than 5 years at their job report "same productivity" more frequently, while newer employees (1-5 years) report a mix of productivity levels. This suggests that seniority might be a stabilizing factor in maintaining productivity during remote work.
Recommendation: Senior employees might need less supervision and more autonomy, whereas newer employees could benefit from more structured support and mentorship. Tailoring remote work policies to different levels of seniority could help optimize productivity across the organization.

**ORGANIZATION SIZE VS REMOTE HOURS**

**Average Monthly Remote Work Hours per Org Size:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Avg%20monthly%20remote%20work%20Hour%20per%20Org%20Size.png" alt="Description" width="400"/>

Insight: The graph indicates that as the size of the organization increases, so do the average monthly remote work hours per employee. For example, in organizations with 200+ employees, the average monthly remote work hours are around 33.08 hours, compared to 28 hours in smaller organizations (1-4 employees).
Recommendation: Larger organizations should leverage their resources to support remote work through advanced technologies, employee training, and flexible work policies. Smaller organizations may need to gradually adopt remote work, ensuring they have the necessary infrastructure and support systems.

**Org of 200+: Remote vs. In-Person Work Hours:**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Org%20200-%20Remote%20Vs%20In-person%20Work%20Hours.png" alt="Description" width="400"/>

Insight: In organizations with 200+ employees, workers spend slightly more hours working remotely (49,322 hours) compared to in-person (47,015 hours) each month. This reflects a trend where larger organizations are likely to have more established remote work practices.
Recommendation: For large organizations, maintaining a balance between remote and in-person work is critical. While remote work is efficient, it should be balanced with in-person interactions to foster team collaboration and company culture. Organizations might also consider strategies to optimize remote working hours, ensuring they don't contribute to burnout.

**REMOTE WORK PREFERENCE**

**Relative Productivity by Actual Number of Remote Days (Heatmap):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Relative%20Productivity%20by%20Actual%20Number%20of%20Remote%20Days.png" alt="Description" width="400"/>

Insight: The heatmap shows that employees working 4-5 remote days tend to report the highest productivity, with 540 respondents indicating "same productivity" and 364 indicating "much more productive." Fewer remote days generally correlate with lower productivity levels.
Recommendation: Encourage employees to adopt a work schedule with 4-5 remote days if it aligns with their role and responsibilities. This configuration appears to be the most productive, according to the data. However, offering flexibility and support for different remote day configurations is also crucial.

**Relative Productivity by Preferred Number of Remote Days (Heatmap):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Relative%20Productivity%20by%20Preferred%20Number%20of%20Remote%20Days.png" alt="Description" width="400"/>

Insight: Employees who prefer 4-5 remote days show the highest productivity, with 395 respondents indicating they are "much more productive" under this arrangement. However, productivity levels vary, with some employees still preferring fewer remote days.
Recommendation: Organizations should focus on providing flexibility, especially for those who prefer more remote days, as this seems to correlate with higher productivity. Tailoring remote work policies to individual preferences can maximize productivity and employee satisfaction.

**Productivity Ratings by Number of Preferred Remote Days (Pie Chart)**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Productivity%20Ratings%20by%20Number%20of%20Preferred%20Remote%20Days.png" alt="Description" width="400"/>

Insight: The chart shows that 26.6% of respondents prefer working 2-3 remote days, while 26.5% prefer 4-5 remote days. There’s a balanced distribution across other preferences, with only 4.5% not providing a preference.
Recommendation: Organizations should consider offering flexibility in remote work schedules, allowing employees to choose the number of remote days that best suit their productivity and personal lives. This can increase job satisfaction and overall productivity.

**TIME SPENT**

 **Average Hours Spent (In-Person vs. Remote) (Pie Charts):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Average%20Hours%20Spent(In%20Person%20Vs%20Remote).png" alt="Description" width="400"/>
 
Insight: The comparison between in-person and remote work reveals that employees spend a higher percentage of their time on work when remote (50.5%) compared to in-person (48.7%). However, remote work also allows for more personal/family time (27.4%) than in-person work (23.5%).
Recommendation: While remote work increases work time, it also enhances personal/family time, which is beneficial for employee well-being. Organizations should encourage employees to maintain this balance, perhaps by offering flexible work schedules that allow for personal and family commitments.

**Average Hours Saved (Remote vs. In-Person):**

<img src="https://github.com/Aishwaryachen11/Optimizing_Remote_Work_Productivity/blob/main/Images/Average%20Hours%20Saved(Remote%20Vs%20In-person).png" alt="Description" width="400"/>

Insight: The graph shows that employees save approximately 1.2 hours on commuting when working remotely, which is the most significant time savings. However, the time saved from commuting is partly reallocated to personal/family time (about 0.6 hours) and domestic responsibilities (about 0.3 hours). Interestingly, there is a minor increase in working hours.
Recommendation: Organizations should recognize the substantial time savings from reduced commuting and encourage employees to use this time for personal well-being, family, or other life-enhancing activities. Additionally, to prevent overwork, organizations might consider monitoring remote working hours and promoting a healthy work-life balance.

These insights and recommendations provide a comprehensive understanding of how different factors such as age, gender, work arrangements, collaboration, and organizational preparedness influence productivity and engagement in remote work environments. By addressing these areas, organizations can optimize their remote work policies to enhance overall productivity and employee satisfaction.
