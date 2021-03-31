---
draft: true
title: Haproxy
categories:
  - Webtech
---
1. setup haproxy and test reloading dynamically 
- first try how it handles adding backends with a reload  when there are long lived connections
- setup haproxy to forward long lived tcp connections to a couple of listening nc servers 
- connect to some of them
- then add one new nc backednto the config and reload the config
- the old connections should still work
- also limit maxconn for backend to 1

2. try reloading using consul to add a new backend definition


— 

## Concepts:

load balancing algos:
leastconn (for long connections, pick the least recently used of the servers
    with the lowest connection count),
    
dynamic weights are supported for round-robin, leastconn and consistent
    hashing ; this allows server weights to be modified on the fly from the CLI
    or even by an agent running on the server;

HAProxy supports information sampling using a wide set of "sample fetch
functions"

Maps are a powerful type of converter consisting in loading a two-columns file
into memory at boot time, then looking up each input sample from the first
column and either returning the corresponding pattern on the second column if
the entry was found, or returning a default value. The output information also
being a sample,

Most operations in HAProxy can be made conditional. Conditions are built by
combining multiple ACLs using logic operators (AND, OR, NOT). Each ACL is a
series of tests based on the following elements 

Stick-table contents may be replicated in active-active mode with other HAProxy
nodes known as "peers" as well as with the new process during a reload operation
so that all load balancing nodes share the same information and take the same
routing decision if client's requests are spread over multiple nodes.

Protocol inspection is not limited to HTTP,
it is also available for other protocols like TLS or RDP.

When a protocol violation or attack is detected, there are various options to
respond to the user, such as returning the common "HTTP 400 bad request",
closing the connection with a TCP reset, or faking an error after a long delay
("tarpit") to confuse the attacker. All of these contribute to protecting the
servers by discouraging the offending client from pursuing an attack that
becomes very expensive to maintain.

## configuration file

HAProxy's configuration process involves 3 major sources of parameters :

  - the arguments from the command-line, which always take precedence
  - the "global" section, which sets process-wide parameters
  - the proxies sections which can take form of "defaults", "listen",
    "frontend" and "backend".


later:

https://cloud.google.com/solutions/best-practices-floating-ip-addresses