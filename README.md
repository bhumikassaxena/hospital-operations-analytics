# Hospital Operations Analytics

SQL, MySQL & Tableau analysis of 100,000 hospital patient records — uncovering patterns in admissions, readmissions, treatment costs, and length of stay to support hospital operations decision-making.

## Overview

This project analyzes a synthetic dataset of 100,000 hospital patients across five relational tables (patients, outcomes, diagnoses, medications, lab results). It combines SQL for data querying and analysis with Tableau for interactive visualization, answering key operational questions: Which diagnoses drive the highest costs? Which conditions have the highest readmission rates? How does length of stay relate to cost and diagnosis?

## Tools & Tech

- **SQL / MySQL** — data querying, joins, aggregation, subqueries, window functions
- **Tableau** — primary dashboarding and visualization
- **Power BI** — secondary dashboard (see below)

## Tableau Dashboards

Four interactive dashboards, each focused on a specific operational question:

1. **Patient & Admissions Overview** — high-level KPIs (total patients, average cost, average length of stay, readmission rate) and average charges by insurance type
2. **Readmissions Dashboard** — which diagnoses drive the highest 30-day readmission rates
3. **Treatment Cost Dashboard** — which diagnoses drive the highest total treatment costs, and how cost relates to length of stay
4. **Length of Stay Dashboard** — which diagnoses are associated with longer hospital stays

 ![Patient & Admissions Overview](tableau/patient_admissions_overview.png)

![Readmissions Dashboard](tableau/readmissions_dashboard.png)

![Treatment Cost Dashboard](tableau/treatment_cost_dashboard.png)

![Length of Stay Dashboard](tableau/length_of_stay_dashboard.png)

🔗 [View the interactive Length of Stay Dashboard on Tableau Public](https://public.tableau.com/views/Healthcare_AnalyticalDashboard_July_2/LengthofStayDashboard?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


## SQL Analysis

The `/sql` folder contains a 20-query script progressing from beginner to advanced SQL, covering:

- Filtering, sorting, and aggregation (`WHERE`, `ORDER BY`, `GROUP BY`, `HAVING`)
- Multi-table joins (`INNER JOIN`, `LEFT JOIN`)
- Derived columns with `CASE WHEN`
- Subqueries, including two anti-join patterns (`NOT IN`, `LEFT JOIN` + `IS NULL`) and an `EXISTS`-based join as an alternative to `INNER JOIN`
- Window functions (`RANK()', 'PARTITION BY`) and CTEs (`WITH`)
- Correlated subqueries, and a rewrite of one as an optimized join (a correlated subquery recalculating a per-group average across 100,000 rows was rewritten as a derived-table join — cutting a 30+ second query down to near-instant by computing each group average once instead of once per row)

📄 [View the full SQL script](sql/hospital_operations_queries.sql)

## Power BI Dashboard (Bonus)

I also built this analysis as a Power BI dashboard to demonstrate versatility across BI tools.

![Power BI Dashboard](powerbi/powerbi_dashboard.png)

## Data Notes

- The database contains **100,000 patients**, of whom **11,001 were admitted** (one admission each, 2018–2024). Cost, length of stay, and readmission metrics are based on these 11,001 admissions.
- The diagnoses table is a visit history (274,592 rows), so a patient can have more than one condition. In the diagnosis charts, each admitted patient appears once **for every condition in their history**. Rates and averages by condition should be read as "among admitted patients with a history of this condition," and **totals by condition overlap, so they don't add up** to overall charges ($200.5M).
- I found this while validating dashboard totals against the source table, and relabeled the Tableau charts to reflect it. The Power BI dashboard uses admission-level measures and is not affected.

## Key Insights

- Admitted patients with hypertension, obesity, and hyperlipidemia account for the most total charges, mainly because they are the largest patient groups. Average cost per admission is similar across conditions (~$17K–$19K).
- Admitted patients with type 1 diabetes have the highest 30-day readmission rate (~21%) and the longest average stay (5.8 days), though this group is small (103 patients), so the rate should be read with caution.
- Overall, the average length of stay is 5.2 days and the 30-day readmission rate is 14.9%.
- Insurance type shows relatively little variation in average charges ($17.3K–$19.1K), suggesting cost drivers are more clinical than payer-related.
  
## Repository Structure

```
hospital-operations-analytics/
├── README.md
├── sql/
│   └── hospital_operations_queries.sql
├── tableau/
│   └── dashboard screenshots
└── powerbi/
    └── powerbi_dashboard.png
```

