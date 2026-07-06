# Project Documentation: Workforce Retention & Talent Analytics Dashboard

## 1. Project Overview

This project is a Power BI workforce analytics dashboard designed to help HR leaders monitor employee retention, understand attrition drivers, and identify active employees who may require retention attention.

The report combines executive KPIs, retention-driver analysis, employee-level risk segmentation, and hiring-quality metrics into a four-page Power BI experience.

## 2. Business Questions

The dashboard is designed around the following analytical questions:

| Area | Questions Answered |
|---|---|
| Workforce health | How many employees are active, exited, and in the total historical population? |
| Workforce movement | How have hires, exits, and net workforce movement changed over time? |
| Department retention | Which departments have the highest historical exit rates? |
| Retention drivers | How do overtime, salary band, work-life balance, and job satisfaction relate to exits? |
| Risk management | Which active employees are classified as high or medium retention risk? |
| Talent growth | Which departments have higher promotion rates and how does promotion status relate to exit rate? |
| Hiring quality | Which recruitment sources bring the most employees, and which sources show higher exits? |

## 3. Source Files

| File | Description |
|---|---|
| `Workforce Retention & Talent Analytics Dashboard.pbix` | Power BI dashboard file. |
| `data/HR_Workforce_Retention.csv` | Employee-level HR source dataset. |
| `README.md` | Repository overview and usage guide. |
| `PROJECT_DOCUMENTATION.md` | Detailed technical and business documentation. |

## 4. Dataset Profile

| Item | Value |
|---|---:|
| Rows | 1,800 |
| Columns | 57 |
| Snapshot Date | 2026-05-31 |
| Earliest Hire Date | 2012-02-26 |
| Latest Hire Date | 2026-04-06 |
| Earliest Exit Date | 2023-03-01 |
| Latest Exit Date | 2026-05-05 |
| Active Employees | 1,557 |
| Exited Employees | 243 |
| Historical Exit Rate | 13.5% |

The dataset uses one row per employee. Active employees have `Attrition = No` and blank `ExitDate`; exited employees have `Attrition = Yes` and a populated `ExitDate`.

## 5. Data Dictionary

| Column | Type | Description |
|---|---|---|
| `EmpID` | Text | Unique employee identifier. |
| `Age` | Whole number | Employee age in years. |
| `AgeGroup` | Text | Age category used for demographic analysis. |
| `Attrition` | Text | Exit flag; `Yes` indicates an exited employee, `No` indicates active. |
| `EmploymentStatus` | Text | Active or Exited status label. |
| `BusinessTravel` | Text | Business travel frequency/category. |
| `DailyRate` | Decimal number | Daily pay/rate field from source data. |
| `Department` | Text | Employee department. |
| `DistanceFromHome(KM)` | Whole number | Distance from home to office in kilometers. |
| `Education` | Whole number | Numeric education level code. |
| `EducationLevel` | Text | Readable education level label. |
| `EducationField` | Text | Education specialization/field. |
| `EmployeeCount` | Whole number | Employee-count helper column, generally 1 per employee. |
| `EmployeeNumber` | Whole number | Numeric employee number. |
| `EnvironmentSatisfaction` | Whole number | Environment satisfaction score. |
| `Gender` | Text | Employee gender value in source data. |
| `JobInvolvement` | Whole number | Job involvement rating. |
| `JobLevel` | Whole number | Job level code. |
| `JobRole` | Text | Role or job family. |
| `JobSatisfaction` | Whole number | Job satisfaction score from 1 to 5. |
| `MaritalStatus` | Text | Employee marital status. |
| `MonthlyIncome` | Whole number | Monthly income amount. |
| `SalarySlab` | Text | Salary band label. |
| `SalarySlabOrder` | Whole number | Sort order for SalarySlab. |
| `HourlyRate` | Decimal number | Hourly rate field from source data. |
| `NumCompaniesWorked` | Whole number | Prior companies worked. |
| `Over18` | Text | Over-18 indicator from source data. |
| `OverTime` | Text | Overtime indicator; Yes/No. |
| `OvertimeHoursMonthly` | Whole number | Monthly overtime hours. |
| `SalaryHike %` | Whole number | Salary hike percentage. |
| `PerformanceRating` | Whole number | Performance rating. |
| `RelationshipSatisfaction` | Whole number | Relationship satisfaction score. |
| `StandardWorkingHours` | Whole number | Standard working-hours field. |
| `StockOptionLevel` | Whole number | Stock option level. |
| `TotalExperience(Years)` | Whole number | Total professional experience in years. |
| `TrainingsLastYear` | Whole number | Number of trainings attended last year. |
| `TrainingHours` | Whole number | Training hours. |
| `WorkLifeBalance` | Whole number | Work-life balance score from 1 to 5. |
| `YearsatCompany` | Whole number | Tenure at company in years. |
| `YearsinCurrentRole` | Whole number | Years in current role. |
| `YearsSincePromotion` | Whole number | Years since last promotion. |
| `YearsWithCurrManager` | Whole number | Years with current manager. |
| `HireDate` | Date | Employee hire date. |
| `ExitDate` | Date | Employee exit date; blank for active employees. |
| `OfficeLocation` | Text | Office/city/location. |
| `WorkMode` | Text | Hybrid, On-site, or Remote work mode. |
| `EmploymentType` | Text | Employment type/category. |
| `EngagementScore` | Whole number | Employee engagement score. |
| `AbsenceDays` | Whole number | Absence-day count. |
| `ManagerRating` | Whole number | Manager rating. |
| `AnnualBonus` | Whole number | Annual bonus amount. |
| `RecruitmentSource` | Text | Source/channel that hired the employee. |
| `LastPromotionDate` | Date | Most recent promotion date; blank when not promoted. |
| `PromotionStatus` | Text | Promoted or Not Yet Promoted. |
| `ExitReason` | Text | Exit reason for exited employees. |
| `SnapshotDate` | Date | Snapshot/reporting date for the data extract. |
| `AttritionFlag` | Whole number | Source field used in dashboard analysis. |

