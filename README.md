###Power BI Project: HR Data Analytics Report
Overview
This report outlines the process of creating an HR Data Analytics report in Power BI using an Excel sheet named "HR Data" with the following columns:

Name
Emp ID
Gender
Educational Qualification
Date of Joining
Job Title
Salary
Age
Leave Balance

The report includes various analyses to gain insights into the HR data, along with the transformations and visualizations used in Power BI.
---
-Data Transformations

Imported Data:

Loaded the Excel sheet into Power BI and named it ‘Staff’.
Enabled ‘Use first row as headers’ in Power Query.
Changed column types:

Date of Joining: Date
Salary: Number
Age: Number

---
Analyses and Visualizations
##1. How Many People Are in Each Job?

Objective: Count the number of employees per job title.
Measure Created:
daxHeadcount = COUNTROWS(Staff)

Visualization:

Clustered Bar Chart

Y-Axis: Job Title
X-Axis: Headcount





##2. Gender Breakdown of the Staff

Objective: Show the gender distribution across the organization, with the ability to filter by job title.
Visualization:

Pie Chart

Legend: Gender
Values: Headcount


Slicer: Job Title (allows filtering by job title to see gender breakdown per role).



##3. Age Spread of the Staff

Objective: Analyze the age distribution of employees, grouped into 5-year bins.
Transformation:

In Power Query, grouped the Age column:

Right-click Age > Group > Set bin size to 5.
Created a new column: Age (bins).




Visualization:

Column Chart

X-Axis: Age (bins)
Y-Axis: Headcount
Small Multiples: Gender


Formatting:

Observed blank graphs in small multiples; adjusted in Format Visuals > Small Multiples > Set Columns to 1 to display only two graphs (one for each gender).





##4. Which Jobs Pay More?

Objective: Compare average, minimum, and maximum salaries across job titles.
Measures Created:
daxAvg. Salary = AVERAGE(Staff[Salary])
Min. Salary = MIN(Staff[Salary])
Max. Salary = MAX(Staff[Salary])

Visualization:

Table

Columns: Job Title, Avg. Salary, Headcount, Min. Salary, Max. Salary





##5. Top Earners in Each Job

Objective: Identify the top 3 and bottom 3 earners for each job title.
Steps:

Copied the clustered bar chart from Analysis 1 (How many people are in each job?).
Created a Table:

Columns: Name, Emp ID, Gender, Salary
Filter: In the filter pane, applied Top N filter on Emp ID:

N: 3
By Value: Salary
Removed Totals to show only the top 3 earners per job.




Repeated the process for another table to display the bottom 3 earners.


Visualization:

Two tables: one for top 3 earners, one for bottom 3 earners.



##6. Qualification vs. Salary

Objective: Analyze the relationship between educational qualifications and salary.
Transformation:

In Power Query:

Selected Educational Qualification column > Add as New Query > Removed duplicates.
Converted to a table (To Table) and sorted in ascending order.
Added a Conditional Column named Qualification ID:

If begins with "H" (High School): 1
If begins with "D" (Diploma): 2
If begins with "B" (Bachelor’s): 3
Else (Master’s): 4


Renamed the original column to Qualification.
Changed Qualification ID to Whole Number.
Loaded the new table into Power BI and set a 1-to-Many relationship between the new table’s Qualification column and the Staff table’s Educational Qualification column.




Visualization:

Scatter Chart

X-Axis: Salary (set to Don’t Summarize to show individual salaries as circles).
Y-Axis: Qualification ID (from the new table).
Legend: Qualification (from the new table).
Formatting:

Y-Axis Range: Set minimum to 0 and maximum to 5 to prevent circles from being cut off.







##7. Staff Growth Trend Over Time

Objective: Show the cumulative growth of employees over time.
Measure Created:
daxCumulative Headcount = 
VAR currentDate = LASTDATE(Staff[Date of Joining])
RETURN
CALCULATE(
    [Headcount],
    ALL(Staff[Date of Joining]),
    Staff[Date of Joining] <= currentDate
)

Explanation:

LASTDATE: Captures the current date in context (e.g., Jan 31, 2020).
CALCULATE: Computes headcount with a modified filter context.
ALL(Staff[Date of Joining]): Removes the default date filter to consider all dates.
Staff[Date of Joining] <= currentDate: Counts employees who joined on or before the current date.
This creates a running total of employees from the company’s start to the current date.




Visualization:

Line Chart

X-Axis: Date of Joining (used raw Date of Joining instead of Date Hierarchy to show individual days).
Y-Axis: Cumulative Headcount


Displays total employees over time, showing company growth.



##8. Employee Filter by Starting Letter

Objective: Filter employees based on the first letter of their name.
Transformation:

In Power Query:

Selected Name column > Add Column > Extract > First Letters (count = 1).
Created a new column: First Characters.
Closed and applied changes.




Visualization:

Slicer: Added First Characters to filter employees by the starting letter of their name.
Table: Included columns Emp ID, Name, Job Title, Salary.
Card Visual: Displayed Headcount to show the count of employees for the selected starting letter.



##9. Leave Balance Analysis

Objective: Analyze the average leave balance and count employees with leave balances exceeding 20 days (4 weeks).
Measures Created:
daxAvg. Leave Balance = AVERAGE(Staff[Leave Balance])
LBL over 20 days = CALCULATE([Headcount], Staff[Leave Balance] > 20)

Explanation:

Avg. Leave Balance: Calculates the average leave balance across employees.
LBL over 20 days: Counts employees with leave balances greater than 20 days using the Headcount measure.




Visualization:

Stacked Bar Chart:

Y-Axis: Job Title
X-Axis: Avg. Leave Balance
Data Labels: Enabled to show values.
Tooltip: Added LBL over 20 days to show the count of employees with leave balances > 20 days.


Pie Chart: Copied the gender pie chart from Analysis 2 to show leave balance distribution by gender.


---

Summary
This Power BI project provides a comprehensive analysis of HR data through various visualizations, including bar charts, pie charts, scatter charts, line charts, and tables. Transformations in Power Query and DAX measures enhance the ability to derive meaningful insights, such as employee headcounts, salary trends, age distributions, and leave balance analysis. The use of slicers and filters allows for interactive exploration of the data, making it a robust tool for HR analytics.
