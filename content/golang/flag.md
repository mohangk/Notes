---
draft: true
title: Flag
categories:
  - Golang
---
##### flag module

###### flag.String

import (
    "flag"
    "log"
)

addr := flag.String("addr", ":4000", "Some basic help")
flag.Parse()
log.Println("addr has been set to %s", *addr)

###### Other flag.X options

- flag.    Int()
- flag.Float64()
- flag.Bool()

###### flag.StrVar()
