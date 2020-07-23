[//]: # (title: Configuring UTF8 Character Set for MySQL)
[//]: # (auxiliary-id: Configuring UTF8 Character Set for MySQL)

[//]: # (Internal note. Do not delete. "Configuring UTF8 Character Set for MySQLd89e3.txt")    
[//]: # (Internal note. Do not delete. "Configuring UTF8 Character Set for MySQLd89e8.txt")    

__To create a MySQL database which uses the utf8 character set:__
	
1. Create a new database:

    ```SQL
    create database <database_name> character set UTF8 collate utf8_bin
    ```
	
2. Open `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/database.properties`, and add the `characterEncoding` property:
  
    ```Plain Text
    connectionProperties.characterEncoding=UTF-8
    ```
 
__To change the character set of an existing MySQL database to utf8:__
	
1. Shut the TeamCity server down.
2. From the `<`[`TeamCity Home`](teamcity-home-directory.md)`>/bin` directory, export the database using the `maintainDB tool`:
    
    ```Plain Text
    maintainDB backup -D -F database_backup
    ```

    More details on this backup are [here](creating-backup-via-maintaindb-command-line-tool.md#Performing+TeamCity+Data+Backup+with+maintainDB+Utility).
3. Create a new database with utf8 as the default character set, as described above.
4. Modify the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/database.properties` file by changing the `connectionUrl` property to:
        
    ```Plain Text
    jdbc:mysql://<host>/<new_database_name>
    ```
5. Import data the new database:

    ```Plain Text
    maintainDB restore -D -F database_backup -T <TeamCity Data Directory>/config/database.properties
    ```
6. Start the TeamCity server.
