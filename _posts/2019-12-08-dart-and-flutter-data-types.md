---
title: "Dart and Flutter data types"
date: "2019-12-08"
---

One of the most fundamental features of a programming language is the set of data types it supports. These are the types of data that can be represented and manipulated in a programming language.

Dart has special support for the following data types:

- numbers
- strings
- booleans
- lists (also known as arrays)
- sets
- maps

You can initialize an object of any of these special types using a literal. For example, `'this is a string'` is a `string` literal and `true` is a `boolean` literal.

Since all variables in Dart refer to an `object` - an instance of a class - you can often use constructors to initialize variables.

Some of the built-in types have their own builders. For example, you can use the `Map()` builder to create a map.

## Dart data types

### int

Integer values ​​no larger than 64 bits, depending on platform. In the Dart VM, values ​​can be from -263 to 263 - 1. The JavaScript-compiled Dart uses JavaScript numbers, allowing values ​​from -253 to 253 - 1.

### double

64-bit floating point numbers (double precision) as specified by the IEEE 754 standard.

The int and double are subtypes of num. The num type includes basic operators like +, -, / and \*, and is also where you'll find abs (), ceil () and floor (), among other methods.

### String

A String is a sequence of UTF-16 code units. You can use single or double quotation marks to create a string.

### bool

To represent boolean values, Dart has a type called bool. Only two objects have bool type: true and false boolean literals, which are constants at compile time.

### List

Perhaps the most common collection in almost all programming languages ​​is the array or ordered group of objects. In Dart, arrays are List objects.

Dart list literals look like JavaScript matrix literals.

### Set

A Dart Set is an unordered collection of unique items. Dart support for sets is provided by set literals and by set type.

### Map

In general, a map is an object that associates keys and values. Keys and values ​​can be any type of object.

Each key occurs only once, but you can use the same value multiple times.