## 6. Power Query Preparation

The `Workforce` table is imported from a CSV source using Power Query. The transformation flow shown in the PBIX is:

1. **Source**: Load `HR_Workforce_Retention.csv` with comma delimiter and UTF-8 encoding.
2. **Promoted Headers**: Use the first row as column headers.
3. **Changed Type**: Assign correct data types for text, whole number, decimal number, and date fields.

Important type assignments:

| Type | Fields |
|---|---|
| Date | `HireDate`, `ExitDate`, `LastPromotionDate`, `SnapshotDate` |
| Whole Number | Age, education, satisfaction scores, tenure fields, engagement, absence, bonus, and ordering columns |
| Decimal Number | `DailyRate`, `HourlyRate` |
| Text | Employee IDs, department, demographic fields, status fields, labels, and exit reasons |

## 7. Data Model

The model uses a star-style layout with one employee fact table, one date dimension, and one dedicated measure table.

![Data Model](assets/screenshots/data-model.png)

### Tables

| Table | Role | Notes |
|---|---|---|
| `Workforce` | Fact table | Employee-level source table with all demographic, job, satisfaction, compensation, hiring, promotion, and exit fields. |
| `Date` | Dimension table | Calendar table used for time-series analysis of hires and exits. |
| `_Measures` | Measures table | Empty/helper table that stores DAX measures for cleaner model organization. |

### Relationships

| From | To | Cardinality | Status | Purpose |
|---|---|---|---|---|
| `Date[Date]` | `Workforce[HireDate]` | 1-to-many | Active | Supports hires and workforce movement by hire date. |
| `Date[Date]` | `Workforce[ExitDate]` | 1-to-many | Inactive | Activated in DAX to analyze exits by exit date. |

The measure `Exits in Period` uses `USERELATIONSHIP()` to activate the exit-date relationship and `CROSSFILTER()` to prevent the hire-date relationship from controlling the same calculation.

## 8. DAX Measures

The project stores measures in the `_Measures` table. The following measure definitions were extracted from the DAX measures workbook.

