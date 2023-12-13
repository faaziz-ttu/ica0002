This files describes the backup measures

Ansible command to install and configure services in the infrastructure:

    ansible-playbook infra.yaml

Restoration of  MySQL data from the backup:

    duplicity --no-encryption restore rsync://{user}@backup./mysql /home/backup/restore/mysql

How can it be checked:

    sudo -u backup mysqldump agama >/dev/null; echo $? /*ensure that user backup can 
     create dumps*/

    ls -la /home/backup/mysql //check that dump is created
    
    mysql agama < /home/backup/mysql/agama.sql /*check that backup can be restored*/

Main services to be backed up.

    - MySQL
    - InfluxDB
    - group_vars all.yaml

Backup coverage
        - MySQL
        - InfluxDB
        - group_vars/all.yaml
        - /etc/roles/"services"/tasks/main.yaml
        this files would be backed up to lessen the blow of data lost.
    
    Recovery Point Objective (RPO)
        The point of the backup in this case is to recreate the system in shortest time possible. Not affordable to loose all files. Back up is here to minimilize the data loss.
    
    Versioning and retention
        3 backups. 2 on site and one off site.Two on site would allow fast recovery ib access of data loss. 3rd backup that is offsite would be used in case it is discovered that backups on the site have been tempered with.
        Site backups would happend every week while of site backups every month
   
    Usability checks
        By running the playbook if there is no errors that indicates backups can be created if there is an error it requires a fix before continuing to restore the backup

    Restoration criteria
        In case of substantial data loss it required to restore data
    
    Recovery Time Objective (RTO)
        5-10 hours during monday to friday to resume normal functioning of the services
        15-24 hours during the weekend 


