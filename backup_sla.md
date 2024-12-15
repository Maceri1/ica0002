# Backup SLA

## Coverage

We back up services that satisfy at least one of these criteria:
 - are primary source of truth for particular data
 - contain customer and/or client data
 - are not feasible (or very costly) to restore by other means

Services that are backed up:
 - MySQL
 - InfluxDB
 - Ansible Git repository


## Schedule

MySQL backups are created every day at 20:00UTC; it takes up to 30 minutes to create and store the backup.

InfluxDB backups are created every day at 20:30UTC; it takes up to 30 minutes to create and store the backup.

Ansible Git repository backups are created; it takes up to 5 minutes to create and store the backup.

All backups are started automatically by cron jobs.

Backup RPO (recovery point objective) is:
 - 24 hours for MySQL
 - 24 hours for InfluxDB
 - 1 hour for Ansible Git repository


## Storage

MySQL and InfluxDB backups are uploaded to the backup server.

Ansible Git repository is mirrored to the internal Git server.

Backup data from both servers will be synchronized to an encrypted AWS S3 bucket in the future (work in progress).


## Retention

MySQL backups are stored for 7 days; 7 versions (recovery points) are available to restore.

InfluxDB backups are stored for 7 days; 7 versions are available to restore.

Ansible Git repository backups are stored for 30 days; 720 versions are available to restore.


## Usability checks

MySQL backups are verified every week by the DBA team.

InfluxDB backups are verified every week by the DBA team.

Ansible Git repository backups are verified every day by the DevOps team.


## Restore process

Service is recovered from the backup in case of an incident, and when service cannot be restored in any other way.

RTO (recovery time objective) is:
 - 2 hours for MySQL
 - 2 hours for InfluxDB
 - 30 minutes for Ansible Git repository

Detailed backup restore procedure is documented in the [backup_restore.md](./backup_restore.md).
