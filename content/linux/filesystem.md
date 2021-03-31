---
draft: true
title: Filesystem
categories:
  - Linux
---

# Structure - partitions & file systems

### partition tables

- mbr

- GPT

- whats the difference?

### parity bit

-     an addional bit added to (normally) 7 bit of data that can be used to determine if the data was corruputed

### parity in raid

- XOR result of 2 data data 1  XOR data 2 --> parity data

- can be used to rebuild the data for any missing data

### raid levels

- 0 - stripping - no mirror or parity. Different from spanned volumes as its stripped. Requires all drives to be available.

- 1 - mirrored to every drive, writes are as slow as the slowest drive, reads - could theoretically be faster depending on the controller. Requires at least 1 drive to be available.

- 5  - block level striping with distributed parity. requires all drives but one to be available. Requires at lease 3 disks. Rebuilding array requires reading from all disks.

- 6 - block level striping with double distributed paritiy. 2 drives can fail. minimum 4 disks.

- 0+1 - Mirror of strips

- 0+3 - byte level striping with partity

- 1+0 - Stripe of mirror - provides the best throughput of all raids (other than raid 0)

Sources:

[ZFS RAIDZ vs. traditional RAID](https://www.klennet.com/notes/2019-07-04-raid5-vs-raidz.aspx)
