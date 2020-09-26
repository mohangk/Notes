####Bumping up the TCP buffer size, i.e.

### Key networking concepts Concepts

 • MTU - maximum transmission unit - is the largest size packet or frame, specified in octets (eight-bit bytes), that can be sent. Too small, headers overhead is larger. Too large - can’t be processed by some routers.  
 • delay (milliseconds) - the time it takes for one packet to get from the beginning to the end of a link
 •    bandwidth (megabits/second) - the number of packets that can get through the link in a second
 •    RTT - time sender sends something, sits in queue, goes over link and get an ack back from recipient will go up
 •    queue - the size of the queue for storing packets waiting to be sent out if a link is full, and the strategy for managing that queue when it hits its capacity
 •    BDP - This is a product of the bandwidth and the delay, and is the number of bytes that can fit on the link at any given point in time. This can be thought of the volume of the pipe in the pipe analogy.

How is it determined that that congestion is happening?

- packet loss and increased round trip times.

How TCP guarantees delivery?

1. Receiver always needs to ACK

### Networks/tcp kernel optimisations notes

[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/linuxonibm/liaag/wkvm/wkvm_c_tune.htm)

[Collection of /etc/sysctl.conf networking notes. - bl.ocks.org](https://bl.ocks.org/magnetikonline/2760f98f6bf654d5ad79)

#### speeding up connection handling

SO_TCP_NODELAY
SO_REUSEADDR SO_REUSEPORT
SO_SYN__RETRY
SO_USER_TIMEOUT

#### pending connections

**net.core.somaxconn = 16384**

To peek into the SYN Queue on Linux we can use the `ss` command and look for `SYN-RECV` sockets. For example, on one of Cloudflare's servers we can see 119 slots used in tcp/80 SYN Queue and 78 on tcp/443.

```
$ ss -n state syn-recv sport = :80 | wc -l
119
$ ss -n state syn-recv sport = :443 | wc -l
78
```



**net.ipv4.tcp_abort_on_overflow**

[Is net.ipv4.tcp_abort_on_overflow good or not? · ton31337/tools Wiki · GitHub](https://github.com/ton31337/tools/wiki/Is-net.ipv4.tcp_abort_on_overflow-good-or-not%3F)

#### Bumping up the TCP buffer size to deal with slow clients

**net.core.rmem_max = 134217728**

The maximum socket receive buffer size which may be set by using the SO_RCVBUF socket option

**net.core.wmem_max = 134217728**

 The maximum socket send buffer size which may be set by using the SO_RCVBUF socket option

Increase TCP read/write buffers to enable scaling to a larger window size. Larger windows increase the amount of data to be transferred before an acknowledgement (ACK) is required. This reduces overall latencies and results in increased throughput.

This setting is typically set to a very conservative value of 262,144 bytes. It is recommended this value be set as large as the kernel allows. The value used in here was 4,136,960 bytes. However, 4.x kernels accept values over 16MB.

**net.ipv4.tcp_rmem = 4096 87380 134217728**

Contains three values that represent the minimum, default and maximum size of the TCP socket receive buffer.

The minimum represents the smallest receive buffer size guaranteed, even under memory pressure. The minimum value defaults to 1 page or 4096 bytes.

The default value represents the initial size of a TCP sockets receive buffer. This value supersedes net.core.rmem_default used by other protocols. The default value for this setting is 87380 bytes. It also sets the tcp_adv_win_scale and initializes the TCP window size to 65535 bytes.

The maximum represents the largest receive buffer size automatically selected for TCP sockets. This value does not override net.core.rmem_max. The default value for this setting is somewhere between 87380 bytes and 6M bytes based on the amount of memory in the system.

The recommendation is to use the maximum value of 16M bytes or higher (kernel level dependent) especially for 10 Gigabit adapters.

**net.ipv4.tcp_wmem = 4096 65536 134217728**

The minimum represents the smallest receive buffer size a newly created socket is entitled to as part of its creation. The minimum value defaults to 1 page or 4096 bytes.

The default value represents the initial size of a TCP sockets receive buffer. This value supersedes net.core.rmem_default used by other protocols. It is typically set lower than net.core.wmem_default. The default value for this setting is 16K bytes.

The maximum represents the largest receive buffer size for auto-tuned send buffers for TCP sockets. This value does not override net.core.rmem_max. The default value for this setting is somewhere between 64K bytes and 4M bytes based on the amount of memory available in the system.

The recommendation is to use the maximum value of 16M bytes or higher (kernel level dependent) especially for 10 Gigabit adapters.

#### keepalive configs

net.ipv4.tcp_keepalive_time = 600
net.ipv4.tcp_keepalive_intvl = 60
net.ipv4.tcp_keepalive_probes = 20

 [When TCP sockets refuse to die](https://blog.cloudflare.com/when-tcp-sockets-refuse-to-die/)
https://blog.cloudflare.com/syn-packet-handling-in-the-wild/
http://veithen.io/2014/01/01/how-tcp-backlog-works-in-linux.html
https://serverfault.com/questions/518862/will-increasing-net-core-somaxconn-make-a-difference

### congestion window

- The congestion window refers to the number of segments that a sender can send without having seen an acknowledgment yet.
- referred to as the fight size as well
- maintained by the sender
- From a theoretical perspective, the right size congestion window to use is the bandwidth-delay product of the link - full capacity of the link

##### TCP Tahoe congestion algo

2 phases:

a. Slow start

- grows the congestion window by 1 after all segments last sent are ack-ed. Doubles the window and every round trip until hits a slow start threshold

b. Congestion avoidance mode

- congestion window increases by 1 on every round trip. So if the congestion window is 4, the window will increase to 5 after all four of those packets in flight have been acknowledged. This increases the window much more slowly.

On packet loss:
If Tahoe detects that a packet is lost, it will resend the packet, the slow start threshold is updated to be half the current congestion window, the congestion window is set back to 1, and the algorithm goes back to slow start.’

##### Detecting packet loss

a. Sender times out - Not receiving ack within specific time 
b. receiver sends back duplicate acks - In TCP, receivers only acknowledge packets that are sent in order. If a packet is sent out of order, it will send out an acknowledgement for the last packet it saw in order. So, if a receiver has received segments 1,2, and 3, and then receives segment #5, it will ack segment #3 again, because #5 came in out of order. In Tahoe, if a sender receives 3 duplicate acks, it considers a packet lost. This is considered “Fast Retransmit”, because it doesn’t wait for the timeout to happen

##### Downsides of Tahoe

- Long time to fully utilise the available bandwidth
- Wifi is lossy - dropping back to one is not ideal
- Waiting for packet loss as an indicator of congestion is too late

##### Other congestion algorithms

CUBIC: This algorithm was implemented in 2005, and is currently the default congestion control algorithm used on Linux systems. Like Tahoe, it relies on packet loss as the indicator of congestion. However, unlike Tahoe, it works far better on high bandwidth networks, since rather than increasing the window by 1 on every round trip, it uses, as the name would suggest, a cubic function to determine what the window size should be, and therefore grows much more quickly.

BBR (Bufferbloat): This is a very recent algorithm developed by Google, and unlike Tahoe or CUBIC, uses delay as the indicator of congestion, rather than packet loss. The rough thinking behind this is that delays are a leading indicator of congestion–they occur before packets actually start getting lost. Slowing down the rate of sending before the packets get lost ends up leading to higher throughput.

http://squidarth.com/rc/programming/networking/2018/07/18/intro-congestion.html
