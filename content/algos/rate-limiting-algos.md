---
draft: false
title: Rate Limiting Algos
categories:
  - Algos
---
## Rate limiting algos

Algorithms are another way to create scalable rate-limited APIs. As with request queue libraries and throttling services, there are many rate-limiting algorithms already available.

### Leaky Bucket

The leaky bucket algorithm is a simple, easy-to-implement rate-limiting solution. It translates requests into a First In First Out (FIFO) format, processing the items on the queue at a regular rate.

Leaky Bucket smooths outbursts of traffic, easy to implement on a single server or load balancer. It’s also small and memory-efficient, due to the limited queue size.

### Fixed Window

Fixed window algorithms use a fixed rate to track the rate of requests using a simple incremental counter. The window is defined for a set number of seconds, like 3600 for one hour, for example. If the counter exceeds the limit for the set duration, the additional requests will be discarded.

The fixed window algorithm is a simple way to ensure your API doesn’t get bogged down with old requests. Your API can still be overloaded using this method, however. If a slew of requests is made when the window refreshes, your API could still be stampeded.

### Sliding Log

A sliding log algorithm involves tracking each request via a time-stamped log. Logs with timestamps that exceed the rate limit are discarded. When a new request comes in, the sum of the logs are calculated to determine the request rate. If the request exceeds the limit threshold, they are simply queued.

Sliding log algorithms don’t suffer from the stampeding issues of fixed windows. It can get quite expensive to store an unlimited amount of logs for each request. Calculating the number of requests across multiple servers can also be expensive. Sliding log algorithms aren’t the best for scalable APIs, preventing overload, or for preventing DoS attacks.

### Sliding Window

Sliding window algorithms combine the best of Fixed Window and Sliding Log algorithms. A cumulative counter for a set period is used, similar to the fixed window algorithm. The previous window is also assessed to help smooth outbursts of traffic.

The small number of data points needed to assess each request makes the sliding window algorithm an ideal choice for processing large amounts of requests while still being light and fast to run.

