# How to setup Replication in POSTGRESQL (12)

### 1. Requirements: 
    1. Two Machines.
    2. POSTGRESQL  installed on both machines.
    3. Updated terminal.

### 2. Steps : 

#### A. On Master Machine :
1. Become postgres user :
`sudo su - postgres`
2. Enter :
`psql`
3. Enter : 
`ALTER SYSTEM SET listen_addresses TO '*';`
![nmknk](/assets/m_alter_listen.png)

4. Return back to postgres user : ctrl + d.
5. Create user  with replication property then it will ask you to enter password by the command : 
`createuser --replication -P -e <name_for_replication_user>`
![nmknk](assets/m_rep_create_user.png)

6. Edit pg_hba.conf file :
` nano /etc/postgresql/12/main/pg_hba.conf`
7. Head to privilege at the bottom, then add this line :

    `host    replication     name_for_replication_user      <slave_machine_ip>/0       md5`
![nmknk](assets/privilege_master.png)


8. Restart your postgresql service : 
`systemctl reload postgresql.service`



#### B. Slave (Standby) Machine:
1. Stop postgresql service : 
`sudo systemctl stop postgresql.service`
2. Copy your database data to someLocation in case if you want your old data, because in case of replication , we must empty our database .
become postgres user then enter :
`cp -R /var/lib/postgresql/12/main /var/lib/postgresql/12/<some_location>`

3. After moving your data, remove your database data :

`rm -rf /var/lib/postgresql/12/main/*`

4. Do the replication by : 
`pg_basebackup -h <master_machine_ip> -D /var/lib/postgresql/12/main -U <name_for_replication_user> -P -v -R -X stream -C -S node2`






# Edit Master IP address

#### In case of your Master device changed his ip address for any reason, you need to edit `primary_conninfo` in **the Slave** machine
## How to restart streaming when your master ip has changed :
1. Head towards and open  recovery.conf file: nano /var/lib/postgresql/11/main/recovery.conf .
2. Change host to the new ip then save and **restart** your standby postgres: systemctl restart postgresql-12.service



# Edit Slave IP address :

#### In case of your Slave device changed his ip address for any reason, you need to edit `pg_hba.conf` in **the Master** machine
1.  pg_hba.conf file :
` nano /etc/postgresql/12/main/pg_hba.conf`
2. Head to privilege at the bottom, then add this line with the new slave ip address or you can edit the old:

    `host    replication     name_for_replication_user      <slave_machine_ip>/0       md5`



For reference : 
1. [Replication Steps](https://minervadb.xyz/step-by-step-postgresql-12-streaming-replication-on-ubuntu/)
2. [LINKEDIN](https://www.linkedin.com/in/mohammadghafri/)


    
