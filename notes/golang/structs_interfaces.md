## Allocation of data with `new`

new(T) -- does not allocate memory, only zero-izes it *T. Zero value can be used.

f := new(File)

f.fd = fd

f.name = name

...

**OR**

f := File{fd, name}

return &f

**OR**

&File{fd, name}

**OR**

&File(fd: fd, name: name)

## Allocation with `make`

Creates slices, maps, channels - returns initiazed value of T

## interfaces

An interface is implemented implicity. There is no implements keyword, used in other languages. If a type defines the entire method set of an interface, it implements it. It’s called structural typing, the compile-time equivalent of duck typing.

https://flaviocopes.com/go-empty-interface/
http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go

<https://npf.io/2014/05/intro-to-go-interfaces/>

## methods

- Go does not have classes, but define methods on types
- A method is a function with a special “receiver” argument

E.g.

type rect struct {
    width, height int
}

//pointer value receiver
func (r *rect) area() int {
    return r.width * r.height
}

//value receiver type
func (r rect) perim() int {
    return 2*r.width + 2*r.height
}

func main() {
    r := rect{width: 10, height: 5}

```
fmt.Println("area: ", r.area())
fmt.Println("perim:", r.perim())

rp := &r
fmt.Println("area: ", rp.area())
fmt.Println("perim:", rp.perim())
```

}

Go automatically handles conversion between values and pointers for method calls. You may want to use a pointer receiver type to avoid copying on 
method calls or to allow the method to mutate the receiving struct.

## 
