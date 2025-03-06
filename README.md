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

