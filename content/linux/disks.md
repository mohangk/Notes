---
draft: true
title: Disks
categories:
  - Linux
---
##### 

#### Disk performance

##### throughput

- measured in MB/sec 

- determined by the speed of the disk reads - e.g. 7200RPM - number of physica sectors / blocks passing beneath the heads

- speed of the controller - SATA - 6Gbps SAS - 22.5 Gbps

- You divide Gbps by 8 to get GB/sec, then multiply by 1024 to get MB/sec. So a SATA-3 6Gbps link can theoretically move up to 768MB/sec.  most SATA controllers won't move much more data than a single link can manage, even with many disks connected to the controller

##### latency

- time it takes to read or write a particular block

- spinning disks latencies
  
  - seek latency - time it takes for the disk head to move into the right track on the disk
  
  - rotational latency - time it takes for the sector to spin to under the disk head
  
  - about 15-25ms
  
  - assuming that the disk is 180MB/sec - if all the data is maximally fragmented, every 4kblock takes a seek takes about 15ms hence 4kb/0.016 - 250kb/sec

##### IOPS

- How many operations can a disk service on the low end - 4k random reads and writes

- SSDs dont have rotational and seek latency but are still impacted by random reads and writes. The data is spread across multiple channels of physical media - it tries to do it in parallel (data is stripped across the channels, hence sequential reads are the best) but if the data is random and its bottlenecked on one channel, it can prove to be a major issue as well. For e.g. a SSD capable of 500MB/sec could drop to around 40MB/sec

##### How are  throughput and IOPS used when discussing disk performance?

Through put is typically discussed as the amount of data a disk moves under optimal conditions (sequential reads), while IOPS is discussed in terms of the amount of IOPS the disk is capable even in random reads

##### cache

Write buffers are used to batch writes back to disk by the OS. Instead of writing individual 4kb blocks, it waits till hits a certain threshold. To improve write IOPS some SSDs have different tier-ing of storage medium - slower medium TLC or QLC (quad layer cell) and faster MLC (multi layer cell) media. But when the MLC is full, the TLC will still need to take all the writes. 

Read cache - keeps copies of the data in memory so repeated requests can be served straight out of the buffer 

#### Performance test using fio for the 4k random io test

Output from lapotop SSD: 

```
Run status group 0 (all jobs):
  WRITE: bw=325MiB/s (341MB/s), 325MiB/s-325MiB/s (341MB/s-341MB/s), io=19.5GiB (20.9GB), run=61356-61356msec
```

```
Output from portable disk:

  WRITE: bw=14.5MiB/s (15.2MB/s), 14.5MiB/s-14.5MiB/s (15.2MB/s-15.2MB/s), io=984MiB (1032MB), run=67980-67980msec
```

First, we're seeing output in both MiB/sec and MB/sec. MiB means "mebibytes"—measured in powers of two—where MB means "megabytes," measured in powers of ten. Mebibytes—1024x1024 bytes—are what operating systems and filesystems actually measure data in, so that's the reading you care about.

###### Test 1: 4KiB single process random

```
fio --name=random-write --ioengine=posixaio --rw=randwrite --bs=4k --size=4g --numjobs=1 --iodepth=1 --runtime=60 --time_based --end_fsync=1
```

###### Test 2: 16 parallel 64KiB random write processes
```
fio --name=random-write --ioengine=posixaio --rw=randwrite --bs=64k --size=256m --numjobs=16 --iodepth=16 --runtime=60 --time_based --end_fsync=1
```

###### Test 3:  Single 1MiB random write process (easiest)

```
fio --name=random-write --ioengine=posixaio --rw=randwrite --bs=1m --size=16g --numjobs=1 --iodepth=1 --runtime=60 --time_based --end_fsync=1
```


References:

- [How fast are your disks? Find out the open source way, with fio | Ars Technica](https://arstechnica.com/gadgets/2020/02/how-fast-are-your-disks-find-out-the-open-source-way-with-fio/)
- https://fio.readthedocs.io/en/latest/fio_doc.html
- https://tobert.github.io/post/2014-04-17-fio-output-explained.html
- https://cloud.google.com/compute/docs/disks/benchmarking-pd-performance