| Measure | DAX |
|---|---|
| `Total Employees` | `DISTINCTCOUNT(Workforce[EmpID])` |
| `Active Employees` | `CALCULATE(<br>    [Total Employees],<br>    Workforce[Attrition] = "No"<br>)` |
| `Exited Employees` | `CALCULATE(<br>    [Total Employees],<br>    Workforce[Attrition] = "Yes"<br>)` |
| `Historical Exit Rate` | `DIVIDE(<br>    [Exited Employees],<br>    [Total Employees]<br>)` |
| `Active Rate` | `DIVIDE(<br>    [Active Employees],<br>    [Total Employees]<br>)` |
| `Average Engagement Score` | `AVERAGE(Workforce[EngagementScore])` |
| `Average Tenure Years` | `AVERAGE(Workforce[YearsatCompany])` |
| `Average Monthly Income` | `AVERAGE(Workforce[MonthlyIncome])` |
| `Average Training Hours` | `AVERAGE(Workforce[TrainingHours])` |
| `Average Years Since Promotion` | `AVERAGE(Workforce[YearsSincePromotion])` |
| `Hires in Period` | `[Total Employees]` |
| `Exits in Period` | `CALCULATE(<br>    [Exited Employees],<br>    CROSSFILTER(<br>        'Date'[Date],<br>        Workforce[HireDate],<br>        NONE<br>    ),<br>    USERELATIONSHIP(<br>        'Date'[Date],<br>        Workforce[ExitDate]<br>    )<br>)` |
| `Net Workforce Movement` | `[Hires in Period] - [Exits in Period]` |
| `Overtime Rate Gap` | `[Overtime Exit Rate] - [No Overtime Exit Rate]` |
| `No Overtime Exit Rate` | `CALCULATE(<br>    [Historical Exit Rate],<br>    Workforce[OverTime] = "No"<br>)` |
| `Overtime Exit Rate` | `CALCULATE(<br>    [Historical Exit Rate],<br>    Workforce[OverTime] = "Yes"<br>)` |
| `High Risk Employees` | `CALCULATE(<br>    [Active Employees],<br>    Workforce[Risk Band] = "High Risk"<br>)` |
| `Medium Risk Employees` | `CALCULATE(<br>    [Active Employees],<br>    Workforce[Risk Band] = "Medium Risk"<br>)` |
| `Low Risk Employees` | `CALCULATE(<br>    [Active Employees],<br>    Workforce[Risk Band] = "Low Risk"<br>)` |
| `High Risk Rate` | `DIVIDE(<br>    [High Risk Employees],<br>    [Active Employees]<br>)` |
| `Medium Risk Rate` | `DIVIDE(<br>    [Medium Risk Employees],<br>    [Active Employees]<br>)` |
| `Promoted Employees` | `CALCULATE(<br>    [Total Employees],<br>    Workforce[PromotionStatus] = "Promoted"<br>)` |
| `Promotion Rate` | `DIVIDE(<br>    [Promoted Employees],<br>    [Total Employees]<br>)` |
| `Risk Band Background Color` | `SWITCH(<br>    SELECTEDVALUE(Workforce[Risk Band]),<br>    "High Risk", "#F88379",<br>    "Medium Risk", "#FBE0A6",<br>    "Low Risk", "#B6D7A8",<br>    "#FFFFFF"<br>)` |

## 9. Calculated Labels and Model Fields

Several report visuals use readable labels and segmentation fields that improve chart sorting and interpretation.

### Work-Life Balance Label

| Source Score | Label |
|---:|---|
| 1 | 1 - Poor |
| 2 | 2 - Below Average |
| 3 | 3 - Average |
| 4 | 4 - Good |
| 5 | 5 - Excellent |

### Job Satisfaction Label

| Source Score | Label |
|---:|---|
| 1 | Very Low |
| 2 | Low |
| 3 | Medium |
| 4 | High |
| 5 | Very High |

### Risk Band

The report uses a `Risk Band` field to classify active employees as `High Risk`, `Medium Risk`, or `Low Risk`.

Current dashboard reconciliation:

| Risk Band | Active Employees | Share of Active Employees |
|---|---:|---:|
| High Risk | 14 | 0.9% |
| Medium Risk | 158 | 10.1% |
| Low Risk | 1,385 | 89.0% |

