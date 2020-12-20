
# Kotlin notes



# Basics
## no main method in class
- to get a static method, it needs to be at the package level

## statements vs expressions
- expression has values, in java all control structure are statements
## expression bodies
- lf  not an expression must set return type and use return keyword

## default getters and setters
## custom accessors

## packages and imports 

## enums
  - soft keywords
  
##when

## smart casting 

- kaitlin - using “is”, java using instanceof but still need to explicitly cast
- only works if var isn’t changed after “is” check

## while / do while
- very similar to Java equivalent

## range
- 1..10
- step
- downTo
- until


## object expressions

- Additional notes
  - http://petersommerhoff.com/dev/kotlin/kotlin-for-java-devs/#objects
  - https://antonioleiva.com/objects-kotlin/

- a way to replace the need for anonymous classes in Java
- anonymous objects can be used as types only in local and private declarations. 
- object declarations can't be local (i.e. be nested
- object expression, object declaration, companion object

	object: X() {
	  fun xxx() {
	  
	  }
	}
	
- companion object
  -  are used to represent static in Kotlin 
  
## static methods 
- https://dzone.com/articles/kotlin-static-methods
- https://stackoverflow.com/questions/33441015/kotlin-object-expressions-comparator-example


## SAM conversions

https://kotlinlang.org/docs/reference/java-interop.html



## collections 

- Kotlin does not have dedicated syntax constructs for creating lists or sets. 
- Use methods from the standard library, such as listOf(), mutableListOf(), setOf(), mutableSetOf()
- Map creation in NOT performance-critical code can be accomplished with a simple idiom: mapOf(a to b, c to d)
- mutable views, read-only views
   - https://kotlinlang.org/docs/reference/collections.html


## lambdas & higher order functions

function type - () -> T


##smart casting
- when you use is, the compiler keeps track of the check and you don’t have to cast the object again

##apply vs with
 1.	apply accepts an instance as the receiver while with requires an instance to be passed as an argument. In both cases the instance will become this within a block.
 2.	apply returns the receiver and with returns a result of the last expression within its block.


https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8 
###implicit and explicit returns
The rule “the last expression in a block is the result” holds in all cases where a block can be used and a result is expected.
As you’ll see at the end of this chapter, the same rule works for the try body and catch clauses, and chapter 5 discusses its application to lambda expressions. But as we mentioned in section 2.2, this rule doesn’t hold for regular functions. A function can have either an expression body that can’t be a block or a block         body with explicit return statements inside.

##in and !in

fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'

