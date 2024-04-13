# SQL_documentation
Connect to MySQL Using MySQL Client
You can use the MySQL command-line client to connect to MySQL.

Install MySQL Client
The MySQL Community Server includes the MySQL client, if you have not installed the MySQL Community Server, download and install

Add mysql location to the PATH Environment Variable
On Windows
Open System Properties -> Environment Variables -> System variables
Select Path, click Edit, then New, add the line C:\Program Files\MySQL\MySQL Server 8.0\bin
Click Close
Connect to MySQL instance
Start the mysql client:

mysql -h <db_endpoint> -P <db_port> -u <db_user> -p
or

mysql --host=<db_endpoint> --port=<db_port> --user=<db_user> -p
For example, if you want to connect to MySQL database instance with following info:

db_endpoint=msba2024-serverless-mysql-production.cluster-cqxikovybdnm.us-east-2.rds.amazonaws.com
db_port=3306
db_user=myusername
db_password=mydbpass
mysql -h msba2024-serverless-mysql-production.cluster-cqxikovybdnm.us-east-2.rds.amazonaws.com -P 3306 -p -u myusername  
or

mysql --host=msba2024-serverless-mysql-production.cluster-cqxikovybdnm.us-east-2.rds.amazonaws.com --port=3306 -p --user=myusername 
enter your password:

Enter password: ******************
then mysql prompt appears

mysql>
At mysql prompt, you can

Get the List of Commands

mysql> help
Show List of Schemas

mysql> show databases;
Change to the database you want to work on

For example, you want to work on MSBA_Team1 database

mysql> use MSBA_Team1;
List tables in the current database (or schema)

mysql> show tables;
Get a table's definition

mysql> desc crsp_daily_data;
Run the query

mysql> select count(*) from crsp_daily_data;

Load CSV File Data into MySQL Table
Enable LOAD DATA LOCAL INFILE in MySQL Workbench
Launch MySQL Workbench.

Click Database on the top menu.

Select Manage Connections…, a Manage Server Connections popup opens.

Select the connection at the left you want to load data, for example, msba2024-serverless-mysql-production

wb20

Click Advanced button, add OPT_LOCAL_INFILE=1 in Others section

wb21

Click Test Connection button.

Click OK if it is successful, then Close

SET Environment Varilable: local_infile
If your MySQL connection is localhost or 127.0. 0.1 , run following command in the Workbench SQL editor:

SET GLOBAL local_infile=1;
If your connection is remote MySQL on AWS, this step can be skipped.

Load CSV File Data into MySQL Table Using Workbench
Create a table, for example, create a table crsp_daily_data in BOOTCAMP_DEV schema , run following SQL statements in Workbench SQL Editor, replace BOOTCAMP_DEV with your schema:

  use BOOTCAMP_DEV;
  CREATE TABLE crsp_daily_data (
  premno int NOT NULL,
  caldt date NOT NULL,
  price double,
  volume int,
  daily_return double,
  shrsout int
  );
wb22

Load CSV file, for example, load crsp_daily_data.csv data into table crsp_daily_data, run following SQL statements in Workbench SQL Editor, replace BOOTCAMP_DEV with your schema:

  use BOOTCAMP_DEV;
  LOAD DATA LOCAL INFILE 'C:/Users/PGU6/Downloads/data/crsp_daily_data.csv'
    INTO TABLE `crsp_daily_data`
    FIELDS TERMINATED BY ','
    ENCLOSED BY '"'
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
You can also combine schema with table, replace BOOTCAMP_DEV with your schema:

  LOAD DATA LOCAL INFILE 'C:/Users/PGU6/Downloads/data/crsp_daily_data.csv'
    INTO TABLE `BOOTCAMP_DEV.crsp_daily_data`
    FIELDS TERMINATED BY ','
    ENCLOSED BY '"'
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
The path of csv file can be written as:

  LOAD DATA LOCAL INFILE 'C:\\Users\\PGU6\\Downloads\\data\\crsp_daily_data.csv'
    INTO TABLE `BOOTCAMP_DEV.crsp_daily_data`
    FIELDS TERMINATED BY ','
    ENCLOSED BY '"'
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
Make sure replace BOOTCAMP_DEV with your schema

Make sure put LOCAL before INFILE in LOAD DATA INFILE, the correct one is: LOAD DATA LOCAL INFILE. If you forget LOCAL, you would get errors

Make sure replace the data file full path with yours at your laptop

### MySQL Scripts
Following SQL scripts are used in Tech Preview bootcamp to create tables, join tables and load CSV files into database tables

#### Create CRSP Header Table 
```sql
USE MSBA_DB1;
CREATE TABLE crsp_header(
   permno INT NOT NULL UNIQUE,
   permco INT NOT NULL,
   ticker VARCHAR(5),
   comnam VARCHAR(32) NOT NULL,
   naics INT,
   prim_exch VARCHAR(1),
   cusip VARCHAR(8),
   siccd INT,
   begdat DATE,
   enddat DATE,
      PRIMARY KEY ( permno )
);
```

**Note** replace ```MSBA_DB1``` with the schema that you have access, add your initial in the table name,
for example, replace ```crsp_header``` with ```crsp_header_ag```

#### Create CRSP Daily Data Table
```sql
USE MSBA_DB1;
CREATE TABLE crsp_daily_data(
   permno INT NOT NULL,
   caldt DATE,
   price DOUBLE,
   volume INT,
   daily_return DOUBLE,
   shrsout INT,
      PRIMARY KEY ( permno, caldt )
);
```

**Note** replace ```MSBA_DB1``` with the schema that you have access, add your initial in the table name,
for example, replace ```crsp_daily_data``` with ```crsp_daily_data_ag```

#### Simple Join CRSP Header and Daily Tables
```sql
USE MSBA_DB1;
SELECT comnam, crsp_header.permno, sum(daily_return), exp(sum(log(daily_return+1)))-1 as cum_daily_return, count(daily_return)
 from crsp_header
  inner join crsp_daily_data
     on crsp_header.permno=crsp_daily_data.permno
     and caldt between '2022-01-01' and '2022-12-31'
 group by crsp_header.permno;
```

**Note** replace ```MSBA_DB1``` with the schema that you have access, add your initial in the table name,
for example, replace ```crsp_header``` with ```crsp_header_ag```, replace ```crsp_daily_data``` with ```crsp_daily_data_ag```

#### Load CRSP Header Table from CSV File
```sql
USE MSBA_DB1;
LOAD DATA LOCAL INFILE 'C:/Users/rharr08/Downloads/fpdqyfoyhasok69f.csv' 
    INTO TABLE crsp_header
    FIELDS TERMINATED BY ',' 
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
```

**Note** replace ```MSBA_DB1``` with the schema that you have access, add your initial in the table name,
for example, replace ```crsp_header``` with ```crsp_header_ag```

#### Load CRSP Daily Data Table from CSV File
```sql
USE MSBA_DB1;
LOAD DATA LOCAL INFILE 'C:/users/rharr08/downloads/socrbvf3sdlcbqcs.csv' 
    INTO TABLE `crsp_daily_data`
    FIELDS TERMINATED BY ',' 
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
```

**Note** replace ```MSBA_DB1``` with the schema that you have access, add your initial in the table name,
for example, replace ```crsp_daily_data``` with ```crsp_daily_data_ag```

#### Troubleshooting
- MySQL Case sensitivity - Some are case-sensitive and some are not
- Access error when loading csv file into table
  - make sure ```OPT_LOCAL_INFILE=1``` is set in Workbench, [see the docs](how_to_load_csv_file_into_mysql_table.pdf)
  - make sure csv file full path is correct
  - On Windows, replace window path seperator \  with / in the csv file path, for example, 
    replace ```C:\users\rharr08\downloads\socrbvf3sdlcbqcs.csv``` with ```C:/users/rharr08/downloads/socrbvf3sdlcbqcs.csv```

## MySQL Troubleshooting

### Load Data into a Table Access Denied
**Problem**

When loading data into a table, you might get Error:
```sql
Error Code: 1290. The MySQL server is running with the --secure-file-priv option so it cannot execute this statement.
so it cannot execute this statement.
```

When you have this error, first, you need to check if you have permission to access the schema and table.
If you do, execute the following line together with **your LOAD DATA LOCAL INFILE ...** statements
```shell
use <my-scheme>;
```

where **<my-scheme>** is the schema you wish to load data into.

for example:
```shell
use STUDENT_AAG332;
```
where **STUDENT_AAG332** is the schema you want to load data into.

If you still get errors, try following workaround.

**Workaround**

1. In the Workbench, add following line  to your instance connection's ```Others``` box under ```Advanced``` settings, see [How to Load csv file into a table using MySQL Workbench](how_to_load_csv_file_into_mysql_table.pdf)
   ```sql
   OPT_LOCAL_INFILE=1
   ```

   ![Configure](../image/mysqloptconfig.PNG)


2. Quit Workbench if ```Test connection``` is successfully, then re-launch Workbench.
3. If your MySQL connection is **localhost** or **127.0. 0.1** , run following command in the Workbench SQL editor:
   ```sql
   SET GLOBAL local_infile=1;
   ``` 
4. In the same SQL Editor, load data into table:
   - **Sample 1, fields seperated by tab in csv file**
   ```sql
    LOAD DATA LOCAL INFILE 'C:\\Users\\PGU6\\Documents\\GBS\\TechBootCamp\\MSBA2024\\etsy_shops_data.tsv'
    INTO TABLE MY_SCHEMA.shops
    FIELDS TERMINATED BY '\t'
    ENCLOSED  BY '"';
   ```
   - **Sample 2, fields seperated by comma in csv file**
   ```sql
   LOAD DATA LOCAL INFILE  'c:/tmp/discounts.csv'
   INTO TABLE MY_SCHEMA.discounts
   FIELDS TERMINATED BY ','
   ENCLOSED BY '"'
   LINES TERMINATED BY '\n'
   IGNORE 1 ROWS;
   ```

- Make sure replace **MY_SCHEMA** with your schema
- Make sure put **LOCAL** before **INFILE** in **LOAD DATA INFILE**, the correct one is: **LOAD DATA LOCAL INFILE**. If you forget **LOCAL**, you would get errors
- Make sure replace the data file full path with yours at your laptop

### Query Timeout Lost connection to MySQL server
**Problem**
```sql
Error Code: 2013. Lost connection to MySQL server during query
```

**Client-Side Solution**

You can increase your MySQL client's timeout values, for instance 1 hour, in MySQL Workbench by editing 
the SQL Editor preferences:

1. Select ```Edit``` -> ```Preferences``` -> ```SQL Editor```
2. Look for the MySQL Session section
3. In ```DBMS connection read timeout interval(in seconds)```, change the value to ```3600``` 
4. In ```DBMS connection timeout interval(in seconds)``` field, change value to ```120```.
<br><br>
   ![Configure](../image/wb30.PNG) <br>

5. Click ```OK```
6. Quit or close Workbench
7. Re launch Workbench
