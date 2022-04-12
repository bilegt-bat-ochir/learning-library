# Welcome to Migrate SQL database to Oracle Database workshop

## Introduction

In this workshop, we will migrate a sample a SQL database to an Autonomous Database in Oracle Cloud Infrastructure using the new GoldenGate Microservices 21.5 for SQL. The purpose of this workshop is to show the simple and yet efficient way to migrate a database to Oracle Cloud Infrastructure. This workshop consists of four labs. 

*Estimated total Workshop Time*: 1.5 hours

### About Oracle GoldenGate Microservices

Oracle Cloud Infrastructure GoldenGate is a managed service providing a real-time data mesh platform, which uses replication to keep data highly available, and enabling real-time analysis. You can design, execute and monitor their data replication and stream data processing solutions without the need to allocate or manage compute environments, it is a fully managed service. Today we will explore its capabilities and migrate our source database to target.

### About Autonomous Database

This is self-driving, converged, multimodel database and machine learning-based automation takes care of its operational lifecycle management. Auto-provisioning and auto-tuning to simplify the creation and optimization of all data stores in the cloud. You can start with the lowest cost and commitment, and autoscale as the business grows. Today we will use this 19c database as our target database.

### About GoldenGate Microservices

Oracle GoldenGate Microservices Architecture allows this premier replication tool to scale out to the cloud and provide a secure, flexible and scalable replication platform. It has an easy interface to manage both extracts and replicate processes from on-premises to cloud, also cloud to cloud. We will use Microservices to apply our extracted data in the target Autonomous Database.

### About Terraform 

Terraform is an open-source tool that allows you to manage programmatically, version, and persist infrastructure through the "infrastructure-as-code" model.
The Oracle Cloud Infrastructure (OCI) Terraform provider is a component that connects Terraform to the OCI services that you want to manage. We will use it as our infrastructure orchestration to deploy all necessary resources.

### Objectives

In this workshop you will :
* Explore Cloud-Shell, web-based terminal
* Benefit from OCI terraform provider
* Explore OCI compute service
* Understand the migration flow of GoldenGate
* Explore Autonomous Database and its capabilities

**Architecture Overview**

- Virtual Cloud Network: we will create a VCN with a public sub-network and internet access to avoid complexity.
- Source SQL Server: we will work on a source database. We need to configure and prepare it for GoldenGate capture process.
- Target Autonomous database: we will provision Oracle Autonomous Database to act as our target database.
- GoldenGate for non-Oracle deployment: we will create a GoldenGate Microservices for SQL Server to extract data from the source and ships trail files to the target.
- GoldenGate Microservices deployment: we will create a Microservices environment for an Autonomous Database that applies trails from source to target autonomous database.

	![Architecture Diagram](/images/architecture.png)

All of the above resources are going to be deployed in Oracle Cloud Infrastructure using Terraform. It is not necessary to have prior knowledge of Terraform scripting. All you need to do is follow every step exactly as described.

### Prerequisites

* Make sure you have OCI account, please proceed to Task 1.

## **Task 1**: Create your OCI account

* The following workshop requires an Oracle Public Cloud Account that will either be supplied by your instructor or can be obtained through **Getting Started** steps.
* A Cloud tenancy where you have the available quotas to provision what mentioned in Architecture Overview.
* Oracle Cloud Infrastructure supports the following browsers and versions: Google Chrome 69 or later, Safari 12.1 or later, Firefox 62 or later.

> **Note:** If you have a **Free Trial** account, when your Free Trial expires your account will be converted to an **Always Free** account. You will not be able to conduct Free Tier workshops unless the Always Free environment is available. **[Click here for the Free Tier FAQ page.](https://www.oracle.com/cloud/free/faq.html)**

This concludes the introduction. You may now **[proceed to next step](#next).**

## Learn More

* [Terraform OCI](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/terraform.htm)
* [Oracle GoldenGate](https://docs.oracle.com/en/middleware/goldengate/core/19.1/oggmp/using-oracle-goldengate-microservices-oracle-cloud-marketplace.html)
* [Oracle Autonomous Database](https://docs.oracle.com/solutions/?q=autonomous&cType=reference-architectures&sort=date-desc&lang=en)

## Acknowledgements

* **Author** - Bilegt Bat-Ochir - Senior Technology Solution Engineer
* **Contributors** - Tsengel Ikhbayar - GenO lift implementation
* **Last Updated By/Date** - Bilegt Bat-Ochir 08/04/2022