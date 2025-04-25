

# Introduction

📊 Dive into the data job market! Focusing on Data Analyst roles, this project explores 💰 top-paying job, 👩🏻‍🏫 in-demand skills, and 📈 where high demand meets high salary in Data Analystics.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background

Drivern by a quest to navigate the Data Analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlinging others work to find optimal jobs.

Data hails from [SQL Course](https://lukebarousse.com/sql). It's packed with insingts on job titles, salaries, locations, and essential skills.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying Data Analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for Data Analysts ?
4. Which skills are associated with higher salaries ?
5. What are the most optimal skills to learn ?

# Tools I Used

For my deep dive into the Data Analyst job market, I harnessed the powe of several key tools:

- **SQL**: The backbone of my analysis, allowing me to quer the database and unearth critical insights.
- **PostgreSQL**: The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code**: My go-to for database management and executing SQL queries.
- **Git & GitHub**: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The Analysis

Each query for this project aimed at investigating swpecific aspects of the Data Analyst job market.
Here's how I approached each question:

### 1. Top Paying Data Analyst Jobs

To identify the highest-paying roles, I filtered Data Analyst positions by average yearly salary and location, focusing on remote jobs. This query higlights the high paying opportunities in the field.

```SQL
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

Here's the breakdown of the top Data Analyst jobs in 2023:

- **Whid Salary Range:** Top 10 paying Data Analyst roles apan from $184,000 to $650,000, indicating significant salary ootential in the field.
- **Diverse Employers:** Compaies like SamrtAsset, Meta, and AT&T are among those offering high salaries, showing abroad interest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, form Data Analyst to director of Analytics, reflecting varied roles and specializations within Data Analysts.

![Top paying Roles](assets/1_top_paying_jobs.png)
_Bar graph visualizing the salary for the top 10 salaries for Data Analyst; ChatGPT generated this graph from my SQL query results_

### 2. Skills for Top Paying Jobs

To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```SQL
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

Here's the breakdown fo the most demanded skills for the top 10 highest paying data analyst jobs in 2023:

- **SQL** is leading with a bold count of 8.
- **Python** follows closely with a bold count of 7.
- **Tableau** is also highly sought after, with a bold of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.

![Top paying Skills](assets/2_top_paying_roles_skills.png)
_Bat graph visualizing the count of skills for the top 10 paying jobs for data analysts, ChatGPT generated this graph from my SQL query results_

### 3. In-Demand Skills for Data Analysts

This quer helped identify the skills most frequently requested in job postings, direction focus to areas with high demand.

```SQL
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

Here's the breakdown of the most demanded skills for data analysts in 2023

- **SQL** and **Excel** remain fundamental, emphasizing the need for strong foundational skils in data processing and spreadsheet manipulation.
- **Programming** and **Visualization Tools** like **Python, Tableau,** and **Power BI** are essential, pointing towards the increasing importance of technical skills in dtata storytelling and decision support.

| Skills   | Demand Count |
| -------- | ------------ |
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

_Table of the demand for the top 5 skills in data analyst job postings_

### 4. Skills Based on Salary

Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```SQL
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```

Here's a breakdown of the reaults for top paying skills for Data Analysts:

- **High Demand for Big Data & ML Skills** : Top salaries are commanded by analysts skilled in big data technologies (PySpark, Couchbase), machine learning toos (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), reflection the industry's high valustion of data processing and predivtive modeling capabilities.
- **Software Development & Deployment Proficiency** : Knowledge in development and deployment tools (GitLab, Kubernetes,Airflow) indicates a lucrative crossover between data analysis and engineering, with apremium on skills the facilitate automation and efficient data pipeline management.
- **Cloud Computing Expertise** : Familarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscores the growing improtance of cloud-based analytics environments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.

| Skills        | Average Salary ($) |
| ------------- | -----------------: |
| pyspark       |            208,172 |
| bitbucket     |            189,155 |
| couchbase     |            160,515 |
| watson        |            160,515 |
| datarobot     |            155,486 |
| gitlab        |            154,500 |
| swift         |            153,750 |
| jupyter       |            152,777 |
| pandas        |            151,821 |
| elasticsearch |            145,000 |

_Table of the average salary for the top 10 paying skills for data analysts_

### 5. Most Pptimal Skills to Learn

Combining insights from demand and salary data , this quwey aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic foucus for skill development.

```SQL
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

| Skill ID | Skills     | Demand Count | Average Salary ($) |
| -------- | ---------- | ------------ | -----------------: |
| 8        | go         | 27           |            115,320 |
| 234      | confluence | 11           |            114,210 |
| 97       | hadoop     | 22           |            113,193 |
| 80       | snowflake  | 37           |            112,948 |
| 74       | azure      | 34           |            111,225 |
| 77       | bigquery   | 13           |            109,654 |
| 76       | aws        | 32           |            108,317 |
| 4        | java       | 17           |            106,906 |
| 194      | ssis       | 12           |            106,683 |
| 233      | jira       | 20           |            104,918 |

_Table of the most optimal skills for data analyst sorted by salary_

Here's a breakdown of the most optimal skills for Data Analysts in 2023:

- **Hight-Demand Programming Languages** : Python and R stand out for their high demand, with demand counts of 236 and 148 respectively.Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency inthese languages is highly valued but alsi widely available.
- **Cloud Tools and Technologies** : Skills inspecialized technologies such as Snowflak, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data techologies in data analysis.
- **Database Technologies** : The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,534 reflects the enduring need for data storage, retrieval, and management expertise.

# What I Learned

Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower :

- **🧩 Complex Query Crafting :** Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
- **📊 Data Aggregation :** Got cozy with GROUP BY and turned aggregate funtions like COUNT() and AVG() into my data-summarizing sidekicks.
- **💡 Analystical Wizardry :** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.

# Conclusions

### Insights

From the analysis, several general insights emerged :

1. **Top-Paying Data Analyst Jobs** : The hightest-paying jobs for data analysts tha6t allow remote work offer a wide range of salaries, the higtest at $650,000 !
2. **Skills for Top-Paying Jobs** : High-paying data analyst jobs require advanced proficiency in SQL, suggesting it's a critical skill for earning a top salary.
3. **Most In-Demand Skills** : SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.
4. **Skills with Higher Salaries** : Specialized skills, such as AVN and Solidity, are associated with the highest average salaries, indicating a premium on niche expertise.
5. **Optimal Skills for Job Market Value** : SQL leads in demand and offers for a ahigh average salary, positioning it as one of the most optimal skills for data analysts to learn to maximize their market value.

### Closing Thoughts

This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This contunuous learning and adaptation to emerging trends in the field of data analytics.
