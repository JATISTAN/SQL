# **SQL**  
This section highlights SQL-based projects, focusing on data cleaning, exploratory data analysis (EDA), and dashboard creation in Tableau.

---

## **Projects Overview**  
This repository includes the following SQL-based projects:

1. [Data Cleaning - Layoffs Data - Project Overview](#data-cleaning---layoffs-data---project-overview)
2. [Exploratory Data Analysis (EDA) - Layoffs Data](#exploratory-data-analysis-eda---project-overview)  
3. [Tableau Dashboard - Layoffs Analysis](#tableau-dashboard---project-overview)

---
---
## **Project 1:**
## **Data Cleaning - Layoffs Data - Project Overview**  
The **Data Cleaning** project demonstrates proficiency in SQL for cleaning a raw layoffs dataset. It involved tasks such as removing duplicates, standardizing data, handling NULL values, and removing unnecessary columns.

### **Key Steps** 
---
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

```
---
#### **2. Create Staging Tables**
- Created new tables for cleaning and transformation:

```sql
CREATE TABLE layoffs_staging2 (
  company TEXT,
  location TEXT,
  industry TEXT,
  total_laid_off INT DEFAULT NULL,
  percentage_laid_off TEXT,
  date TEXT,
  stage TEXT,
  country TEXT,
  funds_raised_millions INT DEFAULT NULL,
  row_num INT
);

```
---

#### **3. Standardizing Data**
- Removed leading/trailing spaces, standardized country names, and formatted dates.

```sql
UPDATE layoffs_staging2 SET company = TRIM(company);
UPDATE layoffs_staging2 SET country = TRIM(TRAILING '.' FROM country);
UPDATE layoffs_staging2 SET date = STR_TO_DATE(date, '%m/%d/%Y');
ALTER TABLE layoffs_staging2 MODIFY COLUMN date DATE;

```
---

#### **4. Handling NULL and Blank Values**
- Checked for blank and NULL values in key columns and updated them accordingly.

```sql
UPDATE layoffs_staging2 SET industry = NULL WHERE industry = '';
UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2 ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL AND t2.industry IS NOT NULL;

```
---
---
## **Project 2:**
## **Exploratory Data Analysis (EDA) - Layoffs Data - Project Overview**
- In the EDA project, SQL queries were used to uncover patterns and trends in layoffs data. This includes grouping data by company, industry, country, and other dimensions.

### **Key Steps**
1. Descriptive Statistics
- Found the maximum and minimum values for layoffs.

```sql
SELECT MAX(total_laid_off), MAX(percentage_laid_off) FROM layoffs_staging2;

```
---
#### **2. Grouping Data by Company and Industry**
- Grouped total layoffs by company and industry.

```sql
SELECT company, SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY company
ORDER BY 2 DESC;
```
---
#### **3. Analyzing Layoffs by Year**
- Analyzed layoffs by year.
```sql
SELECT YEAR(date), SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY YEAR(date)
ORDER BY 1 DESC;

```
---
#### **4. Exploring Monthly Layoffs**
- Investigated layoffs by month.
```sql
SELECT SUBSTRING(date, 1, 7) AS `MONTH`, SUM(total_laid_off)
FROM layoffs_staging2
WHERE SUBSTRING(date, 1, 7) IS NOT NULL
GROUP BY `MONTH`
ORDER BY 1 ASC;

```
---
#### **5. Rolling Total of Layoffs**
- Created a rolling total to track cumulative layoffs over time.
```sql
WITH Rolling_Total AS
(
  SELECT SUBSTRING(date, 1, 7) AS `MONTH`, SUM(total_laid_off) AS total_off
  FROM layoffs_staging2
  WHERE SUBSTRING(date, 1, 7) IS NOT NULL
  GROUP BY `MONTH`
  ORDER BY 1 ASC
)
SELECT `MONTH`, total_off,
       SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total
FROM Rolling_Total;

```
---
---
## **Project 3:**
## **Project 3: Tableau Dashboard - Layoffs Analysis**
- **Project Overview**
- The Tableau Dashboard project involves visualizing the layoffs data using Tableau to create a dynamic and interactive dashboard. This dashboard highlights key insights derived from the cleaned layoffs dataset and allows the user to explore trends in layoffs by industry, location, funding stage, and more.

### **Key Steps**
1. Bar Graph - Layoffs by Industry % Wise
- Visualized layoffs as a percentage of total layoffs by industry, providing an understanding of which industries were hit hardest.

2. Map - Layoffs by Location
- Displayed layoffs by location using a map. Larger bubbles represented higher layoffs in specific regions.

3. Funding Stage Analysis Table
- Created a table to show how much funding each company raised at different stages in their lifecycle. This helped visualize the financial background of companies undergoing layoffs.

4. Line Graph - Funds Raised Month Wise
- Visualized the funds raised in millions month-wise using a line graph to track trends over time.

### **Dashboard Image**

You can view the interactive version of the dashboard here:
Tableau Public Dashboard - Layoffs Analysis

### **Conclusion**
- This project combines advanced SQL techniques for data cleaning and exploratory data analysis with Tableau’s powerful visualization capabilities. The portfolio demonstrates the ability to handle complex data manipulation tasks, uncover actionable insights, and present those insights dynamically through a fully functional Tableau dashboard.

### **Conclusion of SQL Portfolio**
- This SQL Portfolio includes comprehensive projects showcasing proficiency in Data Cleaning, Exploratory Data Analysis (EDA), and Data Visualization using Tableau. It demonstrates a strong command of SQL for data manipulation and the ability to create dynamic visualizations for effective communication of insights.
---
---
