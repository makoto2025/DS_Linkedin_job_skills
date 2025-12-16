# LinkedIn Job Postings Analysis (2024)

## 1. Project Overview

### 1.1 Goal
The primary objective of this project is to analyze the current employment landscape using a large-scale dataset of over 1.3 million LinkedIn job postings. The analysis aims to:
* **Identify Market Trends**: Determine the most frequently advertised job titles to understand hiring volume across different sectors.
* **Analyze Skill Demand**: Pinpoint the most universally required skills and how they correlate with specific job roles.
* **Demonstrate Scalability**: Showcase the use of PySpark for processing and cleaning large datasets that would be inefficient to handle with standard single-machine tools.

### 1.2 Why This Dataset?
The "1.3M LinkedIn Jobs & Skills (2024)" dataset was chosen for its volume and granularity. It allows for a realistic simulation of big data challenges, specifically:
* **Data Cleaning**: Handling inconsistent text inputs (e.g., "Shift Manager" vs. "Shift Manager - 2nd Shift").
* **Complex Relations**: Mapping job postings to their specific skill requirements.

---

## 2. Executive Summary

This report presents a comprehensive analysis of job market trends derived from **1,348,454 job postings**. Key findings include:

* **Dominant Roles**: Unlike the niche IT market, the broader LinkedIn landscape is currently dominated by high-volume operational and service roles. The most frequent titles include "Sales Associate," "Shift Manager," and "Tax Professional".
* **The "Soft Skill" Gap**: Technical skills vary by role, but soft skills are universal. **Communication** and **Customer Service** are the top two most requested skills across the entire dataset, appearing in hundreds of thousands of postings.
* **Data Integrity**: Significant preprocessing was required to standardize job titles, removing special characters and normalizing text to ensure accurate aggregation.

---

## 3. Technical Methodology

### 3.1 Tools & Technologies
The project leveraged industry-standard big data tools to handle the dataset's size (approx. 1.3 million rows):
* **PySpark**: The core engine for data ingestion, schema inference, cleaning, and aggregation.
* **KaggleHub**: Automated the retrieval of the latest dataset version.
* **Python (Matplotlib/Seaborn)**: Used for visualizing the aggregated results exported from the Spark DataFrames.

### 3.2 Data Processing Pipeline
The analysis followed a structured ETL (Extract, Transform, Load) pipeline:

1.  **Ingestion**: 
    * Loaded `linkedin_job_postings.csv`, `job_skills.csv`, and `job_summary.csv` using PySpark.
    * Enabled `multiline` support to correctly parse job summaries containing newline characters.

2.  **Cleaning**:
    * **Null Handling**: Dropped rows with null values in critical columns like `company`, `job_title`, and `job_skills` to ensure analysis quality.
    * **Text Normalization**: Used Regex (`[^a-zA-Z0-9\s\-\/\&]`) to strip special characters/emojis from job titles (e.g., removing "🔥" from "🔥Nurse Manager").
    * **Standardization**: Converted text to lowercase and trimmed whitespace to merge duplicates (e.g., "sales associate " -> "sales associate").

3.  **Transformation**:
    * **Explosion**: The `job_skills` column contained comma-separated strings. These were "exploded" into individual rows to calculate the frequency of each unique skill.
    * **Joining**: Merged job postings with their respective skills using the `job_link` key.

---

## 4. Data Overview

### 4.1 Dataset Statistics
The raw dataset is comprised of three main tables:
* **Job Postings**: 1,348,454 records containing metadata like Job Title, Company, and Location.
* **Job Skills**: 1,296,381 records linking jobs to required skills.
* **Job Summaries**: 1,297,332 records containing the full text description of the roles.

### 4.2 Quality Checks
Initial EDA confirmed the data integrity:
* **Row Counts**: The total rows match across the primary files, verifying a successful download.
* **Nulls**: Approximately 2,007 rows in the skills dataset were null, which is a negligible fraction (<0.2%) of the total dataset.

---

## 5. Job Market Findings

### 5.1 Top 10 Most Frequent Job Titles
The analysis of cleaned job titles reveals that the volume of hiring is heavily skewed towards retail, healthcare, and service management rather than tech-specific roles.


**Top 5 Roles by Volume:**
1.  **LEAD SALES ASSOCIATE-FT**: 7,325 postings.
2.  **Shift Manager**: 5,856 postings.
3.  **First Year Tax Professional**: 5,356 postings.
4.  **Assistant Manager**: 5,346 postings.
5.  **Customer Service Representative**: 5,204 postings.

*Insight*: The prevalence of "First Year Tax Professional" indicates seasonal hiring spikes, while the high volume of "Lead Sales Associate" roles points to high turnover or massive scale in the retail sector.

### 5.2 Top In-Demand Skills
By exploding the skills column and aggregating counts, we identified what employers value most.


**Key Findings:**
* **Communication**: The #1 skill, appearing in **370,041** postings.
* **Customer Service**: The #2 skill, appearing in **278,029** postings.
* **Teamwork**: #3, with **227,543** occurrences.

*Insight*: Technical skills like "Microsoft Office" and "Project Management" appear in the top 20, but they are significantly outnumbered by foundational soft skills. This suggests that for the majority of jobs, interpersonal effectiveness is the primary barrier to entry.

### 5.3 Job-Skill Correlation
We merged titles with skills to see specific requirements for top roles:
* **Hourly Supervisor & Training**: The top associated skill is **Inventory Management** (2,447 counts).
* **Store Manager**: The top associated skill is **Customer Service** (3,900 counts).

---

## 6. Recommendations

### For Job Seekers
1.  **Highlight Soft Skills**: Regardless of the industry, "Communication" and "Teamwork" should be prominent on your resume, as they are the most statistically relevant keywords.
2.  **Target High-Volume Roles**: For entry-level candidates, the retail and service sectors (Sales Associates, Shift Managers) offer the highest probability of employment due to sheer volume.

### For Employers
* **Standardize Titles**: The analysis required complex cleaning because titles like "Shift Manager" and "Shift Manager." were treated differently. Standardizing job titles in postings would improve visibility and searchability for candidates.

---
*Report generated based on analysis codes `eda_general.ipynb` and `job_title_and_skill_analysis.ipynb`.*