Ranges aren’t restricted to characters, either. If you have any class that supports comparing instances (by implementing the         java.lang.Comparable interface

## exceptions

- throw is an expression
- Kotlin doesn’t differentiate between checked and unchecked exceptions. You don’t   specify the exceptions thrown by a function, and you may or may not handle any exceptions.


## defining an iterator
- return an object expression that has the following methods defined, next() and hasNext()

## defining a property

## defining custom getter and setters
- backing fields
-e.g.
var prop: Int? = null
set(value) {
  field = value
}
https://kotlinlang.org/docs/reference/properties.html

## koans 30

- destructuring declarations
- allows variables to be derived from one object on assignment
  e.g. (name,age) = person
- done by creating methods - component1, component2 on the class
- data classes have these destructuring methods defined on them right away
- use _ to ignore destructured variable val (_, status) = getResult()
- can be used as lambda parameters map.mapValues { (key, value) : Map.Entry<Int, String>- -> "$value!" }

## operators
 - invoke
 - plus
 - times
 
 
# Generics

## Java Generics
- They are invariant List<Object> is not a descendent to List<String>

## kotlin deals with covariants
This is called declaration-site variance: we can annotate the type parameter T of Source to make sure that it is only returned (produced) from members of Source<T>, and never consumed. To do this we provide the out modifier:


- can receive/consume subtypes (kotlin: in, java: extends)
- can return/product super type (kotlin: out, java super)
- https://proandroiddev.com/understanding-generics-and-variance-in-kotlin-714c14564c47


### variance, covariance, contravariance

- Int is a subtype of Int?
- covariance: when 2 types have a relationship (<? extends X>)
- invariance: when 2 2 types don’t have a relationship
- contravariance: when the relationship is reversed (<? super X>
- devalaration vs call site variance
https://proandroiddev.com/understanding-generics-and-variance-in-kotlin-714c14564c47


# Constructors 

## Primary constructors

<pre>
class Person constructor(firstName: String) {}
</pre>

same as 

<pre>
class Person(firstName: String) {}
</pre>

cannot contain any code. Code goes into initialization blocks

<pre>
class Customer(name: String) {
    init {
        logger.info("Customer initialized with value ${name}")
    }
}
</pre>

Can be initialized in the class body

<pre>
class Customer(name: String) {
    val customerKey = name.toUpperCase()
}
</pre>

Can be initialized at the declaration point

<pre>
class Person(val firstName: String, val lastName: String, var age: Int) {
    // ...
}
</pre>

## secondary constructors

class Person {
    constructor(parent: Person) {
        parent.children.add(this)
    }
}

If the class has a primary constructor - secondary constructors must delegate to the primary constructor - using this

class Person(val name: String) {
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}

## Constructurs & inheritance

open class Log {
    var data: String = ""
    var numberOfData = 0
    constructor(_data: String) {

    }
    constructor(_data: String, _numberOfData: Int) {
        data = _data
        numberOfData = _numberOfData
        println("$data: $numberOfData times")
    }
}

class AuthLog: Log {
    constructor(_data: String): this("From AuthLog -> " + _data, 10) {
    }

    constructor(_data: String, _numberOfData: Int): super(_data, _numberOfData) {
    }
}

Note: The secondary constructor must initialize the base class or delegate to another constructor (like in above example) if the class has no primary constructor.
	


# Inheritance

## disambiguating multiple inheritance 
- must explicitly redefine 

## modifiers
- all classes, methods are closed by default
- abstract classes are open by default but methods still closed unless explicitly marked as open
- must explicity mark a method as override

### visibility modifiers
- public (default), internal (kotlin specific - visible in a module), protected, private
- private is different then Java
- outer class doesn’t see private members of its inner (or nested) classes in Kotlin


## with, let,  apply, run, also

### run 
non extension and extension version
T.run  -> T.block() - calls the contents of the block implicitly against the instance. Can use “this” optionally. Returns the last item in the block

### apply
T.apply -> T.block() - calls the contents of the block implicitly against the instance. Returns the instance. 

### also
public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this }

### when
No extension version

### let
-> “it” is the argument let is being called on
-> “this” is the method that this code is in 


https://medium.com/@elye.project/mastering-kotlin-standard-functions-run-with-let-also-and-apply-9cd334b0ef84
https://ask.ericlin.info/post/2017/06/subtle-differences-between-kotlins-with-apply-let-also-and-run/
https://proandroiddev.com/the-tldr-on-kotlins-let-apply-also-with-and-run-functions-6253f06d152b
https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8
https://docs.google.com/spreadsheets/d/1P2gMRuu36pSDW4fdwE-fLN9fcA_ZboIU2Q5VtgixBNo/edit#gid=0

## kotlin singleton with arguments

https://medium.com/@BladeCoder/kotlin-singletons-with-argument-194ef06edd9e

## companion object 
- way for classes to have static members and methods

## Kotlin unti testing

https://blog.philipphauer.de/best-practices-unit-testing-kotlin/
https://github.com/phauer/blog-related/tree/master/unit-tests-kotlin/src/test/kotlin/de/philipphauer/blog/unittestkotlin

## mocking with kotlin
- https://github.com/nhaarman/mockito-kotlin/wiki/Mocking-and-verifying
- https://github.com/mockito/mockito/wiki/Mockito-for-Kotlin
- running junit5 - https://www.petrikainulainen.net/programming/testing/junit-5-tutorial-running-unit-tests-with-gradle/

## lateinit

### kotlin tutorials 
https://github.com/eugenp/tutorials/tree/master/core-kotlin