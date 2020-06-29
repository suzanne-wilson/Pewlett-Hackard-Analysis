# Pewlett-Hackard-Analysis
Module 7

Over the next several years, Pewlett-Hackard will have a cohort of experienced employees reach retirement.  This presents an opportunity for PH to transfer some of the collective wisdom from these soon-to-retire employees to mid-career employees through the impementation of a mentoring program.  The objective of this report is to describe the Silver Tsunami cohort to demonstrate the opportunity, identify employees in this cohort who could serve as mentors by department and job title, and employees who are potential mentees born in 1965.  
---
There were four steps taken to meet these objectives:
1) Create an SQL database containing the PH employee data, which has been stored in Excel files.  
    Schema for all tables are [here](/pewlett-hackard-schema.txt), and the full entity relationship diagram of the new postgreSQL database, PH-EmployeeDB, is below: 

![EmployeeDB.png.png](/EmployeeDB.png.png)
---
2) Create a table containing one row for each retiring employee that contains the employee number, full name, current title and salary, and start date in current role.  
    A csv file of this table, [silver_tsunami_by_title](/Data/silver_tsunami_by_title.csv), is available.  The SQL query can be found below.

                                    -- Identify data on the current job titles and salary of retiring employees  
                                    -- Partition the data to show only most recent title per employee
                                    SELECT emp_no,
                                           last_name,
                                           first_name,
                                           title,
                                           salary
                                    INTO silver_tsunami_by_title
                                    FROM
                                     (SELECT emp_no,
                                             last_name,
                                             first_name,
                                             title,
                                             salary, ROW_NUMBER() OVER
                                     (PARTITION BY (last_name, first_name)
                                     ORDER BY to_date DESC) rn
                                     FROM silver_tsunami
                                     ) tmp WHERE rn = 1
                                    ORDER BY title;
---
3) Create a table summarizing the number of retiring employees by job title.
A csv file of the table ret_by_title in the postgreSQL database PH-EmployeeDB can be found [here](/Data/ret_by_title.csv).
---
4) Create a table with all current employees born in 1965.
A csv file of the table eligible_mentees is available [here](/Data/eligible_mentees.csv).  
---
The Silver Tsunami cohort includes 32,859 current employees in seven job titles that will begin retiring in 2017.  Two job titles account for 80% of the retiring roles:  Senior Engineer (n=13,558) and Senior Staff (n=12,762).  The first cohort of mentees, all born in 1965, include 1,549 employees.  This analysis describes the size of the opportunity represented by the Silver Tsunami and identifies eligible mentees for the first cohort of the mentorship program.  Recommended next steps include identifying departments most impacted by the wave of retirements, and developing criteria for matching mentors and mentees.  


