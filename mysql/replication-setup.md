
1. Get mysql packages for debian
- wget http://repo.mysql.com/mysql-apt-config_0.8.13-1_all.deb
- sudo dpkg -i mysql-apt-config_0.8.13-1_all.deb

2. vim /etc/mysql/mysql.conf.d/mysqld.cnf
- set bind-address to 0.0.0.0
- server-id       = 1
- log-bin = mysql-bin
- binlog_format = row
- gtid-mode=ON
- enforce-gtid-consistency
- log-slave-updates  (when running as a slave, will also right to binglog)


3. Start the mysql instance

4. Create the user for the replication process on the master
mysql> create user 'rep_user'@'%' identified by 'password';

5. Create an application user 
mysql> create user 'application'@'%' identified by 'password';
mysql> grant all on *.* to 'application'@'%';

6. Create an snapshot based on this instance disk
gcloud compute disks snapshot mysql5-instance --snapshot-names=mysql5-202009201823

7. Create an image based on this snapshot
gcloud compute images create mysql5-202009201823 --source-snapshot=mysql5-202009201823


8. Spin up a replica
  gcloud compute  instances create mysql5-replica --no-address --zone=us-central1-a --machine-type=n2-standard-4 --image=mysql5-202009201823 --image-project=gcplabtest-286209 --boot-disk-size=10GB --boot-disk-type=pd-ssd --boot-disk-device-name=mysql5-replica --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
 
9. Delete the auto.cnf so that the servers don't share a UUID

rm /var/lib/mysql/auto.cnf

10. Add the following to the mysqld conf

vim /etc/mysql/mysql.conf.d/mysqld.cnf

change: 
server-id = 2

add:
relay-log = relay-log-server
read-only = ON


11. Restart

12. configure the CHANGE MASTER and start the replication

CHANGE MASTER TO
MASTER_HOST = '10.128.0.8',
MASTER_PORT = 3306,
MASTER_USER = 'rep_user',
MASTER_PASSWORD = 'password',
MASTER_AUTO_POSITION = 1;

13. Start the SLAVE process
START SLAVE;

14. Check status
show slave status;

--

