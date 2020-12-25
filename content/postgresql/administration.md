- reload without restart
  - select pg_reload_conf()
  - sudo systemctl reload postgresql (?)

- view the current contents of pg_hba
  - select * from view pg_hba_file_rules

- view configuration options
    - show all;


- view the replication status
  - on primary
    -select client_addr, state, sent_lsn, write_lsn,
    flush_lsn, replay_lsn from pg_stat_replication;
    - select * from pg_replication_slots;
  - on replica
      -  select * from pg_stat_wal_receiver;


-  Starting the database
  - pg_ctl -D /usr/local/var/postgres start
  - pg_ctl -D /usr/local/var/postgres -l logfile start 
