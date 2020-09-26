### Storage

#### Identifying processes stuck on IO

- ps aux
  
  - D - uninteruptable sleep

- vmstat 
  
  - bi, bo

- iostat -dx 1

- iotop -oPa

#### Performance metrics

- IOPS - 

- response times

- Workload - sequential / random - read vs write

- worker threads

- Queue depth

- throughput (data transfer rate)

#### tuning params

- I/O queue depth - deeper queue depth, increases IOPS, might increase latency

- readahead

- parallel sequential IO streams

- Large IO sizes
