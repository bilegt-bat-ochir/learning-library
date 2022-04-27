# Sign up for an APEX Workspace

## Introduction

Oracle Application Express (APEX) is a low-code application platform for Oracle Database. APEX Application Development, Autonomous Data Warehouse (ADW), and Autonomous Transaction Processing (ATP) are fully managed services, pre-integrated and pre-configured with APEX, for rapidly building and deploying modern data-driven applications in Oracle Cloud. Business users, citizen, and application developers can create enterprise apps 20X faster with 100X less code — without having to learn complex web technologies with just a browser. To start, you will need to decide on the service you are going to use for this workshop, and then create an APEX workspace accordingly.

If you already have an APEX 21.1 Workspace provisioned, you can skip this lab.

Estimated Time: 5 minutes

Watch the video below for a quick walk through of the lab.

[](youtube:RcSCnZnDzDE)

### What is an APEX Workspace?
An APEX Workspace is a logical domain where you define APEX applications. Each workspace is associated with one or more database schemas (database users) which are used to store the database objects, such as tables, views, packages, and more. APEX applications are built on top of these database objects.

### How Do I Find My APEX Release Version?
To determine which release of Oracle Application Express you are currently running, do one of the following:
* View the release number on the Workspace home page:
    - Sign in to Oracle Application Express. The Workspace home page appears. The current release version displays in bottom right corner.

    ![](images/release-number.png " ")
    ![](images/release-number2.png " ")

* View the About Application Express page:
    - Sign in to Oracle Application Express. The Workspace home page appears.
    - Click the Help menu at the top of the page and select About. The About Application Express page appears.

  ![](images/version.png)

### Where to Run the Lab
You can run this lab in any Oracle Database with APEX 21.1 installed. This includes the new APEX Application Development Service, the Oracle Autonomous Database, the free, "Development Only" apex.oracle.com service, your on-premises Oracle Database (providing APEX 21.1 is installed), on a third party cloud provider where APEX 21.1 is installed, or even on your laptop by installing Oracle XE or Oracle VirtualBox App Dev VM and installing APEX 21.1.

Below are steps on how to sign up for either an *APEX Application Development* Service, an *Oracle Autonomous Database* cloud service or *apex.oracle.com* service.
- The APEX Application Development Service is great if you would like to go with a flexible paid option that allows to concentrate your efforts on APEX development without worrying about the database management. It provides 1 OCPU and 1 TB and can be extended as needed.
- The Oracle Autonomous Database option is ideal for learning about the Oracle Database and APEX, and provides 1 OCPU and 20 GB of compressed storage. This service can also be utilized for production applications.
- On the other hand, apex.oracle.com is also a free service; however, it is only designated for development purposes, and running production apps is not allowed. For conducting labs in this workshop, either service can be utilized.

Click one of the options below to proceed.


## **Option 1**: Oracle Autonomous Database

In this part, you will create an *Autonomous Transaction Processing* database and provision Oracle APEX.


1. From within your Oracle Cloud environment, you will create an instance of the Autonomous Transaction Processing database service.

    From the Cloud Dashboard, select the navigation menu icon in the upper left-hand corner and then select **Autonomous Transaction Processing**.

    ![](https://raw.githubusercontent.com/oracle/learning-library/master/common/images/console/database-atp.png " ")

2. Click **Create Autonomous Database**.

    ![](images/click-create-autonomous-database.png " ")

3. Enter **```SecretPassw0rd```** for the ADMIN password, then click **Create Autonomous Database**.

    ![](images/atp-settings-1.png " ")
    ![](images/atp-settings-2.png " ")
    ![](images/atp-settings-3.png " ")

4. After clicking **Create Autonomous Database**, you will be redirected to the Autonomous Database Details page for the new instance.

    Continue when the status changes from:

    ![](images/status-provisioning.png " ")

    to:

    ![](images/status-available.png " ")

5. Within your new database, APEX is not yet configured. Therefore, when you first access APEX, you will need to log in as an APEX Instance Administrator to create a workspace.

    Click the **Tools** tab.
    Click **Open APEX**.

    ![](images/click-apex.png " ")


6. Enter the password for the Administration Services and click **Sign In to Administration**.     
    The password is the same as the one entered for the ADMIN user when creating the ATP instance: **```SecretPassw0rd```**

    ![](images/log-in-as-admin.png " ")

7. Click **Create Workspace**.

    ![](images/welcome-create-workspace.png " ")

8. In the Create Workspace dialog, enter the following:

    | Property | Value |
    | --- | --- |
    | Database User | DEMO |
    | Password | **`SecretPassw0rd`** |
    | Workspace Name | DEMO |

    Click **Create Workspace**.

    ![](images/create-workspace.png " ")

9. In the APEX Instance Administration page, click the **DEMO** link in the success message.         
    *Note: This will log you out of APEX Administration so that you can log into your new workspace.*

    ![](images/log-out-from-admin.png " ")

10. On the APEX Workspace log in page, enter **``SecretPassw0rd``** for the password, check the **Remember workspace and username** checkbox, and then click **Sign In**.

    ![](images/log-in-to-workspace.png " ")

## **Option 2**: apex.oracle.com
Signing up for apex.oracle.com is simply a matter of providing details on the workspace you wish to create and then waiting for the approval email.

1. Go to [https://apex.oracle.com](https://apex.oracle.com.).
2. Click **Get Started for Free**.

    ![](images/get-started.png " ")

3. Scroll down until you see details for apex.oracle.com.  Click **Request a Free Workspace**.

    ![](images/request-workspace.png " ")

4. On the Request a Workspace dialog, enter your Identification details – First Name, Last Name, Email, Workspace.
   *Note: For workspace, enter a unique name, such as first initial and last name.*

    Click **Next**.

    ![](images/request-a-workspace.png " ")

5. Complete the remaining wizard steps.

6. Check your email. You should get an email from `oracle-application-express_ww@oracle.com` within a few minutes.  
   *Note: If you don’t get an email go back to Step 3 and make sure to enter your email correctly.*

    Within the email body, click **Create Workspace**.

    ![](images/create-aoc-workspace.png " ")

7. Click **Continue to Sign In Screen**.
8. Enter your password, and click **Apply Changes**.
9. You should now be in the APEX Builder.

    ![](images/apex-builder.png " ")


## **Summary**

At this point, you know how to create an APEX Workspace and you are ready to start building amazing apps, fast.

You may now **proceed to the next lab**.

## **Acknowledgements**

 - **Author** -  Salim Hlayel, Principal Product Manager
 - **Contributors** - Arabella Yao, Product Manager Intern, Database Management | Jaden McElvey, Technical Lead - Oracle LiveLabs Intern
 - **Last Updated By/Date** - Kamryn Vinson, Database Product Manager, April 2021
