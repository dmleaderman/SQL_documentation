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
