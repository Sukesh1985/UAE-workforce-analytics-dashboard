# 🇦🇪 UAE Workforce Analytics Dashboard

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://dax.guide/)
[![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)](https://github.com/yourusername/uae-workforce-analytics)

> **A comprehensive Power BI dashboard analyzing 50,000+ employee records across the UAE, tracking Emiratization compliance, WPS adherence, and workforce trends to support strategic HR decision-making.**

---

## 📊 Project Overview

This dashboard provides deep insights into UAE workforce dynamics, addressing critical business challenges including:
- ✅ **Emiratization Compliance Tracking** (15.1% rate vs. 5% government target)
- ✅ **WPS (Wage Protection System) Monitoring** (95% compliance rate)
- ✅ **Multi-Industry Workforce Analysis** (10 key sectors)
- ✅ **Salary Intelligence & Benchmarking**
- ✅ **Hiring Trends & Workforce Planning**

---

## 🎯 Key Insights & Business Impact

### Emiratization Status
- **Current Rate**: 15.1% (exceeding 5% government mandate by 10.1 percentage points)
- **Surplus**: 5,033 Emirati employees above target
- **Compliance**: All industries meeting or exceeding requirements

### Workforce Composition
- **Total Workforce Analyzed**: 50,000 employees
- **Expatriate %**: 84.9% (42,467 employees)
- **Emirati %**: 15.1% (7,533 employees)
- **Gender Distribution**: 69.9% Male | 30.1% Female
- **Average Monthly Salary**: 14,268 AED

### Hiring Intelligence
- **New Hires (2020-2025)**: 116,684
- **Resignations**: 92,580
- **Net Workforce Growth**: +24,104 (26% growth)
- **Average Time-to-Fill**: 48-56 days (varies by industry)
- **Vacancy Fill Rate**: 59% filled, 30% open, 11% cancelled

### Industry Leaders (by workforce size)
1. **Construction & Real Estate**: 17.74% (8,868 employees)
2. **Wholesale & Retail Trade**: 15.98% (7,989 employees)
3. **Professional & Technical Services**: 14.22% (7,108 employees)

---

## 🛠️ Technical Stack

| Technology | Purpose |
|------------|---------|
| **Power BI Desktop** | Dashboard development & visualization |
| **DAX** | Advanced calculations & measures |
| **Power Query** | Data transformation & cleaning |
| **Data Modeling** | Star schema with fact and dimension tables |

---

## 📈 Dashboard Pages

### 1. **Executive Overview**
High-level KPIs and workforce trends at a glance
- Total workforce metrics
- Emiratization & expatriate breakdown
- Gender distribution
- Industry employment overview
- 5-year growth trend analysis

<img width="1250" height="723" alt="page1-executive" src="https://github.com/user-attachments/assets/cf72e915-f198-4aad-885f-2e2a18e51aa2" />

### 2. **Workforce Demographics**
Deep-dive into employee composition
- Age distribution by gender (18-24, 25-34, 35-44, 45-54, 55+)
- Nationality breakdown (10 major nationalities tracked)
- Education level distribution by industry
- Skill level analysis by gender

<img width="1232" height="716" alt="page2-demographics" src="https://github.com/user-attachments/assets/7f6b8fcb-9547-4047-895a-d8a594828a7f" />

### 3. **Industry Analysis**
Sector-specific workforce intelligence
- 10 industries tracked across UAE
- Average salary by industry
- Male/Female distribution by sector
- Workforce size comparison

<img width="1263" height="720" alt="page3-industry" src="https://github.com/user-attachments/assets/15338cc9-0506-42d4-90fc-8934e84d1ded" />

### 4. **Salary Intelligence**
Comprehensive compensation analysis
- Salary distribution across 7 brackets (0-5K to 50K+ AED)
- Percentile analysis (P25, P50, P75)
- Salary vs. Years of Service correlation
- Average salary by Emirate
- Job category & gender-based compensation

<img width="1254" height="713" alt="page4-salary" src="https://github.com/user-attachments/assets/59a25621-d478-44d0-ae24-0b5a0930a987" />

### 5. **Hiring Trends & Workforce Planning**
Predictive insights for strategic planning
- Monthly hiring patterns (2020-2025)
- Seasonal hiring trends
- Hiring vs. Turnover comparison
- Time-to-fill by industry
- Current vacancy status

<img width="1260" height="717" alt="page5-hiring" src="https://github.com/user-attachments/assets/6372c090-d6c0-47fd-aaa3-2740b5bb692b" />

### 6. **Compliance & Insights**
Regulatory adherence monitoring
- Emiratization progress gauge
- WPS compliance scorecard by industry
- Key findings & recommendations
- Action items for HR leadership

<img width="1243" height="716" alt="page6-compliance" src="https://github.com/user-attachments/assets/9400a436-f21a-4117-b88e-3ec5c9578c70" />

---

## 🔍 Advanced DAX Measures

Key measures implemented:

```dax
// Emiratization Rate
Emiratization Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(Employees), Employees[Nationality] = "UAE National"),
    COUNTROWS(Employees),
    0
) * 100

// WPS Compliance %
WPS Compliance % = 
DIVIDE(
    CALCULATE(COUNTROWS(Employees), Employees[WPS_Status] = "Compliant"),
    COUNTROWS(Employees),
    0
) * 100

// Net Workforce Change
Net Workforce Change = 
CALCULATE(
    COUNTROWS(HiringData),
    HiringData[Event_Type] = "Hire"
) - 
CALCULATE(
    COUNTROWS(HiringData),
    HiringData[Event_Type] = "Resignation"
)

// Average Time to Fill
Avg Time to Fill = 
AVERAGE(Vacancies[Days_to_Fill])

// Year-over-Year Growth
YoY Growth % = 
VAR CurrentYear = CALCULATE(COUNTROWS(Employees), YEAR(Employees[Hire_Date]) = YEAR(TODAY()))
VAR PreviousYear = CALCULATE(COUNTROWS(Employees), YEAR(Employees[Hire_Date]) = YEAR(TODAY()) - 1)
RETURN
DIVIDE(CurrentYear - PreviousYear, PreviousYear, 0) * 100
```

See [full DAX documentation](documentation/dax_measures.md) for all measures.

---

## 🎨 Design Principles

- **Modern Aesthetic**: Clean blue theme aligned with UAE national colors
- **Intuitive Navigation**: Button-based navigation across 6 pages
- **Interactive Filtering**: Year, Emirate, and Industry slicers on every page
- **Mobile-Responsive**: Optimized for desktop and tablet viewing
- **Accessibility**: Color-blind friendly palette with clear labels

---

## 📊 Data Model

The dashboard uses a **star schema** architecture:

**Fact Tables:**
- `EmployeeFact` - Core employee records (50,000 rows)
- `HiringFact` - Hiring events (116,684 hires + 92,580 resignations)
- `SalaryFact` - Compensation data

**Dimension Tables:**
- `DimDate` - Date dimension (2020-2025)
- `DimIndustry` - 10 industry categories
- `DimEmirate` - 7 Emirates
- `DimNationality` - 10+ nationalities
- `DimJobCategory` - 6 job categories

![Data Model](documentation/data_model.png)

---

## 🚀 Key Features

- ✅ **Real-time Filtering**: Dynamic slicers for Year, Emirate, Industry
- ✅ **Drill-through Analysis**: Click-through from summary to detailed views
- ✅ **Trend Analysis**: 5-year historical data (2020-2025)
- ✅ **Compliance Monitoring**: Automated alerts for Emiratization & WPS
- ✅ **Export Capabilities**: Share insights via PDF/PowerPoint
- ✅ **Scheduled Refresh**: Configured for daily data updates

---

## 📥 How to Use

### Option 1: View the PDF
Download the [static PDF version](dashboard/UAE_Workforce_Analytics.pdf) for a quick overview.

### Option 2: Open in Power BI Desktop
1. Download [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/uae-workforce-analytics.git
   ```
3. Open `dashboard/UAE_Workforce_Analytics.pbix`
4. Explore the interactive dashboard

### Option 3: View on Power BI Service
[View Live Dashboard](https://app.powerbi.com/your-published-link) *(link to be updated after publishing)*

---

## 📁 Repository Contents

```
├── README.md                          # This file
├── dashboard/
│   ├── UAE_Workforce_Analytics.pbix  # Power BI file
│   └── UAE_Workforce_Analytics.pdf   # Static export
├── images/                            # Dashboard screenshots
├── documentation/
│   ├── technical_overview.md         # Architecture details
│   ├── dax_measures.md               # All DAX formulas
│   └── data_model.png                # ER diagram
└── insights/
    └── key_findings.md               # Business insights report
```

---

## 💼 Business Value

This dashboard enables HR leaders to:

1. **Ensure Compliance**: Track Emiratization and WPS requirements in real-time
2. **Optimize Hiring**: Identify seasonal patterns and reduce time-to-fill
3. **Control Costs**: Benchmark salaries and identify outliers
4. **Plan Strategically**: Forecast workforce needs based on historical trends
5. **Drive DEI**: Monitor gender and nationality diversity metrics
6. **Reduce Turnover**: Analyze resignation patterns and predict retention risks

**Estimated Annual Impact**: 
- ⏱️ 200+ hours saved in manual reporting
- 💰 15-20% reduction in recruitment costs through data-driven hiring
- ✅ Zero non-compliance penalties through proactive monitoring

---

## 🏆 Skills Demonstrated

- ✅ Power BI dashboard development
- ✅ Advanced DAX formulas and calculations
- ✅ Data modeling (star schema)
- ✅ HR analytics domain expertise
- ✅ UAE labor law & compliance knowledge
- ✅ Data visualization best practices
- ✅ Stakeholder communication through insights

---

## 📝 Data Note

*This dashboard uses synthesized data for demonstration purposes. All employee records, companies, and metrics are fictional and created for portfolio presentation. The dashboard structure, DAX logic, and analytical approach reflect real-world UAE workforce analytics requirements.*

---

## 🎓 About the Author

**Sukesh Kumar**  
HR Analytics Specialist | Workforce Planning Analyst  
📍 Open to opportunities in Dubai, UAE

🔗 **Connect with me:**
- 💼 [LinkedIn](https://www.linkedin.com/in/sukesh-singla-667701a5)
- 📧 Email: your.ssingla25@gmail.com
- 💻 [Portfolio](https://sukesh1985.github.io )
- 📊 [More Projects](https://github.com/Sukesh1985)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- UAE Ministry of Human Resources & Emiratisation (MOHRE) for Emiratization guidelines
- Power BI community for DAX optimization techniques
- Dubai HR professionals who provided domain insights

---

## ⭐ If you find this project valuable, please consider giving it a star!

### 📧 Open to opportunities in HR Analytics, Workforce Planning, and Business Intelligence roles in Dubai, UAE.

---

*Last Updated: November 2025*
