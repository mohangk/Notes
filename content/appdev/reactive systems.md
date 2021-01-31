---
draft: true
title: Reactive Systems
categories:
  - Appdev
---
Reactive Programming is generally Event-driven, 

 •	Callback-based—where anonymous, side-effecting callbacks are attached to event sources, and are being invoked when events pass through the dataflow chain.
 •	Declarative—through functional composition, usually using well established combinators like map, filter, fold etc.

Most libraries provide a mix of these two styles, often with the addition of stream-based operators like windowing, counts, triggers, etc.

 •	Futures/Promises—containers of a single value, many-read/single-write semantics where asynchronous transformations of the value can be added even if it is not yet available.
 •	Streams—as in Reactive Streams: unbounded flows of data processing, enabling asynchronous, non-blocking, back-pressured transformation pipelines between a multitude of sources and destinations.
 •	Dataflow Variables—single assignment variables (memory-cells) which can depend on input, procedures and other cells, so that changes are automatically updated. A practical example is spreadsheets—where the change of the value in a cell ripples through all dependent functions, producing new values “downstream.

 Reactive Streams specification, which is a standard for interoperability between Reactive Programming libraries on the JVM
 
 Reactive Programming solves most of the challenges here since it typically removes the need for explicit coordination between active components.
 This means that an event-driven system focuses on addressable event sources while a message-driven system concentrates on addressable recipients.

Reactive Systems, which are Message-drive


Decoupling in time allows for concurrency, but it is decoupling in space that allows for distribution, and mobility—allowing for not only static but also dynamic topologies—which is essential for Elasticity.


A lack of location transparency makes it hard to scale out a program purely based on Reactive Programming techniques adaptively in an elastic fashion and therefore requires layering additional tools on top, such as a Message Bus, Data Grid or bespoke network protocols. This is where the Message-driven approach of Reactive Systems shines, since it is a communication abstraction that maintains its programming model and semantics across all dimensions of scale, and therefore reduces system complexity and cognitive overhead.

