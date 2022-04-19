# Prepare Target Oracle Database

## Introduction

Up to now we have created all of the necessary resources using Terraform in OCI. It is now time to prepare the Target Database, the Autonomous database. 

*Estimated time*: 10 minutes

### Objectives

We need to create our target tables for our GoldenGate migration and enable GGADMIN for replication to the Autonomous Database.

### Prerequisites

* This lab assumes that you completed all preceding labs.

## **Task 1**: Open SQL Developer Web 

1. Go to the top-left hamburger icon, navigate to **Oracle Database** and choose **Autonomous Transaction Processing**. It will show you all available ATP workload type databases. Click on **Target ATP** database.

	![List of database](/images/atp-0.png)

2. You will see **Database Actions**, please click on it. It would redirect you to the SQL web developer. 

	![Database Actions](/images/atp-1.png)

	**Option A**: You will see a small notification saying that **Please wait. Initializing DB Actions**. This will only take few seconds, please do not press any button until it opens the SQL web developer in a new tab.

	![Wait until opens SQL Web Developer](/images/atp-2.png)

	**Option B**: If a sign-in page opens and asks you to provide username, please enter **ADMIN** and press next. Then it will ask you to enter a password, which is in your terraform output. Go and copy and paste the value here.

	![Manual entry](/images/sql-dev-1.png)

3. In the **DEVELOPMENT** section, click on **SQL**. 

	![SQL worksheet](/images/sql-dev-5.png)

## **Task 2**: Create Target Tables

1. Let's create our target schema and tables for the migration. Please download the script **[from here](./files/create-tables.sql)**. Make sure to save these with the correct extension **.sql** not txt!

2. SQL Developer Web opens a worksheet tab, where you execute queries. Drag your downloaded **create-tables.sql** file and drop it in the worksheet area. Then run create statements.

	![Run query](/images/sql-dev-2.png)

	Script should have created **5** tables after successful execution.

## **Task 3**: Enable GGADMIN 

1. Now let's continue to unlock and change the password for Oracle GoldenGate user (ggadmin) in the Autonomous Database. GGADMIN user is disabled by default, you must enable it by running the following query.

	```
	<copy>
	ALTER USER GGADMIN IDENTIFIED BY "GG##lab12345" ACCOUNT UNLOCK;
	</copy>
	```

	![Enable GGADMIN user](/images/sql-dev-3.png)

## **Task 4**: Connect to GoldenGate Microservices for Oracle 

1. After successful terraform execution, now it is time to explore the GoldenGate servers. Let's make a console connection to GoldenGate Microservice for Oracle Database. Please copy the IP address of `GG_ORACLE_PUBLIC_IP` from the output and connect using the below:

	**`ssh -i ~/.ssh/oci opc@GG_ORACLE_PUBLIC_IP `**

2. Let's retrieve the pre-created oggadmin user credentials for Web console access, it is already created for you. Please issue the below and copy a credential value from the output.

	```
	<copy>
	cat ogg-credentials.json
	</copy>
	```

	![Output showing credential](/images/connect-oracle.png)
	
	Good practice is to keep it in your notepad. You will use it very often in the next steps.

3. Open your web browser and point to `https://GG_ORACLE_PUBLIC_IP`. Provide oggadmin in username and password which you copied, then log in.

	![Connect to Microservices](/images/connect-oracle-1.png)

## **Task 5**: Target Database Configuration

1.  We need to create this connection in the Administration Service. Please click on. Administration Service port **9011**.

	![Administration Service](/images/gg-oracle-admin.png)

2. You should be seeing the empty Extracts and Replicats dashboard. Let's add Autonomous Database credentials. Open the hamburger menu on the top-left corner, choose **Configuration**

	![Open Configuration Window](/images/add-credential-0.png)

3. It will open OGGADMIN Security and you will see we already have a connection to **HOL Target ATP** database. However, you still need to add a password here. Click on a pencil icon to **alter credentials** , then provide the password `GG##lab12345` and verify it. This is your ggadmin password, which we provided in task 3.

	![Add credential](/images/add-credential-1.png)

4. After that click on **Log in** database icon.

	![Login to Target Database](/images/add-credential-2.png)

5. Scroll down to the **Checkpoint** and click on **+** icon, then provide **_`ggadmin.chkpt`_** and click **SUBMIT**. 

	![Add Checkpoint table](/images/add-chkpt.png)

	The checkpoint table contains the data necessary for tracking the progress of capture process from source database's transactions. 

This concludes this lab. You may now **[proceed to the next lab](#next).**

## Acknowledgements

* **Author** - Bilegt Bat-Ochir - Senior Technology Solution Engineer
* **Contributors** - Tsengel Ikhbayar - GenO lift implementation
* **Last Updated By/Date** - Bilegt Bat-Ochir 08/04/2022