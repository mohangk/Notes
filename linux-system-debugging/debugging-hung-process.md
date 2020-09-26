1.  What is the overall load of the box?
   
   1. top - look at load / look at per CPU utilization

2. What causes a process to get hung?
   
   1. cpu exhausted
   
   2. memory preassure
   
   3. run out of disk space or file descriptors
   
   4. run out of disk I/O - waiting
   
   5. run out of network i/o - waiting

3. What can you do to determine what is the cause of the issue

4. Identify PID of the process

5. Look at logs of the process 
   
   1. stdout 
   
   2. stderr
   
   3. syslog

1. Get list of open files and sockets

2. top to look at resource utilization
   
   1. is it maxed out?
   
   2. Is it completely deadlokced or livelocked?

3. For deadlocked
   
   1. Get the process stacktraces 

4. netstat / ss

5. lsof

6. vmstat 5 5 

7. iostat

8. free

9. Java - get a stack trace / get a core dump using gdb

10. check the syslogs - journalctl 

1. 
