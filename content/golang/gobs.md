---
draft: true
title: Gobs
categories:
  - Golang
---
#### Introduction

- binary encoding format - similar to protobuf

- Gobs work with the language in a way that an externally-defined, language-independent encoding cannot. At the same time, there are lessons to be learned from the existing systems. Go centric.

- no need for protocol compile, just uses the data structure and reflection

- binary not text

- self-describing

##### Limitations of protocol buffers

1. Cannot encode basic types, everything needs to be a struct

2. Not self describing

3. Required fields, default values  - requires more implementation overhead. Alternative, everything is optional

##### How values are handled

Doesnt send the types like int8, unint16 - instead sends sizeless numbers, either signed or unsigned. Its up to the receiver how the data is going to be received as long as it fits. Allows for more decoupling and evolution.

##### Key methods

gob.Register - register any types that are used as part of the encoding

[Gobs of data - The Go Blog](https://blog.golang.org/gob)
