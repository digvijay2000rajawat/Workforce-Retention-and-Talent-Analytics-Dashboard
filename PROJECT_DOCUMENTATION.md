# Project Documentation: HR Workforce Retention Analytics

## 1. Document Control

| Item | Value |
|---|---|
| Project | HR Workforce Retention Analytics |
| Primary data file | `HR_Workforce_Retention.csv` |
| Reporting tool | Power BI |
| Snapshot date | 31 May 2026 |
| Dataset size | 1,800 rows × 56 columns |
| Report pages | 4 |
| Documentation status | GitHub-ready |
| 2026 data status | Partial year through the snapshot date |

## 2. Purpose

This project provides a consolidated analytical view of workforce size, employee engagement, historical exits, retention drivers, active-employee risk, internal growth, and recruitment-source quality.

The dashboard is intended for:

- HR leadership
- People analytics teams
- Department heads
- Talent acquisition teams
- Learning and development teams
- Workforce planning and finance partners

## 3. Business Objectives

The report is designed to answer the following questions:

1. What is the current active workforce and historical exit rate?
2. Which departments have the highest historical attrition?
3. How have hiring, exits, and net workforce movement changed over time?
4. Which work arrangements represent the active workforce?
5. Which factors are most strongly associated with employee exits?
6. How large is the overtime-related exit-rate gap?
7. Which active employees and departments require retention attention?
8. How are promotions, training, and compensation distributed?
9. Which recruitment sources deliver the most hires?
10. Which recruitment sources have the strongest or weakest retention outcomes?

## 4. Scope

### Included

- Employee-level snapshot analysis
- Active and exited employees
- Historical exit ratios
- Department, role, location, work mode, and employment type analysis
- Overtime, salary, satisfaction, work-life balance, and exit-reason analysis
- Promotion and training metrics
- Recruitment-source volume and exit rates
- Report-derived retention-risk segmentation
- Yearly hiring and exit movement

### Excluded

- Payroll reconciliation
- Vacancy and requisition tracking
- Time-to-hire and cost-per-hire
- Formal survival analysis
- Forecasted attrition probability
- Employee survey question-level analysis
- Annualized voluntary-turnover calculations
- Replacement cost and productivity-loss estimates

## 5. Source Data Profile

| Property | Value |
|---|---|
| Records | 1,800 |
| Unique employee IDs | 1,800 |
| Active employees | 1,557 |
| Exited employees | 243 |
| Earliest hire date | 26 February 2012 |
| Latest hire date | 6 April 2026 |
| Earliest exit date | 1 March 2023 |
| Latest exit date | 5 May 2026 |
| Snapshot date | 31 May 2026 |
| Departments | 8 |
| Job roles | 34 |
| Office locations | 7 |
| Recruitment sources | 6 |

### Null Handling

Nulls are expected in three fields:

| Field | Null count | Reason |
|---|---:|---|
| `ExitDate` | 1,557 | Active employees have not exited. |
| `ExitReason` | 1,557 | Active employees do not have an exit reason. |
| `LastPromotionDate` | 864 | Employees classified as Not Yet Promoted do not have a promotion date. |

No other source columns contain null values.

## 6. Data Preparation

Recommended Power Query steps:

1. Import `HR_Workforce_Retention.csv`.
2. Promote the first row to headers.
3. Trim and clean all text columns.
4. Assign date types to `HireDate`, `ExitDate`, `LastPromotionDate`, and `SnapshotDate`.
5. Assign whole-number types to counts, score fields, tenure fields, income, bonus, and ordering fields.
6. Assign decimal-number types to `DailyRate` and `HourlyRate`.
7. Confirm `EmpID` remains text so leading formatting is preserved.
8. Confirm `SalarySlab` is sorted by `SalarySlabOrder`.
9. Confirm active rows have blank `ExitDate` and `ExitReason`.
10. Confirm exited rows have an exit date and a populated exit reason.
11. Add `HireYear` and `ExitYear` for movement analysis, or create dedicated date dimensions.
12. Hide technical sort and helper columns from report consumers where appropriate.

