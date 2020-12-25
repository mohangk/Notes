[CodeReviewComments · golang/go Wiki · GitHub](https://github.com/golang/go/wiki/CodeReviewComments#receiver-type)

[go - Pointers vs. values in parameters and return values - Stack Overflow](https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values)

- Methods using receiver pointers are common; [the rule of thumb for receivers is](https://github.com/golang/go/wiki/CodeReviewComments#receiver-type), "If in doubt, use a pointer."
- Slices, maps, channels, strings, function values, and interface values are implemented with pointers internally, and a pointer to them is often redundant.
- Elsewhere, use pointers for big structs or structs you'll have to change, and otherwise [pass values](https://github.com/golang/go/wiki/CodeReviewComments#pass-values), because getting things changed by surprise via a pointer is confusing.

-----

nil values in go

[go - What does nil mean in golang? - Stack Overflow](https://stackoverflow.com/questions/35983118/what-does-nil-mean-in-golang)

In Go, `nil` is the **zero value for pointers, interfaces, maps, slices, channels and function types**, representing an *uninitialized* value.

`nil` doesn't mean some "undefined" state, it's a proper value in itself. An object in Go is `nil` simply if and only if it's value is `nil`, which it can only be if it's of one of the aforementioned types.

An `error` is an interface, so `nil` is a valid value for one, unlike for a `string`. For obvious reasons a `nil` error represents *no error*.
