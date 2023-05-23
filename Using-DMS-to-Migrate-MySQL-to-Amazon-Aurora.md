# Using DMS to Migrate MySQL to Amazon Aurora
 
## Introduction
 
In this lab, we will be migrating a MySQL database to an Aurora database using the Database Migration Service (DMS). We will create the necessary databases, set up the proper source and target for DMS, and then run a transfer task between the databases. This is an important and powerful tool for someone looking to migrate databases to or within AWS.  
 
## Solution
 
Log in to the AWS Management Console using the credentials provided on the lab instructions page. Make sure you're using the `us-east-1` Region.
 
### Create MySQL and Aurora Database

#### Set Up a Default VPC
 
1. Under **Recently visited**, select **VPC**. If **VPC** isn't listed as an option, then enter *vpc* into the search bar on top of the console and select **VPC** from the search results.
2. Select **VPCs**.
3. Click the **Actions** button.
4. From the dropdown menu, select **Create default VPC**.
5. Click the **Create default VPC** button.

#### Create MySQL Database

1. In the top left corner of the page, click the **AWS** icon to return to the AWS Management Console.
2.  Under **Recently visited**, select **RDS**. If **VPC** isn't listed as an option, then enter *rds* into the search bar on top of the console and select **Amazon RDS** from the search results.
3. Click the **Create database** button.
4. On the **Create database** page, set the following parameters:
  - **Engine options**: Select **MySQL**.
  - **Templates**: Select **Free tier**.
  - **Credentials**: Check the box next to **Auto generate a password**.
5. Click the **Create database** button. This may take a few minutes for the database to be created.
6. In the top right corner, click the **View credential details** button.
7. Under **Master password**, select **Copy** to save it to your clipboard. It's recommended to paste it into a file or other location so you have it handy for later steps.
8. Click the **Close** button.

#### Create Aurora Cluster

1. Click the **Create database** button.
2. On the **Create database** page, set the following parameters:
  - **Engine options**: Select **Aurora (MySQL Compatbile)**.
  - **Templates**: Select **Production**.
  - **Credentials**: Check the box next to **Auto generate a password**.
  - **DB instance class**: Select **Burstable classes (includes t classes)**.
3. Click the **Create database** button. It may take a few minutes for the database to be created.
4. In the top right corner, click the **View credential details** button.
5. Under **Master password**, select **Copy** to save it to your clipboard. This password should also be pasted into a file or other location so you have it handy for later steps.
6. Click the **Close** button.

#### Create Transfer Database

1. Once both the MySQL and Aurora databases have been created, enter *dms* in the search bar on top of the console.
2. From the search results, select **Database Migration Service**.
3. In the lefthand navigation menu, select **Replication instances**.
4. Click the **Create replication instance** button.
5. On the **Create replication instance** page, set the following parameters:
  - **Name**: Enter *DMSTest*.
  - **Multi AZ**: Select **Dev or test workload (Single-AZ)** from the dropdown menu.
  - **Public accessible**: Uncheck the box.
6. Click the **Create replication instance** button. You may have to wait a few minutes before the database is created.

### Set Up Source and Target for DMS

#### Create Source Endpoint
 
1. In the lefthand navigation menu, select **Endpoints**.
2. Click the **Create endpoint** button.
3. On the **Create endpoint** page, set the following parameters:
  - **Select RDS DB instance**: Check the box next to this option.
  - **RDS instance**: Select **database-1** from the dropdown menu.
  - **Access to endpoint database**: Select **Provide access information manually**.
  - **Password**: Paste the password generated for the MySQL database.
4. Click the **Create endpoint** button.

#### Create Target Endpoint

1. Click the **Create endpoint** button.
2. On the **Create endpoint** page, set the following parameters:
  - **Target endpoint**: Select this option.
  - **Select RDS DB instance**: Check the box next to this option.
  - **RDS instance**: Select **database-2-instance-1** from the dropdown menu.
  - **Access to endpoint database**: Select **Provide access information manually**.
  - **Password**: Paste the password generated for the Aurora cluster.
4. Click the **Create endpoint** button.

### Configure and Run Transfer Task

#### Create Migration Task

1. In the lefthand navigation menu, select **Database migration tasks**.
2. Click the **Create database migration task** button.
3. On the **Create database migration task** page, set the following parameters:
  - **Task identifier**: Enter *Test*.
  - **Replication instance**: Select **dmstest** from the dropdown menu.
  - **Source database endpoint**: Select **database-1** from the dropdown menu.
  - **Target database endpoint**: Select **database-2-instance-1** from the dropdown menu.
  - **Migration type**: Select **Migrate existing data** from the dropdown menu.
  - **Turn on validation**: Check the box next to this option.
  - **Add new selection rule**: Click this button.
  - **Schema**: Select **Enter a schema** from the dropdown menu.
  - **Source name**: Enter *sys*.
4. Click the **Create task** button. This can take a few minutes to complete.

#### Run Migration Task

1. Once the database migration task is ready, click the checkbox next to **test**.
2. Click the **Actions** button.
3. From the dropdown menu, select **Restart/resume**.
4. If an error message pops up, click **Close**. The task should start in a few minutes.
5. Select **test**.
6. Select the **Table statistics** tab to view information about the task. You may need to refresh the tab a few times before you can see all the information about the migration task.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!