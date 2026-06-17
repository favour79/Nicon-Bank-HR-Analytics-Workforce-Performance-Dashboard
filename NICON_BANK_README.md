# 🏦 Nicon Bank — HR Analytics & Workforce Performance Dashboard

---

## 📸 Dashboard Preview

![Nicon Bank HR Dashboard](Dashboard/dashboard_preview.jpg)

> *Full interactive dashboard built in Power BI Desktop. Use the Location and Hire Year slicers to filter all visuals dynamically.*

---

## 📌 Project Overview

Nicon Bank is a growing financial institution with **200 employees** spread across five locations: Lagos, Ibadan, Kano, Abuja, and Port Harcourt. As the bank expanded over the years, management started flagging a concern that was hard to ignore — people were leaving, and no one had a clear picture of where exactly the exits were happening, which groups were most affected, or whether the bank's hiring pace was keeping up.

**41 employees** had exited the bank, translating to an **attrition rate of 20.5%** — that is 1 in every 5 employees gone, which is significant for an institution still building out its workforce and operational depth.

This project addresses that gap directly. The goal was to build a clean, interactive HR dashboard in Power BI that gives leadership a single-view snapshot of workforce composition, attrition patterns, and hiring trends — the kind of visibility that makes it possible to move from reactive firefighting to actual workforce planning.

The work covered two phases: **data cleaning in Power Query** and **dashboard development in Power BI Desktop**.

---

## 🗂️ Project Structure

```
Nicon Bank — HR Analytics & Workforce Performance Dashboard/
│
├── Data/
│   ├── Nicon_Bank_Raw_HR_Data.xlsx          # Original dataset (messy)
│   └── Nicon_Bank_HR_Data_Cleaned.xlsx      # Cleaned & transformed version
│
├── Dashboard/
│   ├── Nicon_Bank_HR_Dashboard.pbix         # Final Power BI dashboard file
│   └── dashboard_preview.jpg                # Dashboard screenshot
│
└── README.md
```

---

## ⚠️ The Data Problem

The raw HR dataset came in as a single flat table with 11 columns covering all 200 employees. At first glance it looked usable — but once you start querying it, problems surface quickly. Here is a breakdown of the specific issues that were found and how each one was resolved:

| # | Issue Found | Example from Data | Fix Applied |
|---|---|---|---|
| 1 | Gender field typos | `Femalee`, `Malee` | Corrected to `Female`, `Male` via value replacement |
| 2 | Leading whitespace in Department | `"     Operations"`, `"    IT"`, `"   HR"` | Trimmed all whitespace using *Transform → Trim* |
| 3 | Hire Date stored as Excel serial | `43717` instead of a date | Changed column type to **Date** — Power Query auto-resolved |
| 4 | Termination Date stored as serial | `46003`, `46031` | Same fix — type changed to Date format |
| 5 | Active employees with termination dates | EMP011, EMP012 flagged as Active but had exit dates | Reviewed and flagged for HR team validation |
| 6 | Blank Termination Dates for active staff | Expected, but needed explicit handling | Replaced nulls with `"N/A"` to avoid filter errors |
| 7 | Inconsistent spacing in Job Roles | Minor spacing irregularities | Trimmed and standardised |
| 8 | No Age Group / Tenure columns | Raw age only, no groupings | Created calculated columns: `Age Group`, `Tenure (Years)`, `Attrition Flag` |

---

## 🧹 Case Study 1 — Data Cleaning (Power Query)

**Tool Used:** Microsoft Power Query (via Power BI Desktop)

All cleaning was done inside Power Query before the data was ever loaded into the Power BI model. This keeps the transformation logic separate from the raw data and makes it fully reproducible — any new data drop can follow the same pipeline without manual intervention.

**Transformation Steps Applied:**

