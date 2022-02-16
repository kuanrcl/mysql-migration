# Lab 3: Upload data to MySQL Database System

## Introduction
**MySQL Database System (MDS)** is a fully-managed cloud-native database service based on the MySQL Enterprise Edition. 

In this lab, we will walk you through the steps to upload the data into **MDS** using mysql shell.

Estimated lab time: 10 minutes

## Task 1: Connect to oke-operator compute instance

1. Log in to **OCI** and click on the <a href="#menu">&#9776; hamburger menu</a> at the top left corner of the OCI console, and select **Compute**, anc click on **Instances**

![compute-menu](images/compute-menu.png)

2. In the Compute>Instance page, select the right compartment under “Compartment” > “PHP-Compartment” at the right hand side. The list of instances will be showed. Then, find out the **Public IP** of the compute instance **oke-operator**, we will need this to connect to the compute instance to load the data into **MySQL Database Service**

![ccompute-ip](images/compute-ip.png)

3. Launch OCI Cloud Shell to connect to **oke-operator** compute instance
Click on the icon on the top right corner of OCI Console to launch the Cloud Shell. 
![cloud shell](images/cloud-shell.png)

OCI Cloud Shell is a web browser-based terminal accessible from the Oracle Cloud Console to access various OCI services including OCI Compute instances. 

![cloud shell launch](images/cloud-shell-launch.png)

4. Follow the steps below to download the private key for connecting to oke-operator compute instance. Please use the public IP address of oke-operator in Step 2

```
wget https://raw.githubusercontent.com/kuanrcl/mysql-migration/main/lab3/id_rsa
chmod 400 id_rsa
ssh opc@ip_address -i id_rsa
```

![cloud shell connect](images/cloud-shell-connect.png)

## Task 2: Load airportdb data into MDS

1. Download the database (airportdb) export required by the PHP application

```
cd /home/opc
```
```
wget -O airport-db.zip https://bit.ly/3pZ1PiW
```
![download-airportdb](images/download-airportdb.png)

2. Unpack the downloaded zip file

```
unzip airport-db.zip
```
```
cd airport-db
ls
```
![unzip airportdb](images/unzip-airportdb.png)

3. Next we need to connect to the provisioned MDS instance. Click on the <a href="#menu">&#9776; hamburger menu</a> at the top left corner of the OCI console, and select **Databases**, anc click on **DB Systems**
![MDS Menu](images/MDS-menu.png)

4. Select the provisioned MDS instance, **MDSInstance**
![MDS Details](images/MDS-details.png)

5. If you did not note down the private ip of **MDSInstance**
![MDS Private IP](images/MDS-IP.png)

6. Connect to MDS instance. Update password (default: Oracle#123) and host (same as MDS host private IP address). 

```
mysqlsh --user=admin --password=<password> --host=<mysql_private_ip_address> --port=3306 --js
```

7. Load data 

We will first simulate the data loading to make sure that we are able to load the data without any errors. Note the flag **dryRun** to simulate the data loadning without executing the actual data loading
```
util.loadDump("/home/opc/airport-db", {dryRun: true, threads: 8, resetProgress:true, ignoreVersion:true})
```
![Load DryRun](images/load-dryrun.png)

If the command completes without any error, we will execute the data loading by setting the **dryRun** flag to false

```
util.loadDump("/home/opc/airport-db", {dryRun: false, threads: 8, resetProgress:true, ignoreVersion:true})
```
![Load Data](images/load-data.png)

![Load Done](images/load-done.png)

It will take about 5-10 minutes to complete the data loading on our provisioned compute instance. 

## Task 3: Verify the database import
1. Execute query on MDS

Now that we have imported the airportdb data, lets verify the database

```
mysqlsh --user=admin --password=<password> --host=<mysql_private_ip_address> --port=3306 --sql
```
```
show databases;
```
```
use airportdb;
show tables;
```
![check db](images/check-db.png)

Execute the following query to find per-company average age of passengers from Germany, Spain and Greece. This query will take about 12-13s to complete

```
SELECT
airline.airlinename,
AVG(datediff(departure,birthdate)/365.25) as avg_age,
count(*) as nb_people
FROM
booking, flight, airline, passengerdetails
WHERE
booking.flight_id=flight.flight_id AND
airline.airline_id=flight.airline_id AND
booking.passenger_id=passengerdetails.passenger_id AND
country IN ('GERMANY', 'SPAIN', 'GREECE')
GROUP BY
airline.airlinename
ORDER BY
airline.airlinename, avg_age
LIMIT 10;
```

## It works

You just created uploaded airportdb data into **MDS**

## Congratulations, you are ready for the next Lab!

[Home](../README.md) | [**Go to Lab 4 >>>>>**](../lab4/README.md)

