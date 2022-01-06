# Lab 4: Deploy application on Kubernetes

## Introduction

**Oracle Container Engine for Kubernetes (OKE)** is an Oracle-managed container orchestration service that can reduce the time and cost to build modern cloud native applications. Unlike most other vendors, Oracle Cloud Infrastructure provides Container Engine for Kubernetes as a free service that runs on higher-performance, lower-cost compute shapes. 

In this lab, you will deploy a PHP application on **OKE**, and connect it with **MDS**.

Estimated lab time: 10 minutes

## Task 1: Connect to Bastion Host

1. Connect to the Bastion host again using your SSH client.

## Task 2: Deploy Application to OKE

1. Create yaml deployment file [mds_connection.yaml](mds_connection.yaml) and update "mds-host" with the private IP address of your MDS instance, and "mds-password" with the password you gave when provisioning MDS in Resource Manager.

2. Create Kubernetes configmap and secret to store MDS connection metadata.
```
kubectl apply -f  mds_connection.yaml
```
![Apply MDS Connection](images/apply_mds_connection.png)

3. Create yaml deployment file [deploy_webapp.yaml](deploy_webapp.yaml).


4. Deploy the PHP application into OKE.
```
kubectl apply -f  deploy_webapp.yaml
```
![Apply WebApp](images/apply_webapp.png)

5. Check the status of pods and wait until all pods are up and running
```
kubectl -n cloudnative-webapp-airportdb-mds get pod
```
![Get Pod](images/get_pod.png)

6. Get the external IP address of your load balancer. Wait 30 seconds if the external IP address is not ready.
```
kubectl -n cloudnative-webapp-airportdb-mds get service
```
![Get Service](images/get_service.png)

## Task 3: Access the Application 

1. Open a browser and access your PHP application using the external IP address. (e.g. http://xxx.xxx.xxx.xxx:5000/index.php). You will get a page to submit SQL statement against MDS.


![Access App](images/access_app.png)

2. Submit a SQL statement to verify that PHP application connects well with MDS.
```
select count(*) from bookings;
```
 
3. You should get the query result and the execution time.

![Query Result](images/query_result.png)


## It works

You just deployed and tested applications on Kubernetes

## Congratulations, you are ready for the next Lab!

[Home](../README.md) | [**Go to Lab 5 >>>>>**](../lab5/README.md)
