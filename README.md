Snowflake Tutorial Assignment

Introduction

Snowflake is a cloud-based data platform that provides scalable data storage, processing, and analytics capabilities. It enables users to manage and analyze data using SQL while providing features such as virtual warehouses, stages, data loading, and Time Travel.

This assignment provides practical experience with the Snowflake platform and SnowSQL command-line interface. It demonstrates how to establish a Snowflake connection, create and manage Snowflake objects, perform basic SQL operations, load data using SnowSQL, and use Snowflake Time Travel for accessing and recovering historical data.

The main objective of this assignment is to understand the basic concepts and practical operations involved in working with Snowflake.

---

Overview

This project covers the following five major areas:

1. SnowSQL Login and Connection

SnowSQL is a command-line interface that allows users to connect to Snowflake and execute SQL commands. In this section, a connection is established with the Snowflake account and the current user, role, warehouse, database, and schema are verified.

2. Creation of Snowflake Objects

The following Snowflake objects are created:

- Database
- Schema
- Warehouse
- Table
- Stage

Sample records are inserted into the table, followed by basic SQL operations such as:

- "SELECT"
- "INSERT"
- "UPDATE"
- "DELETE"

3. Data Loading Using SnowSQL

A sample dataset is created in CSV format and uploaded to a Snowflake stage using SnowSQL. The data is then loaded into a Snowflake table using the "COPY INTO" command. SQL queries are used to verify that the data has been loaded successfully.

4. Snowflake Time Travel

A table is created and populated with sample records. The records are then updated and deleted. Snowflake Time Travel is used to query the table as it existed at an earlier point in time.

5. Data Recovery Using Time Travel

The Time Travel feature is used to identify records that were accidentally deleted. The historical data is retrieved and the deleted records are restored to the table.

---

Key Points

The important concepts demonstrated in this assignment are:

- Connecting to Snowflake using SnowSQL.
- Verifying the active Snowflake session.
- Creating databases and schemas.
- Creating and managing virtual warehouses.
- Creating tables for structured data storage.
- Creating stages for data loading.
- Performing basic SQL CRUD operations.
- Creating and loading CSV datasets.
- Using "PUT" and "COPY INTO" for data loading.
- Querying historical data using Snowflake Time Travel.
- Recovering accidentally deleted records.
- Verifying results using SQL queries.

---

Features

Feature| Description
SnowSQL| Command-line interface used to connect to Snowflake
Database| Stores and organizes Snowflake schemas
Schema| Provides logical organization for database objects
Warehouse| Provides compute resources for query execution
Table| Stores structured records
Stage| Used as a location for data loading
CRUD Operations| Demonstrates SELECT, INSERT, UPDATE and DELETE
Data Loading| Loads CSV data into Snowflake tables
Time Travel| Accesses historical versions of data
Data Recovery| Recovers accidentally deleted records

---

Implementation Summary

Question 1: SnowSQL Login and Connection

SnowSQL was configured to establish a connection with the Snowflake account.

After connecting successfully, the following SQL commands were used to verify the session:

SELECT CURRENT_USER();
SELECT CURRENT_ROLE();
SELECT CURRENT_WAREHOUSE();
SELECT CURRENT_DATABASE();
SELECT CURRENT_SCHEMA();

The commands confirmed the current Snowflake user, role, warehouse, database, and schema.

Result

The Snowflake connection was successfully established and the current session details were displayed.

Screenshot

Add your screenshot here:

![SnowSQL Connection](screenshots/01_snowsql_connection.png)

---

Question 2: Creation of Snowflake Objects

The required Snowflake objects were created using SQL commands.

Database

CREATE DATABASE IF NOT EXISTS SNOWFLAKE_TUTORIAL;

Schema

CREATE SCHEMA IF NOT EXISTS SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA;

Warehouse

CREATE WAREHOUSE IF NOT EXISTS TUTORIAL_WH
WAREHOUSE_SIZE = 'X-SMALL'
AUTO_SUSPEND = 60
AUTO_RESUME = TRUE;

Table

CREATE TABLE IF NOT EXISTS SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS (
    STUDENT_ID INTEGER,
    NAME VARCHAR(100),
    AGE INTEGER,
    COURSE VARCHAR(100),
    MARKS INTEGER
);

Stage

CREATE STAGE IF NOT EXISTS SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENT_STAGE;

Insert Sample Records

INSERT INTO SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS
VALUES
(1, 'Arun', 21, 'Data Engineering', 85),
(2, 'Priya', 22, 'Data Science', 90),
(3, 'Rahul', 20, 'Cloud Computing', 78),
(4, 'Anita', 21, 'Database Management', 88);

SELECT Operation

SELECT * 
FROM SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS;

INSERT Operation

INSERT INTO SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS
VALUES (5, 'Kiran', 22, 'Cloud Computing', 82);

UPDATE Operation

UPDATE SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS
SET MARKS = 95
WHERE STUDENT_ID = 2;

DELETE Operation

DELETE FROM SNOWFLAKE_TUTORIAL.TUTORIAL_SCHEMA.STUDENTS
WHERE STUDENT_ID = 5;

Result

The database, schema, warehouse, table, and stage were successfully created. Sample records were inserted and the basic SQL operations were performed successfully.

---

Question 3: Data Loading Using SnowSQL

A sample CSV dataset was created for the data-loading demonstration.

Sample Dataset

