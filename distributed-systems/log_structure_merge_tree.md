## log structured merge tree overview

- flushing immutable chunks of data - append only - form of SSTables to disk
- More and more SSTables are written to disk - one partition spread across multiple tables 
- Write amplification - writing the same data over and over again

### interesting facts

The figures in this ACM report [here](http://queue.acm.org/detail.cfm?id=1563874)/[here](http://deliveryimages.acm.org/10.1145/1570000/1563874/jacobs3.jpg) make the point well. They show that, somewhat counter intuitively, sequential disk access is faster than randomly accessing main memory. More relevantly they also show sequential access to disk, be it magnetic or SSD, to be at least three orders of magnitude faster than random IO. This means random operations are to be avoided. Sequential access is well worth designing for.

### search sorted file

### memtable



<http://www.benstopford.com/2015/02/14/log-structured-merge-trees/>