---
draft: true
title: Kafka
categories:
  - Distributed-Systems
---
## kafka up and running

just run zookeeper and kafka locally

to run multiple kafka nodes, set the broker_id, log and port to different values

http://cloudurable.com/blog/kafka-tutorial-kafka-from-command-line/index.html

## key concepts

1. Producer

2. Broker
   
   1. The leader maintains a set of in-sync replicas (ISR): the set of replicas that have fully caught up with the leader. For each partition, we store in Zookeeper the current leader and the current ISR.

3. Consumer
   
   1. Consumer groups
      1. Multiple consumers for a topic from the same group will end up sharing the partitions. Kafka can automatically balance them ensuring that the consumers will always be able to get the messages
      2. Separate consumer groups get the same messages 

4. Partition
   
   1. The partitions in the log serve several purposes. First, they allow the log to scale beyond a size that will fit on a single server. Each individual partition must fit on the servers that host it, but a topic may have many partitions so it can handle an arbitrary amount of data. Second they act as the unit of parallelism—more on that in a bit.
   
   2. The partitions of the log are distributed over the servers in the Kafka cluster with each server handling data and requests for a share of the partitions. Each partition is replicated across a configurable number of servers for fault tolerance.
      
      Each partition has one server which acts as the "leader" and zero or more servers which act as "followers". The leader handles all read and write requests for the partition while the followers passively replicate the leader. If the leader fails, one of the followers will automatically become the new leader. Each server acts as a leader for some of its partitions and a follower for others so load is well balanced within the cluster.
   
   Note however that there cannot be more consumer instances in a consumer group than partitions.

#### important config options

- acks  - 0 => producer dont wait for anything, 1=> producer wait for leader to respond, all => when all IRS (in sync replicas) have ack

- min-insync.replicas broker/topic

### performance metrics

- ISR - in sync replica - The ISR is simply all the replicas of a partition that are "in-sync" with the leader. The definition of "in-sync" depends on the topic configuration, but by default, it means that a replica is or has been fully caught up with the leader in the last 10 seconds. The setting for this time period is: replica.lag.time.max.ms and has a server default which can be overridden on a per topic basis.

- what breaks the zero copy optimisation for kafka
  
  - down conversion for legacy consumers
  
  - lagging consumers
  
  - log compaction

- [https://www.slideshare.net/ConfluentInc/kafka-on-zfs-better-living-through-filesystems](https://www.slideshare.net/ConfluentInc/kafka-on-zfs-better-living-through-filesystems)
