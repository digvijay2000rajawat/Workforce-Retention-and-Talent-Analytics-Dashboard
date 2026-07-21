# HR Workforce Retention Analytics

A Power BI workforce analytics project for monitoring headcount, employee movement, attrition drivers, retention risk, promotions, and hiring quality.

**Important:** 2026 hiring and exit trends are partial-year figures.

## Project Summary

The dashboard converts employee-level HR data into an executive view of workforce health. It supports analysis across department, office location, job role, employment type, work mode, recruitment source, hire year, and exit year.

The report is organized into four pages:

1. **Workforce Executive Overview** — headcount, active and exited employees, historical exit rate, engagement, workforce movement, work mode, and department attrition.
2. **Employee Retention Drivers** — exit reasons, overtime, salary bands, work-life balance, and job satisfaction.
3. **Employee Retention Risk** — active-employee risk segmentation and department-level retention hotspots.
4. **Talent Growth & Hiring Quality** — promotion, training, compensation, recruitment-source volume, and source-level exit rates.

## Key Results

| Indicator | Result |
|---|---:|
| Total employees | 1,800 |
| Active employees | 1,557 |
| Exited employees | 243 |
| Historical exit rate | 13.5% |
| Average engagement score | 75.1 |
| High-risk active employees | 14 |
| Medium-risk active employees | 158 |
| Overall promotion rate | 52.0% |
| Average training hours | 29.5 |
| Average monthly income | ₹99,129 |

Notable findings:

- Product and Sales have the highest historical exit rates at **15.7%** and **14.9%**.
- Employees working overtime exit at **22.4%**, compared with **9.7%** for employees without overtime, a gap of approximately **12.8 percentage points**.
- The most common recorded exit reasons are Better Opportunity, Compensation, and Work-Life Balance.
- Exit rates decline from **25.0%** for poor work-life balance to **6.5%** for excellent work-life balance.
- Exit rates decline from **29.4%** for very low job satisfaction to **8.0%** for very high job satisfaction.
- Employees not yet promoted have a **16.4%** exit rate, compared with **10.8%** among promoted employees.
- Job Portal has the highest recruitment-source exit rate at **16.7%**; LinkedIn has the lowest at **11.9%**.
- Employee Referral is the largest hiring source, with **507** employees.

## Dataset

The dataset contains **1,800 employee records and 56 columns**. It includes employee profile, employment status, compensation, satisfaction, engagement, tenure, promotion, recruitment, attendance, work arrangement, hire date, exit date, and exit reason fields.

See [PROJECT_DOCUMENTATION.md](PROJECT_DOCUMENTATION.md) for the full data dictionary, metric definitions, transformation logic, validation checks, and recommended Power BI model.

## Getting Started

### Prerequisites

- Power BI Desktop
- Git
- A spreadsheet or text editor for inspecting the CSV

### Load the Data in Power BI

1. Clone or download this repository.
2. Open Power BI Desktop.
3. Select **Get data → Text/CSV**.
4. Choose `HR_Workforce_Retention.csv`.
5. Select **Transform Data** and apply the data types documented in `PROJECT_DOCUMENTATION.md`.
6. Create the calculated columns and measures required for the dashboard.
7. Add the supplied images to the repository documentation or replace them after dashboard updates.

### Recommended Refresh Process

1. Replace `HR_Workforce_Retention.csv` with the refreshed file while preserving the column names.
2. Refresh the Power BI model.
3. Run the validation checks listed in the project documentation.
4. Commit the data, report changes, documentation, and screenshots together.

## Core Metric Definitions

- **Historical Exit Rate** = Exited Employees ÷ Total Employees
- **Promotion Rate** = Promoted Employees ÷ Total Employees
- **Overtime Exit Rate** = Exited Employees with overtime ÷ Employees with overtime
- **No Overtime Exit Rate** = Exited Employees without overtime ÷ Employees without overtime
- **Net Workforce Movement** = Hires in period − Exits in period
- **Department High-Risk Rate** = High-Risk Active Employees ÷ Active Employees in the department

Rates are filter-responsive in the dashboard.

## Limitations

- The dataset is a single snapshot dated 31 May 2026 rather than a complete event-history model.
- 2026 comparisons are partial-year and should not be compared directly with full calendar years without adjustment.
- Exit rate is a historical record ratio, not an annualized attrition rate.
- Risk Band is derived in the report and is not stored in the source CSV.
- Employee-level identifiers should be handled as confidential data in a real organizational deployment.

## Documentation

Detailed technical and business documentation is available in:

- [PROJECT_DOCUMENTATION.md](PROJECT_DOCUMENTATION.md)