1. **Connected raw Excel file** to Power BI via *Get Data → Excel Workbook*
2. **Promoted first row as headers** — the data had no header-row issue, but this was confirmed as a first step
3. **Trimmed all text columns** — *Transform → Format → Trim* applied to Department, Gender, Job Role, and Location columns to remove leading/trailing whitespace
4. **Replaced gender typos** — `Femalee → Female`, `Malee → Male` using *Replace Values*
5. **Changed date columns from integer to Date type** — applied to `Hire Date` and `Termination Date`; Power Query resolved the Excel serial numbers automatically
6. **Replaced null Termination Dates** with `"N/A"` for active employees
7. **Created Age Group calculated column:**
   - `20–29`, `30–39`, `40–49`, `50+` banding applied via conditional column logic
8. **Created Tenure (Years) column:**
   - `= Duration.TotalDays(Date.From(DateTime.LocalNow()) - [Hire Date]) / 365`
   - Rounded to nearest whole number
9. **Created Attrition Flag column:**
   - `1` if Employee Status = `"Terminated"`, else `0`
   - Used as the base measure for all attrition rate calculations in DAX
10. **Closed and applied** — loaded the clean table to the Power BI data model

---

## 📊 Case Study 2 — HR Analytics Dashboard (Power BI)

**Tool Used:** Power BI Desktop

### 🖊️ Step 1 — Wireframing the Dashboard Layout

Before a single chart was built in Power BI, the dashboard layout was planned out as a wireframe first. This step is easy to skip but it saves a lot of time — jumping straight into Power BI without a layout plan usually means rearranging visuals multiple times and second-guessing the structure throughout.

The wireframe was sketched to map out the placement of the KPI cards at the top, the trend chart and demographic breakdowns in the middle row, and the attrition analysis charts in the bottom row. Once the layout made sense on paper, it was brought into Power BI as the foundation before any data visuals were added. This meant every chart had a predetermined home — the build process was simply a matter of filling in the structure rather than designing on the fly.

This approach also made it easier to think about what the General Manager would see first when they open the report. The KPIs sit at the very top because that is the summary — everything below it explains the numbers.

### 📊 Step 2 — Building the Dashboard

The dashboard was built on a single cleaned table using DAX measures for all KPI calculations, with slicers for **Location** and **Hire Year** to allow leadership to filter the view by specific offices or cohorts. The colour theme — black canvas with orange accents — was chosen to give the dashboard a sharp, executive feel that stands out in presentations.

---

## 📈 Dashboard Sections & Key Findings

---

### 🔢 KPI Summary Cards

| Metric | Value |
|---|---|
| Total Employees | 200 |
| Employee Exits | 41 |
| Average Tenure | 5 Years |
| Avg. Monthly Salary | ₦301,300 |
| Attrition Rate | **20.5%** |

A 20.5% attrition rate is a serious signal for a 200-person workforce. In practical terms, the bank has lost more than a fifth of its staff base — and at an average tenure of just 5 years, there is a real risk of institutional knowledge walking out the door before it gets transferred. The ₦301.3K average monthly salary is a useful baseline, but salary alone clearly is not enough to retain staff across all segments.

---

### 📅 Employee Hires and Exit Trend (2018 – 2024)

The line chart tells an interesting story. Hiring peaked in 2020 at 31 new employees and again sharply in 2022–2023 with 38 new hires — likely tied to expansion phases. But exits also tracked upward during those same periods, hitting a peak of 10 terminations in 2022–2023. The gap between hires and exits has been narrowing over time, which means the bank's net headcount growth is slowing even as total hiring activity increases. That is a retention problem, not a recruitment problem.

In 2024, the bank brought in 25 new employees but recorded 6 exits. On the surface that looks manageable, but the trajectory warrants attention if the exit rate continues climbing.

---

### 👥 Employees by Age Group

| Age Group | Count |
|---|---|
| 30–39 | 64 |
| 40–49 | 54 |
| 20–29 | 47 |
| 50+ | 35 |

The 30–39 bracket is the backbone of the workforce at 64 employees — these are typically mid-career professionals with enough experience to be high-impact contributors but enough runway to be headhunted aggressively. The 50+ group is the smallest at 35, which is expected given natural retirement timelines.

