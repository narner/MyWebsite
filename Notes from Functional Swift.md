---
title: "Notes from Functional Swift"
date: "2022-06-27"


---

These are my notes from objc.io's [Functional Swift book](https://www.objc.io/books/functional-swift/). For a recent proejct, I've had to dive into a codebase that heavily makes use of functional programming in Swift; this book was helpful in getting a basic understanding of functional programming concepts in the world of Swift and iOS. 



-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



#### Characteristics of Functional Programming: 

* Modularity
* Carteful treatment of mutable state
* Careful use of types



Functions are first-class values

* Can be passed as arguments to other funcitons
* Methods: functions defined on a type
  * **Types guide the development process! **

* Higher-Order Functions - functions that take functions as arguments



#### Generics 

* When functions can work on any type, use generics
* Type signature as <T>



#### Map / Filter / Reduce

* Map - modify each element in a collection
  * Transform one set of values into another set of values

* Filter - returns set of values that passed an `if` statement 

* Reduce - turns a collection into a single value



#### `Any` vs Genertics 

* Generics can be used to define flexible functions

* `Any` used to dodge Swfit's type system

* Optionals represent values that may be missing or computations that may fail

* ?? checks if an optional argument is `nil`

* Optional binding: avoid writing "!", use this instead:

  * ``` 
    if let x = yX {
    	...
    }
    ```

* Optional chaining: calling methods / accessing properties on nested classes or structs 

  * ``` 
    if let myThing = thing.one?.two?.three {
    	...
    }
    ```

* `guard` statement - exit current scope early if some condition isn't met  



#### Value and Reference Types 

* Value types - copied when passes as function arguments (Structs)

* Reference types - references to the type are passed (Classes)



#### Structs

* Examples of Structs 
  * Arrays
  * Dictionaries
  * Numbers 
  * Boolean

* Structs allow for local mutability without global side effects

* Coupling - measures the degree to which individual units of code depend on each other
  * Fucntions that compute the same output for equal inputs = referentially transparent
  * Favoring immutability makes it easier to write referentially transparent + reduces compuling! 



#### Mutating Structs

```
extension Point Struct {
	mutating func fooBar() {
		x = 0
		y = 1
	}
}
```



#### Types 

* USE TYPES EFFECTIVELTY TO RULE OUT INVALID PROGRAMS 

* Unlike Objective-C, enums in Swift create new types distinc from integers or other existing types

* Drawback to using Swift's optional type: don't return error when something goes wrong

* Swift forces you to annotate any function or method that may throw an error with the `trhows` error; forces you to use `try` and variants` 

* Enumerations - referred to as sum types
* Types defined using enumerations + structs are refered to as algebraic data types 



Types are isomorphic if we can convert between them w/o losing any information:

```
f: (A) -> B
g: (B) -> A
```

* For all of x: A, the result of calling g(f(x)) must be equal to x

* For all of y: B, the result of f(g(y)) must equal Y

* These functions are the inverse of each other 



#### DSL's (Domain Specific Languages)

* Shallow embedding of DSL - does not create intermediate data structures

* Deep embedding - explicityl creates intermediate data structures



#### Iterators 

* Used if you don't want to use all emenets in an array or calculate them all 

* Is a process that generates an array's elements on request - adheres to this protocol: 

  ```
  protocol IteratorProtocol{
  	associatedType Element
  	mutating func next() -> Element
  }
  ```

  

* If we want to compute the indices in a different order, we only need to update the iteration, and never the code that uses it 

​	separeate generation of data from usage 

* By defining a protocol for iterators, we can also write generic methods that work for any iterator

* Iteraors can be combined on top of each other 



#### Sequence 

* Iterators provide a one-shot mechyanism for repeatedly computing a next element
* Sequences provide a mechanism for rewinding / replaying generated elements
* Every sequence has an associated iterator type + method to create a new iterator 
* By encapsulating the creation of iterators in teh sequence definition, programmers uising sequences tdon't have to to concerting with underlying iterators 
* Map / reduce do not return new sequence, but traverse the sequence to produce an array 



#### Lazy Sequences

* Chain operations only once we compute the result do operations get applied 



#### Parser Combinators

* Functional approach to parsing 

* Instead of managing mutable state of parser (e.g. what character we're at), parser combinators are pure fu;nctions to avoid mutable state
* Parser - takes some chars as input, reaturns some resulting value + remainder of string if parsing succeeds 
  * Using `string` is bad for performance; use `substring` instead