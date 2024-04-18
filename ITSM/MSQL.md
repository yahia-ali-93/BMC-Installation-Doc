# MSQL Configuration Instructions

Please follow along these steps while having the BMC documentation open

1. Download the Dumb files
2. open SQL Server Configuration Manager
3. Click on SQL Server Network Configuration and click on the installation instance
4. Click on **TCP/IP** and make sure it's **enabled**
5. open the **IP Addresses** tab and make sure all ip addresses are **active** and **enabled**
6. click on **SQL Server Services** and restart all services
7. Download the two SQL scripts inside the documentation
    1. Drop_Innovation_Suite_database_and_users
    2. Innovation_Suite_schema_creation
8. Create a folder inside the c directory and name it **"BACKUP"**
9. Put everything you've downloaded inside that folder
    1. the two scripts from step 7
    2. the db dumb from step 1
10. extract the db dumb archive file in the same directory **c://BACKUP**
11. Edit the **"Innovation_Suite_scheme_creation"** script and update the variables in **"<",">"**
    1. The path to the data and log folders should be inside a folder that looks like this **"C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL"**
        - Note that this might not be the exact path to the parent folder so look for it inside the Microsoft SQL Server folder
    2. the path to the backups should be **c:\BACKUP** if you've followed all instructions above
    3. Set all passwords to password complex **"P@ssw0rd"**
    4. The name of the script files are wrong in the script file and you should update it with the correct names from the db dumb you've downloaded
12. Add **sqlcmd** to your Path env variable
13. Run the following two commands

    ```sql
    sqlcmd -i Drop_Innovation_Suite_database_and_users.sql -U SA -P P@ssw0rd
    sqlcmd -i Innovation_Suite_schema_creation.sql -U SA -P P@ssw0rd
    ```

14. Try both of these commands in a new query on the innovation suite database to create synonyms

    ```sql
    create synonym trace_xe_action_map for sys.trace_xe_action_map;
	create synonym trace_xe_event_map for sys.trace_xe_event_map;
    ```
