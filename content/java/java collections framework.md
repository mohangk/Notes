---
draft: true
title: Java Collections Framework
categories:
  - Java
---

# java collections framework 
- only Objects  - no primitives

## Collection

- contiguous vs non contiguous collections
  - e.g. Array - contiguous 

- contiguous collection classes
  - ArrayList => contiguous
  - LinkedList => x continguous
  
Contiguous faster to do lookups, slower to do operations like remove

  
## Parameterized types / Generic types

- (un)boxing - converting from primitives to wrapper objects


## Collection interface

- Interface
  - add
  - addAll
  - clear
  - contains
  - containsAll
  - equals
  - hashCode
  - isEmpty
  - iterator
  - remove
  - removeAll
  - retainAll
  - size
  - toArray : Object[]
  - toArray: (a: T[]) : T[]
- E => “element” as the type parameter


### Iterator Interface
- boolean hasNext
- E next()
- void remove();

- returned by calling iterator() on the collection

### enhanced “for”

## list 

- List
- AbstractList - > List -> Collection 
- ArrayList, LinkedList, Stack