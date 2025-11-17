# Customer Return Analysis – Power BI Consulting Simulation

This project is a simulated consulting assignment provided by **P3 Adaptive**, designed to assess analytical, modeling, and presentation capabilities in Power BI. It focuses on analyzing customer retention patterns using a star-schema data model and DAX measures.

---

## Problem Statement

P3 Adaptive provided anonymized sales and customer data along with a legacy customer return report. The task was to:
- Build a **Power BI data model** following star-schema best practices.
- Use only **measures (no calculated columns or pre-aggregated tables)** to recreate the existing legacy report.
- Design a report that supports **future data growth and time intelligence**.
- Create a **demo page** showcasing creative analytical insights and visual storytelling.

---

## Data Model Overview

A clean **star-schema model** was created, consisting of:
- **Fact Table**: `Sales` – transactional order data.
- **Dimension Tables**:
  - `Customers` – customer metadata and first purchase date.
  - `Date` – custom, fully marked date table with time-intelligence support.

 The model leverages a custom Date table generated using `CALENDAR()` and serves as the base for cohort analysis.

---

## Key DAX Measures

### Core Logic
- `FP`: First purchase date per customer.
- `First Return After FP`: The first order after the first purchase.
- `Has Return 1-90`: Binary flag if return made within 90 days.
- `Has Return Next 3 Months`: Binary flag for return in next 3 calendar months.

### Aggregated Metrics
- `Cohort Customers`
- `Customers Returned 1-90`
- `Customers Returned Next 3 Months`
- `% Returned within 90 Days`
- `% Returned in Following 3 Months`

> Full DAX code is included [here](./DAX_Measures.md) for reference.

---

## Report Pages

### Legacy Report (Recreated as instructed)
Demonstrates:
- Cohort-wise monthly return percentages.
- Two benchmark measures:  
  - **Returned within 90 Days**  
  - **Returned in Following 3 Months**  
- Grand totals included.

![Legacy Report](./images/PowerBI_Report_1.png)

### Demo Page
Shows a more creative analysis with:
- KPIs for customer retention trends.
- Visualizations for insights beyond the legacy report.

![Demo Report](./images/PowerBI_Report_2.png)

---

## Result & Recommendation

After iterating with recruiter feedback:
- **90-Day Return Rate** is recommended for acquisition effectiveness.
- **3-Month Return Rate** is suitable for retention analysis.
- Both validated to show first valid cohort starting April 2003.

---

## Mistakes & Lessons Learned

### Mistakes
- Used `CustomerKey` instead of `AltCustomerKey` for relationships.
- Misinterpreted the “same-day purchase” as a return.
- Delayed first cohort calculation due to incorrect window logic.

### Lessons
- The power of **detailed validation** during modeling.
- How tiny relationship mismatches can derail DAX logic.
- Importance of precise calendar + date logic in retention analytics.

---

## How to Use This Project

1. **Download the `.pbix` file** from this repo.
2. Open it in **Power BI Desktop** (free).
3. Explore model, DAX, report visuals, and interactions.
4. Optional: replace the data source with your own for hands-on retention analysis.

---

## Tools Used

- Power BI Desktop
- DAX
- Star Schema Data Modeling
- Git/GitHub for version control

---

## Acknowledgments

This project was part of an application process with **P3 Adaptive**, who provided a thoughtful structure and collaborative feedback, treating this like a real consulting scenario.

## Reuse disclaimer
I do not authorize anyone to reuse this project or its contents for job applications or interview submissions, doing so constitutes unethical misrepresentation.


