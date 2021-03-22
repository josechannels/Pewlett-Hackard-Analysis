# Pewlett-Hackard-Analysis

## Project Overview
The overall purpose of this analysis was to determine whether the Pewlett-Hackard company is prepared for the predicted wave of retiring employees.  To address this concern, the analysis focused on answering two questions:
- How many employees are eligible for retirement per title?
- How many retirement eligible employees are qualified to participate in a mentorship program aimed at training the next generation of employees to fill the predicted vacancies?

## Resources
Data Sources: departments.csv, dept_emp.csv, dept_manager.csv, emp_info.csv, employees.csv, salaries.csv, titles.csv  
Software: PostgreSQL11, Visual Studio Code 1.53.2, QuickDBD (https://www.quickdatabasediagrams.com/)

## Results

The following ERD was used as a map to enable SQL queries to answer the two questions:

![](Analysis/EmployeeDB.png) 

The following query was used to determine the number of employees by title eligible for retirement:

	SELECT e.emp_no,
		e.first_name,
		e.last_name,
		ti.title,
		ti.from_date,
		ti.to_date
	INTO retirement_titles
	FROM employees as e
		INNER JOIN titles AS ti
			ON (e.emp_no = ti.emp_no)
	WHERE birth_date BETWEEN '1952-01-01' AND '1955-12-31'
	ORDER BY e.emp_no;

	-- Use Dictinct with Orderby to remove duplicate rows
	SELECT DISTINCT ON (rt.emp_no) rt.emp_no,
	rt.first_name,
	rt.last_name,
	rt.title
	INTO unique_titles
	FROM retirement_titles as rt
	ORDER BY rt.emp_no, to_date DESC;

	--count retiring employees by title
	SELECT COUNT (ut.title), ut.title
	INTO retiring_titles
	FROM unique_titles as ut
	GROUP BY ut.title
	ORDER BY COUNT(ut.title) DESC;``

The results of this query are shown below (the conversion of the excel file to a markdown table was performed using https://www.convertcsv.com/csv-to-markdown.htm):

|count|title             |
|-----|------------------|
|29414|Senior Engineer   |
|28254|Senior Staff      |
|14222|Engineer          |
|12243|Staff             |
|4502 |Technique Leader  |
|1761 |Assistant Engineer|
|2    |Manager           |


To determine the number of employees eligible for the mentorship program the following query was used:

	--mentorship eligibility table
	SELECT DISTINCT ON (e.emp_no) 
	e.emp_no,
	e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	ti.title
	INTO mentorship_eligibility
	FROM employees as e
	INNER JOIN dept_emp as de
	ON (e.emp_no = de.emp_no)
	INNER JOIN titles as ti
	ON (e.emp_no = ti.emp_no)
	WHERE de.to_date = ('9999-01-01')
	AND (birth_date BETWEEN '1965-01-01' AND '1965-12-31')
	ORDER BY e.emp_no ASC;

Together, the results of the two queries show that:
- A total of 90,398 employees will be eligible for retirement.
- The retirement eligible employees are distributed over seven titles:
	- Senior Engineer
	- Senior Staff
	- Engineer
	- Staff
	- Technique leader
	- Assistant Engineer
	- Manager
- "Senior Engineer" and "Senior Staff" retirement eligible employees comprise over 63% of all employees eligible for retirement.
- There a total of 1550 employees that can serve as mentors to train employees to fill the expected vacancies.

## Summary

A total of 90,398 roles will need to be filled across the company as the "silver tsunami" reaches the shore.  These roles are distributed over 7 titles, but the titles of Senior engineer and Senior staff are the most afected (63% of those retiring hold one of these titles).

The observation that only 1,550 employees are eligible to serve as mentors to those qualified to feel the vacancies is worrisome because the ratio of mentors to positions is 58.  It is highly unlikely that each mentor can adequately train 58 trainees.

To better understand where the number of mentors will fall short, a new query was generated to inform on what are the titles of those eligible to mentor other employees.

	--count mentorship eligible employees by title
	SELECT COUNT (me.title), me.title
	INTO mentorship_titles
	FROM mentorship_eligibility as me
	GROUP BY me.title
	ORDER BY COUNT(me.title) DESC;

The results of this query are shown below (the conversion of the excel file to a markdown table was performed using https://www.convertcsv.com/csv-to-markdown.htm):

|count|title             |
|-----|------------------|
|446  |Senior Staff      |
|422  |Engineer          |
|278  |Staff             |
|266  |Senior Engineer   |
|77   |Technique Leader  |
|60   |Assistant Engineer|

Using the results of this query and the results of the query to determine the number of retirement eligible employees we can calculate the ratio of predicted vacancies by title to the number of mentors available that hold the same title:

