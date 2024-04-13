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
