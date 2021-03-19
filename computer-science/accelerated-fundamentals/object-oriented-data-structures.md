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


## Week 2: C++ Part 2

### Stack Memory and Pointers

In C++, the programmer has control over the memory and lifecycle of every created variable. A variable always has a name, a type, a value, and a location in memory (by default it is stack memory). In C++, the `&` operator returns the memory address of a variable.

Stack memory is associated with the current function and the memory's lifecycle is tied to the function. Stack memory is released when the function returns or ends. Stack memory always starts from high addresses and grows down toward `0x0`. C++ can also optimize and re-arrange code which can lead to different variable stacking than expected.

A pointer is a variable that stores the memory address of the data. In C++, a pointer is defined by appending an `*` to the type of the variable. Example: `int * name = &value`. Given a pointer, a level of indrection can be removed with the dereference operator `*` prepended to the variable. `*p` instead of `int * p`.

### Heap Memory

Heap memory is completely independent of the lifecycle of the function. If memory needs to exist for longer than the lifecycle of the function, it is necessary to use heap memory. The only way to create heap memory through C++ is with the `new` operator. It returns a pointer to the memory storing the data, not an instance of the data itself.

The `new` operator does three things:
1. Allocate memory on the heap for the data structure.
2. Initialize the data structure.
3. Return a pointer to the start of the data structure.

This memory is only ever reclaimed when the pointer is passed to the `delete` operator.

`int * numPtr = new int;` will allocate two chunks of memory: (1) memory to store an integer pointer on the stack and (2) memory to store an integer on the heap.

When stack memory is deleted, the pointer no longer points to data, so it becomes a `nullptr` that points to the memory address `0x0` which is nowhere because the address is reserved. When `0x0` is accessed, it will generate a segmentation fault. Calls to `delete 0x0` are also ignored. 

The pointer still stores the address of the deleted variable after `delete` is run, so variables should be manually set to `nullptr` for greater code protection. When an object is stored via a pointer, access can be made to member functions using the `->` arrow operator. For example `c->getVolume();` is the same as `(*c).getVolume();`.

### Headers and Source Files

`.h` header files have definitions of objects and global functions. These contain declarations. `.cpp` files are called implementation files and this is where function definitions and program logic go.

Instructions beginning with an `#` are special preprocessor directives for the compiler which prevents the header file from being included multiple times. Implementation files make requests to various header files which is then compiled into an object file with `.o` extension. The `main.o` file is linked against compiled definitions in a class' `.o` file. The linker program also links against system-wide object files such as iostream. After the compiler and linker programs finishing processing, an executable file is returned.

### Compiling and Running a C++ Program

A MakeFile is a kind of script that tells the compiler how to build a program by running `make` on the script. To clear out compiled object and executable files then run `make clean`.

Common commands:
* `sudo` prefix to do something as the superuser.
* `man` to show the manual for other commands.
* `ls` to list directory contents.
* `cd` to change directory.
* `cp` to copy a file.
* `mkdir` to create a new subdirectory.
* `mv` to move or rename a file.
* `rm` to delete a file.

### Type Casting

Type casting is where a value is automatically converted to a different type in order to allow an operation to continue. An int added to a double will be converted into a double, and the result will be a double as well.

## Week 3: Lifecycle of C++ Objects
