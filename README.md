# mysql-backup

This image runs mysqldump to backup data using cronjob to folder `/backup`

## Example:

docker network create testmysqlbackupnetwork

docker run -d --rm --network testmysqlbackupnetwork -e MYSQL_ROOT_PASSWORD="qwerty12345" --name mysql pedropilla/mysql-dummydb

docker run -d --rm --network testmysqlbackupnetwork -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASS=qwerty12345 -e CRON_TIME='*/1 * * * *' -v $(pwd):/backup --name mysqlbackup pedropilla/mysql-backup

## Parameters

    MYSQL_HOST      the host/ip of your mysql database
    MYSQL_PORT      the port number of your mysql database. Default: 3306
    MYSQL_USER      the username of your mysql database. Default: root
    MYSQL_PASS      the password of your mysql database
    MYSQL_DB        the database name to dump. Default: `--all-databases`
    EXTRA_OPTS      the extra options to pass to mysqldump command
    CRON_TIME       the interval of cron job to run mysqldump. `0 0 * * *` by default, which is every day at 00:00
    MAX_BACKUPS     the number of backups to keep. When reaching the limit, the old backup will be discarded. No limit by default
    INIT_BACKUP     if set, create a backup when the container starts
    INIT_RESTORE_LATEST if set, restores latest backup

## Restore from a backup

See the list of backups, you can run:

    docker exec mysqlbackup ls /backup

To restore database from a certain backup, simply run:

    docker exec mysqlbackup /restore.sh /backup/2020.06.04.010000.sql