In the current dataset, the **High Risk** count reconciles to active employees with `EngagementScore < 50` and `JobSatisfaction <= 2`. The full risk-band formula should be maintained in the PBIX model so threshold changes can be controlled consistently.

## 10. Dashboard Page Documentation

### Page 1: Executive Workforce Overview

![Executive Workforce Overview](assets/screenshots/executive-workforce-overview.png)

Purpose: provide an executive-level summary of total workforce size, active employees, exits, engagement, and movement trends.

KPI cards:

| KPI | Value |
|---|---:|
| Total Employees | 1,800 |
| Active Employees | 1,557 |
| Exited Employees | 243 |
| Historical Exit Rate | 13.5% |
| Average Engagement Score | 75.1 |

Visuals:

| Visual | Description |
|---|---|
| Hiring, Exits and Net Workforce Movement | Combo trend showing hires, exits, and net movement by year. |
| Exit Rate by Department | Department-level historical exit rate ranking. |
| Workforce Distribution by Work Mode | Active workforce share by Hybrid, On-site, and Remote work modes. |
| Key Observation / Recommended Action | Narrative callout highlighting Product and Sales exit-rate concerns. |

Department exit-rate output:

| Department | Historical Exit Rate |
|---|---:|
| Product | 15.7% |
| Sales | 14.9% |
| Operations | 13.9% |
| Engineering | 13.5% |
| Human Resources | 12.9% |
| Customer Success | 12.9% |
| Finance | 11.4% |
| Marketing | 10.8% |

### Page 2: Retention Drivers

![Retention Drivers](assets/screenshots/retention-drivers.png)

Purpose: identify factors associated with employee exits.

KPI cards:

| KPI | Value |
|---|---:|
| Exited Employees | 243 |
| Overtime Rate Gap | 12.7 percentage points |
| No Overtime Exit Rate | 9.7% |
| Overtime Exit Rate | 22.4% |

Visual insights:

| Driver | Interpretation |
|---|---|
| Exit Reasons | Better Opportunity, Compensation, and Work-Life Balance are the top recorded exit reasons. |
| Overtime | Employees with overtime show materially higher exits. |
| Salary Band | Lower salary bands show higher exit rates than higher bands. |
| Work-Life Balance | Poor and below-average work-life balance correlate with higher exits. |
| Job Satisfaction | Very low and low job satisfaction categories show the highest exit rates. |

Exit reasons:

| Exit Reason | Exited Employees |
|---|---:|
| Better Opportunity | 57 |
| Compensation | 51 |
| Work-Life Balance | 43 |
| Manager / Team | 30 |
| Personal Reasons | 20 |
| Relocation | 17 |
| Career Change | 15 |
| Higher Studies | 10 |

Exit rate by salary band:

| Salary Band | Exit Rate |
|---|---:|
| Below 5 LPA | 18.0% |
| 5-8 LPA | 14.5% |
| 8-12 LPA | 14.2% |
| 12-18 LPA | 13.1% |
| 18-25 LPA | 8.3% |
| 25+ LPA | 6.5% |

Exit rate by work-life balance:

| Work-Life Balance | Exit Rate |
|---|---:|
| 1 - Poor | 25.0% |
| 2 - Below Average | 21.8% |
| 3 - Average | 14.8% |
| 4 - Good | 9.2% |
| 5 - Excellent | 6.5% |

Exit rate by job satisfaction:

| Job Satisfaction | Exit Rate |
|---|---:|
| Very Low | 29.4% |
| Low | 18.7% |
| Medium | 15.1% |
| High | 11.2% |
| Very High | 8.0% |

### Page 3: Retention Risk

![Retention Risk](assets/screenshots/retention-risk.png)

Purpose: prioritize active employees who may require engagement or retention intervention.

KPI cards:

| KPI | Value |
|---|---:|
| High Risk Employees | 14 |
| Medium Risk Employees | 158 |
| High Risk Rate | 0.9% |
| Active Employees | 1,557 |

Visuals:

| Visual | Description |
|---|---|
| Risk Band slicer | Lets users filter risk-related visuals by risk band. |
| Active Employees by Risk Band | Donut chart showing high, medium, and low risk active employees. |
| High Risk Rate by Department | Department-level rate of high-risk active employees. |
| Risk table | Employee-level table with risk, employee ID, department, engagement, satisfaction, overtime, and years since promotion. |

