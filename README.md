# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles in Romania?

To find the most in demand skills for the top 3 most popular data roles, I filtered the positions by popularity and selected the 5 most common skills for each one. These insights highlight what aspiring data professionals should focus on depending on their target career path.

You can view my notebook with the detailed steps here: [2_Skills_Count.ipynb](2_Skills_Count.ipynb)

### Visualize Data:
```
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
```
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