---

# 🧩 CFPB Consumer Financial Complaints Analytics Dashboard

### Data-Driven Insights into Consumer Financial Protection and Corporate Accountability

**Developed by:** *Emmanuel Idowu*
**Tool:** Microsoft Power BI | **Dataset Source:** Consumer Financial Protection Bureau (CFPB)

---

## 📘 **Project Overview**

The **CFPB Complaints Analytics Dashboard** provides a comprehensive analysis of consumer complaints lodged with the **Consumer Financial Protection Bureau**.
It enables stakeholders to monitor **trends, product-level complaint patterns, response efficiency, and company performance** over time.

The dashboard was built to answer key questions such as:

* How have consumer complaints evolved across time and states?
* Which products and issues drive the most dissatisfaction?
* How do companies compare in responsiveness and accountability?

---

## 🧠 **Analytical Data Model**

The data model was designed using a **Star Schema** to optimize for performance, interactivity, and clarity.

### **Fact Table**

#### `FactComplaints`

Contains detailed complaint-level data with metrics such as:

* Complaint ID
* Date Received
* Company ID
* Product & Sub-Product
* Issue & Sub-Issue
* State & Census Region
* Timely Response Indicator
* Response Time (Days)
* Market Share %

### **Dimension Tables**

| Table           | Description                                                   |
| --------------- | ------------------------------------------------------------- |
| **DimCompany**  | Company details, size tier, market share, enforcement history |
| **DimProduct**  | Product and sub-product classification                        |
| **DimIssue**    | Issue and sub-issue categories                                |
| **DimLocation** | State, region, population for per-capita complaint analysis   |
| **DimDate**     | Year, quarter, month, and day hierarchies                     |

### **Relationships**

* One-to-many relationships from each dimension to `FactComplaints`.
* Time intelligence and drill-down functionality enabled through `DimDate`.
* Geography and product hierarchies support **ZoomCharts Drill Down Visuals** for deeper exploration.

---

## 📊 **Dashboard Pages & Visuals**

### **Page 1: Complaint Overview**

**Objective:** Understand national complaint trends, timeliness, and regional distribution.

#### **Visuals**

| Visual           | Title                                                                              | Description                                                   |
| ---------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **KPI Cards**    | Total Complaints • Avg Response Time (Days) • Timely Response % • High-Risk States | Tracks overall performance and YoY changes                    |
| **Bar Chart**    | *“How have complaints and response efficiency evolved across time?”*               | Yearly to daily trends of complaints and timely responses     |
| **Map Chart**    | *“Which states experience the highest complaint intensity per 100K residents?”*    | Complaints normalized by population for fairness              |
| **Scatter Plot** | *“Which products face the highest complaint pressure and slowest response?”*       | Compares total complaints vs average response time by product |
| **Donut Chart**  | *“Which regions contribute most to national complaints?”*                          | Visualizes regional distribution and division-level insights  |

#### **Key Insight Measure Example**

```DAX
Page1_Insights =
"📅 Complaints have shown year-on-year fluctuations, with notable spikes in 2021 and 2022. 
States like California and Florida exhibit the highest complaint intensity per 100K residents, 
while the South region contributes the largest share of national complaints. 
Products such as credit cards and savings accounts drive both volume and slower responses, 
indicating areas for operational improvement."
```
<img width="1499" height="845" alt="Screenshot_60" src="https://github.com/user-attachments/assets/b202b367-d557-4053-9e20-8292cca5cf68" />

---

### **Page 2: Product & Issue Deep Dive**

**Objective:** Analyze products, issues, and submission channels driving consumer dissatisfaction.

#### **Visuals**

| Visual                  | Title                                                                                             | Description                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **KPI Cards**           | Top Product Category • Fastest Product Response • Slowest Product Response • High Severity Issues | Highlights products and issues influencing complaint volume    |
| **Bar Chart (Product)** | *“Which products generate the most complaints and how efficiently are they resolved?”*            | Complaint volume vs timely response by product                 |
| **Bar Chart (Issue)**   | *“What are the top issues driving customer dissatisfaction?”*                                     | Top 10 complaint issues for targeted intervention              |
| **Scatter Plot**        | *“Does the submission channel affect complaint handling speed?”*                                  | Compares complaint load and response time by submission method |

#### **Key Insight Measure Example**

```DAX
Page2_Insights =
"💡 Checking and savings accounts record the highest complaint volumes, 
while 'Payday loan and personal loan' categories show the slowest response times. 
The 'Credit reporting and repair services' category demonstrates the fastest resolution speed. 
Major consumer pain points include account management issues and incorrect information on reports, 
indicating a need for stronger data accuracy and communication workflows."
```
<img width="1499" height="848" alt="Screenshot_61" src="https://github.com/user-attachments/assets/8eb7a500-dc78-4a25-95c8-503e67179852" />

---

### **Page 3: Company Performance & Accountability**

**Objective:** Benchmark companies by complaint rate, responsiveness, and enforcement impact.

#### **Visuals**

