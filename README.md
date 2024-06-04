# Waterloo University Enrollment Dashboard

## Project Overview

### Aim
The objective of this project is to create a comprehensive dashboard for Waterloo University's student enrollment data. The dashboard aims to provide valuable insights into enrollment trends, program distributions, faculty, campus, and other key metrics from the available dataset.

### Dataset Description
The dataset contains detailed enrollment information for Waterloo University using 61953 data entries, covering various aspects such as fiscal years, term types, career levels, program levels, study years, faculties, program groupings, coop statuses, work terms, attendance types, visa statuses, and student headcounts. 

## Data Cleaing and Preparation
### Data Quality Checks

**Null Values**: Verified the presence of NULL values in each column.
Select count(*) from enrollment where `ï»¿Fiscal Year` IS NULL;
Select count(*) from enrollment where `Term Type` IS NULL;
Select count(*) from enrollment where career IS NULL;
Select count(*) from enrollment where `Program Level` IS NULL;
Select count(*) from enrollment where `Study Year` IS NULL;
Select count(*) from enrollment where `Faculty (group)` IS NULL;
Select count(*) from enrollment where `Program Grouping` IS NULL;
Select count(*) from enrollment where `Coop Regular` IS NULL;
Select count(*) from enrollment where `Work Term` IS NULL;
Select count(*) from enrollment where Attendance IS NULL;
Select count(*) from enrollment where `Visa Status` IS NULL;
Select count(*) from enrollment where `Student Headcount` IS NULL;
Result: No NULL values found in the dataset.

### Data Validation

**Distinct Values**: Verified distinct values for key columns to ensure data consistency.
Select distinct `ï»¿Fiscal Year` from enrollment;
Select distinct `Term Type` from enrollment;
Select distinct `Career` from enrollment;
Select distinct `Program Level` from enrollment;
Select distinct `Study Year` from enrollment; -- N D M not suitable year--
Select distinct Campus from enrollment;
Select distinct `Faculty (group)` from enrollment;
Select distinct `Program Grouping` from enrollment;
Select distinct `Coop Regular` from enrollment;
Select distinct `Work Term` from enrollment;
Select distinct `Attendance` from enrollment;
Select distinct `Visa Status` from enrollment;
Select distinct `Student Headcounts` from enrollment;
	
Issue Identified: The Study Year column contains invalid values such as N, D, and M.
SELECT COUNT(*) FROM enrollment WHERE `Study Year` = "M";
Result: N = 1598, D = 4085, M = 6760

### Duplicate Row Check
**Duplicates**: Verified if any duplicate rows exist in the dataset.
SELECT `enrollment`.`ï»¿Fiscal Year`, `enrollment`.`Term Type`, `enrollment`.`Career`, `enrollment`.`Program Level`, `enrollment`.`Study Year`, `enrollment`.`Campus`,
       `enrollment`.`Faculty (group)`, `enrollment`.`Program Grouping`, `enrollment`.`Coop Regular`, `enrollment`.`Work Term`, `enrollment`.`Attendance`, `enrollment`.`Visa Status`, `enrollment`.`Student Headcounts`
FROM `waterloo_enrollment`.`enrollment`
GROUP BY `enrollment`.`ï»¿Fiscal Year`, `enrollment`.`Term Type`, `enrollment`.`Career`, `enrollment`.`Program Level`, `enrollment`.`Study Year`, `enrollment`.`Campus`,
         `enrollment`.`Faculty (group)`, `enrollment`.`Program Grouping`, `enrollment`.`Coop Regular`, `enrollment`.`Work Term`, `enrollment`.`Attendance`, `enrollment`.`Visa Status`, `enrollment`.`Student Headcounts`
HAVING COUNT(*) > 1;
Result: No duplicate rows found.

### Data Cleaning Steps

**Rename Columns**: Renamed columns for better readability.
ALTER TABLE enrollment RENAME COLUMN `ï»¿Fiscal Year` TO FiscalYear;
ALTER TABLE enrollment RENAME COLUMN `Faculty (group)` TO Faculty;
ALTER TABLE enrollment RENAME COLUMN `Program Grouping` TO Program;
ALTER TABLE enrollment RENAME COLUMN `Student Headcounts` TO Stu_Headcount;

**Add and Update New Column**: Added a new column FYear to extract the enrollment year from the FiscalYear column.
ALTER TABLE enrollment ADD COLUMN FYear DATE;
ALTER TABLE enrollment MODIFY COLUMN FYear TEXT;
UPDATE enrollment SET FYear = LEFT(FiscalYear, 4);
	
**Drop Column**: Removed the original FiscalYear column.
ALTER TABLE enrollment DROP COLUMN FiscalYear;

## Exploratory Data Analysis (EDA)

### Key Metrics and Insights
•	Total Student Headcount:
SELECT SUM(Stu_Headcount) FROM enrollment;
•	Distinct Programs and Faculties:
SELECT COUNT(DISTINCT Program) FROM enrollment;
SELECT COUNT(DISTINCT Faculty) FROM enrollment;
•	Enrollment by Term Type:
SELECT SUM(Stu_Headcount) FROM enrollment WHERE `Term Type` = 'Fall term';
•	Enrollment by Program Level:
SELECT SUM(Stu_Headcount) FROM enrollment WHERE `Program level` = 'Bachelors';
•	Enrollment Trend by Year:
SELECT SUM(Stu_Headcount), FYear FROM enrollment GROUP BY FYear;
•	Canadian Student Enrollment Trend:
SELECT SUM(Stu_Headcount), FYear FROM enrollment WHERE `Visa Status` = 'Canadian' GROUP BY FYear ORDER BY SUM(Stu_Headcount);
•	Enrollment by Faculty:
SELECT SUM(Stu_Headcount), Faculty FROM enrollment WHERE `Coop Regular` = 'Co-op' GROUP BY Faculty ORDER BY SUM(Stu_Headcount) DESC LIMIT 7;
•	Enrollment by Campus and Program Level:
SELECT SUM(Stu_Headcount), Campus FROM enrollment GROUP BY Campus HAVING SUM(Stu_Headcount) > 8000 ORDER BY `program level`;

## Results and Learnings

### Results
•	Data Integrity: Verified no NULL values and duplicate rows in the dataset.
•	Invalid Data Identification and improved readability
•	Key Insights: Extracted valuable insights on student headcount, program distribution, enrollment trends, faculty selection, campus preferences, and more as highlighted on Power BI Dashboard. 

### Learnings
•	SQL Proficiency: Enhanced skills in data cleaning and EDA using SQL.
•	Data Validation: Improved ability to validate and clean large datasets.
•	Dashboard Creation: Gained experience in preparing data for visualization and interactive dashboard creation to showcase complex information into simple visuals.

## Conclusion
The Waterloo University Enrollment Dashboard project provided a comprehensive analysis of enrollment data, enabling the extraction of key insights and trends. The cleaned and analyzed dataset serves as a valuable resource for understanding enrollment patterns and supporting strategic decision-making processes at Waterloo University. Developed Power BI Student Enrollment Dahsboard comprehends the complex student enrollment information, treedns, and KPIs in a useful business insights.



