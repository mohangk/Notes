grpc discussion


channels
observable -> StreamObserver
   - onNext, onError, onCompleted
metadata & context propogation
server is async
how do you add new items and detect on the server / client?

rxJava

—
mvn -o install
mvn dependency:go-offline 
#list all open ports 
sudo lsof -iTCP -sTCP:LISTEN -P -n
— 

layer4 loadbalancing - proxy based 
layer7 - http loadbalancing- envoy, 

lookaside loadbalancer

- http2 server shutdown, goaway frame 
- connections recycling
---

# grpc call interface
 •	Unary calls: a classic request/response interaction
 •	Server streaming calls: a single client request triggers multiple response from the server
 •	Client streaming calls: a client sends multiple requests to the server which replies with a single response
 •	Bidirectional streaming calls: a client sends multiple requests to the server with replies with multiple responses. Client requests and server responses can be intertwined.


gclb - http2
export stats from the grpc service
promethous, zipkin - request tracing


# testing
https://stackoverflow.com/questions/37552468/how-to-unit-test-grpc-java-server-implementation-functions

# load balancing

## client side
https://github.com/saturnism/grpc-java-demos/blob/master/kubernetes-lb-example/README.md

## server side
- trafeik
- envoy


# overview
https://ndolgov.blogspot.my/2016/09/asynchronous-rpc-server-with-grpc.html

#grpc - production notes
https://news.ycombinator.com/item?id=12345223


# grpc in production
  - 2 thread pools Netty event loops and application executor. 
  - application executors uses a cachedThreadPool by default to reuse threads; on server-side it's a good idea to replace the default executor with your own, generally fixed-size, pool via ServerBuilder.executor().
Internally gRPC Java uses the Netty-style. That means fully non-blocking. You may use ServerBuilder.directExecutor() to run on the Netty threads. Although in that case you may want to specify the NettyServerBuilder.bossEventLoopGroup(), workerEventLoopGroup(), and for compatibility channelType().
- load testing
  - using tools to load test  
- test frameworks
- debugging tools 
  - grpccurl
- java thread introspection
   - how to debug a java process going amok 
     - first you need to identify that its going amok => memory, threads not responding even to SSH  

http://www.baeldung.com/java-util-concurrent




