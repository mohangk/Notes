---
draft: true
title: Go Concurrency Patterns
categories:
  - Golang
---
##### Go Concurrency Patterns

- the perctiption of doing multpile things at the same time

###### Goroutines

- go 'func' - almost like running a cmd with & on bash

###### Channels

- channels enable cross go routine communication
- Channles also serve as a means for cross go routine coordination/ synchronization as they block

###### Select

- blocks until it at least one statement can proceed and executes it
- if more than one ready, chooses it pseudo-randomly
- default: what gets run if nobody can proceed

###### Pattern: Generator function that returns a channel

`func main() {
  c := boring("boring!")
  for i = 0; i < 5; i++ {
      fmt.Println("you say %q\n", <-c)
  }
  fmt.Println("Im done")
}`

`func boring(string) <-chan string {
  c := make(chan string)`

`  go func {
      for i=0; ; i++ {
          c <- fmt.Sprintf("%s %d", string, i)
          time.Sleep(time.Duration(rand.Intn(1e3)) * time.Miliseconds)
      }
  }()`

`  return c
}`

What happens if we do

`func main() {
  c := boring("channel 1")
  c2 := boring("channel 1")
  for i = 0; i < 5; i++ {
      fmt.Println("you say %q\n", <-c)
      fmt.Println("you say %q\n", <-c2)
  }
  fmt.Println("Im done")
}`

c will always block c2 even if c2 is ready

###### Pattern: Multiplexing - fan in

Take multiple channels and push it through one channel

func fanIn(input1, input2 <-chan string) <-chan string {
    c := make(chan string)
    go func() {for { c <- <-input1 }}()
    go func() {for { c <- <-input2 }}()
    return c
}

###### Pattern: Put a wait channel in the channel that was returned, reintroduces sequencing

type Message struct {
    messaging string
    wait chan bool
}

func boring(string) <-chan string {
  c := make(chan string)
  waitForIt := make(chan bool)

  go func {
      for i=0; ; i++ {
          c <- Message {fmt.Sprintf("%s %d", string, i), waitForIt)
          time.Sleep(time.Duration(rand.Intn(1e3)) * time.Miliseconds)
          <- waitforIt
      }
  }()

https://www.youtube.com/watch?v=f6kdp27TYZs&t=376s
