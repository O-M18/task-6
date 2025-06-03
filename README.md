# 🧾 Sales Data Analysis with Microsoft SQL Server

This project performs a comprehensive analysis of an online retail sales dataset using T-SQL in Microsoft SQL Server Management Studio (SSMS). The goal is to uncover business insights across time (year, month, weekday, and time of day) and customer behavior.

---

## 📁 Dataset Overview

- **Table Name:** `SALES`
- **Columns:**
  - `Invoice` – Invoice number
  - `StockCode` – Product code
  - `Description` – Product description
  - `Quantity` – Number of items purchased
  - `InvoiceDate` – Date and time of the transaction
  - `Price` – Price per unit
  - `Customer_ID` – Unique customer identifier
  - `Country` – Customer’s country

---

## 🧠 Objectives & Insights

- Extract **month**, **year**, **weekday**, and **hour** from `InvoiceDate`
- Handle **negative quantities**
- Calculate **revenue** per row
- Analyze:
  - Monthly and yearly revenue trends
  - Top revenue months
  - High-value customers and countries
  - Weekday-wise and shift-wise sales distribution
  - Best-selling products

---

## 🛠 Key Features Implemented

| Feature                          | Description |
|----------------------------------|-------------|
| ✅ Year & Month Extraction        | Using `YEAR()` and `MONTH()` |
| ✅ Revenue Calculation            | `Quantity * Price` |
| ✅ Negative Quantity Handling     | Converted to positive using `CASE` |
| ✅ Views Created                  | For modular queries (`NEW_SALES`, `NEW_VIEW`) |
| ✅ Revenue by Month & Year       | Grouped and sorted using `GROUP BY` and `ORDER BY` |
| ✅ Top Customers & Countries      | Ranked by revenue |
| ✅ Weekday-wise Sales             | Using `DATENAME(WEEKDAY, InvoiceDate)` |
| ✅ Shift-wise Sales Analysis      | Morning, Afternoon, Evening segmentation |
| ✅ Top-Selling Products           | By quantity and price |

---

## Sample query

``` sql
----- CONVERTING NEAGTIVE QUANTITY TO POSITIVE-----
CREATE VIEW NEW_VIEW AS
SELECT *, 
CASE 
WHEN Quantity<0 THEN Quantity*(-1)
ELSE Quantity 
END AS Quantities
FROM NEW_SALES;
```
```sql
SELECT  * FROM NEW_VIEW;
------- TOTAL REVEUNE BY MONTH------
 SELECT MONTHS,ROUND(SUM(REVENUE),2) TOTAL_REVENUE
 FROM (SELECT *, (Quantities* Price) AS REVENUE FROM NEW_VIEW) AS S
 GROUP BY MONTHS
 ORDER BY 1;
```
```sql
 ------ SHIFT WISE SALES------
 WITH HOURLY_SALE AS(
	 SELECT *, 
		CASE 
		  WHEN DATEPART(HOUR,INVOICEDATE)>12 THEN 'MORNING'
		  WHEN DATEPART(HOUR,INVOICEDATE) BETWEEN 12 AND 17 THEN 'AFTERNOON'
		  ELSE 'EVENING'
		  END AS SHIFTS
	FROM NEW_VIEW)
SELECT 
SHIFTS,
SUM(Quantities) AS TOTAL_SALE
FROM HOURLY_SALE
GROUP BY SHIFTS
ORDER BY 2 DESC;
```

---

## 📊  Insights

- Most revenue is generated in **September to November**
- **UK** and a few **European countries** are the top customers by revenue
- Sales peak on **Monday and Thursdays**
- **Morning** is the most profitable time slot
- Few customers contribute to a large portion of total sales (Pareto Principle)

---

## 📂 How to Use

1. Import your dataset into SQL Server as a table named `SALES`.
2. Open SSMS and run the provided SQL scripts step by step.
3. Use the views and queries to create visual dashboards (optional in Power BI/Tableau).

---

## 📝 Recommended Improvements

- Add a **date range filter** for time-based analysis
- Include **distinct invoice count** to measure volume more accurately
- Visualize insights in BI tools like **Power BI** or **Tableau**

---

## 📌 Dependencies

- Microsoft SQL Server 2017+  
- SQL Server Management Studio (SSMS)

---