---

### ⚠️ Attrition by Age Group

| Age Group | Attrition Rate |
|---|---|
| **50+** | **31.4%** |
| 40–49 | 22.2% |
| 20–29 | 19.1% |
| 30–39 | 14.1% |

This is arguably the most important finding in the entire dashboard. **The 50+ group has the highest attrition rate at 31.4%** — nearly one in three employees in this bracket has exited. For a bank, this is a significant risk because senior employees in this age range typically hold client relationships, institutional memory, and mentorship responsibilities that are hard to replace quickly.

Interestingly, the 30–39 group — the largest cohort — has the lowest attrition rate at 14.1%. This suggests that mid-career employees are generally stable, but the bank is haemorrhaging experienced senior talent at one end and losing younger staff (20–29 at 19.1%) before they fully develop at the other.

---

### 🏢 Attrition by Department

| Department | Attrition Rate |
|---|---|
| **Sales** | **27.8%** |
| HR | 22.0% |
| Finance | 20.6% |
| Operations | 19.2% |
| IT | 17.2% |

**Sales has the worst attrition problem at 27.8%**, which should be a red flag for the board. Sales teams are revenue-generating functions — high turnover here directly threatens customer relationships, loan pipeline activity, and cross-selling performance. The fact that Sales is also the smallest department (only 18 employees) makes each exit proportionally more disruptive.

HR at 22% is paradoxical — the very department responsible for talent retention is itself struggling to retain its own people. This points to structural issues possibly around workload, compensation benchmarking, or internal culture rather than role-specific dissatisfaction.

IT at 17.2% is the lowest of the five departments, but in the current Nigerian fintech and banking technology landscape where tech talent is intensely competed for, maintaining that rate will require proactive action.

---

### 🚻 Attrition by Gender

| Gender | Attrition Share |
|---|---|
| Male | 54.21% |
| Female | 45.79% |

Male employees account for a slightly higher share of exits at 54.21%, but the split is close enough that gender alone is not a strong predictor of attrition here. The more useful framing is that both genders are leaving — this is not a single-group problem. The focus should remain on department and age group as the primary drivers rather than gender.

---

### 📍 Employees by Location

| Location | Headcount |
|---|---|
| Lagos | 46 |
| Ibadan | 45 |
| Kano | 43 |
| Abuja | 39 |
| Port Harcourt | 27 |

Lagos leads marginally but the distribution across the five locations is reasonably balanced — which is a good sign for geographic risk. Port Harcourt is the smallest office at 27 employees. Given that it is also operationally active, any concentrated exits there would be felt immediately. The Location slicer on the dashboard allows leadership to isolate each branch and view all metrics specific to that office.

---

### 🏬 Employees by Department

| Department | Headcount |
|---|---|
| Operations | 78 |
| HR | 41 |
| Finance | 34 |
| IT | 29 |
| Sales | 18 |

Operations dominates at 78 employees — nearly 40% of the total workforce. This makes sense for a bank where day-to-day transactional activity, branch management, and card operations require significant staffing. Sales at just 18 is small for a financial institution, and given its 27.8% attrition rate, the department is operating under genuine strain.

---

## 💡 Workforce Recommendations

Based on the patterns the dashboard surfaces, here are the areas that leadership should prioritise:

**1. Build a Senior Retention Programme**
The 50+ group's 31.4% attrition rate is too high to ignore. These employees carry years of client-facing experience and institutional knowledge. Before more of them exit, the bank should consider structured exit interview analysis for this cohort, succession planning for senior roles, and targeted retention incentives — whether through salary reviews, flexible scheduling, or recognition programmes.

**2. Fix the Sales Department Before It Gets Worse**
At 27.8% attrition and just 18 people, the Sales department is operating at near-critical fragility. One or two more exits and performance will visibly suffer. The bank should urgently review sales compensation structure, sales team targets, and whether the current team is adequately supported by CRM tools and management oversight.

