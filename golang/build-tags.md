##### Introduction

- a comment placed at a top of a file, before the packge declaration

- allow customising what gets built - e.g.
  
  - building for multi target environments - eg. linux, macos (crosscompiling)
  
  - for integration testing - allows for mocks to be included instead
  
  - features are included in the build only when the tag is passed into go build

- E.g - file pro.go. There is also main.go

```
// +build pro

package main

func init() {
  features = append(features,
    "Pro Feature #1",
    "Pro Feature #2",
  )
}
```

- To build 
  
  go build -tags pro

- Multiple build tags on  a file 
  
  - space separated - OR logic
  
  - comma separate - AND logic
  
  - ! - NOT tag
  
  