High risk by department is concentrated most strongly in Engineering, followed by Sales in the current dataset.

### Page 4: Talent Growth & Hiring Quality

![Talent Growth & Hiring Quality](assets/screenshots/talent-growth-hiring-quality.png)

Purpose: analyze whether employee growth, promotion, training, and recruitment channels are associated with better retention outcomes.

KPI cards:

| KPI | Value |
|---|---:|
| Promotion Rate | 52.0% |
| Average Training Hours | 29.5 |
| Average Years Since Promotion | 1.3 |
| Average Monthly Income | ₹99,129 |

Visuals:

| Visual | Description |
|---|---|
| Promotion Rate by Department | Compares department-level promotion rates. |
| Exit Rate by Promotion Status | Compares exit rates for promoted vs not-yet-promoted employees. |
| Exit Rate by Recruitment Source | Highlights which hiring channels have higher exit rates. |
| Employees Hired by Recruitment Source | Shows volume contribution from each recruitment channel. |

Promotion rate by department:

| Department | Promotion Rate |
|---|---:|
| Product | 57.5% |
| Engineering | 56.9% |
| Human Resources | 54.3% |
| Marketing | 54.2% |
| Finance | 52.8% |
| Sales | 50.2% |
| Operations | 48.8% |
| Customer Success | 43.4% |

Exit rate by promotion status:

| Promotion Status | Exit Rate |
|---|---:|
| Not Yet Promoted | 16.4% |
| Promoted | 10.8% |

Exit rate by recruitment source:

| Recruitment Source | Historical Exit Rate |
|---|---:|
| Job Portal | 16.7% |
| Company Careers Page | 14.1% |
| Employee Referral | 13.8% |
| Campus Hiring | 13.3% |
| Recruitment Agency | 13.1% |
| LinkedIn | 11.9% |

Employees hired by recruitment source:

| Recruitment Source | Employees |
|---|---:|
| Employee Referral | 507 |
| LinkedIn | 419 |
| Company Careers Page | 347 |
| Recruitment Agency | 222 |
| Campus Hiring | 173 |
| Job Portal | 132 |

## 11. Validation Checks

The following checks were used to validate the dashboard-level numbers:

| Check | Result |
|---|---:|
| Active + Exited = Total Employees | 1,557 + 243 = 1,800 |
| Historical Exit Rate | 243 / 1,800 = 13.5% |
| Average Engagement Score | 75.1 |
| Promotion Rate | 936 / 1,800 = 52.0% |
| High Risk Rate | 14 / 1,557 = 0.9% |

## 12. Refresh and Maintenance Process

Use this process when updating the dataset:

1. Replace or update `data/HR_Workforce_Retention.csv`.
2. Open `HR_Dashboard.pbix` in Power BI Desktop.
3. Open **Transform Data** and confirm the source path points to the updated CSV.
4. Confirm the `Changed Type` step still assigns correct data types.
5. Refresh the report.
6. Verify KPI cards:
   - Total Employees
   - Active Employees
   - Exited Employees
   - Historical Exit Rate
   - Average Engagement Score
7. Review risk-band and label calculations if source field names or thresholds change.
8. Publish or commit the updated PBIX and screenshots.

## 13. Known Limitations

- The dataset is a snapshot-style HR dataset, not a live HRIS connection.
- 2026 data is partial as of the snapshot date, so annual trend comparisons should be interpreted carefully.
- The risk band is a rule-based classification and is not a predictive machine-learning model.
- Exit reasons are only available for exited employees.
- Blank exit dates are expected for active employees.
- The PBIX uses a local CSV file path; refresh requires path adjustment after cloning unless parameterized.

## 14. Recommended Enhancements

Potential future improvements:

- Add a Power Query parameter for the CSV source path.
- Add a dedicated data-quality page.
- Add tenure-band analysis and cohort retention curves.
- Add manager or location filters if business rules allow.
- Build a separate attrition prediction model outside Power BI and import risk scores.
- Add role-level drillthrough pages.
- Add row-level security for department-level HR users.
- Add a formal metric glossary page inside the report.