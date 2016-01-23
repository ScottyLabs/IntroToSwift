---
title: Introduction to Swift Development
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

## Variable Types

### Built-in Types

### Class Types

### Optional Types

## Variable Declarations

Variables in Swift are declared using one of two declaration statements: `var` or `let`. `let` declarations are used for variables that will not change, `var` declarations for the opposite. 

### `let`

Do not use `let` only for constants in the traditional sense (i.e. the handful you put at the top of a file or class). Swift is a very conservative language, and prefers you label as many things as you can with `let` rather than `var`. The most notable place where you may not think to use `let` is for when you are assigning a reference (aka pointer) to the variable. When you have a reference/pointer to an object, the reference itself does not change! Only the object does. So the following would be a correct use of `let`:
```(Swift)
let obj = Student()
// We can call mutating methods!
obj.setName('Harry')
// We can't re-assign the reference
obj = otherStudent // XXXXXX BAD CODE, WON'T COMPILE
```

### `var`

`var` is for mutable variables that will be changed over time. A good example would be an integer you plan on incrementing over time.

## Control Flow

### `if let` Statements

`if let` is a special kind of assignment for optional types. Essentially it is a built-in structure to encourage dereferencing safety. We are checking for 'nil' and assigning

### `For` Loops

### `if` Statements

### Class Declarations

### Protocol Declarations

## Functions

## Classes 

## Protocols

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