Suggested calculated columns:

```DAX
Hire Year =
YEAR('HR_Workforce_Retention'[HireDate])
```

```DAX
Exit Year =
IF(
    ISBLANK('HR_Workforce_Retention'[ExitDate]),
    BLANK(),
    YEAR('HR_Workforce_Retention'[ExitDate])
)
```

```DAX
Job Satisfaction Label =
SWITCH(
    'HR_Workforce_Retention'[JobSatisfaction],
    1, "Very Low",
    2, "Low",
    3, "Medium",
    4, "High",
    5, "Very High"
)
```

```DAX
Work-Life Balance Label =
SWITCH(
    'HR_Workforce_Retention'[WorkLifeBalance],
    1, "1 - Poor",
    2, "2 - Below Average",
    3, "3 - Average",
    4, "4 - Good",
    5, "5 - Excellent"
)
```

## 7. Recommended Data Model

The current dataset supports a simple star-schema design.

### Fact Table

- `HR_Workforce_Retention`

### Recommended Dimensions

- `DimDate`
- `DimDepartment`
- `DimJobRole`
- `DimOfficeLocation`
- `DimRecruitmentSource`
- `DimEmployee` when employee-level drill-through is needed

### Date Modeling

A single employee row contains both `HireDate` and `ExitDate`. Two common approaches are valid:

**Approach A: Role-playing date dimension**

- Active relationship: `DimDate[Date]` → `HR_Workforce_Retention[HireDate]`
- Inactive relationship: `DimDate[Date]` → `HR_Workforce_Retention[ExitDate]`
- Use `USERELATIONSHIP` in exit measures.

**Approach B: Separate date dimensions**

- `DimHireDate`
- `DimExitDate`

Approach A is more compact; Approach B can be easier for report authors who want independent hire and exit filtering.

## 8. Core Measures

Use the table name exactly as shown below, or update the formulas if the Power BI table is renamed.

```DAX
Total Employees =
COUNTROWS('HR_Workforce_Retention')
```

```DAX
Active Employees =
CALCULATE(
    [Total Employees],
    'HR_Workforce_Retention'[EmploymentStatus] = "Active"
)
```

```DAX
Exited Employees =
CALCULATE(
    [Total Employees],
    'HR_Workforce_Retention'[Attrition] = "Yes"
)
```

```DAX
Historical Exit Rate =
DIVIDE([Exited Employees], [Total Employees])
```

```DAX
Average Engagement Score =
AVERAGE('HR_Workforce_Retention'[EngagementScore])
```

```DAX
Promotion Rate =
DIVIDE(
    CALCULATE(
        [Total Employees],
        'HR_Workforce_Retention'[PromotionStatus] = "Promoted"
    ),
    [Total Employees]
)
```

```DAX
Average Training Hours =
AVERAGE('HR_Workforce_Retention'[TrainingHours])
```

```DAX
Average Years Since Promotion =
AVERAGE('HR_Workforce_Retention'[YearsSincePromotion])
```

```DAX
Average Monthly Income =
AVERAGE('HR_Workforce_Retention'[MonthlyIncome])
```

```DAX
Overtime Exit Rate =
CALCULATE(
    [Historical Exit Rate],
    'HR_Workforce_Retention'[OverTime] = "Yes"
)
```

```DAX
No Overtime Exit Rate =
CALCULATE(
    [Historical Exit Rate],
    'HR_Workforce_Retention'[OverTime] = "No"
)
```

```DAX
Overtime Rate Gap =
[Overtime Exit Rate] - [No Overtime Exit Rate]
```

```DAX
Active Work Mode Share =
DIVIDE(
    [Active Employees],
    CALCULATE(
        [Active Employees],
        REMOVEFILTERS('HR_Workforce_Retention'[WorkMode])
    )
)
```

