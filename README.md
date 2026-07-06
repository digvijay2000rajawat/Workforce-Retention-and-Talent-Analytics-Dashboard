# Workforce Retention & Talent Analytics Dashboard

A Power BI dashboard project for monitoring workforce health, attrition, retention risk, and hiring quality across departments, work modes, compensation bands, satisfaction levels, and recruitment sources.

The report is built from an employee-level HR dataset with **1,800 employees**, including **1,557 active employees** and **243 exited employees** as of **May 31, 2026**.


## Business Goal

The dashboard helps HR and leadership teams answer four core questions:

1. How is the workforce changing over time?
2. Which departments and employee segments show the highest attrition?
3. Which active employees are at elevated retention risk?
4. Which hiring and talent-growth channels are associated with healthier retention outcomes?

## Key Findings

| Metric | Value |
|---|---:|
| Total Employees | 1,800 |
| Active Employees | 1,557 |
| Exited Employees | 243 |
| Historical Exit Rate | 13.5% |
| Average Engagement Score | 75.1 |
| Promotion Rate | 52.0% |
| High Risk Employees | 14 |
| Medium Risk Employees | 158 |

Notable insights from the current dataset:

- **Product** and **Sales** have the highest historical exit rates at **15.7%** and **14.9%**.
- Employees working overtime exit at **22.4%**, compared with **9.7%** for employees without overtime.
- Low work-life balance and low job satisfaction are strongly associated with higher exit rates.
- **Job Portal** hires show the highest exit rate among recruitment sources at **16.7%**.
- Employee Referral and LinkedIn are the largest hiring sources by headcount.

## Dashboard Pages

### 1. Executive Workforce Overview


High-level KPIs and workforce movement trends:

- Total, active, and exited employees
- Historical exit rate
- Average engagement score
- Hiring, exits, and net movement by year
- Exit rate by department
- Active workforce distribution by work mode

### 2. Retention Drivers


Deep dive into attrition drivers:

- Exit reasons
- Overtime vs no-overtime exit rate
- Exit rate by annual salary band
- Exit rate by work-life balance
- Exit rate by job satisfaction

### 3. Retention Risk


Active employee risk segmentation:

- High, medium, and low risk employee counts
- High risk rate by department
- Active employees by risk band
- Employee-level risk table with department, engagement, satisfaction, overtime, and years since promotion

### 4. Talent Growth & Hiring Quality


Talent growth and hiring-channel analysis:

- Promotion rate
- Training hours
- Years since promotion
- Average monthly income
- Promotion rate by department
- Exit rate by promotion status
- Exit rate by recruitment source
- Employees hired by recruitment source

## Data Model

The Power BI model contains three main tables:

| Table | Purpose |
|---|---|
| `Workforce` | Employee-level fact table with demographic, compensation, engagement, promotion, hiring, and exit fields. |
| `Date` | Calendar table used for hire and exit trend analysis. |
| `_Measures` | Dedicated table that stores DAX measures used across all dashboard pages. |


The model uses `Date[Date]` to analyze workforce movement by hire and exit dates. The active relationship supports hiring-period analysis, while the inactive relationship to exit date is activated in DAX for exit-period calculations.

## Tools Used

- **Power BI Desktop** for data modeling and dashboard development
- **Power Query** for CSV import, header promotion, and column type assignment
- **DAX** for KPI, attrition, promotion, overtime, and risk measures
- **CSV** as the source data format

## How to Use

1. Clone or download this repository.
2. Open `Workforce Retention & Talent Analytics Dashboard.pbix` in Power BI Desktop.
3. In Power Query, update the CSV source path if your local file path differs.
4. Refresh the model.
5. Review the four report pages and validate KPI cards against the source data.

## Notes

- The 2026 data is partial as of the mid-year snapshot date.
- `ExitDate` is blank for active employees.
- Retention-risk bands are dashboard/model classifications for prioritization, not predictions from a machine-learning model.
- The dataset appears to be prepared for portfolio/demo analytics and should not be treated as production HR data without governance review.