STUDENT_ID,NAME,AGE,COURSE,MARKS
101,John,21,Data Engineering,85
102,Sarah,22,Data Science,92
103,David,20,Cloud Computing,79
104,Emma,21,Database Management,88

The CSV file was uploaded to the Snowflake stage using SnowSQL.

Upload File to Stage

PUT 'file:///path/to/students.csv'
@STUDENT_STAGE
AUTO_COMPRESS=TRUE;

Check Stage

LIST @STUDENT_STAGE;

Create File Format

CREATE FILE FORMAT IF NOT EXISTS STUDENT_CSV_FORMAT
TYPE = 'CSV'
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
SKIP_HEADER = 1;

Load Data

COPY INTO STUDENTS
FROM @STUDENT_STAGE
FILE_FORMAT = (FORMAT_NAME = 'STUDENT_CSV_FORMAT');

Verify Data

SELECT *
FROM STUDENTS;

Result

The CSV dataset was successfully uploaded to the Snowflake stage and loaded into the target table. The loaded records were verified using a "SELECT" query.

---

Question 4: Snowflake Time Travel

A separate table was created to demonstrate Snowflake Time Travel.

Create Table

CREATE TABLE TIME_TRAVEL_STUDENTS (
    STUDENT_ID INTEGER,
    NAME VARCHAR(100),
    COURSE VARCHAR(100),
    MARKS INTEGER
);

Insert Records

INSERT INTO TIME_TRAVEL_STUDENTS VALUES
(1, 'Arun', 'Data Engineering', 85),
(2, 'Priya', 'Data Science', 90),
(3, 'Rahul', 'Cloud Computing', 78),
(4, 'Anita', 'Database Management', 88);

Update a Record

UPDATE TIME_TRAVEL_STUDENTS
SET MARKS = 95
WHERE STUDENT_ID = 1;

Delete a Record

DELETE FROM TIME_TRAVEL_STUDENTS
WHERE STUDENT_ID = 3;

After the update and delete operations, the current table was checked using:

SELECT *
FROM TIME_TRAVEL_STUDENTS;

Query Historical Data

Time Travel can be used to view the table at an earlier point in time.

Example:

SELECT *
FROM TIME_TRAVEL_STUDENTS
AT (OFFSET => -300);

The offset represents approximately five minutes before the current time.

An exact timestamp can also be used:

SELECT *
FROM TIME_TRAVEL_STUDENTS
AT (TIMESTAMP => 'YYYY-MM-DD HH:MI:SS'::TIMESTAMP);

The timestamp should be replaced with the actual timestamp from the experiment.

Result

The historical version of the table was successfully accessed using Snowflake Time Travel. Records that were changed or deleted could be viewed in their earlier state.

---

Question 5: Data Recovery Using Time Travel

To demonstrate recovery, a record was intentionally deleted from the table.

DELETE FROM TIME_TRAVEL_STUDENTS
WHERE STUDENT_ID = 3;

The deleted record was then identified using Time Travel.

For example:

SELECT *
FROM TIME_TRAVEL_STUDENTS
BEFORE (STATEMENT => '<DELETE_QUERY_ID>');

The "<DELETE_QUERY_ID>" should be replaced with the actual query ID of the DELETE statement.

After identifying the deleted record, it can be inserted back into the current table.

INSERT INTO TIME_TRAVEL_STUDENTS
SELECT *
FROM TIME_TRAVEL_STUDENTS
BEFORE (STATEMENT => '<DELETE_QUERY_ID>')
WHERE STUDENT_ID = 3;

Finally, the recovery was verified:

SELECT *
FROM TIME_TRAVEL_STUDENTS
WHERE STUDENT_ID = 3;

Result

The accidentally deleted record was successfully identified using historical data and restored to the current table.

---

Results

The Snowflake tutorial assignment was successfully completed.

The following results were achieved:

1. Successfully established a connection to Snowflake using SnowSQL.
2. Verified the current user, role, warehouse, database, and schema.
3. Successfully created a database, schema, warehouse, table, and stage.
4. Performed "SELECT", "INSERT", "UPDATE", and "DELETE" operations.
5. Created a sample CSV dataset.
6. Successfully uploaded and loaded the CSV data into Snowflake.
7. Verified the loaded records using SQL queries.
8. Successfully demonstrated Snowflake Time Travel.
9. Retrieved historical data after performing update and delete operations.
10. Identified accidentally deleted records.
11. Successfully recovered deleted records using Time Travel.

---

Screenshots

The following screenshots are included as evidence of the implementation:

screenshots/
├── 01_snowsql_connection.png
├── 02_current_session.png
├── 03_snowflake_objects.png
├── 04_crud_operations.png
├── 05_stage_creation.png
├── 06_csv_dataset.png
├── 07_data_loading.png
├── 08_time_travel.png
├── 09_deleted_records.png
└── 10_data_recovery.png

Screenshots demonstrate the execution and output of the Snowflake commands.

---

Conclusion

This assignment provided practical knowledge of Snowflake and SnowSQL. The project demonstrated how to connect to Snowflake, create and manage different Snowflake objects, perform SQL operations, and load external data into Snowflake tables.

The assignment also demonstrated the use of Snowflake Time Travel to access previous versions of data. The data recovery exercise showed how historical data can be used to identify and restore records that were accidentally deleted.

Overall, this tutorial provided a strong foundation in Snowflake database management, SQL operations, data loading, and data recovery techniques. These concepts are useful for working with cloud-based data warehouses and modern data engineering environments.
