# 🚀 **Python Data Job Market Analysis: In-Demand Skills, Trends & Salaries — Data Analyst Portfolio Project**

This project was developed to explore and understand the **global data job market**, with a special focus on **Data Analyst roles**.  
The goal is to identify the **top skills** and **top data jobs** based on **salary analysis** and **in-demand trends**, providing insights for anyone looking to enter or grow in the data field.  

The analysis in this project covers:

- 🔑 **In-Demand Skills**: Identifying the most requested skills in the data field to highlight the most valuable skills for aspiring professionals.  
- 👩‍💻 **In-Demand Roles**: Tracking the most popular data roles in the job market, offering guidance on the best roles to pursue.  
- 💰 **Salary Analysis**: Evaluating salaries across skills and roles, helping job seekers weigh career opportunities and skill investments.  

All data processing and analysis were performed in **Python**, leveraging libraries such as **Pandas**, **Matplotlib**, and **Seaborn**.

# **Problem**

The analysis was guided by the following key business questions:  

1. 🔑 What are the most in-demand skills for the top 3 most popular data roles?  
2. 📉 How are in-demand skills trending for Data Analysts?  
3. 💵 How is the salary distributed among different data job roles?  
4. 🎯 How well do skills pay for Data Analysts?  
5. 🚀 What are the most optimal skills for Data Analysts to learn? 

# **Dataset**

The dataset was sourced from **lukebarousse.com**, containing detailed information on data-related job postings.  

