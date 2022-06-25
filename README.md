# Hadoop HDFS & Hive Simulation

A simple simulation for Hadoop HDFS and Hive using Docker. Credits to Hrishi Shirodkar for this awesome simulation (Ref: [Medium Article](https://hshirodkar.medium.com/apache-hive-on-docker-4d7280ac6f8e)).

## Prerequisites

1. Install Docker. Follow official documentation according to your Operating System: https://docs.docker.com/engine/install/

2. Install Docker Compose. Follow official documentation to your Operating System: https://docs.docker.com/compose/install/

Tips for Linux OS, you can follow these simple steps to install Docker Compose after installing Docker: https://stackoverflow.com/a/36689427/7708925

3. Install Git. You can use typical git executable or any git client as long as you can clone this repository. You can follow this official documentation: https://git-scm.com/downloads

## Overview

![Alt text](asset/hadoop-stack-case-lab-case.drawio.png?raw=true "Lab Overview")

## Steps

1. Clone this repository and move to the cloned repository folder.

For example, in typical Linux OS or bash shell, you can execute the following command:
```
git clone https://github.com/sagaekakristi/hadoop_simple_simulation
cd hadoop_simple_simulation
```

If by any reason you cannot install git on your machine, you can download this repository using download feature provided by Github.

2. Run HDFS and Hive using the following Docker Compose command:
```
docker-compose up
```
You will see logs that indicate Hive metastore and Hive server are running when no problem occured.

Let this terminal window running.

3. Open new terminal window. Make sure we are still in `hadoop_simple_simulation` folder. Run the following command to run a shell inside the docker container in which we can run Hive and HDFS commands:
```
docker exec -it hive-server /bin/bash
```

4. Still inside the docker shell, create Hive database and empty table by running the following commands:
```
cd /employee

hive -f employee_table.hql
```

The command above will submit the query inside `employee/employee_table.hql` file. The employee_table.hql contain `create database ...` and `create external table ...` expression. It is strongly recommended to check the file content to learn more in detail about the query, table schema, and properties in Hive.

You will receive `OK` logs when no problem occured.

5. Upload the CSV data (employee.csv) to HDFS by running the following commands:
```
hadoop fs -put employee.csv hdfs://namenode:8020/user/hive/warehouse/testdb.db/employee
```

6. Open a Hive shell by running the following command:
```
hive
```

7. Still inside Hive shell, check that database and table we created previously are exist.
```
hive> show databases;
OK
default
testdb

...

hive> use testdb;
OK

...
```

8. Still inside Hive shell, try execute SQL query to the see the data inside the Hive tables.
```
hive> select * from employee;
OK
1 Rudolf Bardin 30 cashier 100 New York 40000 5
2 Rob Trask 22 driver 100 New York 50000 4
3 Madie Nakamura 20 janitor 100 New York 30000 4
4 Alesha Huntley 40 cashier 101 Los Angeles 40000 10
5 Iva Moose 50 cashier 102 Phoenix 50000 20
```

9. Still inside Hive shell, try execute SQL query to get the maximum age of all employees.
```
hive> select max(age) from employee;
...
...
MapReduce Jobs Launched: 
Stage-Stage-1:  HDFS Read: 944 HDFS Write: 0 SUCCESS
Total MapReduce CPU Time Spent: 0 msec
OK
50
Time taken: 1.799 seconds, Fetched: 1 row(s)
```
We can see that the maximum age in our employees dataset is 50.

10. You can close the Hive shell by pressing `Ctrl` + `d` . Press `Ctrl` + `d` again to close the Docker shell as well.

11. You can also stop the docker-compose in previous terminal window by pressing `Ctrl` + `c`

12. To clean the docker containers completely, run the following command:
```
docker-compose down
```
