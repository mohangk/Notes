##### zpool

- highest level of storage hierarchy. Consists of one or more vdevs. Data distributed across vdevs. No fault tolerance at the pool level - only within the vdev. 2 zpools cannot share a vdev.

##### vdev

- consists of one or more devices (disks)

support vdevs 

- cache (l2arc) - caches reads in a ring buffer - [ZFS - Add L2ARC Device | Programster's Blog](http://blog.programster.org/zfs-add-l2arc) 

- log - aks SLOG - this fast vdev stores the ZFS intent log [ZFS - Add Intent Log Device | Programster's Blog](https://blog.programster.org/zfs-add-intent-log-device), [Aaron Toponce : ZFS Administration, Part III- The ZFS Intent Log](https://pthree.org/2012/12/06/zfs-administration-part-iii-the-zfs-intent-log/)

- ,special - fast mirror vdev stores metadata

storage vdevs 

- single disk, mirror, RAIDz1, RAIDz2, RAIDz3

- RAIDz1, RAIDz2, and RAIDz3 are special varieties of what storage greybeards call "diagonal parity RAID." The 1, 2, and 3 refer to how many parity blocks are allocated to each data stripe. Rather than having entire disks dedicated to parity, RAIDz vdevs distribute that parity semi-evenly across the disks. A RAIDz array can lose as many disks as it has parity blocks; if it loses another, it fails, and takes the `zpool` down with it.

- Mirror vdevs are precisely what they sound like—in a mirror vdev, each block is stored on every device in the vdev. Although two-wide mirrors are the most common, a mirror vdev can contain any arbitrary number of devices—three-way are common in larger setups for the higher read performance and fault resistance. A mirror vdev can survive any failure, so long as at least one device in the vdev remains healthy.

- Single-device vdevs are also just what they sound like—and they're inherently dangerous. A single-device vdev cannot survive any failure—and if it's being used as a storage or `SPECIAL` vdev, its failure will take the entire `zpool` down with it. Be very, very careful here.

- pool may contain any number of vdevs - topologies and sizes dont have to match

- 

##### devices

- its a random access block devices

##### dataset

- zfs set quote=100G poolname/datasetname

- dataset hierarchy - no leading slash

- mountpoint for a dataset - equivalent to its ZFS hierarchical name, with a leading slash

##### zvol

- dataset missing its filesystem layer - block device

##### blocks & sectors & ashift

- all data is stored in blocks - maximum size of a block is defined for each dataset in the recordsize property. If not already defined the default recordsize is 128KiB

- a block only ever refers to one file

- zvols do not have recordsize property instead they have volblocksize 

- ashift property - 2^ashift == sector size

- dont set the ashift too low (lower than the sector size of the disk)

- By contrast, there is virtually no penalty to setting `ashift` too high. There is no real performance penalty, and slack space increases are infinitesimal (or zero, with compression enabled). We strongly recommend even disks that really *do* use 512 byte sectors should be set `ashift=12` or even `ashift=13` for future-proofing.

##### CoW semantics

- dont change the block in place, but rather copy it to a new block and point to the new block

- ZFS handles sync writes differently from normal filesystems—instead of flushing out sync writes to normal storage immediately, ZFS commits them to a special storage area called the ZFS Intent Log, or ZIL.. ZIL is the zfz WAL

- Snapshots - made possible through the CoW semantics. The filesystem has a tree of pointers marking records that contain the latest data. When you make a snapshot you make a copy of that tree of pointers. Those pointers then cannot be deleted when ZFS is trying to reclaim diskspace.

- Replication - by using the pointers for a snapshot, it becomes very efficient for ZFS to snapshot and send data 

- Inline compression is also made possible due to the CoW 

##### ARC

- Adaptive replacement cache - replaces the os buffer cache which is LRU based with a less naive cache

[ZFS 101—Understanding ZFS storage and performance | Ars Technica](https://www.google.com/url?client=internal-element-cse&cx=009773542741016272635:e6s_fsvpe7o&q=https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/&sa=U&ved=2ahUKEwiVoeGTx9vpAhU04nMBHckSDcwQFjAGegQIAxAC&usg=AOvVaw29-5egOT8eC4xQdAdQ-OYU)

#### Commands to create a pool

zpool create -o ashift=13 <poolname> /dev/sda -o feature@userobj_accounting=disabled \
 -o feature@edonr=disabled \
 -o feature@project_quota=disabled \
 -o feature@allocation_classes=disabled \
 -o feature@resilver_defer=disabled

zfs set atimeoff <poolname>

zfs set compression=on <pool>

zpool import <poolname> #load the pool from disk

zpool export <poolname>  # remove the pool - can be used on external drive

zfs set compression=lz4 <poolname>

https://frommelmak.com/how-to-mount-a-zfs-drive-in-linux.html

[How to Create ZFS Filesystem with File Compression on Linux](https://www.thegeekstuff.com/2015/11/zfs-filesystem-compression/)

[ZFS on Backup USB Drive | The FreeBSD Forums](https://forums.freebsd.org/threads/zfs-on-backup-usb-drive.33812/)
