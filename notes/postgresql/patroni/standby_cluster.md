#### Promoting a standby cluster


run patronictl edit-config
remove the standby_cluster section from the config
Save and exit from the editor
review the config change and agree to apply it.

https://github.com/zalando/patroni/issues/1096
