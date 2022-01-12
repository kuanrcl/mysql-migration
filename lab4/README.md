# Lab 4: Deploy application on Kubernetes

## Introduction

**Oracle Container Engine for Kubernetes (OKE)** is an Oracle-managed container orchestration service that can reduce the time and cost to build modern cloud native applications. Unlike most other vendors, Oracle Cloud Infrastructure provides Container Engine for Kubernetes as a free service that runs on higher-performance, lower-cost compute shapes. 

In this lab, you will deploy a PHP application on **OKE**, and connect it to **MDS**.

Estimated lab time: 15 minutes

## Task 1: Verify OKE cluster

1. Click the **Navigation Menu** in the upper left, navigate to **Developer Services** and select **Kubernetes Cluster (OKE)**

![Navigate to OKE](images/navigate_to_oke.png)

2. Select the Compartment (e.g. PHP-Compartment) that you provisioned the OKE cluster, and verify that the status of OKE cluster 'oke_cluster' is Active

![Locate OKE](images/locate_oke_instance.png)

3. Click 'oke_cluster' to view the status of the OKE cluster and the worker nodes in your OKE cluster. You will deploy a PHP application to this OKE cluster soon.

![Verify OKE](images/oke_worker_nodes.png)

## Task 2: Connect to **oke-operator** compute instance

1. Connect to the **oke-operator** compute instance again using OCI Cloud Shell as in [Lab 3](../lab3/README.md)

## Task 3: Deploy Application to OKE

1. Download yaml deployment file [mds_connection.yaml](mds_connection.yaml) to the operator VM.

```
wget https://raw.githubusercontent.com/kuanrcl/mysql-migration/main/lab4/mds_connection.yaml
```

2. Using vi or any editor to update the two parameters in **mds_connection.yaml**. Update **“mds-host”** with the private IP address of your MDS instance, and **“mds-password”** with the password you gave when provisioning MDS in Resource Manager.

![Update MDS Connection](images/mds_connection.png)

3. Create Kubernetes configmap and secret to store MDS connection metadata.
```
kubectl apply -f  mds_connection.yaml
```
![Apply MDS Connection](images/apply_mds_connection.png)

4. Download yaml deployment file [deploy_webapp.yaml](deploy_webapp.yaml).

```
wget https://raw.githubusercontent.com/kuanrcl/mysql-migration/main/lab4/deploy_webapp.yaml
```

5. Deploy the PHP application into OKE.
```
kubectl apply -f deploy_webapp.yaml
```
![Apply WebApp](images/apply_webapp.png)

5. Check the status of pods and wait until all pods are up and running
```
kubectl -n cloudnative-webapp-airportdb-mds get pod
```
![Get Pod](images/get_pod.png)

6. Get the external IP address of your load balancer. Wait 30 seconds if the external IP address is not ready.
```
kubectl -n cloudnative-webapp-airportdb-mds get service --watch
```
Once you have the External IP provisioned, you can execute CTL+C to kill the command

![Get Service](images/get_service.png)

## Task 4: Access the Application 

1. Open a browser and access your PHP application using the external IP address. (e.g. http://xxx.xxx.xxx.xxx:5000/index.php). You will get a page to submit SQL statement against MDS.


![Access App](images/access_app.png)

2. Submit a SQL statement to verify that PHP application connects well with MDS.
```
select count(*) from booking;
```
 
3. You should get the query result and the execution time.

![Query Result](images/query_result.png)


## It works

You just deployed and tested applications on Kubernetes

## Congratulations, you are ready for the next Lab!

[Home](../README.md) | [**Go to Lab 5 >>>>>**](../lab5/README.md)