**3. Investigate HR's Own Turnover**
An HR department with 22% attrition is a structural red flag. The team responsible for engagement and retention should itself be stable. A confidential pulse survey specifically for the HR function would help identify what is driving this — whether it is workload, compensation gaps, or management style.

**4. Close the Hire-Exit Gap**
The trend chart shows that net headcount growth is slowing because exits are rising alongside hires. Simply recruiting more people is not the answer if the underlying retention issues are not addressed. The bank needs to measure 12-month survival rates for new hires and track where the early exits are concentrating.

**5. Monitor Port Harcourt Closely**
With only 27 employees, Port Harcourt is the most vulnerable office to disruption. Any concentrated exits there — particularly in Operations or Finance — would be immediately felt. A quarterly check-in on Port Harcourt-specific engagement metrics is advisable.

**6. Develop a Mid-Career Engagement Strategy for the 30–39 Group**
This cohort has the lowest attrition at 14.1% and is the largest group — which means the bank is doing something right here. Understanding what is working for the 30–39 bracket and applying those lessons to the higher-risk groups (50+ and 20–29) could meaningfully move the attrition rate in the right direction.

---

## 🛠️ Tools & Techniques Used

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Dashboard layout, charts, KPI cards, slicer interactivity, DAX measures |
| **Power Query** | Data cleaning, type conversion, column creation, whitespace handling |
| **DAX** | Attrition Rate, Avg. Tenure, Avg. Salary, Attrition Flag measures |
| **Power BI Slicers** | Location and Hire Year interactive filters |

**Key DAX Measures Written:**

```dax
Total Employees = 
COUNTROWS('HR_Data')

No. of Employee Exits = 
COUNTROWS(
    FILTER('HR_Data', 'HR_Data'[Employee Status] = "Terminated")
)

Attrition Rate = 
DIVIDE(
    COUNTROWS(FILTER('HR_Data', 'HR_Data'[Employee Status] = "Terminated")),
    COUNTROWS('HR_Data'),
    0
)

Avg Monthly Salary = AVERAGE('HR_Data'[Monthly_Salary])

Avg Tenure = 
AVERAGEX(
    'HR_Data',
    DATEDIFF('HR_Data'[Hire Date], TODAY(), YEAR)
)
```

---

## 🚀 How to Use This Project

1. Download the `.pbix` file from the `dashboard/` folder in this repository
2. Open it in **Power BI Desktop** (free download at [powerbi.microsoft.com](https://powerbi.microsoft.com))
3. The dashboard will load with all visuals fully rendered
4. Use the **Location** dropdown slicer to filter by branch (Lagos, Ibadan, Kano, Abuja, Port Harcourt)
5. Use the **Hire Year** slicer to narrow the view to specific recruitment cohorts
6. All KPI cards and charts update dynamically based on slicer selection

> **Note:** If the data connection prompts for a file path update, go to *Transform Data → Data Source Settings* and redirect to the local path of the Excel file.

---

## 👤 About the Analyst

This project was built by **Favour Chidozie Chukwu**, a Data and Credit Risk Analyst with practical experience in financial data analysis, HR analytics, credit risk reporting, and business intelligence across Nigerian financial institutions.

- 📧 Email: [favour.chukwu@aol.com](mailto:favour.chukwu@aol.com)
- 📞 Phone: +234 806 513 5360
- 💼 LinkedIn: [linkedin.com/in/chidozie-chukwu-145690162](https://www.linkedin.com/in/chidozie-chukwu-145690162)
- 📸 Instagram: [@chidozie_favor](https://www.instagram.com/chidozie_favor?igsh=aG94eWNnbzR4NzA=)

---

## 📄 License

This project is open for reference, learning, and portfolio inspiration. If you use any part of it, a mention is always appreciated.

---

*Built with Power BI · Power Query · A genuine interest in why people leave organisations 📊*
