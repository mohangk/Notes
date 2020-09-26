#### Context

- Exist to provide with a way to timeout and terminate goroutines
- Requests sometime need to be canceled, cancellation might happen in a different goroutine
- Golang servers create a goroutine to handle every request to it
- cancelation involves all the transitive dependencies of a request
- context tries to provide a standard way of handling cancellations
- context carries a cancelation signal and request scoped values to all function running on behalf of the same task
- passed in as the first argument to a function
- Incoming requests to a server should create a context
- Outgoing calls to servers should accept a context

###### Context Type

```
type Context interface {
    // Done returns a channel that is closed when this Context is canceled
    // or times out.
    Done() <-chan struct{}

    // Err indicates why this context was canceled, after the Done channel
    // is closed.
    Err() error

    // Deadline returns the time when this Context will be canceled, if any.
    Deadline() (deadline time.Time, ok bool)

    // Value returns the value associated with key or nil if none.
    Value(key interface{}) interface{}

}
```

Done is a channel that is closed whtn the context is canceled or times out. You select on both on context.Done and what you are waiting for to ensure things finish on time

###### Example of using context for a timeout

```
// All material is licensed under the Apache License Version 2.0, January 2004
// http://www.apache.org/licenses/LICENSE-2.0
// Sample program to show how to use the WithTimeout function
// of the Context package.
package main
import (
    "context"
    "fmt"
    "time"
)
type data struct {
    UserID string
}
func main() {

  // Set a duration.
  duration := 150 * time.Millisecond

  // Create a context that is both manually cancellable and will signal
  // a cancel at the specified duration.
  ctx, cancel := context.WithTimeout(context.Background(), duration)
  defer cancel()

  // Create a channel to received a signal that work is done.
  ch := make(chan data, 1)

  // Ask the goroutine to do some work for us.
  go func() {

      // Simulate work.
      time.Sleep(50 * time.Millisecond)

      // Report the work is done.
      ch <- data{"123"}
  }()

  // Wait for the work to finish. If it takes too long move on.
  select {
  case d := <-ch:
      fmt.Println("work complete", d)

  case <-ctx.Done():
      fmt.Println("work cancelled")
  }
}
```

##### Context using WithCancel, WithDeadline, WithTimeout, or WithValue

- WithCancel

- WithTimeout

- WithValue

##### Ensure that CancelFunc is called before the function returns

```
// Create a context that is both manually cancellable and will signal
// cancel at the specified duration.
ctx, cancel := context.WithTimeout(context.Background(), duration)
defer cancel()
```

- When using WithX - important to pass in the last created context

- It’s critically important that any `cancel` function returned from a `With` function is executed before that function returns. This is why the idiom is to use the `defer` keyword right after the `With` call, as you see on line 26. Not doing this will cause memory leaks in your program.

- Why though - Calling the CancelFunc cancels the child and its children, removes the parent's reference to the child, and stops any associated timers. Failing to call the CancelFunc leaks the child and its children until the parent is canceled or the timer fires.



##### Additonal notes

- [Cancelation, Context, and Plumbing](https://talks.golang.org/2014/gotham-context.slide)

- [Context Package Semantics In Go](https://www.ardanlabs.com/blog/2019/09/context-package-semantics-in-go.html)

- https://blog.golang.org/context
