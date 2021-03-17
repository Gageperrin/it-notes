# Object-Oriented Data Structures

These are my notes to University of Illinois' Object-Oriented Data Structures course on Coursera offered as part of their Computer Science Accelerated Fundamentals Specialization track.

## Week 1: C++ Part 1

C++ is a strongly typed programming language where every variable has a type, name, value, and location in memory. The type of a variable (primitive or user-defined) defines the contents of the variable.

There are six common primitive types:
*  `int`
*  `char`
*  `bool`
*  `float`
*  `double`
*  `void`

There are an unlimited number of user-defined types, common user-defined types are strings and vectors.

C++ classes encapsulate data and associated functionality into an object. Encapsulation separates data and functionality into public and private spheres. Public members can be accessed by client code, private members cannot be accessed by the code.

A header file `.h` defines the interface to the class which includes the declaration of all member variables and functions. The `.cpp` implementation codes contains the code to implement the class and is based on a `main()` function.

The C++ standard library (std) provides a set of commonly used functionality and data. The C++ standard library is organized into sub-libraries, such as `iostream` which includes operations for reading and writing to files and the console itself.

Libraries are included with `#include <iostream>` for example. Namespaces can be used to specify data structures and be more specific than generic data structure names would allow. Namespaces can be specified with `namespace [name] { [content] }`. They can be called with `[namespace]::[class member];`.

