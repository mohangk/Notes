#### OS related params
- `vm.dirty_expire_centisecs`
- `vm.dirty_background_bytes` - On systems with a lot of memory the default value is far too high, allowing the kernel to accumulate a lot of dirty data in filesystem cache. Kernel often decides to flush them all at once, reducing the benefit of spread checkpoints. Set it lower to ensure that the kernel is flushing dirty data out to disk at a frequen enough rate

#### Checkpoints

* happen every time CHECKPOINT is called
* pg\_start\_backup\, CREATE DATABASE\, pg\_ctl stop \| restart
* reach the min time before a checkpoint should happen - `checkpoint_timeout - default 5min`
* hit the max size of WAL - `max_wal_size - default 1 GB`
* How frequently you checkpoint
    * Will effect how long a recorvery takes - so the longer period between checkpoints, during a recovery, there will be more WAL to read to recover from
    * Will effect how frequently you are writing your data changes to disk, increasing the write IO

##### checkpoint_timeout
- 5 mins is too low, 30 minutes is  reasonable
- The checkopoint_timeout doesnt automatically translate to how long it will take to recover, it translates to the amount of WAL generated during that period
- That can be controlled by setting the max_wal_size


##### max_wal_size
- Determine how much WAL is generated in a 30 mins window by:
    - Looking at the actual WAL position using pg_current_xlog_insert_location() - compute different between positions every 30 mins by using pg_xlog_location_diff()
    - Enable log_checkpoints=on - extract the information from the server log
    - Use data from pg_stat_bgwriter - info about the number of checkpoints
- This number is shared by 2-3  checkpoints - so divide number by 3 to get the per checkpoint amount

##### checkpoint_completion_target

 checkpoint_completion_target = 0.5. This configuration parameter say how far towards the next checkpoint should all the writes be complete. For example assuming checkpoints triggered only by checkpoint_timeout = 5min, the database will throttle the writes so that the last write is done after 2.5 minutes. The OS then has another 2.5 minutes to flush the data to disk, so that the fsync calls issued after 5 minutes are cheap and fast.
 
 Instead, you might try setting checkpoint_completion_target roughly using this formula

(checkpoint_timeout - 2min) / checkpoint_timeout


References:
- https://www.youtube.com/watch?v=IFIXpm73qtk - PostgreSQL Configuration for Humans
- https://www.2ndquadrant.com/en/blog/basics-of-tuning-checkpoints/
- https://resources.2ndquadrant.com/webinar-postgres-vacuuming-through-pictures 
- https://resources.2ndquadrant.com/webinar-power-of-indexing-in-postgresql \
- https://news.ycombinator.com/item?id=24606163

##### replication related tunings
On systems that support the keepalive socket option, setting tcp_keepalives_idle, tcp_keepalives_interval and tcp_keepalives_count helps the primary promptly notice a broken connection.

#### 
Video on postgresql config settings

https://www.youtube.com/watch?v=IFIXpm73qtk&t=783s

https://www.percona.com/blog/2020/09/29/postgresql-configuration-changes-you-need-to-make-post-installation/

