[//]: # (title: Configuring UTF8 Character Set for MySQL)
[//]: # (auxiliary-id: Configuring UTF8 Character Set for MySQL)

<!--[//]: # (Internal note. Do not delete. "Configuring UTF8 Character Set for MySQLd89e3.txt")
[//]: # (Internal note. Do not delete. "Configuring UTF8 Character Set for MySQLd89e8.txt")-->  

To create a MySQL database which uses the UTF-8 character set:
	
1. Create a new database:

    ```SQL
    create database <database_name> character set utf8mb4 collate utf8mb4_bin
    ```
	
2. Open [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/database.properties` and add the `characterEncoding` property:
  
    ```
    connectionProperties.characterEncoding=UTF-8
    ```
 
To change the character set of an existing MySQL database to UTF-8:
	
1. Shut down the TeamCity server.
2. From the [`<TeamCity Home>`](teamcity-home-directory.md)`/bin` directory, export the database using the maintainDB tool:
    
    ```
    maintainDB backup -D -F database_backup
    ```

    More details on the backup procedure are [here](creating-backup-via-maintaindb-command-line-tool.md#Performing+TeamCity+Data+Backup+with+maintainDB+Utility).
3. Create a new database with UTF-8 as the default character set, as described in Step 1.
4. Modify the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/database.properties` file by changing the `connectionUrl` property to:
        
    ```
    jdbc:mysql://<host>/<new_database_name>
    ```
5. Import data to the new database:

    ```
    maintainDB restore -D -F database_backup -T <TeamCity Data Directory>/config/database.properties
    ```
6. Start the TeamCity server.
