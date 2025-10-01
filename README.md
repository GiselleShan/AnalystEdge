# The analysis
## 1. What are the most demanded skills for the top 3 most popular data roles?
The objective is to identify the most in-demand skills for the top three most popular data roles. I filtered out the positions based on their popularity and identified the top five skills for the top three roles. This query identifies the most common job titles and their top skills, indicating which skills should be prioritized based on the targeted role.


View my notebook to see detailed steps here: [2_Skills_Demand.ipynb](2_Skill_Demand.ipynb)
### Visualize Data (key codes)
```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)
    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
```

### Results

![Visualisation of Top Skills for Data Roles](images\output_skills.png)

### Insights
- Key Takeaways by Role

    Data Analyst 📊: This role is heavily focused on retrieving and reporting data. SQL (51%) and Excel (41%) are the most requested skills, highlighting the need to query databases and present findings in a common business tool. Visualization skills, represented by Tableau (28%), are also significant.

    Data Engineer ⚙️: This role is all about building and maintaining data infrastructure. SQL (68%) and Python (65%) are nearly equally critical for creating data pipelines. The strong presence of cloud platforms like AWS (43%) and Azure (32%) clearly marks this as a technical, infrastructure-oriented position.

    Data Scientist 🧪: This role is dominated by programming for statistical modeling and analysis. Python (72%) is the most in-demand skill by a large margin. While SQL (51%) is crucial for accessing data, statistical programming languages like R (44%) are also highly valued.

- Cross-Role Comparison

    SQL is the Universal Language: It is a top-two skill for all three roles, making it the most universally required skill in the data field.

    Python's Importance Varies: It is absolutely essential for Data Scientists and Data Engineers but is significantly less requested for Data Analysts.

    Specialized Tools Define the Job:

    The high demand for Excel is unique to the Data Analyst role.

    Expertise in cloud technologies (AWS, Azure) and Spark is a key differentiator for Data Engineers.

    A strong command of both Python and R is most characteristic of a Data Scientist.
## 2. How are in-demand skills trending for Data Roles?
### Visualize Data (Key Codes)
```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() 

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

```
### Results
![Trending Top skills for Data Roles in the US](images\Trending_Top_Skills_for_Data_Analysts_in_the_US.png)

### Insights
- SQL Remains Dominant: SQL was the most consistently demanded skill throughout the year. Despite a gradual decrease from its peak at the beginning of the year, it remained comfortably above all other skills.
- Excel's End-of-Year Surge 📈: After a dip mid-year, Excel saw a significant spike in demand starting in September. This sharp increase allowed it to surpass both Python and Tableau, finishing the year as the second most requested skill.
- Stable Demand for Python & Tableau: The demand for Python and Tableau remained relatively stable, with their trend lines fluctuating closely together around the 30% mark. This indicates they are consistent and essential skills for data analysts.
- Power BI's Steady Growth: While less demanded than the other skills, Power BI maintained a steady presence and showed a slight upward trend in the last quarter, suggesting its growing importance in the field.

## 3. How well do jobs and skills pay for Data?
### Highest Paid & Most Demand Skills for Data Roles
#### Visualize Data (Key Codes)
```Python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Assume df_DA_top_pay and df_DA_skills are pre-existing DataFrames
# df_DA_top_pay: DataFrame with index as skills, 'median' column for salary, sorted by highest paid
# df_DA_skills: DataFrame with index as skills, 'median' column for salary, sorted by most in-demand

# 1. Create a figure with two vertically stacked subplots
fig, ax = plt.subplots(2, 1, figsize=(10, 8)) 
sns.set_theme(style='ticks')

# --- Plot 1: Highest Paid Skills ---
sns.barplot(
    data=df_DA_top_pay, 
    x='median', 
    y=df_DA_top_pay.index, 
    ax=ax[0], 
    palette='dark:b_r'
)
ax[0].set_title('Highest Paid Skills for Data Analysts in the US')

# --- Plot 2: Most In-Demand Skills ---
sns.barplot(
    data=df_DA_skills, 
    x='median', 
    y=df_DA_skills.index, 
    ax=ax[1], 
    palette='light:b'
)
ax[1].set_title('Most In-Demand Skills for Data Analysts in the US')
ax[1].set_xlabel('Median Salary (USD)')


# 2. Synchronize the x-axis for a fair comparison
# This makes sure both charts use the same salary scale.
ax[1].set_xlim(ax[0].get_xlim())

# 3. Format x-axis labels on both plots for readability (e.g., $150K)
formatter = plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K')
ax[0].xaxis.set_major_formatter(formatter)
ax[1].xaxis.set_major_formatter(formatter)

# Clean up layout and display the plot
plt.tight_layout()
plt.show()
```
![The Highest Paid & Most In-demand Skills](images\Highest_Paid_and_Most_In_Demand_Skills_for_Data_Analysts_in_the_US.png)
#### Insights
- The Pay vs. Demand Paradox: The most significant insight is that there is no overlap between the top 10 highest-paid skills and the top 10 most in-demand skills. The skills that make you most hirable are not the ones that earn the most money.
- In-Demand Skills are Foundational: The most requested skills are the core tools of data analysis: programming languages (python, sql, r), BI software (tableau, power bi), and business tools (excel, powerpoint). These skills have median salaries clustered around $90K-$100K.
- High-Paying Skills are Specialized: The highest salaries are tied to specialized, technical skills often found in software engineering, MLOps, or blockchain, such as solidity (blockchain), hugging face (AI/ML), and gitlab (DevOps). These niche skills command salaries starting around $150K and reaching nearly $200K.