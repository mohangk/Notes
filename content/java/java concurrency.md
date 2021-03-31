---
draft: true
title: Java Concurrency
categories:
  - Java
---
# Java executers

- https://zeroturnaround.com/rebellabs/fixedthreadpool-cachedthreadpool-or-forkjoinpool-picking-correct-java-executors-for-background-tasks/
-
Here’s a short summary of how ExecutorServices that are created by the factory methods in the Executors class differ from each other. I hope this will shed some light on how do they resolve the important questions above.

newFixedThreadPool(int nThreads) – n threads will process tasks at the time, when the pool is saturated, new tasks will get added to a queue without a limit on size. Good for CPU intensive tasks.

newWorkStealingPool(int parallelism) – will create and shut down threads dynamically to accommodate the required parallelism level. It also tries to reduce the contention on the task queue, so can be really good in heavily loaded environments. Also good when your tasks create more tasks for the executor, like recursive tasks.

newSingleThreadExecutor() – creates an unconfigurable `newFixedThreadPool(1)`, so you know that only one thread will process everything. Good when you really need predictability and sequential tasks completion.

newCachedThreadPool() – doesn’t put tasks into a queue. Consider this as the same as using a queue with the maximum size of 0. When all current threads are busy, it creates another thread to run the task. Sometimes it can reuse threads. Good for denial of service attacks on your own servers. The problem with a cached thread pool is that it doesn’t know when to stop spawning more and more threads. Imagine the situation where you have computationally intensive tasks that you submit into this executor. The more threads that consuming the CPU, the slower every individual task takes to process. This has a domino effect in that it means more work gets backlogged. As the result you’ll end up with more and more threads spawned making task processing even slower. This negative feedback loop is a hard problem to solve.

So for almost all intents and purposes, Executors::newFixedThreadPool(int nThreads)should be your goto choice for when you need a thread pool. For computationally intensive tasks it will probably give you close to optimal throughput and for IO heavy tasks you won’t be that much worse than anything else. At least until you profile that you got a problem with this kind of executor, you shouldn’t mindlessly try to optimize it.

# fork / join commonPool

Java 8 included minor features in it. It included a default ForkJoinPool object. You can obtain it using the static method, commonPool(), of the ForkJoinPool class. This default fork/join executor will by default use the number of threads determined by the available processors of your computer. You can change this default behavior by changing the value of the system property, java.util.concurrent.ForkJoinPool.common.parallelism. This default pool is used internally by other classes of the Concurrency API. For example, Parallel Streams use it. Java 8 also included the CountedCompleter class mentioned earlier.