```DAX
High Risk Rate =
DIVIDE([High Risk Employees], [Active Employees])
```

### Workforce Movement

With `Hire Year` and `Exit Year` calculated columns and a disconnected `Year` table:

```DAX
Hires in Period =
CALCULATE(
    [Total Employees],
    TREATAS(
        VALUES('Year'[Year]),
        'HR_Workforce_Retention'[Hire Year]
    )
)
```

```DAX
Exits in Period =
CALCULATE(
    [Exited Employees],
    TREATAS(
        VALUES('Year'[Year]),
        'HR_Workforce_Retention'[Exit Year]
    )
)
```

```DAX
Net Workforce Movement =
[Hires in Period] - [Exits in Period]
```

## 9. Retention Risk Logic

`Risk Band` is not present in the CSV and is therefore a report-derived field.

The published page displays:

| Risk band | Active employees | Share of active workforce |
|---|---:|---:|
| High Risk | 14 | 0.9% |
| Medium Risk | 158 | 10.1% |
| Low Risk | 1,385 | 89.0% |

The High Risk population can be validated against the source with the following condition:

```DAX
High Risk Employees =
CALCULATE(
    [Active Employees],
    FILTER(
        'HR_Workforce_Retention',
        'HR_Workforce_Retention'[EngagementScore] < 50
            && 'HR_Workforce_Retention'[JobSatisfaction] <= 2
    )
)
```

This condition returns the 14 high-risk active employees shown in the dashboard.

The original Power BI logic used to distinguish Medium Risk from Low Risk should be retained in the report file. Because the CSV does not store `Risk Band` and the PBIX calculation was not supplied, the exact medium-risk rule cannot be independently reconstructed from the source file alone. Document any future threshold changes in this section and rerun the validation totals.

Recommended governance for risk logic:

- Apply risk classification only to active employees.
- Keep thresholds transparent and version-controlled.
- Avoid presenting the score as a confirmed prediction of resignation.
- Require human review before taking employee-level action.
- Audit risk outcomes for department, gender, age, and employment-type bias.

## 10. Dashboard Page Specifications

### 10.1 Workforce Executive Overview

**Purpose:** Provide senior leaders with a compact view of workforce size, engagement, movement, work mode, and department attrition.

**Filters**

- Department
- Office Location
- Employment Type

**KPI cards**

- Total Employees
- Active Employees
- Exited Employees
- Historical Exit Rate
- Average Engagement Score

**Visuals**

- Hires, exits, and net workforce movement by year
- Exit rate by department
- Active employee distribution by work mode
- Executive observation and recommended action text

**Validated observations**

- Product: 15.7% exit rate
- Sales: 14.9%
- Operations: 13.9%
- Engineering: 13.5%
- Company average: 13.5%
- Active work mode: 52.8% Hybrid, 26.1% On-site, 21.1% Remote

### 10.2 Employee Retention Drivers

**Purpose:** Identify factors associated with employee exits.

**Filters**

- Department
- Job Role
- Exit Year

**KPI cards**

- Exited Employees
- Overtime Rate Gap
- Overtime Exit Rate
- No Overtime Exit Rate

**Visuals**

- Exit reasons
- Exit rate: overtime versus no overtime
- Exit rate by annual salary band
- Exit rate by work-life balance
- Exit rate by job satisfaction

**Validated observations**

- Overtime exit rate: 22.4%
- No-overtime exit rate: 9.7%
- Better Opportunity exits: 57
- Compensation exits: 51
- Work-Life Balance exits: 43
- Below 5 LPA exit rate: 18.0%
- 25+ LPA exit rate: 6.5%
- Poor work-life balance exit rate: 25.0%
- Excellent work-life balance exit rate: 6.5%
- Very low job satisfaction exit rate: 29.4%
- Very high job satisfaction exit rate: 8.0%

