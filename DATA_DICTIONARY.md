# 📖 Data Dictionary
## Dataset: 2024 Data Science Job Postings

Used in both Data Jobs Dashboard V1.0 and V2.0

---

## Table: job_postings_fact (478,895 rows) — FACT TABLE

| Column | Data Type | Description | Sample Values |
|---|---|---|---|
| `job_id` | Integer | Unique identifier for each job posting | 0, 1, 2 … |
| `company_id` | Integer | Foreign key linking to company_dim | 0, 1, 2 … |
| `job_title_short` | String | Standardized job title category | `Data Analyst`, `Data Engineer`, `Data Scientist` |
| `job_title` | String | Full job title as posted by employer | `"Staff Data Analyst Operations, Infrastructure & Systems"` |
| `job_location` | String | City and state / country of the job | `"Fremont, CA"`, `"London, UK"` |
| `job_via` | String | Platform where the job was posted | `via LinkedIn`, `via Indeed`, `via ZipRecruiter` |
| `job_schedule_type` | String | Employment type | `Full-time`, `Contractor`, `Internship`, `Part-time` |
| `job_work_from_home` | Boolean | Whether the posting mentions remote/WFH | `True`, `False` |
| `search_location` | String | Geographic search region used to find posting | `"California, United States"` |
| `job_posted_date` | DateTime | Date and time the posting went live | `2024-01-01 00:00:01` |
| `job_no_degree_mention` | Boolean | Whether degree requirement is omitted | `True`, `False` |
| `job_health_insurance` | Boolean | Whether health insurance is mentioned | `True`, `False` |
| `job_country` | String | Country of the job location | `United States`, `India`, `United Kingdom` |
| `salary_rate` | String | Whether salary is yearly or hourly | `year`, `hour`, or null |
| `salary_year_avg` | Float | Average yearly salary listed (USD) | Range: varies; Median: $113,250 |
| `salary_hour_avg` | Float | Average hourly rate listed (USD) | Populated when `salary_rate = hour` |

---

## Table: company_dim (98,372 rows) — DIMENSION TABLE

| Column | Data Type | Description | Sample Values |
|---|---|---|---|
| `company_id` | Integer | Primary key — links to job_postings_fact | 0, 1, 2 … |
| `name` | String | Company name | `Tesla`, `BJ's Wholesale Club` |
| `link` | String | Company website URL | `http://www.tesla.com/` |
| `link_google` | String | Google search URL for the company | Full Google search URL |
| `thumbnail` | String | URL to company logo thumbnail | Encrypted Google image URL |

---

## Table: skills_dim (254 rows) — DIMENSION TABLE

| Column | Data Type | Description | Sample Values |
|---|---|---|---|
| `skill_id` | Integer | Primary key — links to skills_job_dim | 0, 1, 2 … |
| `skills` | String | Skill name (lowercase) | `python`, `sql`, `aws`, `tableau`, `excel` |
| `type` | String | Skill category | `programming`, `analyst_tools`, `cloud`, `databases`, `libraries`, `webframeworks`, `os`, `other` |

**Skill Categories Breakdown:**

| Category | Count | Examples |
|---|---|---|
| programming | 54 | python, sql, r, java, scala |
| libraries | 44 | pandas, numpy, tensorflow, pytorch |
| webframeworks | 29 | flask, django, fastapi |
| analyst_tools | 28 | tableau, power bi, excel, looker |
| other | 22 | — |
| cloud | 19 | aws, azure, gcp |
| databases | 18 | postgresql, mysql, mongodb |
| async | 16 | kafka, spark |

---

## Table: skills_job_dim (2,274,756 rows) — BRIDGE TABLE

| Column | Data Type | Description |
|---|---|---|
| `job_id` | Integer | Foreign key → job_postings_fact |
| `skill_id` | Integer | Foreign key → skills_dim |

This is the many-to-many bridge table connecting jobs to their required skills. One job can have multiple skills; one skill can appear in multiple jobs.

---

## Top 10 Skills by Job Posting Count

| Rank | Skill | Category | Job Mentions |
|---|---|---|---|
| 1 | Python | programming | 244,416 |
| 2 | SQL | programming | 240,179 |
| 3 | AWS | cloud | 100,386 |
| 4 | Azure | cloud | 93,849 |
| 5 | Tableau | analyst_tools | 73,560 |
| 6 | Spark | async | 71,979 |
| 7 | R | programming | 71,835 |
| 8 | Excel | analyst_tools | 71,807 |
| 9 | Power BI | analyst_tools | 66,183 |
| 10 | Java | programming | 51,294 |

---

## Job Title Distribution

| Job Title | Posting Count | % of Total |
|---|---|---|
| Data Engineer | 128,994 | 26.9% |
| Data Analyst | 112,866 | 23.6% |
| Data Scientist | 97,664 | 20.4% |
| Senior Data Engineer | 30,608 | 6.4% |
| Business Analyst | 28,584 | 6.0% |
| Software Engineer | 23,402 | 4.9% |
| Senior Data Scientist | 21,731 | 4.5% |
| Senior Data Analyst | 15,347 | 3.2% |
| Machine Learning Engineer | 12,860 | 2.7% |

---

## Data Quality Notes

- `salary_year_avg` and `salary_hour_avg` contain many nulls — most postings do not disclose salary
- `job_work_from_home` is Boolean stored as string `True`/`False` — converted to proper Boolean in Power Query
- `job_via` contains duplicate platform names with/without "via" prefix (e.g., `LinkedIn` and `via LinkedIn`) — consolidated during ETL
- `search_location` reflects the geographic filter used during data collection, not necessarily the actual job location

---

*Data Dictionary v1.0 · Data Jobs Market Dashboard V1.0 & V2.0*
