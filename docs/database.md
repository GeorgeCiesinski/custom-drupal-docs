## MySQL

MySQL is the main database used in the Humber development and production environments. This section outlines some common use cases.

### Installation

You need to install two tools to use MySQL locally. 

1. MySQL Workbench

The [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) is the software used to connect to the MySQL database. 

2. MySQL Community Server `8.0.xx`

The [MySQL Community Server](https://dev.mysql.com/downloads/mysql/) is the actual MySQL server you need to run locally. The MySQL Workbench can connect to this server. Currently, MySQL Workbench can only connect to MySQL Community Server `5.7.xx` or `8.0.xx`, so make sure you download an 8.0.xx version. 

### Server Setup

The following instructions are for setup on Mac. 

#### Add Root Connection

The root connection will be used to connect to the local server and write queries. This can be later used to create new users and databases.

1. Install the downloaded MySQL Community Server `8.0.xx`. During the installation it will ask you to set a password. Use the default password option and not legacy. Type in your password carefully as the software will not ask you to verify the password. 
2. Open System Settings and scroll to the bottom in the left panel. Select MySQL. In the right side of the window, click Initialize database and enter your password. Once this is complete, click "Start MySQL Server".
![MySQL Settings](assets/database/mysql-settings.png)
3. Open MySQL Workbench and click the + button.
![Add Connection](assets/database/workbench-add.png)
4. Fill in the connection information as per below:
![Connection Info](assets/database/connection-info.png)
1. Click "Test Connection". This should prompt a popup asking for your password. Fill it in, making sure to checkmark "Save Password" and click OK. 
![Connection Password](assets/database/connection-password.png)
1. If you see the below window, the connection is successful. In the Setup New Connection screen, you can now click OK and the connection will be saved. 
![Success](assets/database/success.png)

### Create New Database

Each local website requires a unique user and schema. In MySQL the schema is synomymous with database. The website is configured to use the created user to access the database. 

Learn more about [Creating a New Database](https://git.drupalcode.org/project/drupal/-/blob/10.1.x/core/INSTALL.mysql.txt).

#### Create User

1. Connect to the root database which was setup in [Add Root Connection](#add-root-connection). 
2. Click Users and Privileges in the left side menu.
![users](assets/database/users.png)
3. Click Create User, and then fill out the username and password.
![create-user](assets/database/create-user.png)
4. Click Apply to save the new user.

**Note:** Make sure you set the host to localhost in order for the later steps to work.

#### Create Schema

1. Connect to the root database which was setup in [Add Root Connection](#add-root-connection). 
2. Click the Create New Schema button on the top toolbar.
![Create New Schema](assets/database/create-new-schema.png)
3. Name the new database, then select the Character Set `utf8mb4` and the Collation `utf8mb4_unicode_ci`. Click Apply, then Apply once again. 
![Configure Schema](assets/database/configure-schema.png)

#### Grant User Access to Schema

1. Connect to the root database which was setup in [Add Root Connection](#add-root-connection). 
2. In the Query tab, type the following SQL command where `its` is replaced by the database, and `itsuser`@localhost is replaced by the username.

```
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES
ON its.*
TO 'itsuser'@'localhost';
```

3. Click CMD+Enter on Mac or Ctrl+Enter on Windows to run the query.

### Exporting & Importing MySQL dump

When it comes time to migrate the local database to staging or production, or vice versa, it will become necessary to Export the data from one database, and Import it into the other. This can be done in MySQL workbench. 

#### Exporting data

1. Connect to the database you are exporting.
2. Click Server > Data Export. 
3. Checkmark the name of the database, and select "Export to Self-Contained File" in the **Export Options**. 
4. Click Start Export, and the database should export to a file similar to `date.sql`. 

#### Importing data

1. Connect to the database you want to import the data into.
2. Click Server > Data Import.
3. Select "Import from a Self-Contained File" in Import Options, and then select the dump file created above. 
4. Select the name of the database in **Default Schema to be Imported To**. 
5. Click Start Import. The import process should output a success message. If it outputs an error instead, read below. 

#### Error 1273

During the import process, you may experience an error similar to: 

```
ERROR 1273 (HY000) at line 25: Unknown collation: 'utf8mb4_0900_ai_ci'

Operation failed with exitcode 1
17:46:09 Import of /Users/ciesinsg/Documents/Backups/2023-11-07-backup.sql has finished with 1 errors
```

This indicates that the database, and/or one or more tables & columns may be using the wrong collation. This is due to a version mismatch between the exported database and the imported one. In this case, the exported database is MySQL version 8, while the importing database is MySQL version 5.7, and does not recognize 'utf8mb4_0900_ai_ci'. To correct this, it is necessary to fix the database, tables, and column [collation](#collation), and then export it once again. If this is done correctly, the database should import successfully. 

### Collation

Collation determines how strings are sorted in the database, including case, and diacritics. 

#### Check Collation

```sql title="Check database collation"
use database_name;
SELECT @@character_set_database, @@collation_database;
```

```sql title="Check table collation"
SELECT TABLE_SCHEMA
    , TABLE_NAME
    , TABLE_COLLATION 
FROM INFORMATION_SCHEMA.TABLES;
```

```sql title="Check column collation"
SELECT TABLE_NAME 
    , COLUMN_NAME 
    , COLLATION_NAME 
FROM INFORMATION_SCHEMA.COLUMNS;
```

```sql title="Aggregated list of Database Objects with Collation and Character Set"
SELECT TABLE_SCHEMA,
       TABLE_NAME,
       CCSA.CHARACTER_SET_NAME AS DEFAULT_CHAR_SET,
       COLUMN_NAME,
       COLUMN_TYPE,
       C.CHARACTER_SET_NAME, CCSA.COLLATION_NAME, ENGINE
  FROM information_schema.TABLES AS T
  JOIN information_schema.COLUMNS AS C USING (TABLE_SCHEMA, TABLE_NAME)
  JOIN information_schema.COLLATION_CHARACTER_SET_APPLICABILITY AS CCSA
       ON (T.TABLE_COLLATION = CCSA.COLLATION_NAME)
 WHERE TABLE_SCHEMA=SCHEMA()
   AND C.DATA_TYPE IN ('enum', 'varchar', 'char', 'text', 'mediumtext', 'longtext')
 ORDER BY TABLE_SCHEMA,
          TABLE_NAME,
          COLUMN_NAME, CCSA.COLLATION_NAME, ENGINE;
```

#### Fixing Collation

Fixing collation requires a number of steps to construct a SQL query. Shorter queries are needed to construct the main query which will fix the collation of the database, tables, and columns.

**Note:** The charset `utf8mb4` and collation `utf8mb4_unicode_ci` are required in all database structures. 

1. Start the main query by disabling Foreign Key checks at the start, and re-enabling them at the end.

```sql title="Disable Foreign Key Checks"
SET FOREIGN_KEY_CHECKS=0;
 
-- Insert your other SQL Queries here...
 
SET FOREIGN_KEY_CHECKS=1;
```

2. Modify the Alter Database query by adding in your database. Once modified, simply place it in the main query.

```sql title="Create the Alter Database query"
ALTER DATABASE <yourDB> CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
```

3. Modify each of the below scripts by adding in your database, then run the scripts to generate the queries required to alter each table. Copy the outputted query rows "without quotations", and then paste them into the main query. 

```sql title="Generate the Alter Tables queries"
SELECT CONCAT('ALTER TABLE `',  table_name, '` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;')
FROM information_schema.TABLES AS T, information_schema.`COLLATION_CHARACTER_SET_APPLICABILITY` AS C
WHERE C.collation_name = T.table_collation
AND T.table_schema = '<yourDB>'
AND
(
    C.CHARACTER_SET_NAME != 'utf8mb4'
    OR
    C.COLLATION_NAME != 'utf8mb4_unicode_ci'
);
```

```sql title="Generate the queries to alter varchar columns"
SELECT CONCAT('ALTER TABLE `', table_name, '` MODIFY `', column_name, '` ', DATA_TYPE, '(', CHARACTER_MAXIMUM_LENGTH, ') CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci', (CASE WHEN IS_NULLABLE = 'NO' THEN ' NOT NULL' ELSE '' END), ';')
FROM information_schema.COLUMNS 
WHERE TABLE_SCHEMA = '<yourDB>'
AND DATA_TYPE = 'varchar'
AND
(
    CHARACTER_SET_NAME != 'utf8mb4'
    OR
    COLLATION_NAME != 'utf8mb4_unicode_ci'
);
```

```sql title="Generate the queries to alter non-varchar columns"
SELECT CONCAT('ALTER TABLE `', table_name, '` MODIFY `', column_name, '` ', DATA_TYPE, ' CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci', (CASE WHEN IS_NULLABLE = 'NO' THEN ' NOT NULL' ELSE '' END), ';')
FROM information_schema.COLUMNS 
WHERE TABLE_SCHEMA = '<yourDB>'
AND DATA_TYPE != 'varchar'
AND
(
    CHARACTER_SET_NAME != 'utf8mb4'
    OR
    COLLATION_NAME != 'utf8mb4_unicode_ci'
);
```

4. Run the main query once all of the altered and generated query rows are pasted into it. 

### Useful Queries

#### Check SQL Version

```sql title="Simple version check"
SELECT VERSION();
```

```sql title="Detailed version check"
SHOW VARIABLES LIKE "%version%"
```

#### Drop Database Tables

To drop the database tables, first run this script to generate SQL drop queries:

```sql
SELECT CONCAT('DROP TABLE IF EXISTS `', table_name, '`;')
FROM information_schema.tables
WHERE table_schema = 'database_name';
```

After running this script, copy the outputted queries without quotations and 
run them to delete the tables.