### 10.3 Employee Retention Risk

**Purpose:** Surface active employees and departments that may require retention review.

**Filters**

- Risk Band
- Department
- Job Role

**KPI cards**

- High Risk Employees
- Medium Risk Employees
- High Risk Rate
- Active Employees

**Visuals**

- Active employees by risk band
- High-risk rate by department
- Employee-level review table containing risk, ID, department, engagement, satisfaction, overtime, and years since promotion

**Validated observations**

- High Risk: 14 employees
- Medium Risk: 158 employees
- Low Risk: 1,385 employees
- Engineering high-risk rate: 1.6%
- Sales: 1.1%
- Marketing: 0.9%
- Finance: 0.9%
- Operations: 0.6%
- Customer Success: 0.4%

### 10.4 Talent Growth & Hiring Quality

**Purpose:** Evaluate promotion, training, compensation, and recruitment-channel outcomes.

**Filters**

- Department
- Recruitment Source
- Hire Year

**KPI cards**

- Promotion Rate
- Average Training Hours
- Average Years Since Promotion
- Average Monthly Income

**Visuals**

- Promotion rate by department
- Exit rate by promotion status
- Exit rate by recruitment source
- Employees hired by recruitment source

**Validated observations**

- Promotion rate: 52.0%
- Average training hours: 29.5
- Average years since promotion: 1.3
- Average monthly income: ₹99,129
- Product promotion rate: 57.5%
- Customer Success promotion rate: 43.4%
- Not Yet Promoted exit rate: 16.4%
- Promoted exit rate: 10.8%
- Job Portal exit rate: 16.7%
- LinkedIn exit rate: 11.9%
- Employee Referral hires: 507
- LinkedIn hires: 419

## 11. Data Dictionary

| Column | Recommended type | Description |
|---|---|---|
| `EmpID` | Text | Unique employee identifier. |
| `Age` | Whole number | Employee age in years. |
| `AgeGroup` | Text | Derived age band: 18-25, 26-35, 36-45, 46-55, or 55+. |
| `Attrition` | Text | Whether the employee has exited: Yes or No. |
| `EmploymentStatus` | Text | Current status: Active or Exited. |
| `BusinessTravel` | Text | Travel frequency category. |
| `DailyRate` | Decimal number | Daily compensation or rate field supplied by the source. |
| `Department` | Text | Organizational department. |
| `DistanceFromHome(KM)` | Whole number | Home-to-work distance in kilometres. |
| `Education` | Whole number | Numeric education code from 1 to 5. |
| `EducationLevel` | Text | Readable education qualification. |
| `EducationField` | Text | Primary field of study. |
| `EmployeeCount` | Whole number | Constant row-count helper; value is 1 for every record. |
| `EmployeeNumber` | Whole number | Numeric employee key. |
| `EnvironmentSatisfaction` | Whole number | Environment satisfaction score from 1 to 5. |
| `Gender` | Text | Employee gender category. |
| `JobInvolvement` | Whole number | Job involvement score from 1 to 5. |
| `JobLevel` | Whole number | Organizational level from 1 to 5. |
| `JobRole` | Text | Employee job role. |
| `JobSatisfaction` | Whole number | Job satisfaction score from 1 to 5. |
| `MaritalStatus` | Text | Marital status category. |
| `MonthlyIncome` | Whole number | Monthly income in Indian rupees. |
| `SalarySlab` | Text | Annual salary band in lakh per annum. |
| `SalarySlabOrder` | Whole number | Sort order for salary bands. |
| `HourlyRate` | Decimal number | Hourly compensation or rate field supplied by the source. |
| `NumCompaniesWorked` | Whole number | Number of previous employers. |
| `Over18` | Text | Adult-status flag; constant value in this dataset. |
| `OverTime` | Text | Whether the employee works overtime: Yes or No. |
| `OvertimeHoursMonthly` | Whole number | Monthly overtime hours. |
| `SalaryHike %` | Whole number | Most recent salary increase percentage. |
| `PerformanceRating` | Whole number | Performance score from 1 to 5. |
| `RelationshipSatisfaction` | Whole number | Workplace relationship satisfaction score from 1 to 5. |
| `StandardWorkingHours` | Whole number | Standard weekly or configured work-hours field; constant in this dataset. |
| `StockOptionLevel` | Whole number | Stock-option level from 0 to 3. |
| `TotalExperience(Years)` | Whole number | Total professional experience in years. |
| `TrainingsLastYear` | Whole number | Number of training sessions completed in the previous year. |
| `TrainingHours` | Whole number | Total training hours. |
| `WorkLifeBalance` | Whole number | Work-life balance score from 1 to 5. |
| `YearsatCompany` | Whole number | Tenure at the company in years. |
| `YearsinCurrentRole` | Whole number | Years in the current role. |
| `YearsSincePromotion` | Whole number | Years since the most recent promotion. |
| `YearsWithCurrManager` | Whole number | Years working with the current manager. |
| `HireDate` | Date | Employee hire date. |
| `ExitDate` | Date | Exit date; blank for active employees. |
| `OfficeLocation` | Text | Assigned office location. |
| `WorkMode` | Text | Hybrid, On-site, or Remote. |
| `EmploymentType` | Text | Full-time, Contract, or Intern. |
| `EngagementScore` | Whole number | Employee engagement score. |
| `AbsenceDays` | Whole number | Recorded absence days. |
| `ManagerRating` | Whole number | Manager rating from 1 to 5. |
| `AnnualBonus` | Whole number | Annual bonus amount. |
| `RecruitmentSource` | Text | Channel through which the employee was recruited. |
| `LastPromotionDate` | Date | Most recent promotion date; blank where unavailable or not applicable. |
| `PromotionStatus` | Text | Promoted or Not Yet Promoted. |
| `ExitReason` | Text | Recorded reason for exit; blank for active employees. |
| `SnapshotDate` | Date | Dataset reporting date; 31 May 2026 for all records. |

