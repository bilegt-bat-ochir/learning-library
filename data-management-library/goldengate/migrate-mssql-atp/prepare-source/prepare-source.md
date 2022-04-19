# Prepare Source SQL Server

## Introduction

Up to now we have created all of the necessary resources using Terraform in OCI and created the target schema in the Autonomous database. You now need to prepare your own source database. This lab doesn't show you to install a source database, however, we will show you to prepare your source MSSQL database for GoldenGate Microservices.

*Estimated time*: 10 minutes

### Objectives

Learn about the requirements and necessary steps for preparing a MSSQL Server databases.
Learn how to enable supplemental logging in the source database tables that are to be used for capture by the Extract for SQL Server and how to purge older CDC staging data. 

### Prerequisites
 - This lab assumes that you completed all preceding labs.
 - Required to be a database user who is a member of the SQL Server System Administrators (sysadmin) role. Membership in the server's [local Administrators Group](https://docs.oracle.com/en/middleware/goldengate/core/21.1/gghdb/sql-server-preparing-system-oracle-goldengate.html#GUID-5EA5A276-304F-4368-8C20-511DAF166FD6).
 - Having a thorough understanding of [what's supported for SQL Server](https://docs.oracle.com/en/middleware/goldengate/core/21.1/gghdb/understanding-whats-supported-sql-server.html#GUID-1AB22DD3-A55E-424D-A201-DEBC40C21689)
 - Comply with supported [data types](https://docs.oracle.com/en/middleware/goldengate/core/21.1/gghdb/understanding-whats-supported-sql-server.html#GUID-02DDD6EF-F546-4543-B385-C054A85D264C).
 - **Your source database** either have Enterprise edition of versions 2014, 2016, 2017, and 2019, or Standard editions of versions 2016 with Service Pack1 (or above), 2017, and 2019, to configure an extract process. If you are using SQL Server 2014, 2016, or 2017 as a source database, Oracle highly recommends that you apply the latest Service Pack or Cumulative Update includes KB3030352, KB3166120, and KB4073684.
 - The SQL Server server name (@@SERVERNAME) must not be NULL. 
 - Only user databases are supported for capture process. The following schemas or objects are not be automatically replicated by Oracle GoldenGate:"sys", "cdc", "INFORMATION_SCHEMA" and "guest", unless they are explicitly specified without a wildcard.
 - SQL Server Agent must be running on your source SQL Server instance and the SQL Server CDC job must be running against the database. If SQL Server Transactional Replication is also enabled for the database, then the SQL Server Log Reader Agent must be running.
 - Oracle GoldenGate supports remote capture and delivery for Azure SQL database managed instance and remote delivery for Azure SQL Database.
 - Oracle GoldenGate supports remote capture and delivery for Amazon RDS for SQL Server. But you must have the [appropriate permission](https://docs.oracle.com/en/middleware/goldengate/core/21.1/gghdb/sql-server-preparing-system-oracle-goldengate.html#GUID-94980576-7376-4E88-ADC8-A23E6ABC9AC7).


## **Task 1**: Connect to GoldenGate Microservices MSSQL 

1. After successful terraform execution, now it is time to explore your GoldenGate servers. Let's make a console connection to GoldenGate MSSQL Microservice server. Please copy the IP address of `GG_MSSQL_PUBLIC_IP` from the output and connect using the below:

	**`ssh opc@GG_MSSQL_PUBLIC_IP -i ~/.ssh/oci`**

2. Let's retrieve the pre-created oggadmin user credentials for Web console access, it is already created for you. Please issue the below and copy a credential value from the output.

	```
	<copy>
	cat ogg-credentials.json
	</copy>
	```

	![Output showing credential](/images/connect-mssql.png)
	
	Good practice is to keep it in your notepad. You will use it very often in the next steps.

3. Open your web browser and point to `https://GG_MSSQL_PUBLIC_IP`. Provide oggadmin in username and password which you copied, then log in.

	![Output showing credential](/images/connect-mssql-1.png)

	> **Note:** After provisioning a GoldenGate Microservices for MSSQL, the administration server on port 9011 has a status of **Abended**.

	![Microservices web console](/images/deployment-mssql.png)

	It will not start until you install ODBC drivers in the next task.

## **Task 2**: Install ODBC drivers for Linux 

1. ODBC is the primary native data access API for applications written in C and C++ for SQL Server. The Microsoft ODBC Drivers for Linux are required to connect to a remote source or target SQL Server database. The following tasks are required to install the Linux drivers. Please check the [official document](https://docs.oracle.com/en/middleware/goldengate/core/21.1/installing/installing-sql-server1.html#GUID-E815278E-8F9A-4B74-95CB-F968DC56144E) for your reference.
	First of all we need edit the /etc/passwd file, to grant a temporary shell access to the root user.

	```
	<copy>
	sudo vi /etc/passwd
	</copy>
	```
	
2. We need to modify the value for the root user from **/usr/sbin/nologin** to **/bin/bash**. 

	> **Note:** To edit /etc/passwd file, you have to press **i** key to enable editing. When you are done editing press **esc** key and press **:wq** keys, then hit enter for save and close the file.

3. To install Microsoft ODBC driver 17 in the workshop environment, perform the following steps with default values by answering 'y' when prompted.

	```
	<copy>
	sudo su

	curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

	exit
	</copy>
	```
	```
	<copy>
	sudo yum remove unixODBC-utf16 unixODBC-utf16-devel -y
	</copy>
	```
	```
	<copy>
	sudo ACCEPT_EULA=Y yum install msodbcsql17
	</copy>
	```
	```
	<copy>
	sudo ACCEPT_EULA=Y yum install mssql-tools
	</copy>
	```
	```
	<copy>
	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
	source ~/.bashrc
	</copy>
	```
	
	![Install the ODBC driver](/images/odbc.gif)

4. After installing the Linux drivers, you can set the original shell access values back for the root user.	

	```
	<copy>
	sudo vi /etc/passwd
	</copy>
	```

## **Task 3**: Configuring a Database Connection on Linux 
1. Oracle GoldenGate for SQL Server database configuration provides the same support for Linux and Windows. However, you need the msodbcsql13* or msodbcsql17* drivers to set up the connections in a Linux environment. Let's create an ODBC data source in a Linux environment.
	```
	<copy>
	vi odbc_template_file.ini
	</copy>
	```

2. Add the below line in the template file and modify **your source database ip address**.
	```
	<copy>
	[mssqlserver_war]            
	Driver = ODBC Driver 17 for SQL Server            
	Server = my_source_db_ip_address,1433            
	Database = warrior           
	</copy>
	```

3. Install the data source using the command:

	```
	<copy>
	odbcinst -i -s -f odbc_template_file.ini
	</copy>
	```
	We will use this **mssqlserver_war** as a data source to create a connection in Oracle GoldenGate.

## **Task 4**: Create a GoldenGate user and a schema at your source

1. Login to your source database and create a GoldenGate user with the correct permission on a database.
	```
	<copy>
	CREATE LOGIN [oggsourceuser] WITH PASSWORD=N'MyC0mpl3xPassword';
	ALTER SERVER ROLE [sysadmin] ADD MEMBER [oggsourceuser];
	USE [Warrior];
	</copy>
	```

	![Create a user at source](/images/create-user.png)

	I have created **oggsourceuser** at my source SQL server, and granted sysadmin permission on my **Warrior** database. 

2. You also have to have a schema in the source database that will be used for Oracle GoldenGate objects. If you run the below code, this means every supporting objects for Oracle GoldenGate will be created in this oggadmin schema. This is only a recommendation.

	```
	<copy>
	CREATE SCHEMA [oggadmin];
	</copy>
	```

## **Task 5**: Start GG_MSSQL Administration Service

1. After successful installation of ODBC driver, now it is time to start the **Abended** service. Scroll down to the **Deployments** area. Click on **Action** button, then choose **Start** to start the abended services.

	![Start the deployment](/images/gg-mssql-start.png)

	Confirm your action by clicking **OK**. This will now start the Administration service in your GoldenGate deployment.

	![Start the deployment video](/images/start-gg-mssql.gif)

2. After you have installed the SQL Server client drivers, the next step is to create connection data sources to your source database. We need to create this connection in the Administration Service. Please click on. Administration Service port **9011**.

	![Administration Service](/images/gg-mssql-admin.png)

## **Task 6**: Create a data source in Administration Service

1. After you launched the Administration Services interface, please use same **oggadmin** credential to log in.

2. Click the left-top hamburger icon, navigate to **Configuration**.

	![Configuration](/images/add-credential-0.png)

3. Cick the **+** sign next to Credentials, and set up your new credential using the below inputs. 

	|Parameter Name |Value|
	|----------------|----------------------------|
	|Credential Domain | GoldenGate |
	|Credential Alias | mssqlserver |
	|Data Source Name (DSN): | In task 3, we defined **mssqlserver_war** in `odbc_template_file.ini`|
	|User ID:| In task 4, we created a user **oggsourceuser** |
	|Password:| Enter your user password, MyC0mpl3xPassword |

	![Configuration](/images/add-credential-1.png)

4. Click the Login icon to verify that the new alias can correctly log in to the database. If an error occurs, then click the Alter Credential icon to correct the credential information, and then test the log in.

## **Task 7**: Change the default GGSCHEMA value

1. Oracle GoldenGate needs to create necessary objects in the source SQL Server for extract process. Microservices deployments already has a default GGSCHEMA oggadmin parameter. It means GoldenGate will try to create database objects in oggadmin schema. If you don't change this, many operations will fail. Therefore, you must create an **oggadmin** schema in your source database, or must change the default value in GLOBALS file. We will change this default parameter in this lab, go to **Parameter Files** tab in the administration server configuration window. 

	![Parameter files](/images/add-ggschema-0.png)

2. Select **GLOBALS** file from the list, you will see current content of the parameter file. Click on the edit icon. Modify it according to your database information. In my case it is `ggschema parking` and submit to save.

	![Parameter files](/images/add-ggschema-2.png)

## **Task 8**: Enable CDC Supplemental logging

1. You need to enable supplemental logging at SQL server, it is called adding trandata. Switch back to **Database** tab, and connect to your SQL server.

	![Connect to SQL server](/images/add-trandata-0.png)

2. Find **TRANDATA Information** and click on the **+** icon. Enter your schema.table according to your case. I will add **_`parking.*`_** in the **Table Name**, then click **Submit** to save. This will enable trandata for all objects in the Parking schema. You can also selectively enable supplemental logging by specifying the actual names. This step must be done for all tables that are there to be captured by Oracle GoldenGate. 

	![Connect to SQL server](/images/add-trandata-1.png)

	> **Note:**  The database user that enables TRANDATA must have **sysadmin** rights.

3. You will see the **Successfully added Trandata!** notification when you click on a bell icon located at the left-top corner. You can verify it by your choice, enter **`Parking.*`** in the search field and click on the search icon.

	![Connect to SQL server](/images/add-trandata-2.png)

	The result will verify that you have prepared your tables for trandata instantiation in a schema. These are necessary steps for the source database.

## **Task 9**: Creating the Oracle GoldenGate CDC Cleanup Job for SQL Server

1. After you have enabled TRANDATA for SQL Server, the next step is to disable the default SQL Server CDC Cleanup job and install the Oracle GoldenGate CDC Cleanup job. In the Management Studio, within a query window for the source database. You must manually drop the SQL Server CDC cleanup job for the database because it may cause data loss for the Extract.  

	```
	EXECUTE sys.sp_cdc_drop_job 'cleanup';
	```

2. To create a cleanup job run the `ogg_cdc_cleanup_setup.sh` file, which is located in the /u02/deployments/GG_MSSQL/etc/conf/ogg directory.

	```
	cd /u02/deployments/GG_MSSQL/etc/conf/ogg
	./ogg_cdc_cleanup_setup.sh createJob oggsourceuser MyC0mpl3xPassword warrior source_db_ip,1433 parking
	```
	
This concludes this lab. You may now **[proceed to the next lab](#next).**

## Acknowledgements

* **Author** - Bilegt Bat-Ochir - Senior Technology Solution Engineer
* **Contributors** - Tsengel Ikhbayar - GenO lift implementation
* **Last Updated By/Date** - Bilegt Bat-Ochir 08/04/2022