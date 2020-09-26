### Arrays

- fixed size

- a := [5]int

- b := [10]SomeStruct

- c :=[...]int{1,2,3}  // but this is a slice --> c:=[]int{12,13,14,15}

- Arrays cannot change 

### Slices

- slices are references to arrays, size can change

- Methods
  
  - append - 

- Should you create an array of pointers. For example, which is better?
  
  sayans := []*Sayan{
    &Sayan{Name: "Goku", Power: 9001,}
  }
  //or
  sayans := []Sayan{
    Sayan{Name: "Goku", Power: 9001,}
  }

[https://www.openmymind.net/The-Minimum-You-Need-To-Know-About-Arrays-And-Slices-In-Go/](https://www.openmymind.net/The-Minimum-You-Need-To-Know-About-Arrays-And-Slices-In-Go/)

[https://blog.fraixed.es/post/golang-slices-structs-or-pointers-to-structs-dilemma/](https://blog.fraixed.es/post/golang-slices-structs-or-pointers-to-structs-dilemma/)

### Maps

```
var m map[string]int
```

- Map types are reference types, like pointers or slices, and so the value of `m` above is `nil`; it doesn't point to an initialized map

- Initialise a nil map with make `m := make(map[string]int)`

- Getting and setting
  
  - a := m["doesntexist"] - a will be assigned the "zero value" of that type
  
  - a, ok := m["doesntexist"] - ok will be assigned false
  
  - _, ok := m["doesntexist"]_- same thing as above
  
  - m["exists"] = 1

- Deleting
  
  - delete(m, "exists")

- Iterating
  
  - for k, v := range m {}

- Valid keys - comparable types:  boolean, numeric, string, pointer, channel, and interface types, and structs or arrays that contain only those types

[Go maps in action - The Go Blog](https://blog.golang.org/maps)
