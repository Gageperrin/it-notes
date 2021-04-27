# Accelerated Fundamentals

# Object-Oriented Data Structures

These are my notes to University of Illinois' Object-Oriented Data Structures course on Coursera offered as part 1 of their Computer Science Accelerated Fundamentals Specialization track.

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

### Class Constructors

Without any custom constructors, the C++ compiler automatically uses an automatic default. The automatic default constructor will only initialize all member variables to their default variables. Non-default custom constructors can be used as well. They require client code to supply arguments. If any custom constructor is defined, an automatic default constructor is not defined.

### Copy Constructors

A copy constructor is a pseical constructor that exists to make a copy of an existing object. The automatic copy constructor is provided autoamtically by default and will copy the contents of all member variables. A custom copy constructor is a class constructor and has exactly one argument.

Copy constructors are invoked automatically by passing an object as a parameter, returning an object from a function, and initializing a new object.

### Copy Assignment Operator

In C++, a copy assignment operator defines the behavior when an object is copied using the assignment operator `=`. While a copy constructor creates a new object, an assignment operator assigns a value to an existing object. C++ provides an automatic default assignemtn operator.

A custom assignment operator is required to be a public member function of the class, have the function name `operator=`, have a return value of a reference of the class type, and have exactly one argument.

### Variable Storage

In C++, an instance of a variable can be stored directly in memory, accessed by a pointer, or accessed by a reference. By default, variables are stored directly in memory. The type of a variable has no modifiers, the object takes up exactly its size in memory. The type of a variable is modified with an `*`. A pointer takes a memory address width of memory (e.g. 64 bits on a 64-bit system). The pointer points to the allocated space of the object.

A reference is an alias to existing memory and is denoted with an `&`. A reference does not store memory but is only an alias. The alias must be assigned when the variable is initialized.

Just like with storage, arguments can be passed to functions via value, pointer, or reference. It is important to never return a reference to a stack variable created on the stack of the current function.

### Class Destructors

A destructor should never be called directly. Instead it is automatically called when the object's memory is being reclaimed by the system: if the object is on the stack when the function returns or if the object is on the heap when delete is used. The deconstructors are not called at compile time but during runtime.

To add custom behavior to the function's end of life, use a custom destructor. A custom destructor is a member function which is the name of the class preceded by a `~`. All destructors have zero arguments and no return type. A custom destructor is essential when an object allocates an external resource that must be closed or freed when the object is destroyed. Examples include heap memory, open files, and shared memory.

### Uninitialized Pointers, Segfaults, and Undefined Behavior

A segmentation fault (`segfault`) occurs when a referenced address is dereferenced.

Undefined behavior is when a program compiles and runs as expected but does not follow best practices is not completely safe and valid. Often caused by using uninitialized variables.

Initialization is specifying a value for a variable from the moment it is created. If a pointer is not initialized, it should not be dereferenced. Uninitialized pointers are not a good practice for generating randomnness. If a value is initialized with `()` after it, the parameters will be given to the class types' corresponding constructor.

Pointers should be manually reset to `nullptr` when they are done being used since using `delete` on a pointer frees the heap memeory allocated at that address but does not change the pointer value itself. Manually resetting pointers will prevent deleting the same allocated address more than once and a pointer being dereferenced to deallocated memory. 

Delete and `nullptr` only need to be used with pointers not variables since variables are automatically de-allocated.

### Unsigned Integer Types

Unsigned integers cannot represent negative values but have an increased upper positive value range for memory usage. Unsigned integers help with memory optimization. A negative signed integer may be reinterpreted as a very large positive unsigned integer and vice versa. This can cause problems operating arithmetic on signed and unsigned integers.

A casting operation may be used to convert back to a signed integer but it is not always the best method. It is better to create temporary working copies of unsigned values as signed types.

The generic data structure class is often referred to as a container. The C++ STL provides many such as `std::vector`.

## Week 4: Engineering C++ Software Solutions

### Template Types

A template type is a special tpye in C++ that can take on different types when it is initialized like the standard vector. `std::vector` is a standard library class that provides the functionality of a dynamically growing array with a templated type.

Key ideas:
* Defined in `#include <vector>`
* Initializiation `std::vector<T> v;`
* Add to back of array `::push_back(T);`
* Access specific element `::operator[](unsigned pos);`
* Number of elements `::size()`

### Tower of Hanoi

The Tower of Hanoi problem is the problem of moving a stack of differently sized cubes from one stack to another with only three possible stacks. This can be cached out and solved in C++ syntax (as the next few lessons demonstrate).

### Templates and Classes

A template variable is defined by declaring it before the beginning of a class or function. Templated variables are checked at compile time which allows for errors to be caught before running the program.

### Inheritance

Inheritance allows for a class to inherit all member functions and data from a base class into a derived class. A base class is a generic form of a specialized, derived class. When a derived class is initialized, the derived class must construct the base class. By default it uses the default constructor. It can access all public members but no private members.


# Ordered Data Structures

These are my notes to University of Illinois' Ordered Data Structures course on Coursera offered as part 2 of their Computer Science Accelerated Fundamentals Specialization track.

## Week 1

### 1.1 Arrays

An array stores data in blocks of sequential memory. Instantiate an array in C++ with `int values[10]` for an array of 10 values starting with index `0`. All data in an array must be of the same type, and therefore the byte size of the data is also known. This makes it easier for iterating through arrays. Arrays also have a fixed capacity. They must store their data sequentially in memory. The capcity of an array is the maximum number of elements that can be stored. The size of an array is the current number of elements stored in the array. An array is full when the size of the array is equal to the capacity.

### 1.2 Linked Memory

Instead of storing data sequentially, linked memory (or linked lists) store data together with a link to the in-memory location of the next list node. A list node refers to the pair of both data and the link. Zero or more ListNode elements linked together form a linked list. A pointer called the head pointer stores the link to the beginning of the list. A pointer to nullptr marks the end of the list.

A linked list takes longer to access than an array. In a list, the capacity is bounded not by the list but by the memory available on the system. In both arrays and lists, all data wihtin an instance must be the same type.

### 1.3 Run Time Analysis

Run-time analysis formally compares the speed of an algorithm as the size of input grows. One-dimensional arrays are accessed at a given index in O(1) constant time while linked lists are O(n) linear time. O(n^2) is polynomial time.

### 1.4 Array and List Operations

Action | Array | List
------ | ------|----------
Access a Given Index | O(1)    | O(n)
Insert at front  | O(1)*  |  O(1)
Find data | O(n)    |   O(n)
Binary search | O(log(n)) | N/A
Insert after |  O(n)  | O(1)
Delete after |  O(n)  | O(1)

### 1.5 Queue (Data Structure)

A queue is a FIFO data structure with an enter point and exit point. A structure's abstract data type (ADT) is how data interacts with the structure.

The Queue ADT has four operations:
* `create` creates an empty queue.
* `push` adds data to the back of the queue.
* `pop` removes data from the front of the queue.
* `empty` returns true if the queue is empty.

All four have O(1) for both arrays and linked lists. `push` and `pop` can have an amortized runtime for arrays.

Queues can be array-based or doubly linked list-based. List-based queues can have a tail pointer to access the end of the list while new elements can be chained onto the head.

### 1.6 Stack (Data Structure)

A stack is a LIFO data structure like a stack of papers.

The Stack ADT has four operations:
* `create` creates an empty stack.
* `push` adds data to the top of the stack.
* `pop` removes data from the top of the stack.
* `empty` returns true if the stack is empty.

Both an array-based and a list-based implementation can be built to run in constant, O(1) running time.