| Visual                                      | Title                                                                                    | Description                                                 |
| ------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **KPI Cards**                               | Worse Performer • Best Performer • Enforcement Impact • Large/Medium/Small Co. Advantage | Evaluates corporate performance and enforcement correlation |
| **Clustered Bar Chart (Top 10 Companies)**  | *“Which top 10 companies handle complaints most efficiently relative to market size?”*   | Shows complaint rate per 1% market share vs timeliness      |
| **Clustered Bar Chart (Company Size Tier)** | *“Does company size influence complaint handling effectiveness?”*                        | Compares load, responsiveness, and speed across tiers       |
| **Matrix Table**                            | *“How do complaint performance metrics compare across companies?”*                       | Consolidated company-level breakdown for accountability     |
| **Bar Chart (Top/Bottom 5)**                | *“Which companies are improving or worsening in complaint management (YoY)?”*            | Tracks complaint growth rate by company                     |

#### **Key Insight Measure Example**

```DAX
Page3_Insights =
VAR _WorstComp = [Worse Performer]
VAR _WorstRate = CALCULATE(SELECTEDVALUE([Complaints per 1pct Share]), FILTER(ALL(DimCompany), DimCompany[Company_ID] = _WorstComp))
VAR _BestComp = [Best Performer]
VAR _BestRate = CALCULATE(SELECTEDVALUE([Complaints per 1pct Share]), FILTER(ALL(DimCompany), DimCompany[Company_ID] = _BestComp))
VAR _EnforceImpact = [Enforcement Impact for Complaint]
VAR _LargeAdv = [Large Co. Advantage]
VAR _TopGrowth = CALCULATE(MAXX(TOPN(1, VALUES(DimCompany[Company_ID]), [Complaint Growth Rate]), [Complaint Growth Rate]))
VAR _BottomGrowth = CALCULATE(MINX(TOPN(1, VALUES(DimCompany[Company_ID]), [Complaint Growth Rate], ASC), [Complaint Growth Rate]))
RETURN
"🏢 Company " & _WorstComp & " recorded the highest complaint rate (" &
FORMAT(_WorstRate, "#,0") & " complaints per 1% market share), while " &
_BestComp & " maintained the lowest (" & FORMAT(_BestRate, "#,0") & "). " &
"Enforcement-linked firms showed a " & FORMAT(_EnforceImpact, "0.0%") &
" difference in complaint rates. Large companies achieved an average timely response rate of " &
FORMAT(_LargeAdv, "0.0%") &
". Complaint growth rates varied — Top improver at " &
FORMAT(_TopGrowth, "0.0%") & "% vs biggest decliner at " &
FORMAT(_BottomGrowth, "0.0%") & "% — spotlighting accountability and performance gaps."
```
<img width="1498" height="846" alt="Screenshot_62" src="https://github.com/user-attachments/assets/f3d80d99-e722-4264-a1dd-dfbadc7b2765" />

---

## 📈 **Core DAX Measures**

| Measure                            | Description                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| `Total Complaints`                 | COUNT of complaint IDs                                                          |
| `Avg Response Time (Days)`         | AVERAGEX of response duration                                                   |
| `Timely Response %`                | % of complaints responded to within 15 days                                     |
| `Complaints Per 1% Market Share`   | `DIVIDE([Total Complaints], [Total Market Share %], 0)`                         |
| `Complaints Per 100K`              | `DIVIDE([Total Complaints], [Total Population]) * 100000`                       |
| `Complaint Growth Rate (YoY)`      | `DIVIDE([Total Complaints] - [Total Complaints LY], [Total Complaints LY])`     |
| `Enforcement Impact for Complaint` | Compares complaint rates between companies with and without enforcement history |
| `Large/Medium/Small Co. Advantage` | Average timely response rate by company size tier                               |
| `High Severity Issues Display`     | Returns top issue, affected consumers, and % of total                           |

---

## 🧩 **Key Insights & Recommendations**

* **Complaint Volume:** Significant increase in 2021–2022, with high concentration in the **South and West regions**.
* **Performance Bottlenecks:** Financial products such as **loans and savings accounts** face the most service delays.
* **Company Accountability:** Large companies generally perform better in response rates, though some still exhibit high complaint intensity.
* **Enforcement Impact:** Companies with enforcement records show only marginally fewer complaints — indicating compliance measures are yet to yield strong behavioral shifts.
* **Operational Focus:** Target underperforming states and issues for faster resolution and improved consumer trust.

---

## REPORT LINK: [CLICK HERE](https://app.powerbi.com/view?r=eyJrIjoiMzM5NGEzYjEtYzdlZC00ZGVkLWEyMjctMjc0ODNmMmU1M2U5IiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)

---

## 🧰 **Tools and Techniques**

* **Power BI Desktop**
* **DAX (Data Analysis Expressions)**
* **Power Query for ETL**
* **ZoomCharts Drill Down Visuals**
* **Data Modeling (Star Schema)**
* **KPI Indicators & YoY Calculations**
* **Dynamic Narrative Measures (Text Insights)**

---

## 🏁 **Conclusion**

This dashboard provides **a unified and actionable view** of consumer complaint data, blending **regulatory transparency, product analysis, and company accountability**.
It empowers regulators and financial institutions to **identify systemic issues, benchmark performance, and improve consumer experience** through data-backed decisions.

---
