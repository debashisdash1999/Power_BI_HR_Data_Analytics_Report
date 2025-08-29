# Power BI Project: HR Data Analytics Report

## Project Overview
This project demonstrates HR data analysis and reporting using **Power BI**.  
The dataset (`HR Data.xlsx`) contains the following columns:
- Name  
- Emp ID  
- Gender  
- Educational Qualification  
- Date of Joining  
- Job Title  
- Salary  
- Age  
- Leave Balance  

The goal is to build an **interactive HR Analytics Report** that helps track headcount, gender distribution, salary insights, employee growth, and leave balance patterns.

---

## Data Preparation
Steps performed in **Power Query**:
1. Imported the Excel sheet into Power BI.  
2. Named the table as **`staff`**.  
3. Enabled **"Use First Row as Header"**.  
4. Converted data types:  
   - `Date of Join` → Date  
   - `Salary` & `Age` → Whole Number  

---

## Analysis & Visuals

### 1. How many people are in each job?
- **Measure:**  
  ```DAX
  Headcount = COUNTROWS(staff)
  
  ```
Visual: Clustered Bar Chart
Y-axis = Job Title
X-axis = Headcount

## 2. Gender Breakdown of the Staff  

**Visuals:**  
- **Pie Chart** → Legend = Gender, Values = Headcount  
- **Slicer** → Job Title (to filter)  

---

## 3. Age Spread of the Staff  

**Steps:**  
- Grouped Age into 5-year bins:  
  - Right click *Age* → Group → Bin size = 5  
- Created column: **Age (bins)**  

**Visual:** Column Chart  
- **X-axis:** Age (bins)  
- **Y-axis:** Headcount  
- **Small Multiples:** Gender  

Adjusted formatting so only 2 small multiple charts (Male & Female) are displayed.  

---

## 4. Which Jobs Pay More?  

**Measures:**  
```DAX
Avg. Salary = AVERAGE(staff[Salary])
Min. Salary = MIN(staff[Salary])
Max. Salary = MAX(staff[Salary])
```

**Visual:** Table  
- **Columns:** Job Title, Avg Salary, Min Salary, Max Salary, Headcount  

---

## 5. Top Earners in Each Job  

- Copied the **headcount visual** as base  
- Added a **Table** with columns: Name, Emp ID, Gender, Salary  
- Applied **Top N Filter** → Top 3 by Salary  
- Removed Totals  
- Created another table for **Bottom 3 earners**  

---

## 6. Qualification vs Salary  

**Steps:**  
- Converted *Educational Qualification* into numeric codes for ordering:  
  - High School = 1  
  - Diploma = 2  
  - Bachelor’s = 3  
  - Master’s = 4  
- Created a new **Qualification table** in Power Query  
- Set **1-to-many relationship** between Qualification → Staff  

**Visual:** Scatter Chart  
- **X-axis:** Salary (Do not summarize)  
- **Y-axis:** Qualification ID  
- **Legend:** Qualification  
- Adjusted axis range (0–5) to fix alignment of points  

---

## 7. Staff Growth Trend Over Time  

**Measure (Running Total Headcount):**  
```DAX
Cumulative Headcount =
    VAR currentDate = LASTDATE(staff[Date of Join])
    RETURN
        CALCULATE(
            [Headcount],
            ALL(staff[Date of Join]),
            staff[Date of Join] <= currentDate
        )
```
**Meaning:**  
- For each date on the X-axis, counts all employees who joined up to that date.  
- Shows **cumulative growth trend** rather than hires per month.  
{1. VAR currentDate = LASTDATE(Staff[date of join])
•	For each point on the X-axis (each date shown), this grabs the current date in context.
•	Example: if the chart is showing Jan 2020 → currentDate = Jan 31, 2020.
2. CALCULATE([Headcount], … )
•	CALCULATE changes the filter context to compute [Headcount] in a custom way.
•	[Headcount] is probably a measure like COUNTROWS(Staff) or DISTINCTCOUNT(Staff[EmployeeID]).
3. ALL(Staff[date of join])
•	Normally, Power BI applies the filter for the current date only (e.g., only Jan 31, 2020).
•	ALL() removes that filter → so we can look across all dates in the column.
4. Staff[date of join] <= currentDate
•	This reapplies a filter that says:
“Only include rows where the join date is less than or equal to the current date.”
So, if currentDate = Jan 31, 2020 → it will count everyone who joined up to that date.
 Final Meaning
This measure calculates the running total of employees (headcount) from the start of the company up to the current date on the X-axis.
This line chart is not showing just “hires that month” → it’s showing “total employees in the company over time” (growth trend). }


**Visual:** Line Chart  
- **X-axis:** Date of Join  
- **Y-axis:** Cumulative Headcount  

---

## 8. Employee Filter by Starting Letter  

**Steps in Power Query:**  
- Extracted **First Character** from *Name* → Created new column **First Letter**  

**Visuals:**  
- **Slicer:** First Letter  
- **Table:** Emp ID, Name, Job Title, Salary  
- **Card:** Headcount  

---

## 9. Leave Balance Analysis  

**Measures:**  
```DAX
Avg. Leave Balance = AVERAGE(staff[Leave Balance])

LBL over 20 days =
    CALCULATE(
        [Headcount],
        staff[Leave Balance] > 20
    )
```
**Explanation:**  
- **Avg. Leave Balance** → Average across all employees  
- **LBL over 20 days** → Number of employees with more than 20 days leave balance
  {Breakdown:
•	[HEADCOUNT] is a measure = COUNTROWS(Staff).
•	CALCULATE() changes the filter context.
•	Staff[leave balance] > 20 → applies a filter where only staff with leave balance more than 20 are considered.}

 ## Visuals
- **Stacked Bar Chart** → Job Title vs Avg Leave Balance  
- **Tooltip** → Shows Leave Balance (LBL) over 20 days  
- **Gender Pie Chart (Copied)** → Analyzes leave balance by Gender  

---

## Final Deliverables
An **interactive HR Analytics Report** in Power BI with the following insights:

- Job distribution and headcount  
- Gender diversity  
- Age demographics  
- Salary analysis (avg, min, max, top/bottom earners)  
- Education vs pay insights  
- Staff growth over time  
- Employee lookup by starting letter  
- Leave balance trends  

---

## Tools Used
- **Microsoft Power BI**  
- **DAX (Data Analysis Expressions)**  
- **Power Query** (ETL & Data Cleaning)  

---

## Key Outcomes
This report empowers HR teams to:
- Track workforce composition and diversity  
- Identify salary trends across roles and qualifications  
- Monitor employee growth patterns  
- Optimize leave management policies  
