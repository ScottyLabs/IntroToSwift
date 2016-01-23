---
title: Introduction to Swift
layout: page
---

_Part of ScottyLabs [CrashCourse](http://tartanhacks.com/crashcourse/)_


<ul>
<!-- TOC HERE -->
</ul>
 
## Language Features

  * Strong Type System
  * Automatic Reference Counting
  * Compiled (although Playgrounds simulate scripting)
  * Protocols and Protocol Extensions
  * Object Orientation
  * Closures (Objective C blocks)
  * Advanced Enum Types with Pattern Matching

## Syntax Overview

Swift syntax does not rely on whitespace except for line breaks, so uses curly braces, in a way similar to C or Java, but does not require semi-colons!!! `for` loops, `while` loops and `if` statements don't require parentheses, so their use is left as a style choice.

For those familiar with Objective C blocks (and to some extent, javascript), the idea of an anonymous Closure as a callback or function may be familiar. Essentially, "blocks" are anonymous functions, whose code appears inside of `{}`. It is important to note that this code will (most likely) NOT execute in the exact area of the code it is defined; closures are typically executed asynchronously.

Comments are structured the same way as the C family of languages:
  * `//` Is a single line comment
  * `/* */` denotes a multi-line comment

Formatting strings varies from language to language. Swift calls its format method _String Interpolation_. To print any variable inside of a string, you simply put the variable inside an escaped parenthesis sequence `\(<variable>)` inside of the string. To print the value of a variable `score` regardless of its type, you can write the following `print("Score: \(score)")`.


## Variable Declarations

Variables in Swift are declared using one of two declaration statements: `var` or `let`. `let` declarations are used for variables that will not change, `var` declarations for the opposite. 

### `let`

Do not use `let` only for constants in the traditional sense (i.e. the handful you put at the top of a file or class). Swift is a very conservative language, and prefers you label as many things as you can with `let` rather than `var`. The most notable place where you may not think to use `let` is for when you are assigning a reference (aka pointer) to the variable. When you have a reference/pointer to an object, the reference itself does not change! Only the object does. So the following would be a correct use of `let`:

```
let obj = Student()
// We can call mutating methods!
obj.setName('Harry')
// We can't re-assign the reference
obj = otherStudent // XXXXXX BAD CODE, WON'T COMPILE
```

### `var`

`var` is for mutable variables that will be changed over time. The following is an example:

```
var score = 0

func madeGoal() {
  ++score
}
```

### Try it!

What happens when you try to run the following code?

```
let score = 0

func madeGoal() {
  ++score
}

madeGoal()
```

## Variable Types

Swift has a very strong type system, whose main goal is to catch errors at compile time rather than runtime. For this reason, Swift also includes _optional_ types to reduce dereferencing `nil` wherever possible. (`nil` is the Swift equivalent to `null` or `None`.)

### Primitive Types

Swift provides the following primitives:

  - `Bool`
  - `Int`
  - `Float`/`Double`
  - `String`

They behave exactly as you expect they would, so we won't get too detailed witht them. These primitives can be used like any other (immutable) class, there is no wrapper class (I'm looking at you, Java!).

### Function Types

Swift makes liberal use of anonymous functions, called blocks, for continuation and asynchronous tasks. Unfortunately, blocks are too much to go into for this talk, but it is still useful to recognize function types. The general format is:

```
(Type1, Type2, ...) -> ReturnType
```

#### Declaring a Function

Functions are declared with the `func` keyword.

```
func approximateRoot(num: Double,  withTolerance tolerance: Double) -> Double {
  ...
}
```

The above declaration is a simple declaration illustrating a multiple argument function with a return type. If the return type is `Void`, the `-> T` is not required.

You can see the `withTolerance` for the second parameter, this is a display name for the public interface. Swift, like objective C, differentiates between the public names and local names of function parameters, so the 2nd to last parameters expect to be given a public name. This can make code more readable, the above function would be called like this:

```
approximateRoot(72, withTolerance: 0.1)
```

I won't cover it in class, but you can disable the public interface name, set default parameter values, and have variable length parameters if you want.

### Class Types

Swift classes do not inheret from a base class, but if you are using Objective-C in your project, you may want to inherit from `NSObject`.

Inheritance in Swift works very similarly to inheritance in other object oriented languages. One advantage of class types is the ability to make subclasses, so any method written for `MyClass` will also work with `SubclassOfMyClass`. But this technique has its limits because inheritance is limited to one superclass in Swift. Instead, we can have `protocol` types, similar to a Java `interface`, that objects can conform to. The classic example is writing a generic sort function:

```
protocol Comparable {
  func compare(other: Self) -> Int
}

func sort(unsorted: [Comparable]) {
  ...
}
```

