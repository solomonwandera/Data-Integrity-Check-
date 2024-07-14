# Data Integrity Check Between HR and Internal ERP Systems

### Project Overview
This project aims to leverage SQL to conduct comprehensive data integrity checks between two databases HR system and an internal ERP system. 
By identifying and rectifying discrepancies, we aim to enhance data accuracy, consistency, and reliability, ultimately improving decision-making processes within the organization

### Project Objective
- Identify and rectify duplicate user accounts within the ERP system.
- Determine the number of active users in the ERP system who are not present as active employees in the HR system.
- Identify inactive high-level users (directors, chiefs, VPs, presidents) who haven't logged in since a specified date.

### Data Sources
Data Org ERP.csv: Contains user account data from the internal ERP system.[Downlaod]()

Data Org HRMS.csv: Contains active employee data from the HR management information system.[Downlaod]()

###SQL Quesries

```sql
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