## 12. Score Labels

The dashboard uses readable labels for 1-to-5 ordinal scales.

| Score | Job satisfaction | Work-life balance |
|---:|---|---|
| 1 | Very Low | Poor |
| 2 | Low | Below Average |
| 3 | Medium | Average |
| 4 | High | Good |
| 5 | Very High | Excellent |

The same numeric direction applies to other satisfaction and rating fields: larger values indicate more favorable ratings unless the source owner specifies otherwise.

## 13. Validation Checklist

Run these tests after every refresh.

| Test | Expected result for current file |
|---|---:|
| Row count | 1,800 |
| Unique `EmpID` count | 1,800 |
| Active employee count | 1,557 |
| Exited employee count | 243 |
| Active + exited | 1,800 |
| Historical exit rate | 13.5% |
| Average engagement score | 75.1 |
| Promotion rate | 52.0% |
| Overtime exit rate | 22.4% |
| No-overtime exit rate | 9.7% |
| High-risk active employees | 14 |
| Snapshot-date distinct count | 1 |
| Snapshot date | 31 May 2026 |
| Active employees with nonblank exit date | 0 |
| Exited employees with blank exit date | 0 |
| Exited employees with blank exit reason | 0 |
| Duplicate employee IDs | 0 |

Additional checks:

- `SalarySlabOrder` must sort `SalarySlab` from lowest to highest.
- `ExitYear` must remain blank for active employees.
- 2026 movement visuals must include a partial-year notice.
- Slicer selections must affect cards and visuals consistently.
- Percentages should use one decimal place unless a smaller precision is analytically necessary.
- Employee table exports should be restricted in production environments.

## 14. Interpretation Notes

### Historical Exit Rate

The report calculates:

```text
Exited employee records ÷ all employee records
```

