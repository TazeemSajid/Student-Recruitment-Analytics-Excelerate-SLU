# 🎓 Student Recruitment Analytics: Data Pipeline, BI Design & Dashboard Development
### Excelerate × Saint Louis University (SLU) — Data Visualization Associate Internship

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue)
![SQL](https://img.shields.io/badge/SQL-Advanced-blue)
![Looker Studio](https://img.shields.io/badge/Looker%20Studio-Dashboard-orange)
![Excel](https://img.shields.io/badge/Excel-Mapping%20Table-green)
![Figma](https://img.shields.io/badge/Figma-Wireframing-purple)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Project Overview
A real-world analytics internship project completed at **Excelerate, powered by Saint Louis 
University (SLU)**. The project covers a complete analytics workflow — from raw data 
exploration to ETL pipeline, data modeling, dashboard wireframing and final 
visualization delivery — across learner enrollment and marketing campaign datasets.

---

## 🧠 Business Problem
The organization needed to:
- Understand learner enrollment patterns and dropout trends
- Track cohort performance and program effectiveness
- Analyze marketing campaign efficiency and ROI
- Deliver actionable insights through interactive dashboards

---

## 🛠️ Tech Stack
| Tool | Purpose |
|---|---|
| PostgreSQL | Data storage, quality checks & ETL pipeline |
| SQL | Data cleaning, transformation & analysis |
| Excel | Mapping table & dashboard field documentation |
| Google Looker Studio | Interactive dashboard development |
| Figma | Dashboard wireframing & UX planning |

---

## 🔄 Project Phases

### 📍 Week 1 — Data Exploration
- Explored 6 raw datasets covering learner enrollment, demographics, cohort programs, opportunities and marketing campaigns
- Identified primary and foreign key relationships across tables
- Planned ETL pipeline structure based on data relationships

### 📍 Week 2 — ETL Pipeline & Master Table Creation
- Performed comprehensive data quality assessment using SQL
- Detected and resolved:
  - Missing values in critical fields
  - Duplicate enrollment records
  - Inconsistent date formats
  - Orphan records with broken foreign key references
- Built raw master table joining 5 datasets using LEFT JOIN logic
- Cleaned 113,602 raw records → 51,108 validated clean records
- Applied ROW_NUMBER() for deduplication
- Used TO_DATE() and TO_TIMESTAMP() for date standardization
- Validated foreign key integrity across all tables

### 📍 Week 3 — BI Design & Dashboard Planning
- Designed detailed Mapping Table in Excel linking dashboard fields to source columns
- Classified all fields into KPIs, supporting metrics and dimensions
- Created annotated wireframes in Figma for:
  - Learner Analytics Dashboard
  - Marketing Campaign Dashboard
- Documented business logic and transformation rules for each field

### 📍 Week 4 — Dashboard Development
- Built interactive dashboards in Google Looker Studio
- Learner Dashboard covering:
  - Total enrollments & dropout rate
  - Gender distribution & age analysis
  - Cohort performance comparison
  - Enrollment trend over time
  - Regional learner distribution
- Marketing Dashboard covering:
  - Total campaigns & amount spent (AED)
  - Cost per result by campaign
  - Reach vs outbound clicks scatter plot
  - Top performing campaigns by results

---

## 🗄️ Data Model

### Datasets Used
| Dataset | Description | Rows | Columns |
|---|---|---|---|
| User Data (learner1) | User profiles, sign-up info, education & location | 129,259 | 5 |
| Opportunity Data (opportunity1) | Learning opportunities & program information | 187 | 5 |
| Cohort Data (cohort1) | Cohorts, associated opportunities & timeframes | 639 | 5 |
| Marketing Data (marketing1) | Campaign impressions, engagement & cost metrics | 148 | 13 |
| Learner Opportunity (learneropp1) | Maps learners to opportunities & application metadata | 113,602 | 6 |
| Cognito Data (cognito2) | Profile authentication, gender, city & timestamps | 13,318 | 5 |

---

### Master Table Creation

master_table_clean (51,108 rows | 29 columns)

├── learneropp1  (113,602 rows) → enrollment_id, learner_id,

│                                  assigned_cohort, apply_date, status

├── learner1     (129,259 rows) → country, degree, institution, major

├── cognito2      (13,318 rows) → email, gender, birthdate, city, state

├── cohort1          (639 rows) → cohort_code, start_date, end_date, size

└── opportunity1     (187 rows) → opportunity_id, opportunity_name, category

<img width="1536" height="1024" alt="Database Table Relationships and Keys Diagram" src="https://github.com/user-attachments/assets/98389fba-e1b7-480e-b18c-eadf65f86710" />


### Key Relationships
| From | To | Join Key |
|---|---|---|
| learneropp1.enrollment_id | learner1.learner_id | enrollment ↔ user |
| learneropp1.enrollment_id | cognito2.user_id | enrollment ↔ profile |
| learneropp1.assigned_cohort | cohort1.cohort_code | enrollment ↔ cohort |
| learneropp1.learner_id | opportunity1.opportunity_id | enrollment ↔ program |

> ⚠️ Note: marketing1 dataset was NOT joined into the master table
> as it has no matching foreign keys. It was used separately
> for marketing campaign visualization only.


---

## 🧹 ETL Pipeline Summary

```sql
-- Step 1: Create Raw Master Table
CREATE TABLE master_table_raw AS
SELECT lo.enrollment_id, lo.learner_id, lo.assigned_cohort,
       lo.apply_date, lo.status, l.country, l.degree,
       c.email, c.gender, c.birthdate,
       co.cohort_code, co.start_date, co.end_date,
       o.opportunity_name, o.category
FROM learneropp1 lo
LEFT JOIN learner1 l    ON lo.enrollment_id = l.learner_id
LEFT JOIN cognito2 c    ON lo.enrollment_id = c.user_id
LEFT JOIN cohort1 co    ON lo.assigned_cohort = co.cohort_code
LEFT JOIN opportunity1 o ON lo.learner_id = o.opportunity_id;

-- Step 2: Remove Duplicates
CREATE TABLE master_table_noduplicates AS
SELECT * FROM (
    SELECT *, ROW_NUMBER() OVER (
        PARTITION BY enrollment_id ORDER BY enrollment_id
    ) AS rn FROM master_table_raw
) t WHERE rn = 1;

-- Step 3: Remove Nulls in Critical Fields
CREATE TABLE master_table_nonulls AS
SELECT * FROM master_table_noduplicates
WHERE enrollment_id IS NOT NULL
  AND email IS NOT NULL
  AND apply_date IS NOT NULL;

-- Step 4: Fix Date Formats
CREATE TABLE master_table_clean AS
SELECT *,
    TO_DATE(birthdate, 'MM/DD/YYYY') AS birthdate_clean,
    TO_TIMESTAMP(usercreatedate, 'YYYY-MM-DD HH24:MI:SS') AS created_clean
FROM master_table_nonulls;
```

---

## 📊 Data Quality Results

| Check | Result |
|---|---|
| Raw Records | 113,602 |
| Cleaned Records | 51,108 |
| Records Removed | 55,636 |
| Duplicates Found | 20,572 enrollment IDs |
| Null Values After Cleaning | 0 |
| Foreign Key Violations | 0 |
| Date Format Issues | Resolved ✅ |

---

## 📈 Dashboard Screenshots

### Learner Analytics Dashboard
<img width="2434" height="1392" alt="Learner Enrollment" src="https://github.com/user-attachments/assets/8fb11597-dba5-4292-82d8-c8cb3630c4c1" />


### Marketing Campaign Dashboard
<img width="2290" height="1052" alt="Marketing Data" src="https://github.com/user-attachments/assets/721a0a70-8343-40dc-b7be-a688bd5fd9c6" />


---

## 🔍 Key Insights Delivered
- Identified dropout patterns across cohorts and regions
- Mapped enrollment trends over time for strategic planning
- Analyzed marketing campaign cost efficiency in AED
- March Brand Awareness campaign had highest total results
- Delivered actionable recommendations for learner retention

---

## 💡 Business Recommendations
1. Focus retention efforts on cohorts with highest dropout rates
2. Reallocate marketing budget toward cost-efficient campaigns
3. Expand outreach in high-enrollment regions
4. Standardize data collection to reduce null values in future

---

## 📄 Reports
> Reports available in the `reports/` folder

---

## 👤 Author
**Muhammad Tazeem Sajid**
- 🔗 LinkedIn: `linkedin.com/in/muhammad-tazeem-sajid-2642622b1`
- 💻 GitHub: `github.com/TazeemSajid`
- 📧 Email: `tazeemsajid22@gmail.com`
