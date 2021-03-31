---
draft: true
title: Database
categories:
  - Golang
---




#### pq

##### type conversion

- integer types smallint, integer, and bigint are returned as int64
- floating-point types real and double precision are returned as float64
- character types char, varchar, and text are returned as string
- temporal types date, time, timetz, timestamp, and timestamptz are
  returned as time.Time
- the boolean type is returned as bool
- the bytea type is returned as []byte
