# AWS-Data-Migration-of-MySQL-Database-to-AWS
AWS Data Migration of MySQL Database to AWS

To migrate the data from an on-premises MySQL Database to AWS there are a few things that need to be setup in AWS Database Migration Service (DMS) and AWS Relational Database Service (RDS). First there must be a replication instance created in the AWS DMS console.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/migration_Image.png)

In the AWS management console navigate to DMS.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/DMS.png)

Click on replication instances in the left menu.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/rep!.png)

Click on Create replication Instance.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/create1.png)

From here create a unique name for the replication instance and a short description. Choose an appropriately sized instance class. For the purposes of this example the micro size has been chosen. Select the default or most up to date engine version unless you know you need a specific one. Choose how much allocated GiB storage works for the purpose and select an appropriate VPC. You may check the box for the Multi AZ for fault tolerance if you want. Then check the box to make the instance publicly accessible.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/rep_in_1.png)

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/rep_in_2.png)

From there open the advanced security and network configuration drop down and choose the correct replication subnet group. Then choose the correct Availability Zone, VPC security group(s), and KMS master key.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/adv1.png)

Add any needed maintenance window information or needed tags and click create.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/creat%40.png)

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/rep_in_4.png)

Next an endpoint must be created to connect to the source database. To do that click on endpoints in the left menu of the DMS console. Then click on the create endpoint button near the top right.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/create_end.png)

Next, create a label for the endpoint identifier and enter the correct info for the on-prem server.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/opn_prem1.png)

Then test the endpoint to make sure it’s working and click create endpoint.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/create_end_4.png)

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/endpoint_created.png)

To create a target endpoint, we need to have a database to connect to first. Navigate to the RDS console. Click on databases in the left menu and then the create database button.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/Create_data.png)

Click the circle next to easy create and choose MySQL in the configuration.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/easy.png)

Next, choose the free tier for the DB instance size. Make a DB instance identifier. Then create a master username and password. Make sure to take a note of the username and password as you will need it for your target endpoint and MySQL Workbench later.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/tier.png)

After that, click on create database.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/database_1.png)

Navigate back to the DMS console and click on endpoints in the left menu and then the create endpoint button near the top right.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/endpoint5.png)

Select the circle for target endpoint and create an endpoint identifier. Choose Aurora as the target engine. Enter the correct server name, port, username, and password.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/target_end.png)

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/target_end2.png)

The server name is located in the RDS database created earlier. It’s in the connectivity & security tab and under where it says endpoint. 

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/End-arn.png)

If the MySQL client is having trouble connecting, click on the database and click on modify near the top right.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/modify.png)

Then scroll down to network & security and click on the circle for yes under public accessibility. Then click continue. Click apply immediately then click modify DB instance.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/immed.png)

Test the endpoint connection and click create endpoint.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/End1and2.png)

Now that the infrastructure is created, we must create a migration task to move the data. To do this, click on database migration tasks in the left menu of the DMS console and then create task.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/task_1.png)

Create a task identifier name and choose the correct replication instance. Then choose the correct source database endpoint and target database endpoint. Select migrate existing data for migration type and leave the box checked for start task on create.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/existing.png)

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/existing2.png)

Click on JSON editor and paste the following JSON script:https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/template1.jscsrc

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/editor.png)

Click create task.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/task_loaded.png)

To verify that the data has been migrated download the third-party MySQL Workbench software. In the program, click on the plus sign. Create a connection name. For the hostname enter the endpoint for the RDS database you entered in the target endpoint and port number 3306. Then use the username and password for the RDS database you created as well. Then click OK.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/connection1.png)

Click on the connection you made, and you should see a database and tables for cc640mysql.

![alt text](https://github.com/doyle199/AWS-Data-Migration-of-MySQL-Database-to-AWS/blob/master/connection2.png)

