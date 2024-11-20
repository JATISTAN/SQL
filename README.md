# **SQL**  
This section highlights SQL-based projects, focusing on data cleaning, exploratory data analysis (EDA), and dashboard creation in Tableau.

---

## **Projects Overview**  
This repository includes the following SQL-based projects:

1. [Data Cleaning - Layoffs Data](#data-cleaning---project-overview)  
2. [Exploratory Data Analysis (EDA) - Layoffs Data](#exploratory-data-analysis-eda---project-overview)  
3. [Tableau Dashboard - Layoffs Analysis](#tableau-dashboard---project-overview)

---

## **Data Cleaning - Layoffs Data - Project Overview**  
The **Data Cleaning** project demonstrates proficiency in SQL for cleaning a raw layoffs dataset. It involved tasks such as removing duplicates, standardizing data, handling NULL values, and removing unnecessary columns.

### **Key Steps** 

#### **1. Remove Duplicates**
- Created a staging table `layoffs_staging`.
- Used `ROW_NUMBER()` and `PARTITION BY` to identify duplicate rows.
```sql
WITH duplicate_cte AS
(
  SELECT *, ROW_NUMBER() OVER(PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, stage, country, funds_raised_millions) AS row_num
  FROM layoffs_staging
)
SELECT * 
FROM duplicate_cte
WHERE row_num > 1;





## **Tableau Dashboard - Layoffs Analysis - Project Overview**
The **Tableau Dashboard** project involves visualizing the layoffs data using Tableau to create a dynamic and interactive dashboard.


### **Key Steps**
1. Bar Graph - Layoffs by Industry % Wise
Visualized layoffs as a percentage of total layoffs by industry.
2. Map - Layoffs by Location
Displayed layoffs by location, using bubbles to represent higher layoffs.
3. Funding Stage Analysis Table
Created a table to show how much funding was raised in each stage of the company lifecycle.
4. Line Graph - Funds Raised Month Wise
Visualized the funds raised in millions month-wise using a line graph.
### **Conclusion**
The SQL Portfolio includes data cleaning using advanced SQL techniques, exploratory data analysis (EDA) with SQL queries, and a fully functional Tableau dashboard for visualizing key insights. The portfolio demonstrates a deep understanding of SQL for data manipulation and the ability to present findings through dynamic visualizations.
