# 📊 IT Expenditure Analysis: Actual vs. Plan Variance

## Project Overview

This Power BI project provides a detailed, interactive analysis of Information Technology (IT) expenditure, focusing on the variance between **Actual Spend** and **Planned Budget**. The primary goal is to quickly identify organizational and functional areas ("hotspots") responsible for the largest overspend (unfavorable variance) to support proactive cost management and financial alignment.

The dashboard follows a standardized executive reporting structure, moving from high-level Key Performance Indicators (KPIs) to specific dimensional drill-downs (Region, IT Area, Cost Group).

***

## ⚙️ Data Source and ETL Process

### Source File

The project utilizes a single Microsoft Excel workbook (`IT Expenditure dataset.xlsx`) containing two critical sheets:
1.  **`Data`**: Contains historical **Actual**, **Forecast**, and **Plan** data.
2.  **`Future Data`**: Contains the most recent **Actual** spend figures (used to extend the time series).

### ETL (Power Query) Steps

The data transformation was handled entirely within the Power BI Power Query Editor (`M` language) to create a clean, unified model:

1.  **Load Sheets:** Both `Data` and `Future Data` sheets were loaded as separate queries.
2.  **Append:** The two sheets were combined using **Append Queries as New** into a master table named **`IT_Spend_Master`**.
3.  **Clean & Standardize:**
    * **Null Handling:** Null values (created when appending columns that didn't exist in both sheets) were replaced with **0**.
    * **Sign Standardization (Crucial):** All cost columns (`Actual`, `Forecast`, `Plan`) were converted to **Absolute Values** using the `Number.Abs()` function to ensure all expenditure figures are treated as positive numbers, simplifying variance interpretation.
4.  **Final Load:** Only the clean `IT_Spend_Master` table was enabled for load into the final data model.

***

## 📈 Data Model & Key DAX Measures

The model uses the `IT_Spend_Master` table as the fact table.

### Core DAX Measures

The following key measures drive the variance analysis on the dashboard:

| Measure Name | DAX Formula Summary | Purpose |
| :--- | :--- | :--- |
| **Total Actual Spend** | `SUM('IT_Spend_Master'[Actual_Final])` | Total realized cost. |
| **Total Plan Spend** | `SUM('IT_Spend_Master'[Plan_Final])` | Total budgeted cost. |
| **Actual vs. Plan ($)** | `[Total Actual Spend] - [Total Plan Spend]` | Absolute dollar variance. (Positive = Overspend) |
| **Actual vs. Plan (%)** | `DIVIDE([Actual vs. Plan ($)], [Total Plan Spend])` | Percentage variance against budget. |

***

## 🖥️ Dashboard Features and Deliverables

The report is structured in three logical sections: KPI Summary, Analytical Visuals, and Drill-Down Tables.

### 1. Executive KPIs (Top Row)

* **Four Card Visuals** displaying total **Plan**, **Actual**, **Variance ($)**, and **Variance (%)**.
* **Conditional Formatting:** The **Variance (%)** card is set to turn **Red** if variance is greater than 0 (Unfavorable/Overspend) and **Green** if variance is less than 0 (Favorable/Underspend).

### 2. Core Variance Visualizations (Mid-Section)

| Visualization | Type | Fields Analyzed | Key Insight |
| :--- | :--- | :--- | :--- |
| **Variance by Cost Group** | Clustered Column Chart | `Cost Element Group`, `Total Actual Spend`, `Total Plan Spend` | Isolates the highest variance drivers (e.g., confirming **Shared Services** as a primary issue). |
| **Cost Trend Over Time** | Line Chart | `Date`, `Actual`, `Plan`, `Forecast` | Visualizes the time-series performance, showing *when* the spending deviated from the plan. |
| **Cost Distribution** | Tree Map | `IT Sub Area`, `Total Actual Spend` | Shows the concentration of actual spend across different IT functions. |

### 3. Regional Deviation Table (Drill-Down)

* **Fields:** `Region`, `IT Area`, `Actual vs. Plan ($)`, `Actual vs. Plan (%)`.
* **Conditional Formatting:** Uses a **Diverging Background Color Scale** on `Actual vs. Plan ($)`: **Red** for high positive variance (overspend) and **Green** for high negative variance (underspend).
* **Purpose:** Allows users to quickly pinpoint the exact geographic and functional intersection causing the biggest dollar variances after filtering the dashboard (e.g., Europe's Shared Services).

***

## 🚀 How to Use the Report

The dashboard is designed for fast diagnostic analysis:

1.  **Assess Status:** Check the **Variance (%) KPI** for overall budget status (Red/Green alert).
2.  **Identify Driver:** Look at the **Variance by Cost Group** chart to find the key problematic cost category (e.g., Shared Services).
3.  **Drill Down:** Use the slicers or click on the problematic column in the bar chart to filter the entire report.
4.  **Pinpoint Hotspot:** View the **Regional Deviation Table** to see which specific **Region** is driving the variance for the selected cost group.
5.  **Action:** The resulting analysis provides a clear, actionable target for investigation (e.g., the root cause of Shared Services allocations in the Europe region).

---
*(End of README)*
