---
draft: false
title: Arrays, slices & maps
categories:
  - Golang
---

## Arrays

- Fixed size, cannot be changed. From a syntax standpoint, this is what separates it from slices.
- The size is part of the type definition. [5]int != [4]int
- An array is a value, passing it around means copying the value (unlike C where an array is a pointer to the first element in memory). Can create pointers to arrays and pass it around for the same effect.

```golang
  a := [5]int
  b := [10]SomeStruct
  c :=[...]int{1,2,3}  // but this is a slice --> c:=[]int{12,13,14,15}
```

## Slices

- Slices are references to arrays, built on top of array
- Size is not fixed and can be increased using the `append` built-in function 
- Syntatically the difference between a slice and array is omitting the size or `...` in the definition

### Slicing
- You can create a new slice  by "slicing" an existing slice or array
- An important point to remember is when "sliced" the new slice shares the underlying storage

```golang
first := []byte{'g','o','l','a','n','g'}
second := first[1:3]
fmt.Printf("%s\n",first) // golang
fmt.Printf("%s\n",second) // ol

second[1]='X'
fmt.Printf("%s\n",first) // goXang
fmt.Printf("%s\n",second) // oX
```
### References 
 - https://blog.golang.org/slices-intro
 - https://blog.golang.org/slices
 - https://research.swtch.com/godata
 - [https://www.openmymind.net/The-Minimum-You-Need-To-Know-About-Arrays-And-Slices-In-Go/](https://www.openmymind.net/The-Minimum-You-Need-To-Know-About-Arrays-And-Slices-In-Go/)
- 
- [https://blog.fraixed.es/post/golang-slices-structs-or-pointers-to-structs-dilemma/](https://blog.fraixed.es/post/golang-slices-structs-or-pointers-to-structs-dilemma/)

## Maps
- Valid keys - comparable types:  boolean, numeric, string, pointer, channel, and interface types, and structs or arrays that contain only those types

### Initialising

```golang
var m map[string]int
```

- Map types are reference types, like pointers or slices, and so the value of `m` above is `nil`; it doesn't point to an initialized map

- Initialise a nil map with make `m := make(map[string]int)`

### Getting and setting
  - a := m["doesntexist"] - a will be assigned the "zero value" of that type
  - a, ok := m["doesntexist"] - ok will be assigned false
  - _, ok := m["doesntexist"]_- same thing as above
  - m["exists"] = 1

### Deleting
  - delete(m, "exists")

### Iterating
  - for k, v := range m {}

### References 
- [Go maps in action - The Go Blog](https://blog.golang.org/maps)

## FAQ
- Should you create an array of pointers. For example, which is better?
```golang  
  sayans := []*Sayan{
    &Sayan{Name: "Goku", Power: 9001,}
  }
```
