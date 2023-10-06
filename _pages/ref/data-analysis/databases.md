---
layout: page
permalink: /ref/data-analysis/databases/
title: data
description: Intro to Data Analysis
nav: false
toc: true
---

**Databases**  

Types of databases  
- Relational db (SQL) has tables with rows/cols to store structured data. The tables are related to each other thru keys and you can join based on certain key/col(s) values.
- Non-relational may contain less structured data  
- Time based db (influxdb). Optimized for storing/retrieving time based data.  

# Influxdb (Time Based)  
Some database options  
* influxdb (time series for monitoring measurements over time)   
* mysql (relation based) Can be used to collect a set of measurements over varying conditions.    
* Text, csv, excel file      
* 
For time series data I usually use node-red (with its charting options) connected to influxdb  
For experimental data I read a csv using Jupyter Notebook or dash plotted by plotly    

[Influxdb](https://www.influxdata.com/) is a time series database (the time stamp is automatically added to the data when storing). It does not require a schema so it is a quick and easy install for collecting data over time. Node-red can store data in the influxdb and retrieve data for charting. (port 8086 used for communication)  
* [Design Principles](https://docs.influxdata.com/influxdb/v2.0/reference/key-concepts/design-principles/)
* [Explanation of Data Elements](https://docs.influxdata.com/influxdb/v2.0/reference/key-concepts/data-elements/)

Field - used to store the data and is required (not indexed)  
Tags - descriptions of the data (metadata). Indexed to make data retrieval faster. Not required but recommended.  
Measurement - similar to a table  

Note - with ver 1.8+ there is an alternate query option called flux and it differs quite a bit from InfluxQL syntax. The advantage is that flux provides a lot more options for querying and analyzing data. Some examples in my node-red will use flux instead of InfluxQL.  

Installation   
Add the InfluxDB repository key so the RPi package manager is allowed to search that repository. The key gets passed into the gpg program and de-armored. Then saved into the /usr/share/keyrings/ directory.  
```curl https://repos.influxdata.com/influxdb.key | gpg --dearmor | sudo tee /usr/share/keyrings/influxdb-archive-keyring.gpg >/dev/null```  
```lsb_release -cs```  
Add influxDB repository to the sources list using signed-by. lsb_release -cs will automatically fill in the RPi version.   
```echo "deb [signed-by=/usr/share/keyrings/influxdb-archive-keyring.gpg] https://repos.influxdata.com/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/influxdb.list```   

```sudo apt update``` Update the package list. Should see influxdata added  
Now install  
```sudo apt install influxdb ```  
```sudo systemctl unmask influxdb.service ```  
```sudo systemctl enable influxdb  ```  
```sudo systemctl start influxdb ```  
```journalctl -u influxdb ```  

unmask allows all/any attempts to start the service
 
​For node-red to pull data you need to enable HTTP authentication.  With HTTP authentication enabled InfluxDB also requires you to create at least one admin user before you can interact with the system. Enter the influx interface by typing influx at a command line and then create an admin user/pswd.  
```console
influx 
CREATE USER admin WITH PASSWORD '<passwd>' WITH ALL PRIVILEGES 
show users 
exit 
```

Now edit configuration file to enable HTTP authentication.  
```sudo nano /etc/influxdb/influxdb.conf ```  
Set HTTP service enabled and address  
```console
[http] 
enabled = true 
flux-enabled = true 
bind-address = ":8086" 
```
*Note - After restarting influxdb service you may get "Failed to connect" if trying to enter REPL. It may take 5-15sec to setup and allow you back in.  
```sudo systemctl restart influxdb ```  
```influx -username admin -password <passwd> ```  

Helpful commands   
```console
> CREATE DATABASE mydb
(will use default retention policy that equals shard group duration of 7 days. Data will be deleted after 7 days. Shard group is compressed data, stored in a TSM file.)

To use a different retnetion policy ..
> CREATE DATABASE mydb WITH DURATION 26w (default shard for +6mo of data is 1w) 
> CREATE DATABASE mydb WITH DURATION 2w REPLICATION 1 SHARD DURATION 1w 

> DROP DATABASE mydb (to delete) 
> SHOW DATABASES 
> USE mydb 
> SHOW MEASUREMENTS 
> SHOW SERIES 
> SELECT * FROM measurement-name 
> SELECT * FROM temperature 
> SELECT * FROM "temp" WHERE "value" > 0 
> SELECT count(*) FROM temperature; (will show how many recoreds in MEASUREMENT temperature) 
```

To use influx cli at command prompt  
```influx -execute 'SHOW DATABASES' ```  

To delete data
```console
>use mydb
> delete from mydb where time > 161798006580600 and time < 161798007032600
> delete from mydb where time = 1617980056650000
> delete from mydb where tag = 'value' 

Retention Policy and Shard. To alter the autogen RP on a db (autogen is the default RP used in nodered influx node)
> use mydb 
> ALTER RETENTION POLICY autogen ON mydb DURATION 2w REPLICATION 1 SHARD DURATION 1w 
> show retention policies 
> show shard groups 
```
Sharding - facilitates horizontal scaling, or scaling out (vs vertical scaling). Allow for faster processing.
Default shard groups based on RP duration.    
Retention Policy’s DURATION -- Shard Group Duration    
\< 2 days                   --   1 hour  
\>= 2 days and <= 6 months  --   1 day  
\> 6 months                 --   7 days  

```console
CREATE RETENTION POLICY "52weeks" ON "mydb" DURATION 52w REPLICATION 1 DEFAULT
```
creates an RP called 52weeks that exists in "mydb" database. 52w keeps data for a DURATION of 52w and it’s the DEFAULT RP for the database.  
For testing  
```console
CREATE RETENTION POLICY "1d" ON "mydb" DURATION 1d REPLICATION 1
``````
will keep data for 1 day. Have 1 day to get the testing data retrieved and saved offline in a spreadsheet.  

To export a database/table(measurement) to a csv file  
```influx -database 'database name' -execute "SELECT * FROM measurement" -format csv > test.csv```  

To backup/restore data  
```sudo -u influxdb influxd backup -portable -host <IPadd>:8088 /path/to/backup/ ```  
```sudo -u influxdb influxd restore -portable -host <IPadd>:8088 /path/to/backup/ ```   

To change the location where your data is stored  
Create data and meta folders in the newlocation  
Make influxdb owner of these new locations   
```sudo chown -R influxdb:influxdb newlocation/influxdb/meta/```  
```sudo chown -R influxdb:influxdb newlocation/influxdb/data/```  
Update permission  
```sudo chmod 777 -R data```  
```sudo chmod 777 -R meta```  

changed folders in /etc/influxdb/influxdb.conf to /home/pi/influxdb (data and meta) along with IP address and under [http] at bottom enable=true for endpoint and bind-address = 8086   

# MariaDB (SQL)  

Mariadb is based on mysql.  Port 3306
> Note for remote connection from  excel. 
> Will need to configure mariadab config file by commenting out bind statement and add user privileges to all db and wild card host (see comments below).
> Also may need to install ODBC (Open DataBase Connectivity) connector from https://dev.mysql.com/downloads/connector/odbc/  and visual studio update. Then search/open up ODBC 64bit administrator and add MySQL ODBC 8.0 ANSI driver. Set up the connection and chose SQL server authentication. 


Run sudo apt update to update the package list
* ```sudo apt update```  
* ```sudo apt install mariadb-server```
* ```sudo mysql_secure_installation``` utility  
(options to allow for remote access on port 3306)  
*```sudo mysql -u root -p```
* ```sudo ufw allow mysql```  
* ```sudo systemctl start mysql```  
* ```sudo systemctl enable mysql```  
Config file is at /etc/mysql/mysql.conf.d/mysqld.cnf  

If you don't run as sudo then the first time you get "MySQL ERROR" then run as sudo  
* ```$ sudo mysql```  
Then run alter command at mysql prompt  
```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';```  
CTRL-D to exit  

Create a db with ```CREATE DATABASE name;```  
Then create a user and privileges  
```CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';```  

```GRANT ALL PRIVILEGES ON <dbname>.* TO 'username'@'localhost';```  
```FLUSH PRIVILEGES;```  

Can check user login with same ```sudo mysql -u user -p```  
To see users  
```SELECT User, Host FROM mysql.user WHERE Host <> 'localhost';```  

To grant remote access you wild card all db with * and use % to wild card IP addresses  
```GRANT ALL PRIVILEGES ON *.* TO 'sj4u'@'192.168.100.%' IDENTIFIED BY 'password' WITH GRANT OPTION;```  

To see grants for a specific user  
```SHOW GRANTS FOR CURRENT_USER```  
```SHOW GRANTS FOR 'user'@'localhost'```  

**note** When creating a new database I found I had to still grant privileges to the user for that specific database.  


Firewall config
```console
firewall config  
firewall-cmd --add-port=3306/tcp 
firewall-cmd --permanent --add-port=3306/tcp
```  

Configuration file is in /etc/mysql/mariadb.conf.d/50-server.cnf  
for remote access may need to update bind-address from 127.0.0.1 to 0.0.0.0 or comment it out  
and add  
```console
skip-networking
skip-bind-address
```  
and restart with ```sudo systemctl restart mysql.service```  
can see starting param with ```mysqld --print-defaults```  

or install PHPMyAdmin  
```sudo apt install php libapache2-mod-php```  
```sudo apt install phpmyadmin```  
Select apache web server and say No to config with dbconfig-common   

Go to http://RPi-IPAddress/phpmyadmin  
if you need to restart the server use ```sudo service apache2 restart```  

> Test connection with ``` mysql --host=localhost --password='password' --protocol=tcp --port=3306 esp2nred
```  

Common mysql commands  
```console
SHOW DATABASES;
USE <database>;
SHOW TABLES;
SELECT * FROM <table>;
UPDATE, INSERT, DELETE```

To import a csv (can also save a xls file as csv)  
Create a database and empty table with fields then import with the LOAD command.  
--local-infile may be needed  
* ```$ mysql -u root -p --local-infile```  
```sql
mysql> CREATE DATABASE testdb;
mysql> USE testdb;
```
Create the table/define schema using options below  
A good example is in node-red  
```sql
mysql> CREATE TABLE mytable (
'no' INT NULL AUTO_INCREMENT,
'date' TIMESTAMP NOT NULL,
field1 INT NOT NULL PRIMARY KEY,
field2 VARCHAR(20),
field3 DATE,
field3 INT,
field4 FLOAT);
```
Now import the data with LOAD  
```sql 
mysql> LOAD DATA LOCAL INFILE "/path/to/data.csv" INTO TABLE testdb.mytable FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' IGNORE 1 LINES (field1, field2, @field3, field4) set field3 = NOW();
```
Description:  
column_name data_type(length) [NOT NULL] [DEFAULT value] [AUTO_INCREMENT] column_constraint;

* column_name   
* data type and optional size VARCHAR(255)   
* NOT NULL ensures the column will not contain NULL. Other options are CHECK and UNIQUE.  
* DEFAULT is default value for the column.
* AUTO_INCREMENT col is incremented by one automatically whenever new row is inserted. Each table can only have one AUTO_INCREMENT column.
* After the column list can define table constraints like UNIQUE, CHECK, PRIMARY KEY and FOREIGN KEY.

To verify  
```sql
mysql> SELECT * FROM mytable ORDER BY field1;
```
or  
```sql
mysql> SELECT * FROM mytable limit 10;
```

## SELECT   
SELECT 'columns to return' FROM a database  
- SELECT table.col1, table.col2 ..
- Can use DISTINCT after SELECT for distinct values  
- Can use ORDER BY to sort the data

```console
SELECT * FROM `esp2nred`.`data` ORDER BY date DESC LIMIT 20;
```
WHERE is a search condition used to limit the number of rows returned  
 - WHERE expression OPERATOR expression
 - WHERE humidity > 50 
 - WHERE device = 'esp32'
 - Can use wildcards
 - WHERE device LIKE '%esp%'

```console
SELECT * FROM `esp2nred`.`data` WHERE humidity > 50;
```

 JOIN is used to combine multiple tables based on a related column between them.  
 - INNER only returns rows when there is a match between the columns
 - LEFT returns all rows from left and matching records from right
 - RIGHT returns all rows from right and matching records from left
 - FULL returns all matching records from both tables whether the other table matches or not

```console
SELECT * FROM esp2nred.data INNER JOIN rpi2nred.data ON esp2nred.data.device=rpi2nred.data.device AND esp2nred.data.location=rpi2nred.data.location;
```

To update a value in a column. Example below replaces " with a space    
```console
UPDATE table SET colA =  REPLACE(colA, '\"', '');
```
# MS Access (SQL)

Access makes importing data into excel convenient. You can use PowerQuery to import sheets and transform them. You can also modify tables (add col/rows) with append/merge using the gui and drop down menus vs writing a query.

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/msaccess-query.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

