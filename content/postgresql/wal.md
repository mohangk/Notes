---
draft: true
title: Wal
categories:
  - Postgresql
---


### postgres writing to disk

#### Key config options related to WAL
- https://www.postgresql.org/docs/13/runtime-config-wal.htm

##### wal checkpoint

1. Point in the squest of transactions at which it is guaranteed that the head and index data files have been updated with all informaition before that checkpont

2. At checkout time, all dirty data pages are fulesh toe disk and a specifial checkout record is writtend to the log file. In the case of a failure, on the the wal segments after the checkpoint are replayed. Older WAL segments can be deleted, unless WAL archiving is enabled

3. Key checkpoint parameters
   
   1. **checkout_timeout** seconds - seconds before a checkpoint is carried out (5min)
   
   2. **max_wal_size** - max pending wal before doing a checkpoing (1GB) 
   
   3. **archive_timeout** - determine how frequently the WAL logs are archived, put a top bound  ([PostgreSQL: Documentation: 12: 19.5. Write Ahead Log - Archiving](https://www.postgresql.org/docs/12/runtime-config-wal.html#RUNTIME-CONFIG-WAL-ARCHIVING))
   
   4. **full_page_writes** - default on. Writes the full page out, not just the portion of the page that has changed after every checkpoint.  This is needed because a page write that is in process during an operating system crash might be only partially completed, leading to an on-disk page that contains a mix of old and new data. The row-level change data normally stored in WAL will not be enough to completely restore such a page during post-crash recovery. Storing the full page image guarantees that the page can be correctly restored, but at the price of increasing the amount of data that must be written to WAL.
   
   5. **checkpoint_warning** _As a simple sanity check on your checkpointing parameters, you can set the [checkpoint_warning](https://www.postgresql.org/docs/12/runtime-config-wal.html#GUC-CHECKPOINT-WARNING) parameter. If checkpoints happen closer together than `checkpoint_warning` seconds, a message will be output to the server log recommending increasing `max_wal_size`. Occasional appearance of such a message is not cause for alarm, but if it appears often then the checkpoint control parameters should be increased. Bulk operations such as large `COPY` transfers might cause a number of such warnings to appear if you have not set `max_wal_size` high enough.

4. Increasing frequencey of checkpoint reduces recovery time at the cost of more disk IO while running.
   
   1. writing dirty buffers during a checkpoint is spread over a period of time. That period is controlled by [checkpoint_completion_target](https://www.postgresql.org/docs/12/runtime-config-wal.html#GUC-CHECKPOINT-COMPLETION-TARGET), which is given as a fraction of the checkpoint interval - default value 0.5. So enough IO will be used to ensure checkpoint is completed in 1/2 the time it takes for the next checkout_timeout to happen_
   
   2. If IO is constrained, increase the checkpoint_completion_target so it can take longer (use less disk IO)__

##### How is writing of diry buffers carried out during a checkpoint

##### table structure

- rows of an array of 8-KB pages or blocks - anything more then that is stored in TOAST - "The oversized-attribute storage technique"

- block / page - page includes a header stores metadata about the block, 

#### writing wal

a. wal_sync_method - how is the data written to the WAL

- open_datasync, open_sync , with no streaming or archiving - uses direct io
- fdatasync (default) - uses bufferedIO

#### writing dirty buffers from shared buffers

Upon checkpoint, dirty buffers from shared buffers are written to page cache managed by OS. fsync() is called to commit to disk. This is managed by the OS. Problem lies with the fact that when a fsync fails - there is no way for user space(postgres) to know about this before kernel 4.13. 

Reference

[PostgreSQL: Documentation: 12: 29.4. WAL Configuration](https://www.postgresql.org/docs/12/wal-configuration.html)

[PostgreSQL: Documentation: 12: 29.1. Reliability](https://www.postgresql.org/docs/current/wal-reliability.html)

#### Key WAL config options on the replica
- When a conflicting query is short, it's typically desirable to allow it to complete by delaying WAL application for a little bit; but a long delay in WAL application is usually not desirable. So the cancel mechanism has parameters, max_standby_archive_delay and max_standby_streaming_delay, that define the maximum allowed delay in WAL application. 
- Remedial possibilities exist if the number of standby-query cancellations is found to be unacceptable. The first option is to set the parameter hot_standby_feedback, which prevents VACUUM from removing recently-dead rows and so cleanup conflicts do not occur. If you do this, you should note that this will delay cleanup of dead rows on the primary, which may result in undesirable table bloat.

## Fermi numbers

 Assumption - 100byte rows
 Cache hit ratio - 99% and above

 Scan 5-10 mil rows a sec
 Aggregate - 1-2 mil rows a sec
 inserts - 10,000 rows a sec 
 copy - 100,000 rows a sec
