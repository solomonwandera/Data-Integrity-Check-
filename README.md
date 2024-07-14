## Eliminating Data Discrepancies Between HR and ERP Systems

###Table of Content

- [Project Overview](#Project-Overview)
- [Project Objective](#Project-Objective)
- [Data Sources](#Data-Sources)
- [SQL Queries](#SQL-Queries)
- [Expected Outcome and Summary](#Expected-Outcome)

### Project Overview
This project aims to leverage SQL to conduct comprehensive data integrity checks between two databases HR system and an internal ERP system. 
By identifying and rectifying discrepancies, we aim to enhance data accuracy, consistency, and reliability, ultimately improving decision-making processes within the organization

### Project Objective
- Identify and rectify duplicate user accounts within the ERP system.
- Determine the number of active users in the ERP system who are not present as active employees in the HR system.
- Identify inactive high-level users (directors, chiefs, VPs, presidents) who haven't logged in since a specified date.

### Data Sources
Data Org ERP.csv: Contains user account data from the internal ERP system.[Downlaod](https://drive.google.com/file/d/1B9LkmFdqHejdjlOu2sDLsPgnpwBCdXD_/view?usp=sharing)

Data Org HRMS.csv: Contains active employee data from the HR management information system.[Downlaod](https://drive.google.com/file/d/1JTL8cOhtpMhjkZ0vyv6EghWp00obBmD5/view?usp=sharing)

### SQL Queries

```sql
1. -- The provided SQL code creates a database named org_data and two tables, hr_system_data and system_data

-- CREATE DATABASE org_data;

-- Use the newly created database
USE org_data;

-- Create the HR system data table
CREATE TABLE hr_system_data (
  email VARCHAR(255),  -- Email address (unique identifier)
  title VARCHAR(255)      -- User's job title
);

-- Create the system data table
CREATE TABLE system_data (
  first_name VARCHAR(255),
  last_name VARCHAR(255),
  email VARCHAR(255),  -- Email address (unique identifier)
  status VARCHAR(255),             -- User's status (e.g., active, inactive)
  last_login_date VARCHAR(255)
);
```

```sql
2.  -- The query below identifies email addresses with multiple active user accounts in the ERP system.

SELECT email, COUNT(*) AS number_of_active_accounts
FROM system_data
WHERE status ="Active"
GROUP BY email
HAVING COUNT(*) > 1;
```
```sql
3. -- This query counts the number of active users in the ERP system who do not have corresponding active records in the HR system.

SELECT system_data.email, Count(system_data.email) as active_users
FROM system_data
LEFT JOIN hr_system_data ON system_data.email = hr_system_data.email
WHERE system_data.status = 'Active'
AND hr_system_data.email IS NULL
GROUP BY system_data.email;
```
```sql
4. /* The code below aims to clean and transform data within the system_data table, focusing on:
        - Converting last_login_date to a proper datetime format.
        - Enriching user data by adding system_title from the hr_system_data table.
        - Identifying active high-level users based on login date and title criteria. */

ALTER TABLE system_data ADD COLUMN login_date_time DATETIME;

-- we use a case statement to  handle potential conversion errors for last_login_date.

UPDATE system_data 
SET login_date_time = 
  CASE 
    WHEN last_login_date REGEXP '^[0-9]{1,2}/[0-9]{1,2}/[0-9]{2} [0-9]{1,2}:[0-9]{2}$' -- this regex ensures last_login_date matches the original pattern date format D/M/YY H:MM.
    THEN STR_TO_DATE(last_login_date, '%m/%d/%y %H:%i') 
    ELSE NULL 
  END;
  
-- We add a new system_title column to the system_data table to store user titles from the hr_system_data table

ALTER TABLE system_data ADD COLUMN system_title VARCHAR(255); -- Fetches the title form HR table
UPDATE system_data
JOIN hr_system_data ON system_data.email = hr_system_data.email
SET system_data.system_title =  hr_system_data.title;

-- This query identifies active users who have not logged in since January 1, 2023. 
SELECT system_data.email,
system_data.login_date_time,
system_data.system_title,
system_data.status
FROM system_data
WHERE system_data.status = "Active"
AND (system_data.login_date_time < "23/01/01 23:59:00" OR system_data.login_date_time IS NULL);
```

### Expected Outcome
By leveraging SQL
- We are able to create a detailed report outlining duplicate user accounts, missing active users and inactive users.
- Improve our data consistency and accuracy between the ERP and HR systems.

### Summary
By conducting these data integrity checks, we can significantly improve the quality and reliability of data within the organization, leading to better decision-making and operational efficiency.



