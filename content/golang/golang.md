---
draft: true
title: Golang
categories:
  - Golang
---
### io

#### key concepts

io.Reader, io.Writer, ioutil.ReadAll, ioutil.Tee , strings.NewReader, ioutil.NopCloser

### []byte vs string in golang

[]byte is mutable, string is immutable

Convert from []byte to string --> string([]byte)

#### useful functions in ioutil, strings

ioutil.ReadAll ==> read everyting from io.Reader into a byte array

ioutil.Tee ==> duplicate the io.Reader

strings.NewReader ==> convert from string to io.Reader

### defer

execute after the function returns. Replacement for finally

`f, err = os.Create("/tmp/test.log")`
`defer f.Close() `
`f.WriteString("blabla")`

### delv goland step debugger

#### start debugging

`delv debug <path to compile>`

e.g.

`delv debug app/generator/main.go`

#### setting breakpoint

while in debugger set breakpoint:

`b main.main`

or at the code level

`runtime.Breakpoint()`

### maps

m := make(map[key-type][value-type])

#### initialize the map

Eiter with a map literal
n := map[string]int{"foo": 1, "bar": 2}

Or builtin make function
m := make(map[string]float64)
m["pi"] = 3.1416

### single line if...else statements in Go

`if err := doStuff(); err != nil {`
  `// handle the error here`
`}`

https://www.calhoun.io/one-liner-if-statements-with-errors/

### unit testing in go

#### Running  tests

go test -- run local tests
go test ./... -- list all packages and run tests for them

#### Conventions

_test.go and func TestXxx(t *testing.T) 

### select

Wait for channels to have a response and then executes the case and comes out of the loop. If want to keep executing needs to be put into an infinite for loop “for { }”

select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }

#### select & channels to implement timeouts

c2 := make(chan string, 1)
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "result 2"
    }()
    select {
    case res := <-c2:
        fmt.Println(res)
    case <-time.After(3 * time.Second):
        fmt.Println("timeout 2")
    }

### channels

Create a new channel with “ channelName := make(chan val-type)”. Channels are typed by the values they convey.

go func() { messages <- "ping" }()

By default sends and receives block until both the sender and receiver are ready. This property allowed us to wait at the end of our program for the "ping" message without having to use any other synchronization.

#### channel buffering

messages := make(chan string, 2)
Here we make a channel of strings buffering up to 2 values.

#### channels synchronisation

- due to the fact that the by default the receive and send blocks, by adding a “<- message” at the end of a function, can block execution to wait for the “-> message” to be sent first, which could be happening in a go routine

#### channels as params to functions

values. It would be a compile-time error to try to receive on this channel.
func ping(pings chan<- string, msg string) {
    pings <- msg
}

The pong function accepts one channel for receives (pings) and a second for sends (pongs).

func pong(pings <-chan string, pongs chan<- string) {
    msg := <-pings
    pongs <- msg
}

Any direction

func worker(done chan bool) {

### go routines

go f(s). This new goroutine will execute concurrently with the calling one.

### errors

Error is a  builtin interface 

type error interface {
    Error() string
}

errors.New constructs a basic error value with the given error message.
        return -1, errors.New("can't work with 42")

A nil value in the error position indicates that there was no error.

### variables & constants

var a String = “blah” 
var b, c int = 1, 2

“:= “ - shorthand for declare and initialise

### asterix and ampersands

- In Go assigning one variable to another one creates a copy of the source value and no reference between them is kept. When the source changes the target variable will not change to follow.
  package main

import "fmt"

func main() {
    var a int   // a variable of type 'int'
    var b int   // another 'int'
    var c *int  // a variable of type 'pointer to int'

    a = 42      // assigning value '42' to 'a'
    b = a       // copying the value from 'a' to 'b'
    c = &a      // assigning the address of 'a' to 'c'
    
    fmt.Printf("Values:\n;
     a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
    
    a = 21      // assigning a new value of '21' to 'a'
    
    fmt.Printf("Values:\n a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
    fmt.Printf("Types:\n a:  %T\n b:  %T\n c: %T\n*c:  %T\n", a, b, c, *c)

}

Values:           // after the initial assignment
 a = 42           // no surprizes here
 b = 42           // exactly what we expect
 c = 0x21015a018  // OK, so that's that an 'address' looks like
*c = 42           // 'unwraped' or 'dereferenced' value of '*c'
Values:           // after the second assignment
 a = 21           // ok, that's to be expected
 b = 42           // ooo... ok, so 'b' is a copy of 'a' stored under different address
 c = 0x21015a018  // hmmm... and this is the same address that we have seen previously
*c = 21           // but the value stored under this addres has changed
Types:            // to ilustrate once more that a pointer is a different type
 a:  int          // than the variable that it points to
 b:  int
 c: *int
*c:  int
Run this code at
