# sql_practice
SQL Server & SSMS Developer Installation

Prerequisites
Operating System: Windows 10 or higher.
Administrative Rights: Required for installing software.
Internet Connection: To download installation files.

Installation Steps
1. Install SQL Server Developer Edition
Download SQL Server Developer Edition:
Visit the Microsoft SQL Server Downloads page and select the Developer edition.

Run the Installer:
Launch the downloaded installer (.exe).
Choose the Basic installation option for a quick setup, or Custom if you need to adjust specific features.

Authentication Mode:

Windows Authentication: Recommended for local development.
Mixed Mode: Use this option if you need to configure SQL Server Authentication (you will be prompted to set a password for the sa account).
Complete the Setup:
Follow the installation wizard and restart your computer if prompted.

2. Install SQL Server Management Studio (SSMS)
Download SSMS:
Go to the SSMS Download Page and download the latest version of SSMS.

Run the Installer:
Launch the downloaded installer and follow the prompts to install SSMS.

Launch SSMS:
Open SSMS from the Start menu once installation is complete.


Open SSMS:
Launch SQL Server Management Studio.

Connect to Your SQL Server Instance:

Use (local), localhost, or . as the server name if you installed a default instance.
If you installed a named instance (like SQLEXPRESS), use YourComputerName\SQLEXPRESS.
Choose Windows Authentication (or SQL Server Authentication if configured).
Verify Installation:
Once connected, check the Object Explorer for system databases (master, model, msdb, tempdb).

Usage
This setup is intended for development and testing. You can use SSMS to:

Create and manage databases.
Write and execute T-SQL queries.
Explore SQL Server features.
Troubleshooting
Cannot Connect to SQL Server:

Ensure that the SQL Server service is running (check via SQL Server Configuration Manager or Windows Services).
Verify that your server name and authentication mode are correct.
Check Windows Firewall settings to ensure SQL Server is allowed.
-----------------------------------------------------------------------------------------------------------------------------------------------
SSMS installation guide:
https://www.youtube.com/watch?v=Q8gBvsUjTLw&ab_channel=JoeyBlue


create_table simple guide:
https://www.w3schools.com/sql/sql_create_table.asp

-------Table creation challanges and probable solutions

Handling Table Existence and Conditional Creation
Unlike some other database systems (such as MySQL), SQL Server does not support the CREATE TABLE IF NOT EXISTS syntax directly. So if you try to create a table that already exists, SQL Server will return an error. 

The Issue
In MySQL you might write:

CREATE TABLE IF NOT EXISTS my_table (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

However, SQL Server does not recognize IF NOT EXISTS as part of the CREATE TABLE command.

The Solution

To safely create a table only if it does not already exist, you can use the OBJECT_ID function along with an IF statement. This method checks the system catalog for the table’s existence and only executes the CREATE TABLE statement if the table isn’t found.

in sql that will look like:

IF OBJECT_ID('dbo.my_table', 'U') IS NULL
BEGIN
    CREATE TABLE dbo.my_table (
        id INT IDENTITY(1,1) PRIMARY KEY,
        name NVARCHAR(50) NOT NULL
    );
END;



OBJECT_ID('dbo.my_table', 'U'): -returns the object ID for my_table if it exists. The 'U' parameter specifies that it is a user-defined table.
IS NULL: If the function returns NULL, it indicates that the table does not exist.
BEGIN ... END: The CREATE TABLE statement inside this block is executed only if the table is not found.


----Drop table challanges and probable solutions

1. Dropping a Non-Existent Table
If you try to drop a table that does not exist, SQL Server will throw an error. This can interrupt scripts or processes.

Solution:  Conditional Drops usage
For SQL Server 2016+

DROP TABLE IF EXISTS dbo.my_table;
This command will drop the table only if it exists, preventing errors.

For earlier versions:

IF OBJECT_ID('dbo.my_table', 'U') IS NOT NULL
BEGIN
    DROP TABLE dbo.my_table;
END;


..................................................................................................................................
2. Dropping Tables with Dependent Constraints
Another common issue occurs when a table is referenced by foreign key constraints from other tables. SQL Server will not allow you to drop a table that is being referenced, as this would break referential integrity.

Solution: Dropping Dependent Constraints First
Before dropping a table, you need to drop the foreign key constraints that depend on it. For example, if you have a grade table that references the student table, you must drop the foreign key constraint from grade before dropping student:

-- First, drop the foreign key constraint from the dependent table
ALTER TABLE dbo.grade
DROP CONSTRAINT fk_student_id;

-- Then, drop the referenced table safely
IF OBJECT_ID('dbo.student', 'U') IS NOT NULL
BEGIN
    DROP TABLE dbo.student;
END;

Expl:
Conditional Dropping: Use DROP TABLE IF EXISTS (SQL Server 2016+) or check with OBJECT_ID to avoid errors when a table doesn't exist.
Handling Dependencies: Ensure that you drop or disable any foreign key constraints on dependent tables before dropping the table they reference.
Including these strategies in your scripts makes your database management tasks smoother and prevents common errors during schema changes.


--------------------------------------------------------------------------------------------------------------------------
relations check(foreign key creation check) 
Verifying Foreign Key Constraints and Relationships:

When you define foreign key relationships between tables, it's essential to ensure they are properly created. This verification helps maintain data integrity by enforcing referential integrity between related tables. SQL Server stores metadata about foreign keys in system views like sys.foreign_keys.

Checking Foreign Key Constraints
run a query against the system view to list all foreign keys defined on a particular table. For example, to check the foreign keys on the grade table, use:

SELECT name 
FROM sys.foreign_keys 
WHERE parent_object_id = OBJECT_ID('dbo.grade');



OBJECT_ID('dbo.grade'): Retrieves the unique object ID of the grade table.
sys.foreign_keys: Contains a row for every foreign key constraint in the database. Filtering by the parent_object_id returns only those constraints defined on the grade table.
Expected Result:

If your grade table is designed to reference both the student and subj tables, you should see two constraints (e.g., fk_student_id and fk_subject_id).
If only one is returned, it may indicate that one of the constraints wasn’t created correctly or that there’s a schema reference issue.

Why It’s Important
Ensuring Referential Integrity:
By verifying foreign keys, you confirm that your data model enforces relationships correctly (e.g., every grade must belong to an existing student and a valid subject).


Troubleshooting:
If the query does not list an expected foreign key, check for issues such as:
A syntax error or omission in your CREATE TABLE statement.

Schema mismatches (e.g., using subj instead of dbo.subj).

Incomplete execution of the script.


Data Consistency:

Proper foreign key constraints prevent orphan records and help maintain the consistency of your database.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
