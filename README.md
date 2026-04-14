<div align="center">

# Novalife Health Services — Clinical & Operational Analytics (FY 2023)

**Uncovering revenue gaps, pharmacy compliance risks, and patient demand patterns across a Nigerian community hospital using real-world dirty data.**

[![License](https://img.shields.io/badge/License-MIT-green.svg)](#license)
[![Excel](https://img.shields.io/badge/Microsoft_Excel-2019%2B-217346?logo=microsoft-excel&logoColor=white)](#tech-stack)
[![Data](https://img.shields.io/badge/Records-432_rows-1D9E75)](#dataset)
[![Status](https://img.shields.io/badge/Status-Completed-085041)](#project-overview)

</div>

---

## Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Dataset](#dataset)
- [Data Quality Issues Found & Fixed](#data-quality-issues-found--fixed)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Pharmacy Compliance & Patient Safety](#pharmacy-compliance--patient-safety)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Visualizations](#visualizations)
- [Limitations & Future Work](#limitations--future-work)
- [License](#license)
- [Contact](#contact)

---

## Project Overview

This project is a full end-to-end data analytics exercise carried out on a deliberately dirty, real-world-style healthcare dataset representing **Novalife Health Services**, a fictional Nigerian community hospital. The dataset covers patient visits, pharmacy sales, and staff attendance records for the financial year 2023.

The project follows a complete analytics workflow from raw dirty data through cleaning, exploratory analysis, pivot table construction, financial analysis, interactive dashboard building, and a final written report addressed to hospital management.

> **Key result:** Of the ₦10,243,233 billed to patients in 2023, only 40.1% (₦4,103,685) has been fully collected — while ₦9,530 worth of expired drugs were sold to patients, representing both a financial liability and a patient safety risk requiring immediate action.

---

## Business Problem

**Problem statement:** Novalife Health Services had no structured way of understanding its own operational performance. Patient billing records, pharmacy sales, and staff data were stored in separate raw tables with no consistent formatting, multiple entry errors, and no analytical summary. The hospital manager needed clear answers to key questions: Are we collecting the money we bill? Which departments and conditions are driving demand? Is our pharmacy operating safely and legally? How reliable is our data?

**Objectives:**
- Clean and standardise three dirty datasets covering 450 total records across patient visits, pharmacy sales, and staff attendance
- Identify and document every data quality issue with an audit trail
- Build pivot tables and an interactive Excel dashboard to answer key operational questions
- Conduct financial analysis to quantify the revenue gap and pharmacy compliance exposure
- Deliver a plain-language report with actionable recommendations to the Hospital Manager

---

## Dataset

| Attribute | Details |
|-----------|---------|
| **Source** | Synthetically generated dirty dataset (realistic simulation) |
| **Total Records** | 432 rows across 3 tables |
| **Time Period** | January 2023 – December 2023 |
| **Granularity** | Per patient visit / per pharmacy sale / per staff attendance record |
| **Hospital** | Novalife Health Services (fictional) |
| **Geography** | 8 Nigerian states: Enugu, Lagos, Oyo, Rivers, Kaduna, Abuja, Kano, Delta |
| **License** | MIT |

### Table 1 — Patient_Visits (190 records, 21 columns)

| Column | Type | Description |
|--------|------|-------------|
| `Patient_ID` | `string` | Unique patient identifier (P001–P200 format) |
| `Patient_Name` | `string` | Full name of the patient |
| `Age` | `integer` | Patient age in years (range: 0–75) |
| `Gender` | `string` | Male / Female (8 records had unknown gender — cleaned) |
| `State` | `string` | Nigerian state the patient came from |
| `Visit_Date` | `date` | Date of hospital visit (all within 2023 after cleaning) |
| `Month` | `string` | Month name derived from visit date |
| `Department` | `string` | Hospital department visited |
| `Doctor_Name` | `string` | Attending doctor |
| `Diagnosis` | `string` | Primary diagnosis recorded |
| `Visit_Type` | `string` | First Visit / Follow-Up / Routine Check-Up / Emergency |
| `Blood_Pressure` | `string` | Systolic/Diastolic reading (e.g. 120/80) |
| `Heart_Rate_bpm` | `integer` | Heart rate in beats per minute |
| `Body_Temp_C` | `float` | Body temperature in Celsius (some values in Fahrenheit were converted to Celsius) |
| `Weight_kg` | `float` | Patient weight in kilograms |
| `Height_cm` | `integer` | Patient height in centimetres |
| `BMI` | `float` | Body Mass Index (calculated) |
| `Payment_Method` | `string` | Cash / NHIS / HMO / POS / Bank Transfer |
| `Bill_Amount_NGN` | `integer` | Total bill charged in Nigerian Naira |
| `Payment_Status` | `string` | Paid / Unpaid / Pending / Waived |
| `Follow_Up_Needed` | `string` | Yes / No |

### Table 2 — Pharmacy_Sales (142 records, 14 columns)

| Column | Type | Description |
|--------|------|-------------|
| `Sale_ID` | `string` | Unique sale identifier |
| `Patient_ID` | `string` | Foreign key linking to Patient_Visits |
| `Sale_Date` | `date` | Date of drug sale |
| `Drug_Name` | `string` | Name and dosage of drug sold |
| `Quantity_Sold` | `integer` | Number of units dispensed |
| `Unit_Price_NGN` | `integer` | Price per unit in Naira |
| `Total_Price_NGN` | `integer` | Total sale value (Quantity × Unit Price) |
| `Pharmacist` | `string` | Name of dispensing pharmacist |
| `Prescription_Required` | `string` | Whether a prescription is legally required for the drug |
| `Prescription_Provided` | `string` | Whether the patient actually brought a prescription |
| `Prescription_Compliance` | `string` | Yes if both match; No if required but not provided |
| `Stock_After_Sale` | `integer` | Remaining stock units after the sale |
| `Expiry_Date` | `date` | Drug expiry date |
| `Expiry_Drug_Sold` | `string` | Yes if sale date was after expiry date |

### Table 3 — Staff_Attendance (100 records, 10 columns)

| Column | Type | Description |
|--------|------|-------------|
| `Record_ID` | `string` | Unique attendance record identifier |
| `Staff_Name` | `string` | First name of the staff member |
| `Staff_Role` | `string` | Doctor / Nurse / Pharmacist / Lab Technician / Porter / Admin |
| `Department` | `string` | Department the staff member belongs to |
| `Date` | `date` | Date of attendance record |
| `Clock_In` | `time` | Time the staff member clocked in |
| `Clock_Out` | `time` | Time the staff member clocked out |
| `Hours_Worked` | `integer` | Total hours worked that shift |
| `Shift_Type` | `string` | Morning / Afternoon / Night (derived from Clock_In hour) |
| `Attendance_Status` | `string` | Present / Absent |

> **Note:** Raw dirty data is intentionally included in this repo for educational purposes. The cleaned version is the primary working file.

---

## Data Quality Issues Found & Fixed

A total of **23 dirty issues** were identified and resolved across all three tables. Every fix was documented in a `Data_Issue_Flag` column added to each cleaned sheet.

### Patient_Visits Issues (11 issues)

| # | Issue | Example | Fix Applied |
|---|-------|---------|-------------|
| 1 | Missing patient names | Blank cells | Removed them |
| 2 | Extra spaces in names |  `"  Chidi Nwosu  "`  | Trimmed to remove the space |
| 3 | Negative / impossible ages | `-3`, `150` | Changed to positive values |
| 4 | Inconsistent gender coding | `M`, `F`, `1`, `0`, `male` | Standardised to Male / Female |
| 4 | Missing gender | Blank Cells | Flagged as `Unknown`  |
| 5 | Future visit dates | Year 2027 entries | Removed because it was an erroneous entry |
| 6 | Missing diagnosis entries | Blank cells | Flagged as `Incomplete Record` |
| 7 | Impossible blood pressure | Systolic < Diastolic (e.g. 60/110) | Changed the positons of the numbers so that Diastolic > Systoic |
| 8 | Temperature in Fahrenheit | Values above 95 in Celsius column | Converted using `(F-32)×5/9` |
| 9 | Negative bill amounts | `-500`, `-2000` | Changed to Positive Values |
| 10 | Bill = ₦0 but Status = Paid | Zero amount, "Paid" label | Status changed to `Waived` |
| 11 | Duplicate Patient IDs | P015 appearing twice | Removed Duplicate |

### Pharmacy_Sales Issues (9 issues)

| # | Issue | Example | Fix Applied |
|---|-------|---------|-------------|
| 12 | Missing Patient IDs | Blank cells | Removed them |
| 13 | Negative quantity sold | `-1`, `-5` | Changed to Positive Values |
| 14 | Total ≠ Quantity × Unit Price | ₦2,500 instead of ₦2,000 | Recalculated |
| 15 | Future sale dates | Year 2028 entries | Removed because it was an erroneous entry |
| 16 | Prescription required but not provided | Required = Yes, Provided = No | Recorded as `Yes` in newly-created `Prescription_Compliance` |
| 17 | Prescription not required and not provided | Required = No, Provided = No | Recorded as `No` in newly-created `Prescription_Compliance` |
| 18 | Expired drugs sold | Expiry 2021, sold in 2023 | Recorded as `Yes` in newly-created `Expiry_Drug_Sold` |
| 19 | Expired drugs sold | Expiry 2024, sold in 2023 | Recorded as `No` in newly-created `Expiry_Drug_Sold` |
| 20 | Negative stock after sale | `-10`, `-50` | Changed to Positive Values |

### Staff_Attendance Issues (9 issues)

| # | Issue | Fix Applied |
|---|-------|-------------|
| 21 | Staff name attached with their titles | Removed the titles |
| 22 | `Clock-in` and `clock-out` not properly formatted | Changed the datatype |
| 23 | Clock-out time before clock-in time | Swapped |
| 24 | Hours_Worked does not match actual time difference | Recalculated |
| 25 | Future attendance dates | Removed |
| 26 | Negative hours worked | Changed to Positive Values |
| 27 | No clock-in or clock-out recorded | Recorded as O in `Hours_Worked (Updated)` |
| 28 | Clock-in or clock-out recorded | Recorded as `Present` in `Attendance_Status (Updated)` |
| 29 | No clock-in or clock-out recorded | Recorded as `absent` in `Attendance_Status (Updated)` |

---

## Methodology

```
Raw Dirty Data → Audit & Profiling → Cleaning → EDA → Pivot Analysis → Financial Analysis → Dashboard → Report
```

1. **Data Audit** — Manually profiled all 3 sheets to identify every dirty issue before touching any data. 

2. **Data Cleaning** — Fixed or flagged every dirty issue using Excel formulas and Power Query.  

3. **Exploratory Data Analysis (EDA)** — Built 12 pivot tables covering department visit counts, diagnosis frequency, gender distribution, state breakdown, follow-up rates, average billing by payment method, and payment status by payment method.

4. **Financial Analysis** — Calculated total billed vs collected revenue, quantified the revenue gap by payment status, identified the peak and lowest revenue months, top 3 drugs by revenue, and assessed pharmacy compliance risk in monetary terms.

5. **Dashboard** — Built a single-sheet interactive Excel dashboard with 5 KPI cards, 5 charts (line, bar,pie). All charts use the dark teal green to represent the highest value, except the stacked bar chart.

6. **Slicer & Timeline Setup** — Created a Month slicer, Department slicer, and State slicer connected to all patient visit pivot tables. 

7. **Report Writing** — Produced a formal report addressed to the Hospital Manager, covering executive summary, KPI table, key findings, pharmacy safety section, and 8 prioritised recommendations.

---

## Key Findings

- **January was the busiest visit month** with 24 patient visits, while February had the lowest footfall at 10 visits. November and December tied for second with 20 visits each, suggesting year-end demand.

- **Eye Clinic was the most visited department** (41 visits, 21.6% of all visits), closely followed by Cardiology (39 visits). Together these two specialist departments handled 42% of total patient volume.

- **Malaria was the most common diagnosis** — 31 cases, representing 16.3% of all diagnosed visits. Upper Respiratory Infections followed at 27 cases, and Anaemia at 24. The top 5 diagnoses alone accounted for 63.2% of all cases.

- **Visit type was evenly split** between Routine Check-Ups (51), Follow-Ups (51), First Visits (44), and Emergencies (44) — indicating a balanced mix of scheduled and unplanned care.

- **Enugu State sent the most patients** (40 visits, 21.1%), while Delta had the fewest at 15. The hospital draws patients from all 8 states in the dataset, suggesting a reasonably wide geographic reach.

- **Gender was almost exactly equal** — 91 female and 91 male patients after cleaning. 8 records (4.2%) could not be verified and remained as Unknown.

- **Dr. Adeyemi handled the most patients** (43 visits), closely followed by Dr. Bello (42). Dr. Ibrahim and Dr. Chukwu had the fewest at 34, suggesting a slight imbalance in patient distribution across doctors.

- **Total revenue billed in 2023 was ₦10,243,233** but only ₦4,103,685 (40.1%) was fully paid. A further ₦2,821,759 is pending and ₦3,317,789 is unpaid, meaning 59.9% of billed revenue (₦6,139,548) has not been collected.

- **December was the strongest revenue month** at ₦1,230,181, followed by January at ₦1,197,230. October was the weakest month at just ₦515,372 — a 58% drop from the December peak.

- **NHIS patients generated the highest average bill** per visit at ₦60,306, compared to Bank Transfer patients who averaged the lowest at ₦51,025. Despite the high average, NHIS had the worst pending rate — 18 out of 38 NHIS patients (47.4%) had pending bills, likely due to slow insurer reimbursement cycles.

- **Bank Transfer payments had the highest unpaid rate** — 18 out of 29 Bank Transfer patients (62.1%) had unpaid bills, the worst non-payment rate of any payment method.

- **48% of patients required a follow-up** appointment. This is a meaningful figure for capacity planning since nearly half of all visitors are expected to return, which should inform scheduling and bed/room availability decisions.

---

## Pharmacy Compliance & Patient Safety

This section represents the most urgent findings in the entire project.

### Non-Compliant Prescription Sales

- **4 transactions** were identified where a prescription-only drug was dispensed without a valid prescription being presented by the patient.
- The **combined sales value of non-compliant transactions is ₦10,950** (5.74% of total pharmacy revenue).
- Drugs involved include **Metformin 500mg** (antidiabetic), and **Amoxicillin 500mg** (antibiotic). They are all prescription-only medications with real health risks if misused.
- Under the **NAFDAC Act and PCN guidelines**, dispensing prescription drugs without a valid prescription is a regulatory offence. A routine NAFDAC audit would flag these records and could result in fines, licence suspension, or prosecution.
- **Non-compliant sales were attributed to Pharm. Bello and Pharm. Tunde with two sales each.**

### Expired Drug Sales

- **Multiple transactions** were found where drugs with expiry dates in **March 2021** were sold to patients in 2023 — a gap of over two years past expiry.
- The **total value of expired drug sales is ₦9,530**.
- Drugs sold expired include **ORS Sachets** and other medications. Expired drugs may be completely ineffective or chemically degraded, meaning patients received no real treatment.
- This is classified as a **Critical Patient Safety Issue** and a violation of the Counterfeit and Fake Drugs Miscellaneous Provisions Act.
- **Pharm. Bello and Pharm. Tunde were also responsible for the sale of expiry drugs, totaling 4 and 1 sales respectively.**

### Pharmacy Volume by Pharmacist

| Pharmacist | Transactions | % of Total |
|------------|-------------|------------|
| Pharm. Tunde | 53 | 37.3% |
| Pharm. Bello | 45 | 31.7% |
| Pharm. Eze | 44 | 31.0% |

### Top Revenue-Generating Drugs

| Drug | Total Revenue |
|------|--------------|
| Artemether-Lumefantrine | ₦58,800 |
| Amlodipine 5mg | ₦26,250 |
| Vitamin C 500mg | ₦20,100 |
| **Grand Total (all drugs)** | **₦190,720** |


---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Microsoft Excel 2019+ | Pivot tables, dashboard, charts |
| Power Query (Excel) | Data cleaning, Column type conversion, data transformation |
| Excel Pivot Tables | Aggregations and summaries across all 3 datasets |
| Excel Slicers | Interactive filtering on the dashboard |
| Microsoft Word | Formal analyst report for hospital management |

---

## Project Structure

```
novalife-health-analytics/
├── data/
│   ├── raw/
│   │   └── Nigeria_Community_Health_EntryLevel.xlsx   # Original dirty dataset
│   └── processed/
│       └── health.xlsx                                # Cleaned & analysed workbook
├── outputs/
│   ├── dashboard/
│   │   └── Novalife_Dashboard.xlsx                    # Final interactive dashboard
│   ├── reports/
│   │   └── Novalife_Hospital_Manager_Report.docx      # Formal Word report
├── README.md
└── LICENSE
```

---

## Visualizations

The Excel dashboard contains 5 charts built from pivot data.

| Chart | Type | Key Insight |
|-------|------|-------------|
| Visits by Department | Bar chart | Eye Clinic leads at 41 visits |
| Top 5 Diagnoses | Bar chart | Malaria dominates at 31 cases |
| Monthly Revenue Trend | Line chart | December peak ₦1.23M; October low ₦515K |
| Patient Gender Distribution | Pie chart | Near-equal split: 91F / 91M / 8 Unknown |
| Payment Method by Payment Status | Stacked bar chart | Bank Transafer has the highest unpaid rate |

> Dashboard: <img width="1031" height="417" alt="Image" src="https://github.com/user-attachments/assets/4cb22220-2b85-4d3e-b6e9-3f1968bdcce0" />

**KPI Cards on Dashboard:**

| KPI | Value |
|-----|-------|
| Total Billed Amount | ₦10,243,233 |
| Paid Revenue Rate |  ₦4,103,685  |
| Highest-Demand State | Enugu (40 visits) |
| Peak Revenue Month | December (₦1,230,181) |
| Most-Visited Department | Eye Clinic (41 visits) |

---

## Limitations & Future Work

**Current limitations:**
- The dataset is synthetically generated and covers only one financial year (2023), which is not enough to establish reliable multi-year trends
- Staff attendance data covers 100 records but only 8 unique staff names, limiting the depth of workforce analysis
- The pharmacy dataset (142 records) does not include a Department column, which prevents cross-filtering pharmacy charts by department on the dashboard
- Payment Status does not capture partial payments — a bill is either fully Paid, Unpaid, or Pending, with no middle ground for instalment tracking
- 8 patient records (4.2%) have unresolved Unknown gender after cleaning, which slightly affects gender-based analysis

**Planned improvements:**
- [ ] Add a Department column to Pharmacy_Sales to enable full cross-sheet slicer connectivity
- [ ] Expand dataset to cover multiple years for trend and seasonality analysis
- [ ] Build a predictive model to flag patients at high risk of non-payment before discharge
- [ ] Add a patient readmission tracking table to identify frequent visitors

---

## License

Distributed under the **MIT License**. This project is for educational and portfolio purposes. The hospital name, patient names, and all data are entirely fictional.

---

## Contact


* **LinkedIn:** [al-ameen-lamina](https://www.linkedin.com/in/al-ameen-lamina/)
* **Email:** [laminaalameen2022@gmail.com](mailto:laminaalameen2022@gmail.com)


