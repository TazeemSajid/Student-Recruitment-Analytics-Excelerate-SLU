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
| Dataset | Records | Purpose |
|---|---|---|
| learneropp1 | 113,602 | Central enrollment table |
| learner1 | 129,260 | Learner demographics |
| cognito2 | 34,111 | User identity & contact |
| cohort1 | — | Cohort program details |
| opportunity1 | 374 | Course & opportunity info |
| marketing1 | — | Campaign metrics |

### Master Table
