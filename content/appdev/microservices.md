---
draft: true
title: Microservices
categories:
  - Appdev
---
## Monolith to microservices

https://martinfowler.com/articles/break-monolith-into-microservices.html


## Common patterns

### CQRS 

- At the very basic: Different models for reading data and writing data. At the most extrement, different data models for the read and writing data.

#### Use cases
- Use sparingly, can get super complicated quickly and diminish the benefits
- One use case: High performance systems where the read and write systems need to be modeled differently

#### References
- https://martinfowler.com/bliki/CQRS.html
- http://codebetter.com/gregyoung/2010/02/16/cqrs-task-based-uis-event-sourcing-agh/

### Event sourcing

#### Benefits
- The ability to create event driven systems. Clients that are added to the processing stream with minimal impact on the overall system

#### Reference
- https://martinfowler.com/eaaDev/EventSourcing.html

