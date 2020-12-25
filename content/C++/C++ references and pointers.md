- Can’t have references to references
- References can’t be uninitialised

Unlike a reference, a pointer is an object in its own right. Pointers can be assigned and copied; a single pointer can point to several different objects over its lifetime. Unlike a reference, a pointer need not be initialized at the time it is defined. Like other built-in types, pointers defined at block scope have undefined value if they are not initialized

when we dereference a pointer, we get the object to which the pointer points, which is a reference