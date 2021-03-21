# Pewlett-Hackard-Analysis

## Project Overview
The overall purpose of this analysis was to determine whether the Pewlett-Hackard company is prepared for the predicted wave of retiring employees.  To address this concern, the analysis focused on answering two questions:
- How many employees are retiring per title?
- How many employees are eligible to participate in a mentorship program aimed at promoting from within the company to fill the predicted vacancies?

## Resources
Data Sources: departments.csv, dept_emp.csv, dept_manager.csv, emp_info.csv, employees.csv, salaries.csv, titles.csv
Software: PostgreSQL11, Visual Studio Code 1.53.2, QuickDBD (https://www.quickdatabasediagrams.com/)

## Results

The results of the analysis are depicted in the following plot and table:

![](Analysis/PyBer_fare_summary.png)

![](Analysis/Final_Table_Analsys.PNG)

The analysis shows that:

- The revenue is greatest in Urban settings, followed by Suburban and Rural (lowest).
- The main contributor to the higher revenue in Urban settings is the significantly larger number of rides (13x number of rides in rural settings and 2.6x number of rides in suburban settings)
- The highest average fare per ride is obtained in rural areas ($34.62), followed by rides in suburband areas ($30.97) and rides in urban areas ($24.53).
- The average fare per driver is highest in rural areas ($55.49), followed by suburban ($39.50) and urban settings ($16.57).
- The revenue is not constant:  The analysis of the revenue from January to end of April, aggregated on a weekly basis shows that there are peaks of revenue.  These peaks do not coincide across different city types.

## Summary

The analysis suggests that:
- Increasing the number of drivers in rural areas may increase total revenue since 
    - in rural areas each driver completed an average of 1.6 rides compared to drivers in urban areas that completed only an average of 0.67 rides 
    - and the average fare per ride is significantly higher in rural than urban areas. 
- Rides in rural areas may cover longer distances since the average fare per ride is higher in these settings.  If that is case, placing vehicles that are energy efficient in these areas may result in an increase in profit.
- it may be worthwhile to increase the number of drivers in urban settings in the month of March - where there are the highest peaks of revenue.
