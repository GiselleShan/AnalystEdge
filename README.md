# The analysis
## 1. What are the most demanded skills for the top 3 most popular data roles?
The objective is to identify the most in-demand skills for the top three most popular data roles. I filtered out the positions based on their popularity and identified the top five skills for the top three roles. This query identifies the most common job titles and their top skills, indicating which skills should be prioritized based on the targeted role.


View my notebook to see detailed steps here: [2_Skills_Demand.ipynb](2_Skill_Demand.ipynb)
### Visualise Data (key codes)
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
