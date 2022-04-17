
# Provisioning an Autonomous VM Cluster on Exadata Cloud@Customer

## Introduction
Oracle Autonomous Database on Oracle Exadata Cloud@Customer provides the benefits of a self-driving, self-securing, and self-repairing database management system, bringing it closer to your applications on-premises while deployed securely behind your firewall. Setting up and using Autonomous databases on your Exadata Cloud@Customer consists of two steps:

1. **Provision an Exadata Infrastructure**

    This step is common regardless whether you chose to deploy the Autonomous service using database 19c and above or Exadata Database service using database 11g and above. To provision an Oracle Exadata Cloud@Customer system, you must work with Oracle to set up and configure the system. **This step was completed in the previous lab**.

2. **Provision an Autonomous VM Cluster on your Exadata Infrastructure**

    The type of VM Cluster you deploy on your Exadata Infrastrucure determines if its Autonomous  or Exadata Database service. Once your Exadata Infrastruture is deployed and active, you may then create an Autonomous VM Cluster which runs your Autonomous Container Databases.

**This lab provides steps to set up an Autonomous VM Cluster on your on-premises Exadata Cloud@Customer**.

### Objectives

- Create an Autonomous VM Cluster on a pre-provisioned Exadata Cloud@Customer infrastructure.


### Required Artifacts
- An Oracle Cloud Infrastructure account with a pre-provisioned instance of Exadata Infrastructure

Watch the video below for step by step directions on creating an Autonomous VM Cluster on your Exadata Infrastructure.

[](youtube:MDe9y3spjdI)

## Task 1: Create an Autonomous VM Cluster on your Exadata Cloud@Customer infrastructure

*Log in to your OCI account as a fleet administrator.*

- Navigate to the **Exadata Cloud@Customer** option in the top left hamburger menu from your OCI home screen.

    ![This image shows the result of performing the above step.](./images/create_EI1.png " ")

- Select **Autonomous Exadata VM Clusters** from the menu on the left and click **Create VM Cluster**.

    ![This image shows the result of performing the above step.](./images/create_VMC1.png " ")

- Choose a compartment to deploy the VM Cluster and provide a display name.

- In the dropdown titled **Select Exadata Cloud@Customer infrastructure in CompartmentName**, pick the **Exadata Infrastructure**. Change the compartment if your Exadata Infrastructure was created in a different compartment than shown in the title.

- **VM Cluster Network:** Select a VM Cluster Network. Once again, ensure you change the compartment to where your VM Cluster is deployed.

- **Configure the Exadata Storage:** Optionally, you can Allocate Storage for Local Backups.

- Choose the license type you wish to use.
    **Bring your own license:** If you choose this option, make sure you have proper entitlements to use for new service instances that you create.
    **License included:** With this choice, the cost of the cloud service includes a license for the Database service.

- In the advanced options, you may pick a different timezone than the default UTC.

- Click **Create Autonomous Exadata VM Cluster**.

Once created, your Autonomous Exadata VM Cluster is ready to deploy Autonomous Container Databases.

*All Done! You have successfully setup your Autonomous VM Cluster on Exadata Cloud @ Customer environment. It is now ready to deploy Autonomous Container Databases*

## Acknowledgements

- **Author** - Simon Law & Kris Bhanushali
- **Last Updated By/Date** - Kris Bhanushali, March 2022

## See an issue or have feedback?  
Please submit feedback [here](https://apexapps.oracle.com/pls/apex/f?p=133:1:::::P1_FEEDBACK:1).   Select 'Autonomous DB on Dedicated Exadata' as workshop name, include Lab name and issue / feedback details. Thank you!
