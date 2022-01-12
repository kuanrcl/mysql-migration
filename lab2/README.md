# Lab 2: Provision OCI resources

## Introduction

Resource Manager is an Oracle Cloud Infrastructure service that allows you to automate the process of provisioning your Oracle Cloud Infrastructure resources. Using Terraform, Resource Manager helps you install, configure, and manage resources through the "infrastructure-as-code" model.

In this lab, you will provision the following OCI resources using Resource Manager:
  - Virtual Cloud Network with related network resources and policies
  - Oracle Container Engine for Kubernetes and a node pool with 2 worker nodes
  - MySQL Database System
  - Operator Virtual Machine with kubectl and MySQL client tools installed

Estimated lab time: 30 minutes

### Task 1: Create Stack in Resource Manager

1. Visit the [Terraform scripts](https://github.com/rayeswong/terraform-oke-mds) in a browser, and click the image **"Deploy to Oracle Cloud"** at the bottom of the page. It would redirect you to OCI console to create a new stack in Resource Manager.

![Deploy to Oracle Cloud](https://oci-resourcemanager-plugin.plugins.oci.oraclecloud.com/latest/deploy-to-oracle-cloud.svg)

2. Sign in to **Oracle Cloud** if you haven't yet. In the page of 'Create Stack', check to accept terms, give a name to your stack (e.g. "MySQL on OKE Lab"), select the compartment (e.g. PHP-Compartment) to provision OCI resources, and click **Next**.

![Create Stack 1](images/resource_manager_create_stack_1.png)

3. Review the values pre-populated for your OCI Resources, update the password for "MDS Admin User's Password" (default password: Oracle#123), and click **Next**.

![Create Stack 2](images/resource_manager_create_stack_2.png)

4. Check **Run Apply** and click **Create** to create the stack and apply the Terraform scripts.


![Create Stack 3](images/resource_manager_create_stack_3.png)

5. A job will be created to apply your Terraform scripts to provision OCI resources.


![Create Stack 4](images/resource_manager_create_stack_4.png)

6. It takes about **20 minutes** to complete this job. At the meanwhile, you can click on your Terraform job to view logs of progress of your job.

![Create Stack 5](images/resource_manager_create_stack_5.png)

![Create Stack 6](images/resource_manager_create_stack_6.png)

7. Once your job has been executed successfully, you can find the public IP address of your operator VM, and the private IP address of the MySQL Database from the outputs. **PLEASE** note down these two IP addresses that will be used in the subsequent labs.

![Create Stack 7](images/resource_manager_create_stack_7.png)

## It works

You just provisioned all OCI resources needed for this lab.

## Congratulations, you are ready for the next Lab!

[Home](../README.md) | [**Go to Lab 3 >>>>>**](../lab3/README.md)

