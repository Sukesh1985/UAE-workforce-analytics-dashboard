# DAX Measures - UAE Workforce Analytics Dashboard

This document contains all DAX measures used in the UAE Workforce Analytics Dashboard, organized by category.

## Table of Contents
- [Core Workforce Metrics](#core-workforce-metrics)
- [Demographic Analysis](#demographic-analysis)
- [Emiratization Metrics](#emiratization-metrics)
- [Salary Intelligence](#salary-intelligence)
- [Hiring & Attrition](#hiring--attrition)
- [Industry Analysis](#industry-analysis)
- [Time Intelligence](#time-intelligence)
- [Compliance Metrics](#compliance-metrics)

---

## Core Workforce Metrics

### Total Employees
```dax
Total Employees = 
COUNTROWS('Workforce')
```

### Active Employees
```dax
Active Employees = 
CALCULATE(
    COUNTROWS('Workforce'),
    'Workforce'[Employment Status] = "Active"
)
```

### Total Headcount by Department
```dax
Headcount by Department = 
CALCULATE(
    [Total Employees],
    ALLEXCEPT('Workforce', 'Workforce'[Department])
)
```

### Average Tenure (Years)
```dax
Average Tenure = 
AVERAGE('Workforce'[Tenure Years])
```

### Median Tenure
```dax
Median Tenure = 
MEDIAN('Workforce'[Tenure Years])
```

---

## Demographic Analysis

### Gender Distribution %
```dax
Gender Distribution % = 
DIVIDE(
    CALCULATE(COUNTROWS('Workforce')),
    CALCULATE(COUNTROWS('Workforce'), ALL('Workforce'[Gender])),
    0
) * 100
```

### Average Age
```dax
Average Age = 
AVERAGE('Workforce'[Age])
```

### Age Group Distribution
```dax
Age Group Count = 
SWITCH(
    TRUE(),
    'Workforce'[Age] < 25, "Under 25",
    'Workforce'[Age] >= 25 && 'Workforce'[Age] < 35, "25-34",
    'Workforce'[Age] >= 35 && 'Workforce'[Age] < 45, "35-44",
    'Workforce'[Age] >= 45 && 'Workforce'[Age] < 55, "45-54",
    "55+"
)
```

### Nationality Mix Count
```dax
Nationality Count = 
DISTINCTCOUNT('Workforce'[Nationality])
```

### Top 5 Nationalities
```dax
Top 5 Nationalities = 
CALCULATE(
    COUNTROWS('Workforce'),
    TOPN(
        5,
        ALL('Workforce'[Nationality]),
        COUNTROWS('Workforce'),
        DESC
    )
)
```

---

## Emiratization Metrics

### UAE National Count
```dax
UAE Nationals = 
CALCULATE(
    COUNTROWS('Workforce'),
    'Workforce'[Nationality] = "UAE"
)
```

### Emiratization Rate %
```dax
Emiratization Rate = 
DIVIDE(
    [UAE Nationals],
    [Total Employees],
    0
) * 100
```

### Emiratization Target %
```dax
Emiratization Target = 15.1
```

### Emiratization Gap
```dax
Emiratization Gap = 
[Emiratization Target] - [Emiratization Rate]
```

### Emiratization Compliance Status
```dax
Emiratization Status = 
IF(
    [Emiratization Rate] >= [Emiratization Target],
    "✓ Compliant",
    "⚠ Below Target"
)
```

### UAE Nationals in Leadership
```dax
UAE Nationals Leadership = 
CALCULATE(
    [UAE Nationals],
    'Workforce'[Level] IN {"Manager", "Senior Manager", "Director", "VP", "C-Level"}
)
```

### Emiratization Rate by Department
```dax
Emiratization by Dept = 
DIVIDE(
    CALCULATE([UAE Nationals]),
    CALCULATE([Total Employees]),
    0
) * 100
```

---

## Salary Intelligence

### Average Salary
```dax
Average Salary = 
AVERAGE('Workforce'[Salary])
```

### Median Salary
```dax
Median Salary = 
MEDIAN('Workforce'[Salary])
```

### Total Payroll
```dax
Total Payroll = 
SUM('Workforce'[Salary])
```

### Annual Payroll Cost
```dax
Annual Payroll = 
[Total Payroll] * 12
```

### Salary by Gender Gap %
```dax
Gender Pay Gap = 
VAR MaleSalary = 
    CALCULATE(
        [Average Salary],
        'Workforce'[Gender] = "Male"
    )
VAR FemaleSalary = 
    CALCULATE(
        [Average Salary],
        'Workforce'[Gender] = "Female"
    )
RETURN
    DIVIDE(
        MaleSalary - FemaleSalary,
        MaleSalary,
        0
    ) * 100
```

### Salary Percentile (P25, P50, P75, P90)
```dax
Salary P25 = 
PERCENTILE.INC('Workforce'[Salary], 0.25)

Salary P50 = 
PERCENTILE.INC('Workforce'[Salary], 0.50)

Salary P75 = 
PERCENTILE.INC('Workforce'[Salary], 0.75)

Salary P90 = 
PERCENTILE.INC('Workforce'[Salary], 0.90)
```

### Compensation Ratio
```dax
Compensation Ratio = 
DIVIDE(
    'Workforce'[Salary],
    [Median Salary],
    0
)
```

### Above Market Rate Count
```dax
Above Market Count = 
CALCULATE(
    COUNTROWS('Workforce'),
    'Workforce'[Salary] > [Average Salary] * 1.15
)
```

---

## Hiring & Attrition

### New Hires (YTD)
```dax
New Hires YTD = 
CALCULATE(
    COUNTROWS('Workforce'),
    'Workforce'[Hire Date] >= DATE(YEAR(TODAY()), 1, 1),
    'Workforce'[Hire Date] <= TODAY()
)
```

### Exits (YTD)
```dax
Exits YTD = 
CALCULATE(
    COUNTROWS('Attrition'),
    'Attrition'[Exit Date] >= DATE(YEAR(TODAY()), 1, 1),
    'Attrition'[Exit Date] <= TODAY()
)
```

### Attrition Rate %
```dax
Attrition Rate = 
VAR BeginningHeadcount = 
    CALCULATE(
        [Total Employees],
        DATESBETWEEN(
            'Date'[Date],
            DATE(YEAR(TODAY()), 1, 1),
            DATE(YEAR(TODAY()), 1, 1)
        )
    )
RETURN
    DIVIDE(
        [Exits YTD],
        BeginningHeadcount,
        0
    ) * 100
```

### Voluntary vs Involuntary Attrition
```dax
Voluntary Attrition = 
CALCULATE(
    [Exits YTD],
    'Attrition'[Exit Type] = "Voluntary"
)

Involuntary Attrition = 
CALCULATE(
    [Exits YTD],
    'Attrition'[Exit Type] = "Involuntary"
)
```

### Retention Rate %
```dax
Retention Rate = 
100 - [Attrition Rate]
```

### Time to Hire (Average Days)
```dax
Avg Time to Hire = 
AVERAGE('Hiring'[Days to Hire])
```

### Hiring Velocity
```dax
Hiring Velocity = 
DIVIDE(
    [New Hires YTD],
    DATEDIFF(
        DATE(YEAR(TODAY()), 1, 1),
        TODAY(),
        DAY
    ),
    0
) * 30
```

---

## Industry Analysis

### Industry Distribution
```dax
Industry % = 
DIVIDE(
    CALCULATE([Total Employees]),
    CALCULATE([Total Employees], ALL('Workforce'[Industry])),
    0
) * 100
```

### Top Industry by Headcount
```dax
Top Industry = 
CALCULATE(
    FIRSTNONBLANK('Workforce'[Industry], 1),
    TOPN(
        1,
        ALL('Workforce'[Industry]),
        [Total Employees],
        DESC
    )
)
```

### Industry Salary Benchmark
```dax
Industry Salary Benchmark = 
CALCULATE(
    [Average Salary],
    ALL('Workforce'[Department])
)
```

### Industry Growth Rate
```dax
Industry Growth = 
VAR CurrentYear = 
    CALCULATE(
        [Total Employees],
        'Date'[Year] = YEAR(TODAY())
    )
VAR PreviousYear = 
    CALCULATE(
        [Total Employees],
        'Date'[Year] = YEAR(TODAY()) - 1
    )
RETURN
    DIVIDE(
        CurrentYear - PreviousYear,
        PreviousYear,
        0
    ) * 100
```

---

## Time Intelligence

### YoY Growth %
```dax
YoY Growth = 
VAR CurrentPeriod = [Total Employees]
VAR PreviousPeriod = 
    CALCULATE(
        [Total Employees],
        SAMEPERIODLASTYEAR('Date'[Date])
    )
RETURN
    DIVIDE(
        CurrentPeriod - PreviousPeriod,
        PreviousPeriod,
        0
    ) * 100
```

### MoM Change
```dax
MoM Change = 
VAR CurrentMonth = [Total Employees]
VAR PreviousMonth = 
    CALCULATE(
        [Total Employees],
        DATEADD('Date'[Date], -1, MONTH)
    )
RETURN
    CurrentMonth - PreviousMonth
```

### QoQ Growth %
```dax
QoQ Growth = 
VAR CurrentQuarter = [Total Employees]
VAR PreviousQuarter = 
    CALCULATE(
        [Total Employees],
        DATEADD('Date'[Date], -1, QUARTER)
    )
RETURN
    DIVIDE(
        CurrentQuarter - PreviousQuarter,
        PreviousQuarter,
        0
    ) * 100
```

### Rolling 12-Month Average
```dax
Rolling 12M Avg = 
CALCULATE(
    [Average Salary],
    DATESINPERIOD(
        'Date'[Date],
        LASTDATE('Date'[Date]),
        -12,
        MONTH
    )
)
```

---

## Compliance Metrics

### WPS Compliance Rate
```dax
WPS Compliance = 
DIVIDE(
    CALCULATE(
        COUNTROWS('Workforce'),
        'Workforce'[WPS Status] = "Compliant"
    ),
    [Total Employees],
    0
) * 100
```

### Labor Contract Compliance
```dax
Contract Compliance = 
DIVIDE(
    CALCULATE(
        COUNTROWS('Workforce'),
        'Workforce'[Contract Status] = "Valid"
    ),
    [Total Employees],
    0
) * 100
```

### Visa Expiry Alert
```dax
Visa Expiring (30 Days) = 
CALCULATE(
    COUNTROWS('Workforce'),
    'Workforce'[Visa Expiry Date] >= TODAY(),
    'Workforce'[Visa Expiry Date] <= TODAY() + 30
)
```

### Insurance Coverage Rate
```dax
Insurance Coverage = 
DIVIDE(
    CALCULATE(
        COUNTROWS('Workforce'),
        'Workforce'[Insurance Status] = "Active"
    ),
    [Total Employees],
    0
) * 100
```

### Non-Compliant Employees
```dax
Non Compliant Count = 
CALCULATE(
    COUNTROWS('Workforce'),
    OR(
        'Workforce'[WPS Status] = "Non-Compliant",
        'Workforce'[Contract Status] = "Expired"
    )
)
```

---

## Advanced KPIs

### Diversity Index
```dax
Diversity Index = 
VAR TotalNationalities = DISTINCTCOUNT('Workforce'[Nationality])
VAR MaxDiversity = 100
RETURN
    DIVIDE(TotalNationalities, MaxDiversity, 0) * 100
```

### Span of Control
```dax
Span of Control = 
DIVIDE(
    CALCULATE(
        COUNTROWS('Workforce'),
        'Workforce'[Level] <> "Manager"
    ),
    CALCULATE(
        COUNTROWS('Workforce'),
        'Workforce'[Level] = "Manager"
    ),
    0
)
```

### Flight Risk Score
```dax
Flight Risk = 
SWITCH(
    TRUE(),
    AND('Workforce'[Tenure Years] < 1, 'Workforce'[Performance Rating] <= 2), "High",
    AND('Workforce'[Tenure Years] < 2, 'Workforce'[Salary] < [Median Salary]), "Medium",
    "Low"
)
```

### Cost per Hire
```dax
Cost per Hire = 
DIVIDE(
    SUM('Hiring'[Recruitment Cost]),
    [New Hires YTD],
    0
)
```

### Productivity Score
```dax
Productivity Score = 
DIVIDE(
    SUM('Workforce'[Output]),
    [Total Employees],
    0
)
```

---

## Dynamic Formatting Measures

### Emiratization Color
```dax
Emiratization Color = 
IF(
    [Emiratization Rate] >= [Emiratization Target],
    "#2E7D32",  // Green
    "#C62828"   // Red
)
```

### Attrition Color
```dax
Attrition Color = 
SWITCH(
    TRUE(),
    [Attrition Rate] <= 10, "#2E7D32",   // Green - Healthy
    [Attrition Rate] <= 15, "#F57C00",   // Orange - Warning
    "#C62828"                             // Red - Critical
)
```

### Trend Indicator
```dax
Trend = 
IF(
    [YoY Growth] > 0,
    "▲",
    IF([YoY Growth] < 0, "▼", "●")
)
```

---

## Calculated Columns

### Age Group Column
```dax
Age Group = 
SWITCH(
    TRUE(),
    'Workforce'[Age] < 25, "1. Under 25",
    'Workforce'[Age] >= 25 && 'Workforce'[Age] < 35, "2. 25-34",
    'Workforce'[Age] >= 35 && 'Workforce'[Age] < 45, "3. 35-44",
    'Workforce'[Age] >= 45 && 'Workforce'[Age] < 55, "4. 45-54",
    "5. 55+"
)
```

### Tenure Band
```dax
Tenure Band = 
SWITCH(
    TRUE(),
    'Workforce'[Tenure Years] < 1, "1. < 1 Year",
    'Workforce'[Tenure Years] >= 1 && 'Workforce'[Tenure Years] < 3, "2. 1-3 Years",
    'Workforce'[Tenure Years] >= 3 && 'Workforce'[Tenure Years] < 5, "3. 3-5 Years",
    'Workforce'[Tenure Years] >= 5 && 'Workforce'[Tenure Years] < 10, "4. 5-10 Years",
    "5. 10+ Years"
)
```

### Salary Band
```dax
Salary Band = 
SWITCH(
    TRUE(),
    'Workforce'[Salary] < 5000, "1. < 5K",
    'Workforce'[Salary] >= 5000 && 'Workforce'[Salary] < 10000, "2. 5K-10K",
    'Workforce'[Salary] >= 10000 && 'Workforce'[Salary] < 20000, "3. 10K-20K",
    'Workforce'[Salary] >= 20000 && 'Workforce'[Salary] < 50000, "4. 20K-50K",
    "5. 50K+"
)
```

---

## Notes

- All percentage calculations include error handling using `DIVIDE()` to avoid division by zero errors
- Salary figures are assumed to be in AED (UAE Dirhams)
- Date calculations use the 'Date' dimension table for proper time intelligence
- Emiratization target is set at 15.1% as per UAE regulations
- Color codes follow accessibility standards for dashboards

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025 | Initial DAX measures documentation |

---

**Dashboard:** UAE Workforce Analytics Dashboard  
**Author:** Sukesh Kumar  
**Last Updated:** November 2025
