### difference between new, composite literal and make

#### new

new - creates a zerofied version of the variable

#### composite literal

Struct{field1: "val field1", field2: "val field2"}
new is a simple case of composite literal

#### make

It creates slices, maps, and channels only, and it returns an initialized (not zeroed) value of type T (not *T). The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is nil. For slices, maps, and channels, make initializes the internal data structure and prepares the value for use. For instance,