📂 Since the dataset is too large, you can access the dataset by download it manually via google drive here [data_jobs.csv](https://drive.google.com/file/d/17EV2fVz8hW40wr5C9b9D8l5H5n-_1-6D/view?usp=sharing).  
It contains **785,741 rows** and **17 columns**. 

# **Methodology**

The project followed a structured workflow:  

1. 🧹 **Data Preparation and Cleaning**  
2. 📈 **Data Analysis**  
3. 📊 **Data Visualization**  
4. 🧐 **Interpretation of Findings**   

# **Tools**

The following tools were used throughout the project:  

- 🐍 **Python**: Main programming language for analysis and extracting insights.  
   - **Pandas**: For data manipulation and preparation.  
   - **Matplotlib**: For visualizing data and supporting insights.  
   - **Seaborn**: For creating advanced and professional visualizations.  
- 📓 **Jupyter Notebook**: Interactive environment for analysis and exploration.  
- 💻 **Visual Studio Code**: IDE for developing and managing Python scripts.  
- 🌐 **Git & GitHub**: For version control, collaboration, and project tracking.  

# **Data Preparation and Cleaning**

This stage ensured the dataset was **clean, consistent, and ready for analysis**.  

- Imported all required libraries.  
- Loaded the dataset.  
- Clean and transform the dataset.

```python
#Import required libraries
import pandas as pd
import ast
import matplotlib.pyplot as plt
import seaborn as sns

#Load the dataset
df = pd.read_csv(r'C:\Users\harto\OneDrive\Dokumen\Python Course\data_jobs.csv')
df

#Change the datatype from str into datetime
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])

#Change the datatype from str into list
def str_to_list(column):
    if pd.notna(column) :
        return ast.literal_eval(column)
    
df['job_skills'] = df['job_skills'].apply(str_to_list)
```
Further filtering and transformations were applied later in each problem section, since every business question required different approaches. 

# **Data Analysis, Visualization & Interpretation**

Each Jupyter Notebook in this project addresses a **specific business problem**.  
Visualizations and insights are included to maximize clarity and depth.  

## **1. What are the most in-demand skills for the top 3 most popular data roles?**

**Metric used**: Likelihood percentage of a skill appearing in job postings.  

Filtered `job_country` to `United States`, retrieved the **top 3 most in-demand roles** and their **top 5 skills** using `value_counts()`.  

📑 Step-by-step method: [2. Skill Demand Analysis](code/2_Skill_Demand.ipynb)

```python
fig, ax = plt.subplots(3,1)

sns.set_theme(style = 'ticks')

for i, job_title in enumerate(top_3_jobs):
    df_plot = df_final[df_final['job_title_short'] == job_title].head(5)
    sns.barplot(data = df_plot, x = 'percentage', y = 'job_skills', ax = ax[i], hue = 'percentage', palette = 'dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,80)
    ax[i].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos : f'{int(x)}%'))
    if i != 2:
        ax[i].set_xticks([])
    for n, v in enumerate(df_plot['percentage']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Percentage of Skills Requested In United States Job Posting')
fig.tight_layout()
plt.show()
```

### **Visualization**

<p align="center">
  <img src="images/Fig.1 Likelihood Percentage of Skills Requested In United States Job Posting.png" alt="" />
</p>

**<center>Fig.1 Likelihood Percentage of Skills Requested In United States Job Posting</center>**

### **Insights:**

- 🟦 **SQL As A Universal Requirement Skills**: The visualization indicates that SQL is always be a universal requirement skills. Across all three roles: **Data Analyst (51%), Data Scientist (51%), and Data Engineer (68%)**. SQL consistently ranks as one of the top skills. This highlights SQL as the **foundation of data roles**, essential for querying, managing, and analyzing structured data.
- 🐍 **Python’s Dominance in Technical Roles**: Python is the most demanded skill for **Data Scientists (72%)**, reinforcing its role as the backbone of advanced analytics, machine learning, and AI applications. It is also critical for **Data Engineers (65%)** and highly requested for **Data Analysts (27%)**, showing its broad applicability across the data career spectrum.
- 📊 **Excel Still Matters for Data Analysts**: Despite the rise of programming tools, **Excel (41%)** remains highly relevant for **Data Analysts**. This indicates employers still value practical, business-oriented analysis tools alongside technical programming skills.
- 📉 **Role-Specific Skill Trends**: Data Analysts rely on visualization and reporting tools like **Tableau (28%)** and **SAS (19%)**. While Data Scientists require advanced statistical and modeling languages such as **R (44%)** and **SAS (24%)**. In other hand Data Engineers are expected to master cloud and big data ecosystems, with strong demand for **AWS (43%)**, **Azure (32%)**, and **Spark (32%)**.

## **2. How are in-demand skills trending for Data Analysts?**

**Metric used**: Likelihood percentage of skill requirements over time.  

Filtered `job_country` to `United States` and `job_title_short` to `Data Analyst`. Extracted month from `job_posted_date` for trend analysis.  

📑 Step-by-step method: [3. Monthly Skills Trend Analysis](code/3_Skill_Trends.ipynb)

```python
sns.set_theme(style = 'ticks')

sns.lineplot(data = df_plot_percent, dashes = False, legend = 'full', palette = 'tab10')
sns.despine()

plt.title('Likelihood Percentage of Data Analyst Top Skills Trend In United States')
plt.xlabel('2023')
plt.ylabel('')
plt.legend(title = 'Skills', bbox_to_anchor=(1, 1), loc='upper left')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos : f'{int(x)}%'))
plt.show()
```

### **Visualization**

<p align="center">
  <img src="images/Fig.2 Likelihood Percentage of Data Analysts Top Skills Trend In United States.png" alt="" />
</p>

**<center>Fig.2 Likelihood Percentage of Data Analysts Top Skills Trend In United States</center>**

### **Insights:**

- 🟦 **SQL Mantains its Dominance**: Throughout 2023, SQL consistently remained as the top skill for data analysts, staying above **45% likelihood** and significantly ahead of others. Even with fluctuations, SQL’s stability underscores its **non-negotiable role** for data analysts across industries.
- 📉 **Excel Shows Declining Demand**: Started strong (42–43%) but gradually **declined below 35%** by late 2023, suggesting that employers are shifting from traditional tools to more technical and automated skill sets.
- 📊 **Tableau and Python Shows Steady Rise**: Tableau maintained a **stable presence (~28–30%)** with slight increases mid-year, highlighting the **continued importance of visualization** for business-driven insights. While Python shows an upward trend in the middle of the year, peaking around August (~31%), reinforcing its growing relevance for **analytical automation and scalability**.
- 📉 **SAS Continues to Decline**: SAS started at **~20% in January but dropped to ~16–17%** toward the end of the year. This confirms the industry-wide shift from legacy tools (SAS) to open-source alternatives like Python and R.
- ⏳ **Seasonal Hiring Fluctuations**: There is an indiciation of **Seasonal Hiring Fluctuations**. Both SQL and Excel show **noticeable dips mid-year (June–July)**, which could reflect **hiring slowdowns** rather than skill irrelevance.

# **3. How is the salary distribution among data jobs role?**

To address the distribution of salary among each data roles, we used boxplot chart.

You can see my step by step analysis method [here: 4. Salary Analysis](code/4_Salary_Analysis.ipynb)

```python
sns.set_theme(style = 'white')

sns.boxplot(data = df_boxplot, x = 'salary_year_avg', y = 'job_title_short')
sns.despine()

plt.title('Salary Distributions of Data Enthusiast Roles In United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 650000)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos : f'${int(x/1000)}K'))
plt.show()
```

### **Visualization**

<p align="center">
  <img src="images/Fig.3 Salary Distributions of Data jobs Roles In United States.png" alt="" />
</p>

**<center>Fig.3 Salary Distributions of Data jobs Roles In United States</center>**

### **Insights:**

- 💵 **Data Scientists and Data Enggineers Earn The Most**: Data Scientists and Data Engineers generally earn more than Data Analysts, reflecting the **higher technical complexity and specialization** required.
- 📈 **Senior Roles Valued More Than Entry Level**: Senior roles in all categories show significantly higher medians and upper ranges, underlining the **value of experience and expertise**.
- 📊 **Data Scientist Show The Highest Upside**: Both Data Scientist and Senior Data Scientists have the **widest salary range (often exceeding $300K, with outliers beyond $500K)**. highlighting the premiumity on advanced modeling, machine learning, and AI skills, especially for those with leadership or senior expertise.
- 🔧 **Data Engineers rival Data Scientists**: Both of them have strong salary ranges, Data Engineers even overlap the data scientist in many cases. Reflecting the **strategic importance of infrastructure, pipelines, and cloud engineering**, as organizations scale up their data ecosystems.
- 📊 **Data Analyst Earn Less, but Provide an Entry Points**: Data Analysts and Senior Data Analysts show **lower medians** compared to technical counterparts. However, these roles still provide **strong career entry points** and can transition into higher-paying roles if analysts upskill in SQL, Python, and cloud/visualization tools.
- 🌍 **High Variability Across All Roles**: Wide salary distributions and outliers across all roles suggest that compensation is **heavily influenced by location, industry, and specialization**.

# **4. How well skills pay for Data Analysts?**

This section we will focused on the salary of the data analyst skills. We will analyze how well the salary of the top 6 of most in-demand skills for Data Analyst, and we will compare it with the salary top 6 skills with highest salary for data analyst. Are the most demanded skills will be the highest paid skill too or not.

Medain salary will be the metrics used for this analysis, since it has a good retention from outliers, it considered as the most representative aggregation for this analysis.

You can see my step by step analysis method [here: 4. Skill Salary Analysis](code/4_Salary_Analysis.ipynb)

```python
fig, ax = plt.subplots(2,1)

sns.set_theme(style = 'ticks')

# plot most demand skills
sns.barplot(data = df_most_demand, x = 'median_salary', y = 'job_skills', ax = ax[0], palette = 'dark:b_r', hue ='median_salary')
ax[0].set_ylabel('')
ax[0].set_xlabel('Median Salary (USD)')
ax[0].set_title('Median Salary of Most Demanded Skills In Data Analyst Job')
ax[0].legend().remove()
ax[0].set_xlim(0, 200000)
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos : f'${int(x/1000)}K'))

# plot most paid skills
sns.barplot(data = df_top_paid, x = 'median_salary', y = 'job_skills', ax = ax[1], palette = 'dark:b_r', hue = 'median_salary')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_title('Median Salary of Highest Paid Skills In Data Analyst Job')
ax[1].legend().remove()
ax[1].set_xlim(0, 200000)
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos : f'${int(x/1000)}K'))

fig.tight_layout()

plt.show()
```

### **Visualization**

<p align="center">
  <img src="images/Fig.4 Salary Analysis of Data Analysts Skills.png" alt="" />
</p>

**<center>Fig.4 Salary Analysis of Data Analysts Skills</center>**

### **Insights:**

- ⚖️ **Gap Between "Most Demanded" and "Highest Paid" Skills**: There is a big gap salary between “Most Demanded” and “Highest Paid” Skills. While The **most demanded skills (SQL, Excel, Tableau, Python, R, SAS, Power BI)** offer median salaries between **$80K–$95K**, contrary the **highest paid skills (dplyr, GitLab, Bitbucket, Solidity, Hugging Face, Couchbase, Ansible)** push median salaries well above **$150K–$200K**, showing a clear **market imbalance** where the skills most employers request do not always deliver the highest salary premiums.
- 🐍 **Python as The Best Skill to Master**: Among the “most demanded skills,” **Python** stands out with the **highest median salary (~$95K)** and high demand across roles, suggesting **Python as the most strategic skill to learn.**
- 📊 **Excel’s Limitations**: Excel, despite being heavily demanded (~41% of job postings), offers the **lowest salary (~$75K)** among core analyst tools. Making it as an **entry-level baseline skill**.
- 🚀 **Niche Skills Drive Salary Premiums**: Skills like **Solidity (blockchain), Hugging Face (AI/ML NLP tools), and Ansible (automation)** are not widely demanded but command **premium salaries (~$150K–$190K)**, representing **specialized, emerging technologies** where supply is limited and expertise is highly valued.
- 🎯 **The Best Strategy**: Its strategic to **expertising broad, in-demand skills (SQL, Tableau, Excel, Python) first** to secure opportunities and enter the job market, **then targeting niche, cutting-edge skills to maximize salary**.

# **5. What are the optimal skills for data analysts to learn?**

To determine the optimal skills for data analysts to learn, we will aggregate the **median salary** and **likelihood percentage of skill will be reqired**, the plot the distribution of the skills based on the relationship of those 2 metrics in scatter plot. The higher medain salary and the likelihood percentage the skill has, the more optimal it is.

We will filter the `job_country` only for `United States` and only focus on `Data Analyst` role in `job_title_short`.

You can see my step by step analysis method [here: 5. Optimal Skills Analysis](code/5_Optimal_Skills.ipynb)

```python
from adjustText import adjust_text

sns.set_theme(style='ticks')

# Scatter plot
df_agg.plot(kind='scatter', y='median', x='percentage')
plt.xlabel('Likelohood Percentage Will Be Needed on Data Analyst Job')
plt.ylabel('Median Salary ($ USD)')
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{int(x)}%'))
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.title('Most Potential Skills For Data Analyst In United States')

# Add labels (with small nudges for overlapping ones)
texts = []
for i, txt in enumerate(df_agg.index):
    x = df_agg['percentage'].iloc[i]
    y = df_agg['median'].iloc[i]
    
    if txt.lower() == "power bi":
        y += 300   # nudge upwards
    elif txt.lower() == "sas":
        y -= 300   # nudge downwards

    texts.append(plt.text(x, y, " " + txt))

# Adjust text with arrows
adjust_text(
    texts,
    arrowprops=dict(arrowstyle="->", color="gray", shrinkA=5, shrinkB=5),
    expand_points=(1.3, 1.6),
    expand_text=(1.3, 1.6),
    force_points=0.5,
    force_text=0.5,
    lim=2000
)
plt.show()
```
### **Visualization**

<p align="center">
  <img src="images/Fig.5 Most Potential Skills For Data Analyst In United States.png" alt="" />
</p>

### **Insights:**

- 🟦 **SQL as A Core Universal Skill, ensuring Employability**: SQL sits far right with **~60% demand likelihood**, making it the **most widely required skill** for Data Analysts. Its **salary (~$92K median)** is strong though not the very highest, but its job security and universality make it non-negotiable.
- 🐍 **Python Offers Best Overall Potential, Suggesting Strategic Investment**: Python offers the best overall potential with the **highest salary (~$98K)** combined with **moderate demand (~30%)**, making it a **strategic investment skill** with its high reward and high demand in another data job roles. Analysis on **Fig.2** shows python have an **increasing trend demand, making it the most optimal skills to learn**.
- 📊 **Excel Is Widely Reqired but Earn Less**: Excel shows **high demand (~40%)** but **lower median salary (~$85K)**. Making it a baseline skill that crucial but not enough to drive a higher compensation.
- 📈 **Tableau & R Balance Salary and Demand**: Tableau (~$93K) and R (~$92K) sit in a strong middle ground with **moderate demand and solid salaries**. They **complement SQL and Python well**, offering value in visualization (Tableau) and statistical depth (R).
- 📉 **Legacy of Office Tools Has Ended**: Word and PowerPoint appear with **low salaries (~$82–86K)** and **very low demand**, meaning they are **basic workplace tools but not career-advancing skills**.
- 📉 **SAS Being Replaced by Oper-Source Alternatives**: Similarly, SAS is slowly being replaced by open-source alternatives, with **lower growth potential** as shown in **Fig.2** despite having a decent salary levels.
- 📊 **Tableau Is Better Than Power BI**: Power BI as a BI Tools has **lower demand and salaries** compared to its rival, **Tableau**. It indicates that **Tableau is the most optimal skills to have for BI and Visualization Tools**.
- 💼 **Niche Tools: Strong Salaries but Low Demand**: Niche tools like **Oracle and Go** show **strong salaries but relatively low demand**. These may be lucrative in specific industries (finance, enterprise IT), but are not widely transferable across all analyst jobs.

# **What I Learned**

This project improved both my **market understanding** and **technical expertise**. 

💡 Here are some of the most important takeaways:

- 🐍 **Advanced Python Usage**: Mastery of Pandas, Matplotlib, Seaborn for efficient analysis and visualization.  
- 🧹 **Data Cleaning**: Essential for ensuring **accuracy and reliability** in insights.  
- 🎯 **Analytical Thinking**: Developed ability to detect anomalies, apply correct methods, and design impactful visuals. 
- 📖 **Storytelling with Data**: Translated raw data into **clear, narrative, and actionable insights** for both technical and non-technical audiences.

# **Challenges I Faced**

- 🛠️ **Poor Data Quality**: Required strong cleaning and anomaly handling.  
- 🔍 **Complex Analysis**: Designing correct approaches to solve problems was demanding but rewarding.  
- 📊 **Visualization in Python**: Required precise configuration to achieve professional best practices (unlike drag-and-drop BI tools). 

# **General Conclusion**

This project provides a comprehensive overview of the global data job market, with a particular focus on Data Analyst roles. By combining skill demand analysis, salary distribution, and trend evaluation, several key themes emerge:  

1. 🟦 **SQL as the Universal Core Skill**  
   - SQL consistently appears as the most in-demand skill across Data Analysts, Data Scientists, and Data Engineers.  
   - Its strong median salary and unmatched demand highlight it as the **non-negotiable foundation** for anyone entering the data field.  

2. 🐍 **Python as the Strategic Growth Driver**  
   - Python stands out as the most versatile and future-proof skill, combining high salaries, growing demand, and applicability across roles.  
   - It not only empowers Data Analysts with automation and scalability but also opens pathways toward advanced roles such as Data Scientist or Machine Learning Engineer.  

3. 📊 **Excel as a Baseline, Not a Differentiator**  
   - Excel remains highly requested for Data Analyst positions and offers strong entry-level opportunities.  
   - However, its lower salary ceiling confirms that **Excel alone is insufficient for long-term career growth**, and must be complemented with technical and analytical tools.  

4. 📈 **Visualization and Statistical Tools Add Competitive Value**  
   - Tools like Tableau and R occupy a middle ground of solid demand and salary, making them valuable complements to SQL and Python.  
   - Tableau strengthens business-facing communication of insights, while R deepens statistical and research-oriented analysis.  

5. 💵 **Role-Based Salary Differentiation Matters**  
   - Data Scientists and Data Engineers earn higher median salaries due to their technical depth and specialization.  
   - Data Analysts, while offering strong entry points, face lower compensation ceilings unless they upskill into programming, cloud, or advanced analytics.  

6. 🚀 **Emerging and Niche Skills Offer Premiums but Limited Reach**  
   - Technologies like Solidity, Hugging Face, or Oracle command very high salaries but remain in low demand.  
   - These are best pursued once foundational, broad-demand skills are mastered, serving as **career accelerators** rather than entry points.  
