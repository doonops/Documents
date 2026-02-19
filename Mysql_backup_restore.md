# MySQL Database Backup and Restore to AWS S3 - Implementation Guide

## 1. Overview

This document describes the process of backing up a MySQL database running on Ubuntu and uploading the backup to AWS S3 on a daily basis. It also includes a restore procedure to retrieve and restore the database from S3 when required.

### Solution Components
- MySQL Database Backup using mysqldump
- AWS S3 Storage for backup files
- Cron Job for scheduling daily backups
- Restore script for database recovery

### Architecture Flow
MySQL Database -> mysqldump -> Backup File -> Upload to AWS S3 -> Scheduled via Cron

---

## 2. Prerequisites

Ensure the following tools are installed on the Ubuntu server.

### Install MySQL Client
```bash
sudo apt update
sudo apt install mysql-client -y


### Install AWS CLI

```bash
sudo apt install awscli -y
```

### Verify Installation

```bash
aws --version
```

### Configure AWS CLI

```bash
aws configure
```

Provide:

* AWS Access Key
* AWS Secret Key
* Default Region (example ap-south-1)
* Output format json

Ensure IAM user has S3 read and write permissions.

---

## 3. MySQL Backup Script Creation

### Create Backup Script

```bash
nano mysql_backup_to_s3.sh
```

### Script Content

```bash
#!/bin/bash

DB_USER="root"
DB_PASSWORD="yourpassword"
DB_NAME="yourdbname"
S3_BUCKET="s3://your-bucket-name/mysql-backups"
BACKUP_DIR="/tmp"

DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_$DATE.sql"

echo "Starting MySQL backup..."

mysqldump -u$DB_USER -p$DB_PASSWORD $DB_NAME > $BACKUP_FILE

if [ $? -eq 0 ]; then
    echo "Backup successful: $BACKUP_FILE"
    aws s3 cp $BACKUP_FILE $S3_BUCKET/

    if [ $? -eq 0 ]; then
        echo "Uploaded to S3 successfully"
        rm $BACKUP_FILE
    else
        echo "S3 upload failed"
    fi
else
    echo "Backup failed"
fi
```

### Make Script Executable

```bash
chmod +x mysql_backup_to_s3.sh
```

### Test Backup

```bash
./mysql_backup_to_s3.sh
```

---

## 4. Schedule Daily Backup using Cron

### Open Crontab

```bash
crontab -e
```

### Add Cron Job (Daily 12 PM)

```bash
0 12 * * * /full/path/mysql_backup_to_s3.sh >> /var/log/mysql_backup.log 2>&1
```

This executes the script daily at 12 PM and logs output.

---

## 5. Database Restore Script from AWS S3

### Create Restore Script

```bash
nano mysql_restore_from_s3.sh
```

### Script Content

```bash
#!/bin/bash

DB_USER="root"
DB_PASSWORD="yourpassword"
DB_NAME="yourdbname"
S3_BUCKET="s3://your-bucket-name/mysql-backups"

echo "Enter backup file name:"
read BACKUP_NAME

echo "Downloading backup from S3..."
aws s3 cp $S3_BUCKET/$BACKUP_NAME /tmp/$BACKUP_NAME

if [ $? -eq 0 ]; then
    echo "Restoring database..."
    mysql -u$DB_USER -p$DB_PASSWORD $DB_NAME < /tmp/$BACKUP_NAME
    echo "Restore completed"
else
    echo "Download failed"
fi
```

### Make Script Executable

```bash
chmod +x mysql_restore_from_s3.sh
```

### Run Restore Process

```bash
./mysql_restore_from_s3.sh
```

---

## 6. Production Best Practices

* Avoid storing database passwords directly in scripts. Use `~/.my.cnf` for secure authentication.
* Enable backup compression using gzip to reduce storage usage.
* Configure AWS S3 lifecycle policy to automatically delete old backups.
* Enable encryption for backup files before uploading to S3.
* Monitor backup logs and configure alerting for failures.
* Ensure IAM roles follow least privilege access principles.

---

## 7. Conclusion

This setup provides an automated and reliable mechanism for backing up MySQL databases and storing them securely in AWS S3. The scheduled cron job ensures regular backups, while the restore script enables quick recovery during incidents or disaster scenarios.

```

---



bhi bana deta hoon 👍
```
