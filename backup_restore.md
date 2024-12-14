# Backup and Restore Instructions

This document provides step-by-step instructions for restoring backups for MySQL and InfluxDB databases. Follow these steps to ensure a successful restoration.

## Prerequisites
- Ensure you have access to the necessary servers and user permissions.
- Ensure Ansible is installed and configured on your machine.

## 1. Run Ansible Playbook

Run the Ansible playbook to install all the necessary configurations. Execute this command from your machine:
``` ansible-playbook infra.yaml ```
Keep in the mind the variables that are in ```group_vars/all.yaml``` that contain the MySQL usernames and passwords which might be relevant depending on the situation.

## 2. Restoration of MySQL
### 1. Restoring from the backup from the server
To restore the backup from the server:
Log into the backup user on the VM using ```sudo su - backup```
and then run the command ```duplicity --no-encryption restore rsync://Maceri1@backup.hannibal.io//home/Maceri1 /home/backup/restore/ ```

To upload the backup to the server, use the commands as the root user
```cd```
```mysql agama < /home/backup/restore/agama.sql```

## 3. Restoration of InfluxDB
### 1. Restoring backup from server
Log in as backup user using ```sudo su - backup```
and then run the command ```duplicity --no-encryption restore rsync://Maceri1@backup.hannibal.io//home/Maceri1 /home/backup/restore```

To restore the backup you shall delete existing Telegraf Database and Stop Telegraf Service.
This must be done as root user.
```cd```
```service telegraf stop```
```influx -execute 'DROP DATABASE telegraf'```

And then, the final command that will apply the backup:
```influxd restore -portable -database telegraf /home/backup/restore```

### 4. Final checks
Verify that the backups are up and running and run the ansible-playbook to get telegraf service running again
```ansible-playbook infra.yaml```

## Final words
By following these instructions, you will be able to restore MySQL and InfluxDB databases from backups. Ensure that you have the necessary permissions and access to the servers before starting the restoration process. If you encounter any issues, refer to the logs and verify the steps to troubleshoot the problem.
