Phase 1:  Replatform and transition

- CI 
  - Approach to source code mgmt and collaboration (trunk based development, rebase not merge, feature toggles, short lived branches), ensure Github is your source of truth
  - Approach to testing - test pyramid, developers drive tests, test granularity => just started

- CD
  - Approach to config management (Garuda) 
  - Approach to deployment and testing  
  - Cloud enabled
  - Client release management

Phase 2:  Rearchitecture

- Cloud native
- Improve testing => TDD
- Improve speed
- 

# Why 

## Targets
- time to build
- time for tests to run
- time to deploy
- time to ramp up new developers
  - remove need for evolutionary knowledge of the system 

### Faster
- Faster build 
- Faster test cycle
- Easier to ramp up new engineers
- Deployment - faster & less 
- Consistent


## Client

-  bbmcore plans
  - Move REST layer over
  - Enable DB  in native code (Android - Java, iOS - Objective C
  -  Remove dead code
    - Olympia, worker, 
- Moving to a standard code organisation 
  - iOS - clean architecure approach on BBMID 
  - get swift support into the codebase
  - introduce real dependency management - CocoaPods

## Server
## How

- Make it responsibility more clearer   - especially between major components

- split up services
  - tradeoffs, more complexity -> response times, reliabilty, changing contracts vs faster build, test, reduce dependency, faster deployments, maps to pillars
  - smaller code bases
  - clean up dead code
    - e.g. olympia dependencies
    - sip code base larger than C* 
  - add more tests
  - document configuration options
  
  - BUS
    - groups goes away
    - identity is split    
    - contacts 
    - profile
  
  - SIP
    - S&F
    - Sip and Directory
    - PNS


Build for a 
  
-  enable autoscaling
  - simplify bootups
  - instances come and go - the amount of instances are not fixed
    - e.g. DAI requiring a fix amount of instances to manage  

- Refine dependencies between components
  - BUS & SIP - pin management => move it behind an API
  - Channels and DAI => move moderation out of DAI and into channels
  - Move client ad config out of channels and potentially into its own service
  - Split up Ad Ops - build your own portals, potentially within the same app

  
- New RP token with all identifiers (pin, ecoid, regid)
  - Removes need for bus internal API for users who have upgraded

- Use GRPC instead of REST for internal services
  - faster
  - better contract management .proto files 

- Use Google cloud pub/sub 
  - build fire and forget interfaces 

- remove dependency on bbm-commons
  - better then where we were when we first cut over, but not the ideal direction
    
- Leverage Google cloud services
  - Cloud pub sub
  - Bigdata / Cloud spanner
     
- Upgrade dependencies
  - Netty 
  - Spring boot
  - C* drivers?
  - common deployment and config models
    
- New languages - Kotlin

- Moving to our own builds
  - 
##

  
# Considerations

- How do we make the change
- Adding rest calls - how do we make it more resiliant
- Circuit breakers - services put a CB in place