This is a historical ratio within the snapshot population. It is not equivalent to an annualized turnover rate based on average headcount.

### Recruitment Source Exit Rate

A high source-level exit rate does not prove that the channel causes exits. Job mix, seniority, department, location, and hire cohort may affect the result. Source performance should be reviewed with volume and employee mix.

### Promotion and Exit

The lower exit rate among promoted employees is an association. The dashboard does not establish that promotion alone caused higher retention.

### Overtime

The overtime gap is a strong operational signal, but overtime may also be correlated with department, workload, job role, or seniority. Use drill-downs before making policy decisions.

### Employee Risk

Risk segmentation is a prioritization aid, not a disciplinary tool or a definitive prediction. Employee-level decisions require context, manager input, and appropriate HR review.

## 15. Known Limitations

1. The source is a single employee-level snapshot.
2. Exit rate is not annualized.
3. 2026 is incomplete through 31 May.
4. No formal employee-survey question detail is available.
5. No hiring requisition, vacancy, time-to-fill, or recruiting-cost data is included.
6. No manager hierarchy is included.
7. No employee performance history or compensation history is included.
8. Risk Band is report-derived rather than sourced.
9. Causal conclusions cannot be made from descriptive dashboard relationships.
10. Employee IDs create privacy and access-control obligations.

## 16. Security and Privacy

For a real organizational deployment:

- Treat employee-level data as confidential.
- Restrict access through Power BI workspaces and row-level security where required.
- Do not publish employee IDs in a public repository.
- Replace production identifiers with anonymized values before sharing.
- Limit export permissions on employee-level tables.
- Review local employment, privacy, and automated-decision requirements.
- Record the purpose and approved users of retention-risk analysis.

The supplied repository structure is suitable for a portfolio or demonstration project only when its data is authorized for public use.

## 17. Refresh and Maintenance Procedure

1. Obtain the approved workforce snapshot.
2. Confirm that the schema matches the documented 56 columns.
3. Replace `HR_Workforce_Retention.csv`.
4. Refresh Power Query.
5. Resolve type or schema errors.
6. Run the validation checklist.
7. Check that risk-band totals remain explainable.
8. Review partial-year annotations.
9. Export updated dashboard screenshots.
10. Update key findings in `README.md` and this document.
11. Commit the CSV, report, screenshots, and documentation with a clear version note.

Suggested commit message:

```text
Refresh workforce retention dashboard for YYYY-MM-DD snapshot
```

## 18. Versioning Recommendations

- Use semantic tags such as `v1.0.0`.
- Record the snapshot date in release notes.
- Store major risk-logic changes as separate versions.
- Document added, removed, or renamed columns.
- Do not silently change metric denominators.
- Include screenshots with each published release.

## 19. Future Enhancements

- Add annualized and rolling-12-month turnover measures.
- Separate voluntary and involuntary exits.
- Add cohort retention by hire month or quarter.
- Add tenure-based survival curves.
- Add department and role benchmarking.
- Add promotion velocity and internal-mobility analysis.
- Add manager hierarchy and span-of-control analysis.
- Add recruitment cost, time-to-fill, and quality-of-hire metrics.
- Add explainable statistical or machine-learning attrition models.
- Add drill-through pages with controlled employee detail.
- Add automated data-quality alerts.
- Add row-level security and deployment-pipeline documentation.

## 20. Repository Assets

| File | Purpose |
|---|---|
| `README.md` | Public project overview and quick-start instructions. |
| `PROJECT_DOCUMENTATION.md` | Technical, analytical, and governance documentation. |
| `HR_Workforce_Retention.csv` | Source dataset. |
| `workforce-executive-overview.png` | Dashboard page screenshot. |
| `employee-retention-drivers.png` | Dashboard page screenshot. |
| `employee-retention-risk.png` | Dashboard page screenshot. |
| `talent-growth-hiring-quality.png` | Dashboard page screenshot. |