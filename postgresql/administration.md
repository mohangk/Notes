- reload without restart
  - select pg_reload_conf()
  - sudo systemctl reload postgresql (?)

- view the current contents of pg_hba
  - select * from view pg_hba_file_rules

- view configuration options
    - show all;


-  Starting the database
  - pg_ctl -D /usr/local/var/postgres start
  - pg_ctl -D /usr/local/var/postgres -l logfile start 