The use of protocols to create generic interfaces is a powerful paradigm in Swift, especially since Swift 2 added default implementations for protocols and protocol extensions.

#### Declaring a Class

A class declaration must is made up of a unique _class name_, an optional _superclass_, and, zero or more _implemented interfaces_.

```
class <class name>: <super class?>, <interface 1?>, <interface 2?>, ... {
  ...
}
```

One way to think of it is that after the colon you are making a list of types that your object could be treated as. Just make sure that the _superclass_ appears first in the list.

##### Properties

Properties, fields, or instance variables (use whichever nomenclature you want), are declared inside of the class, regardless of their access (this is actually different from Objective C). They may have default values, or may be set in the constructor.

```
class Person {
  private var age = 0
  private let creationDate: NSDate
  public let name: String

  init(name: String) {
    self.creationDate = NSDate()
    self.name = name
  }
}
```

You can also use `static` and `lazy` properties, but we won't cover them today.

##### Initializers

If none of your properties need to be set, there is a default no-args initializer generated by Swift. Otherwise, you must implement at least one initializer. Swift gets all worked up about _designated initializer_s, _required initiailizers_ and _convenience initializers_, but to be honest just do what the error messages tell you before jumping down the rabbit hole of trying to understand those terms. In general, you should be fine if you use a standard initializer, then added convenience ones (which simply call the first initializer). We could implement similar behavior to the last example like so: 


```
class Person {
  private var age: Int
  private let creationDate: NSDate
  public let name: String

  init(name: String, age: Int) {
    self.creationDate = NSDate()
    self.name = name
    self.age = age
  }
  convenience init(name: String) {
    self.init(name: name, age: 0)
  }
}
```

##### Overriding

To override a method, the `override` keyword _must_ be used. This is to let the compiler check to make sure overriden methods were overriden on purpose.

```
class CustomView: UIView {
  override func drawRect(rect: CGRect) {
    ...
  }
}
```

#### Declaring a Protocol

Protocols declare an interface of public properties and functions to be implemented. Public variable can may or may not be settable.

```
protocol 2DShape {
    var numSides: Int { get }
    var area: Int { get }
    var color: UIColor { get set }
    func intersect(other: 2DShape) -> 2DShape
}
```

Protocols are actually very powerful, so I encourage you to google default protocol implementations and protocol inheritance after the seminar.



### Optional Types

The `let` statement was added to encourage the default behavior of variables to be constant, so they could only be changed on purpose. Similarly, _optional_ and _non-optional_ types were created so that pointers could only be set to `nil` on purpose. By default, all types are non-optional. To mark a type optional, you simply add a question mark to the type name (e.g. `String?`).

#### Try It!

Using optional types, try to make the following code compile:

```
var gameStatus: String = null

func startGame() {
  gameStatus = "Playing"
}

func endGame() {
  gameStatus = "Game Over"
}

func displayStatus() {
  print(gameStatus)
}

displayStatus()
startGame()
displayStatus()
endGame()
displayStatus()

```

## Control Flow

### Conditionals

`if` statements and `while` loops have very similar syntax to other languages, minus the parenthesis. Swift uses the standard boolean operators `&&` and `||`.

```
if bored {
  var areWeThereYet = false
  while bored && !areWeThereYet {
    areWeThereYet = askMom("Are we there yet?")
  }
}
```

### `for` Loops

Usualy Swift `for` loops use `for-in` syntax. To create a range a numbers, use `start...end` for inclusive or `start..<end` for exclusive. 

```
for i in 0..<10 {
  ...
}

for obj in objArray {
  ...
}
```

_\*\*It is also possible to use a more traditional `(;;)` syntax, but it seems to be frowned upon, so don't do it._


### `if let - else` Statements

`if let - else` is a special kind of assignment for optional types. Essentially it is a built-in structure to encourage dereferencing safety. We are checking for `nil` and assigning to a variable in the same step.

If the assignment succeeds, the code inside of the `if` brackets is executed, otherwise the `else` code is executed.

```
// crazyExperiment() has return type Dinosaur?
if let dino = crazyExperiment() {
  panic()
} else {
  tryAgain()
}
```


# Additional Resources

## Things You Wish I Covered (But I Didn't):

* CoreData - Apple's persistent storage framework (abstraction of a Database)
* CloudKit - Apple's iCloud storage interface
* Custom UI Elements - Make apps look great
	* IBDesignable and IBInspectable for storyboard previews
	* PaintCode (3rd party) - Draw the UI Element $\rightarrow$ Get the code
* Swift Language Features:
	* Pattern Matching - For those who miss SML ;)
	* Protocol Extensions - Can be a huge time-saver

## Where to learn
## Where to find help
## Where to report bugs

