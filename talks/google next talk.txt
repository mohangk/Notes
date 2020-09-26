Improvements

Slide 23 - provide a diagram
Slide 22 - rewrite the lift & Shift vs re-engineer points
#talk feedback
  - provide more graphics
  - why not migrate to kurbenetes
  - more discussion about what GCP enabled for us

1. Get some bigdata numbers
1. Get number for logging and metric
2. Get GCC setup network diagram
3. Get GCC performance metrics - throughput, 
4. Get BBM service dependency graph
5. Get information from C* deployment sizing
6. Get information on C* driver configs - coordinator, CL & strategy
- https://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlClientRequestsWrite.html
7. Get some graphs of client traffic cutover

https://docs.google.com/presentation/d/1CExrgkz4Z73We8g7Nes0R8AF78Hu6qUg5RDwWe6siaI/edit#slide=id.g2bf8eaf876_3_97

Background



Migrating from legacy systems

1. Inventory of the existing systems

2. Cloud selection
- Cost
- Performance
- compatibilty


2. Mapping of services to GCP components
   - Loadbalancers
     -  F5 loadbalancers with fancy operations
   - Storage backends
      - Large NAS appliances
   - Networking MULTICAST
     -  gridcaching
   - Database migrations
     - Moved off Oracle 

3. Service reengineering - shims
  - Reengineer or lift and shift  - hybrid approach
  - Any large on prem application has file stores - mapping that to GCP
  - mapping users systems?
  
  
  5. Service rollout on GCP
   - Setup GCC and allow for routable between DC
   - Rollout DBs first
   - Rollout services for internal APIs (over GCC)

  
4.  Taffic cutover 

- Avoid big bang cutovers
- Leverage GCC to setup connections between your DCs  
- Databases
  - RDBMS - can’t avoid, master slave promotion
  - C* - separate DCs
  - caches - dual writes
- Proxying requests through for internal service calls
  
      
6. Client side traffic cutover
- Traffic cutover
 - Client side  traffic cutover
 - Proxying request over GCC
 
 - Add some before and after numbers
 - include the tactical work - who implemented it, what lessons learnt, you can’t provide too much information, tools that were used, methods that were relied on, progress lessons learnt, too much
 
 
 Learnings
 
1. Communication between groups will be your largest non technical challenge - prepare for that
2. Lift and shift is not always the easiest
3. At scale cost / performance of some of the default solutions might not be suitable
4. Get advice, but always determine solutions for your use case 
5. Bake in the ability to fail and learn - no plan ever goes 100% well
 