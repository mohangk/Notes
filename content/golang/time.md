---
draft: true
title: Time
categories:
  - Golang
---
##### time

##### important concepts

- Duration - represents the elapsed time as an int64 nanosecondcount
- to convert seconds to Duration --> time.Duration(seconds)*time.Second
- to convert minutes to Duration --> time.Duration(minutes)*time.Minute
- to convert from Duration to minute --> seconds := time.Second*10; seconds/time.Second
  
  ##### important Constants
- time.Second, time.Minute etc
