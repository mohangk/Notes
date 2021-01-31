---
draft: true
title: C++ Namespaces, Type Conversion(Coercion)
categories:
  - C++
---
#namespace
- :: - namespace resolution operator
- group methods under the same namespace across multiple files
- namespace aliases - namespace Boo = Foo::Goo; 

#type conversion

- promotions, conversion
- #include <typeinfo> — typeid().name()

#casting

5 types of casts
- C-style casts 
	- e.g. (float)i1 or float(i1)
	- doesn’t get checked at compile time and hence should be avoided

- static casts
	- static_cast<int>(i1) 
	- gets checked at compile time, less powerful

- const casts 
	- should be avoided

- dynamic casts

- reinterpret casts 
	- should be avoided

# enums

enum Color
{
  COLOR_BLACK,
  COLOR_RED
};

Color paint = COLOR_WHITE;
Color house(COLOR_BLUE);

- despite being in different namespaces, 2 enums cannot have the same name
- can’t go from number to enum
- implicitly numbered starting from 0
- integer family of types
- Because the compiler needs to know how much memory to allocate for an enumeration, you cannot forward declare enum types.  Because defining an enumeration does not allocate any memory, if an enumeration is needed in multiple files, it is fine to define the enumeration in a header, and #include that header wherever needed.

#enum class (scoped enumeration)





