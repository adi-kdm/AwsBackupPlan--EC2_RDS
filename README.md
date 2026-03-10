# Project 2 : Set Up AWS Backup Plan for EC2 and RDS -

## Objective :
The objective of this project is to learn how to configure and manage automated backup and recovery for AWS resources using AWS Backup. This project demonstrates how to protect cloud infrastructure by creating a centralized backup plan for an EC2 instance and an RDS database.

---

## Architecture Used :

- Amazon EC2 (Linux Server)
- Amazon RDS (MySQL Database)
- AWS Backup
- Backup Vault
- IAM Role (AWSBackupDefaultServiceRole)

---

## Infrastructure Setup :

### 1. Launch EC2 Instance
An EC2 instance was launched to simulate an application server.

Configuration:
- Instance Type: t2.micro
- Operating System: Amazon Linux 2
- Web Server: Apache

![](/img/Instance_Running.png)

Commands used to install Apache:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

A sample HTML file was created:

```bash
cd /var/www/html
echo "AWS Backup Project Test Page" > index.html
```

This file acts as test data stored on the EC2 instance.

---

### 2. Launch RDS Database :

An Amazon RDS database instance was created.

Configuration:
- Engine: MySQL
- Instance Type: db.t3.micro
- Database Name: testdb

![](/img/RDS_Database.png)

Connected to the database using MySQL client and created a sample table.

```sql
CREATE DATABASE testdb;

USE testdb;

CREATE TABLE users (
id INT PRIMARY KEY,
name VARCHAR(50)
);

INSERT INTO users VALUES (1,'Aditya');
INSERT INTO users VALUES (2,'Rahul');

SELECT * FROM users;
```

This created sample data inside the database for backup testing.

## Data Store Successfully :

![](/img/Data_Store.png)
---

## AWS Backup Setup :

### 1. Create Backup Vault

AWS Backup Vault was created to securely store backup recovery points.

Vault Name:
```
project-backup-vault
```

Encryption Key:
```
aws/backup
```

![](/img/Backup_Vault.png)
---

### 2. Create Backup Plan :

A backup plan was created to define how backups should be performed.

Backup Plan Name:
```
project-backup-plan
```

Backup Rule Configuration:

| Setting | Value |
|------|------|
Backup Rule Name | daily-backup-rule |
Backup Frequency | Daily |
Backup Vault | project-backup-vault |
Retention Period | 7 Days |


![](/img/Backup_Plan.png)
---

### 3. Assign Resources :

Resources were assigned to the backup plan using **Resource ID selection**.

Protected resources:

- Amazon EC2 instance
- Amazon RDS database

IAM Role used:

```
AWSBackupDefaultServiceRole
```

This role allows AWS Backup to perform backup operations on the selected resources.

---

## Validation :

To validate the configuration, an **On-Demand Backup** was triggered.

Steps:
1. Navigate to AWS Backup Console
2. Open Backup Vault
3. Click **Create On-Demand Backup**
4. Select resource type
5. Start backup job

Backup jobs were successfully created for:

- EC2 Instance
- RDS Database

The status of the backup jobs was monitored under the **Backup Jobs** section.

![](/img/Backup_Jobs.png)

---

## Recovery Points :

After the backup process completed, recovery points were created in the Backup Vault.

Location:

AWS Backup → Backup Vaults → project-backup-vault

Recovery points were generated for:

- EC2 Instance
- RDS Database

These recovery points can be used to restore the resources in case of failure or data loss.

---

## Tools and Services Used :

- AWS Management Console
- Amazon EC2
- Amazon RDS
- AWS Backup
- IAM
- MySQL Client
- Apache Web Server

---

## Issues Faced and Resolution :


### Issue 2: Resources Not Visible in Protected Resources

Initially, EC2 and RDS were not visible under **Protected Resources**.

Reason:
Backup had not been triggered yet.

Solution:
Created an **On-Demand Backup**, after which the resources appeared under Protected Resources.

---

## Conclusion :

This project successfully demonstrated how to configure AWS Backup to protect critical cloud resources such as EC2 instances and RDS databases. A centralized backup plan was implemented with automated scheduling and retention policies. Recovery points were created and validated to ensure that data can be restored when required.

This setup helps organizations protect infrastructure from data loss, accidental deletion, and system failures.