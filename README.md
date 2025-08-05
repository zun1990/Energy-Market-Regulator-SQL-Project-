#                                                                                                   Energy Market Regulator

**Project Title**: Energy Market Regulator

**Database**: `americanenergymarket`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, analyze data. The project involves performing exploratory data analysis (EDA), and answering specific problem related questions through SQL queries.This prjocet used as a capston project at SPRINGBOARD Certification on Data Analyst Degree.

### Objectives
* Energy Stability over the year  and common Market Outage type.**

* Energy Losses and Market Reliability among all the participants.**

**Data Analysis & Findings**

Analysis of hypothetical outage data from several producers to determine which companies have the most outages, the most average MW of lost energy due to those outages, and longest average outage duration.
The following SQL queries were developed to answer problem and finding related questions from the database:

## PART 1 What are the most common outage types?
```sql
/*Question 1.1: In the AEMR dataset, write a SQL statement that COUNTS the number of valid (i.e. Status = Approved) outage events for 2016.
 This should be grouped by and ordered by the outage reason. */
 
SELECT COUNT(*) AS Total_Number_Outage_Events,
Status,
Reason
FROM aemr
WHERE Status = 'Approved' AND YEAR(Start_Time) = 2016
GROUP BY  Reason
ORDER BY Reason DESC;

/*Question 1.2: Which outage type occurred most frequently in 2016? */ 

SELECT COUNT(*) AS No_Outage_occured,
Reason,
YEAR(Start_Time) AS Year
FROM aemr
WHERE YEAR(Start_Time) = 2016
GROUP BY Reason
ORDER BY No_Outage_occured DESC;

/*Question 1.3: Write a SQL statement to COUNT the number of valid (i.e. Status = Approved) outage events sorted by their reason 
(i.e. Forced, Consequential, Scheduled, Opportunistic)* for 2017. */

SELECT COUNT(*) AS no_valid_outrage,
Status,
Reason,
YEAR(Start_Time) AS Year
FROM aemr
WHERE Status = 'Approved' AND YEAR(Start_Time) = 2017
GROUP BY Reason , Status
ORDER BY Reason;

/*Which outage type occurred the least in 2017? */
SELECT COUNT(*) AS No_Outage_Occured,
Reason
FROM aemr
WHERE YEAR(Start_Time) = 2017
GROUP BY Reason 
ORDER BY No_Outage_Occured;
/*Question 1.5: Write a SQL statement that calculates the average duration in days rounded to 2 decimal places for each approved outage type
 across both 2016 and 2017. Don't forget to Order this by Reason and Year. */
SELECT Status,
Reason,
COUNT(*) AS Total_Number_Outage_Events ,
ROUND(AVG(TIMESTAMPDIFF(Hour,Start_time,End_time)/24),2) AS Average_Outage_Duration_Time_inDays,
Year(Start_Time) AS Year
FROM aemr
WHERE Status = 'Approved' AND Year(Start_time) BETWEEN 2016 AND 2017
GROUP BY Reason, Year
ORDER BY Reason,Year;
```

## PART 2 How frequently do outages occur?
```sql
/*Question 2.1: Write a SQL statement showing the monthly COUNT of all approved outage types (Forced, Consequential, Scheduled, Opportunistic)
 that occurred for 2016. Order by Reason and Month. */
 SELECT Status,
 Reason,
 COUNT(*) AS Total_number_Outage_Events,
 MONTHNAME(Start_Time) AS Month
 FROM aemr
 WHERE Status = 'Approved' AND YEAR(Start_Time) = 2016
 GROUP BY Reason,Month
 ORDER BY Reason, Month;
 
 /*Question 2.2: Write a SQL Statement showing the monthly COUNT of all approved outage types (Forced, Consequential, Scheduled, Opportunistic)
 that occurred during 2017.Order by Reason and Month. */
 
SELECT Status,
 Reason,
 COUNT(*) AS Total_number_Outage_Events,
 MONTHNAME(Start_Time) AS Month
 FROM aemr
 WHERE Status = 'Approved' AND YEAR(Start_Time) = 2017
 GROUP BY Reason,Month
 ORDER BY Reason, Month;
 
 /*Write a SQL statement showing the total number of all approved outage types (Forced, Consequential, Scheduled, Opportunistic)
 that occurred for both 2016 and 2017 per month (i.e. 1 – 12). Don't forget to Order this by by Month and Year. */
SELECT Reason,
 COUNT(*) AS Total_number_Outage_Events,
 MONTHNAME(Start_Time) AS Month,
 YEAR(Start_Time) AS Year
 FROM aemr
 WHERE Status = 'Approved' AND YEAR(Start_Time) BETWEEN 2016 AND 2017
 GROUP BY Reason,Month
 ORDER BY Month, Year;
```

## PART 3 Are there any energy providers that have more outages than their peers that may be indicative of being unreliable?
```sql
/*Question 3.1: Write a SQL statement showing the count of all approved outage types (Forced, Consequential, Scheduled, Opportunistic)
 for all participant codes for 2016 and 2017. Order by Year and Participant_Code. */
 SELECT COUNT(*) AS Total_number_Outage_Events,
 Participant_Code,
 Status,
 YEAR(Start_Time) AS Year
 FROM aemr
 WHERE Status = 'Approved'
 GROUP BY Year, Participant_Code,Status
 ORDER BY Year ,Participant_Code;
 
 /*Question 3.2: Write a SQL statement showing the average duration of all approved outage types (Forced, Consequential, Scheduled, Opportunistic)
 for all participant codes from 2016 to 2017.Don't forget to order the average duration in descending order with the DESC keyword. */
 
 SELECT Participant_Code,
 Status,
 YEAR(Start_Time) AS Year,
 ROUND(AVG(TIMESTAMPDIFF(Hour,Start_time,End_time)/24),2) AS Average_Outage_Duration_Time_inDays
 FROM aemr
 WHERE Status = 'Approved'
 GROUP BY Participant_Code,Year,Status
 ORDER BY Average_Outage_Duration_Time_inDays DESC;
```

## Part 4: Energy Losses and Market Reliability
```sql
 /*Question 4.1: Write a SQL Statement to COUNT the total number of approved forced outage events for 2016 and 2017. Order by Reason and Year. */
 
 SELECT COUNT(*) AS Total_number_Outage_Events,
 Reason,
 YEAR(Start_Time) AS Year
 FROM aemr
 WHERE Status = 'Approved' AND Reason = 'Forced'
 GROUP BY Reason,Year
 ORDER BY Reason , Year;
 
 /*Question 4.2: Building upon the query you completed in the previous question,
 calculate the proportion of outages that were forced and Consequential for both 2016 and 2017. Order from 2016 to 2017. */
SELECT COUNT(*) AS Total_number_Outage_Events,
SUM( CASE WHEN Reason = 'Forced' THEN 1 Else 0 END) AS Total_Number_Forced_Outage_Events,
SUM(CASE WHEN Reason = 'Consequential' THEN 1 ELSE 0 END) AS Total_Number_Consequential_Outage_Events,
ROUND((SUM( CASE WHEN Reason = 'Forced' THEN 1 Else 0 END) /COUNT(*)*100),2) AS Forced_Outage_Percentage,
ROUND((SUM(CASE WHEN Reason = 'Consequential' THEN 1 ELSE 0 END)/COUNT(*)*100),2) AS Consequential_Outage_perventage ,
 YEAR(Start_Time) AS Year
 FROM aemr
 WHERE Status = 'Approved' 
 GROUP BY Year
 ORDER BY Year;
```
