Setting up an external master

6. Dump from the replica that for the external master

mysqldump -h10.128.0.9 -uapplication -ppassword --databases test --hex-blob --skip-triggers --master-data=1 --order-by-primary --no-autocommit --default-character-set=utf8mb4 --single-transaction --set-gtid-purged=on | gzip | gsutil cp - gs://gcptestlab-dbdump/test.sql.gz

7. gcloud beta
gcloud beta sql instances create external-master --database-version=MYSQL_5_7 --region=us-central1 --source-ip-address=10.128.0.8 --source-port=3306 

8. gcloud beta sql instances create gcloud-replica --master-instance-name external-master --master-username rep_user --master-password password --master-dump-file-path gs://gcptestlab-dbdump/test.sql.gz --no-assign-ip --region us-central1 --network default --async

9. Ensure that the CloudSQL subnect can connect to the primary MySQL instance

Reference
- https://dinfratechsource.com/2019/05/14/setting-up-mysql-master-slave-replication-with-gtid/

