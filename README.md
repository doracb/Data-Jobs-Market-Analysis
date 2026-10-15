# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles in Romania?

To find the most in demand skills for the top 3 most popular data roles, I filtered the positions by popularity and selected the 5 most common skills for each one. These insights highlight what aspiring data professionals should focus on depending on their target career path.

You can view my notebook with the detailed steps here: [2_Skills_Count.ipynb](2_Skills_Count.ipynb)

### Visualize Data:
```python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style="whitegrid", palette="flare")

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 70)
    ax[i].legend().set_visible(False)
    
    for index, value in enumerate(df_plot['skill_percent']):
        ax[i].text(value + 1, index, f'{value:.0f}%', va='center')
    
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in Job Postings in Romania', fontsize=16)
fig.tight_layout(h_pad=1)
plt.show()
```

### Results:
![Visualization of Top Skills](./Images/skill_demand_all_data_roles.png)

### Insights:
- **Python** is a good skill to have for its overall versatility, but not the most in demand for these particular roles.
- **SQL** is the most requested skill with the exception of Data Analysts, where **Excel** wins by a small margin.
- **Data Engineers** require heavy cloud infrastructure stack skills (Azure, AWS, Databricks), while analysts focus on BI dashboards and reporting (Excel, Power BI, Tableau, SAS).
- **Power BI** and **Tableau** seem to be equally as important. Chosing to focus on one of them instead of attempting to master both can be a good idea.
- **Data Scientists** are not in the top 3 most popular data roles in Romania yet. Their absence here indicates that the Romanian data market is more focused on business intelligence.

## 2. How are in-demand skills trending for Data Analysts?

It's important to know whether the most common skills for Data Analysts are growing in popularity or, on the contrary, losing their importance. To obtain this data, I had another look at the job postings in Romania in 2023, this time looking at each month separately.

You can view the detailed steps for this analysis here: [3_Skills_Trend.ipynb](3_Skills_Trend.ipynb)

### Visualize Data:
```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_RO_perc.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())

plt.show()
```

### Results:
![Trending Top Skills for Data Analysis in Romania](./Images/skill_trend_DA.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights:
- **SQL** was the most consistently demanded skill throughout 2023.
- All skills showed an increase in demand at the end of the year except for **Excel**, which had an unexpected drop around December.
- It's hard to tell if **Power BI** or **Tableau** is better considering how much they fluctuate in importance. Overall, they are similar.

## 3. How well do jobs and skills pay for Data Analysts?

### A Salary Analysis

### Visualize Data:
```python
sns.boxplot(data=df_RO_top6, x='salary_year_avg', y='job_title_short', order = job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distribution in Romania')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results:
![Salary Distributions of Data Jobs in Romania](./Images/salary_boxplot.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights:
- **Software Engineer** salary distributions have the widest spread, spanning from roughly $60K up to $190K USD. This high variance can have many causes such as company size, experience levels, or remote B2B/contracting opportunities common in Romania.
- **Machine Learning Engineer** and **Software Engineer** roles have higher median salaries compared to analytical roles. ML Engineers show a consistently high base salary range (clustering tightly around $130K–$160K USD).
- **Data Engineers** enjoy a solid salary floor starting around $80K USD, outearning baseline **Data Analysts** (with a median around $75K USD). This aligns with earlier findings showing Data Engineers require more specialized cloud and technical stacks.
- Moving from **Data Analyst** to **Senior Data Analyst** or pivoting to **Data Scientist** nearly doubles the median earning potential, raising base expectations from around $75K USD toward $110K–$125K USD.
- Regarding outliers, the **Senior Data Analyst** data shows a high outlier reaching near $200K USD, which could reflect specialized international or remote consulting contracts within Romania. The **Data Engineer** section also features a low outlier near $45K USD, pointing to entry-level or non-standard localized postings.

### Highest Paid & Most Demanded Skills for Data Analysts

### Visualize Data:
```python
fig, ax = plt.subplots(2, 1)

# Top 10 highest paying skills for Data Analysts in Romania
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 most common skills for Data Analysts in Romania
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

### Results:
![The Highest Paid & Most Demanded Skills for Data Analysts in Romania](./Images/highest_paid_and_most_demanded_skills_for_data_analysts.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in Romania.*

### Insights:
- More niche skills like **JavaScript** and **Tableau** top the chart around $110K USD, followed closely by enterprise and big data platforms like **Oracle, Azure, Spark,** and **SAP**. Specialized skills carry a distinct premium over traditional office tools.
- There is significant overlap between the two lists. High-paying enterprise tech like **JavaScript, SAP, Azure,** and **Spark** also feature prominently among the most common skills, showing that Romanian employers actively pay top dollar for infrastructure and web-data-capable analysts.
- Core analysis tools like Excel, VBA, and PowerPoint are the backbone of the market, but cluster near the lower end of the median salary spectrum (around $56K USD). They are fundamental, but mastering them alone yields lower compensation ceiling compared to cloud and software platforms.
- General-purpose programming languages like **JavaScript** (~$110K) and **Go** (~$52K) show varying salary medians, while **Python** sits around $48K USD in median salary for analyst positions. This suggests that Data Analysts that incorporate web scripting/full-stack data integration or backend data tools see higher compensation than entry-level Python script users.