# Great Ideas in Computer Architecture

These are my notes for UC Berkeley's CS61C FA20 course "Great Ideas in Computer Architecture" presented by Dan Garcia and Borivoje Nikolic. It is provided here on [YouTube](https://www.youtube.com/playlist?list=PL0j-r-omG7i0-mnsxN5T4UcVS1Di0isqf) for the Fall 2020 course.

## Table of Contents

1. [Introduction](#lecture-1---intro)
2. [Number Representation](#lecture-2---number-representation)
3. [Introduction to C](#lecture-3---introduction-to-c)
4. Floating Point
5. RISC-V
6. Compilation, Assembly, Linking, Loading
7. Introduction to Synchronous Digital Systems
8. State, State Machines
9. Combinational Logic
10. Single-Cycle CPU
11. Pipelining
12. Caches
13. Introduction to OS and Virtual Memory
14. I/O
15. Flynn Taxonomy, SIMD Instructions
16. Thread-Level Parallelism
17. MapReduce, Spark
18. Data Centers, Cloud Computing
19. Dependability-Parity, ECC, RAID
20. GPU

## Lecture 1 - Intro

This course is concerned with the hardware-software interface. It is not concerned primarily with C programming.

Six Ideas in Computer Architecture:
1. Abstraction through layers of representation and interpretation
2. Moore's Law
3. Principle of Locality and Memory Hierarchy
4. Parallelism
5. Performance Measurement and Improvement
6. Dependability via Redundancy

A high-level language such as C compiles an assembly language program (RISC-V) which in turn assembles machine language program (RISC-V) which in turn is derived from the hardware architecture description (e.g. block diagrams) which is an architecture implementation of the logic circuit description (circuit schematic diagrams), and so on down through transistors.

## Lecture 2 - Number Representation

The real world is analog. To import data to digital format, analog must be sampled and quantized. Not all digital data is necessarily rooted in analog data.

Bits can represent characters, logical values, colors, and just about anything.

N bits is at most 2^n things.

Numerals are representations of numbers. Computer science in one sense can be understood as the economics of numerals and numbers.

### Bases

By default, numbers are written in base 10.

* Base 1 - tally system - |||
* Base 2 - binary (0 and 1). bits are *bi*nary digi*ts*
* Base 10 - decimal (0-9)
* Base 16 - hexadecimal (0-9 and a-f)

Example conversions:
* Binary 1101 -> 2^3+2^2+0+2^0 -> 8+4+0+1 -> 13 in decimal
* Hexadecimal A5 -> 10 x 16^1 + 5 x 16^0 = 165

Quick reference
| D | H | B | 
| -- | -- | -- |
| 00 | 0 | 0000 |
| 01 | 1 | 0001 |
| 02 | 2 | 0010 |
| 03 | 3 | 0011 |
| 04 | 4 | 0100 |
| 05 | 5 | 0101 |
| 06 | 6 | 0110 |
| 07 | 7 | 0111 |
| 08 | 8 | 1000 |
| 09 | 9 | 1001 |
| 10 | A | 1010 |
| 11 | B | 1011 |
| 12 | C | 1100 |
| 13 | D | 1101 |
| 14 | E | 1110 |
| 15 | F | 1111 |

Binary ->
Always left-pad with 0's to make full 4-bit balues.
0b11110 -> 0x1E


### Unsigned Integers
Unsigned integers are a simple form of number representation in computers where all the bits of a given size (e.g., 8, 16, 32, or 64 bits) are used to represent non-negative integers. Because there is no need to represent a sign (positive or negative), unsigned integers can represent a wider range of positive numbers than their signed counterparts of the same bit size. The highest number is represented by all bits set to 1, and the lowest is 0. Overflow occurs if you try to store a number too large for the allocated bits, causing it to "wrap around" to the lowest value.


### Signed Integer Representations
When it comes to representing both positive and negative numbers, several methods have been developed, each with its own set of benefits and drawbacks:

#### Sign and Magnitude ####
Sign and Magnitude: This method uses the leftmost bit to indicate the sign of the number, with, say, 0 for positive and 1 for negative. The remaining bits represent the magnitude (absolute value) of the number. This approach is straightforward but introduces complexities such as having two representations of zero (positive zero and negative zero) and makes arithmetic operations more complex.


#### One's Complement ####

One's Complement: To represent a negative number in one's complement, you flip all the bits of its positive counterpart. This method also suffers from having two representations of zero (all bits 0 for positive zero and all bits 1 for negative zero) and introduces additional complexity in arithmetic operations, especially around carrying bits during addition.


#### Two's Complement ####
Two's Complement addresses many of the issues found in sign and magnitude and one's complement representations. To find the two's complement (and thus, the negative representation) of a number, you first invert all the bits (as in one's complement) and then add 1 to the least significant bit. This method has several advantages, such as a single representation for zero and more straightforward arithmetic operations. The process you described (1101 -> 0010 -> 0011) correctly demonstrates how to convert a binary number to its negative counterpart in two's complement notation.


#### Bias Representation ####

Bias (or Excess-N) is another method to represent signed numbers, where a fixed number, known as the "bias," is added to the actual number to keep all representations positive. For example, if the bias is 15 and the range is 5 bits, then the number zero is represented as 01111 (15 in binary), and the maximum positive value would be represented as 11110 (30 in binary, which is 15 + 15), while the maximum negative value starts from 00000 (which is -15 in this scheme). This method is less common in general-purpose computing but can be found in specific applications like floating-point number representations.

Unsigned numbers, Two's complement, and Bias Representation are the most common ways for handling number representation today.

## Lecture 3 - Introduction to C

ENIAC was the first electronic general-purpose computer in 1946. It could multiply in 2.8ms but needed 2-3 days to program using patch cords and switches.

EDSAC was the first general stored-program computer in 1949 which could hold numbers in memory. Bits are not just the numbers but the instructions on how to manipulate the numbers.

### Great Idea #1 - Abstraction

THe layers of computing:
* High level language program - C
* Compiled by an Assembly Language Program - RISC-V
* Assembled by a Machine Language Program - RISC-V
* Hardware architecture description - block diagram
* Logic circuit description - circuit schematic diagram

First developed in the early 1970s, C enabled the first operating system not written in assembly. UNIX became a portable OS (1973).

### Compile vs. Interpret

C compilers map C programs directly into architecture-specific machine code. For C, programs are first compiled from `.c` files to `.o` files then linking the `.o` files into executables. Assembling is hidden to the user.

Compile advantages:
* Makefiles require only modified files to recompiled, not every file
* Excellent run-time performance

Compile disadvantages:
* Compiled files and executables are architecture-specific and depend on both processor type and the OS
* Because of this, the executable must be rebuilt on each new system
* The iteration cycle of change, compile, run can be slow during development.
* Linking can introduce bottlenecks

### C Pre-Processor (CPP)

C source files first pass through macro processor (CPP) before compiler sees code. CCP commands begin with `#` such as `#include "file.h"`. `-save-temps` options can be used with gcc to see result of pre-processing.

CPP macros can be defined to create small functions to change the text of the function (e.g. `#define` is string replacement). This can produce unintentional errors when these functions have unintentional side-effects.

### C Language

* Function oriented
* Programming unit at function level
* Compilation: `gcc hello.c`
* Execution: `a.out`
* Storage: manual (`malloc`, `free`)
* Comments: `/* */`
* Constants: `#define`, `const`
* Variable declaration at beginning of a block
* No clear naming conventions in C
* Libraries accessed with `#include`

Hello World example

```
#include <stdio.h>
int main<void>
{
    printf("Hi\n");
    return 0;
}
```

C99 is an update to C which adds:
* Declaration in `for` loops
* `//` at end of line
* Variable-length non-global arrays
* `<inttypes.h>` - explicit integer types
* `<stdboolh>` for boolean logic definitions

C11 and C18 were further updates with:
* Multi-threading
* Unicode strings and constants
* Removal of `gets()`
* Type-generic Macros

### C Syntax

To get the `main` function to accept arguments use `int main (int argc, char *argv[])`.

`argc` will contain the number of strings on the command lines, with the executable counting as one. `argv` is a pointer to an array containing the arguments as strings.

C by default is true except for `0`, `null`, and boolean false types defined in `stdbool.h`.

Types:
* `int` - signed integers of at least 16b
* `unsigned int` - unsigned integers
* `float` - floating point decimal
* `double` - equal or higher precision floating point
* `char` - single character
* `long` - longer `int` of at least 32b
* `long long` - even long `int` of at least 64b

Integer type should be the most efficient type for the processor.

Constant is assigned a typed value once in declaration, and the value cannot change during the entire execution of the program. Enums are a group of related integer constants. Color and card suits are an example of enums.

Functions in C are typed. Every function needs an explicit return type or it can be specified as `void` to indicate no value will be returned. Variables and functions must be declared before they are used.

`typedef` allows you to define new types. `struct` is for a structured group of variables.

If else - `if (expression) statement1 else statement2`
While - `while (expression) statement` or `do statement while (expression)`
For - `for (initialize; check; update) statement`
Switch -
```
switch (expression) {
    case const1: statements
    case const2: statements
    default: statements
    }
    break;
```

`goto` - An advanced technique for navigating non-local control flow

### Memory Addressing and Pointers

Memory is a single huge array. Each cell of the array has an address associated with it and stores some value. The address refers to the memory location not the value of the location itself. The address points to a memory location. A pointer is a variable that contains the address of a variable.

Pointer syntax:
* `int *p` - The variable `p` is the address of an `int`.
* `p = &y` - The address of y is assigned to the variable `p`.
* `z = p*` - The value at address stored in `p` is assigned to the variable `z`

Pointers are useful when dealing with large data values as it can pass along a reference to the data without the data itself. No need to copy large data amounts. Pointers can point any data type (even functions) but normally only one type. `void *` can point to anything but is dangerous to use.

The drawback is that pointers are typically one of the largest source of bugs in C programming, especially with dynamic memory management, dangling references, and memory leaks.

For structures, pointers can use arrow or dot notation to access properties. Pointer arithmetic can be used to move a certain number of bytes along memory address.

Pointers can be reassigned or point to another pointer. 
Null pointers can cause the program to crash is it is used to read or write. However, tests can evaluate for the null pointer (since it is false) to easily handle exceptions.

Modern machines are byte-addressable with memory composed of 8-bit storage cells with a unique address. A C pointer is abstracted memory address. The type declaration tells compiler how many bytes to fetch on each access through the pointer. The `sizeof()` operator returns the number of bytes in an object.

### Arrays

Arrays are a contiguous block of memory. They are almost identical to pointers (`char *string` and `char string[]`). They differ in a subtle way. Arrays are declared as filled and are not incremented as pointers are. An array variable is a pointer to the first element. Declared arrays are only allocated while the scope is valid.

Arrays maintain a single source of truth through indirection that allows you to forego multiple copies of the same memory value.

The pitfall is that an array in C does not know its own length and does not check its bound. You can thus accidentally access outside the end of an array, so the array and its size must be passed to a procedure. Because of this shortcoming, segmentation faults and bus errors can emerge and these are difficult to solve.

### Dynamic Memory Allocation

The `sizeof()` gives the size of objects in bytes, includign arrays. To allocate room for something new, use `malloc()` with the help of a typecast. `malloc()` initializes a memory location with garbage.

To dynamically free space, use `free(ptr)`. The program frees all memory on exit, however be wary for when `main` may become a subroutine that is never freed. 

Common stumbling blocks include running `free()` on the same memory value twice or calling `free()` on something not gotten back from `malloc()`.

The memory heap can be managed with `realloc(p, size)`. It resizes a previously allocated block at `p` to a new size. If `p` is null, then `realloc` behaves like `malloc`. If size is 0, then `realloc` behaves like `free`, deallocatingteh block from the heap. This returns the new address of the memory block which likely has moved.

Dynamic memory management introduces additional overhead due to the requirements of freeing memory afterward.

#### Example: Linked List ####
```
struct Node {
    char *value;
    struct Node *next;
};
typedef struct Node *List;

/* Initialize an empty list */
List ListNew(void)
{ return NULL; }

/* Add a string to an existing list */
List list_add(List list, char *string)
{
    struct Node *node = (struct Node*) malloc(sizeof(struct Node));
    node -> value = (char*) malloc(strlen(string) + 1);
    strcpy(node->value, string);
    node->next = list;
    return node;
}
```

### Memory Locations

Structure declaration does not allocate memory, but variable declaration does.

Memory can be allocated to declare a local variable, for dynamic allocation at runtime, or to declare data outside of any procedure (global variables).

C has three pools of memory:
* Static storage - global variable storage, permanent during entire runtime.
* Stack - local variable storage, parameters, return address
* Heap - dynamic `malloc()` storage

Address space container four region:
* `stack` local variables, grows downward
* `heap` space requested for pointers via `malloc()`, grows upward
* `static data` is locked during runtime
* `code` is locked during runtime

Variables declared outside a procedure are allocated in static storage. Variables declared inside a procedure are allocated on the stack and freed when the procedure returns.

The heap is a large pool of memory not allocated in contiguous order. External fragmentation is when most of free memory is in many small chunks.

`malloc()` searches the free list for a block that is big enough. If none is found, more memory is requested from the OS. If it can't find sufficient memory it fails. `free()` checks if the blocks adjacent to the fred block are also free. If they are it coalesces the blocks into a larger, free block of memory.

`malloc()` can choose blocks of memory based on best fit, first fit, and next fit

Pointers are used for passing large structs or arrays of data but also cause the most bugs in conjunction with dynamic memory management. Segmentation faults are when a program tries to read or write memory at illegal locations.

Common examples:
* A dangling reference occurs if you use `ptr` before `malloc()`.
* A memory leak occurs if the tardy is free and loses the pointer. 
* A buffer overflow occurs when a pointer writes off the end of an array.
* Trying to free unallocated or deallocated data
*  `realloc` can move data which means that reads can be corrupted if they point to the wrong, original address space via a stale pointer.


## Floating Point

In the representation of fractions, binary point like a decimal point signifies the boundary between the integer and fractional parts.

Example: 10.1010 in binary = 2.625 in base ten 

xx.yyyy

The last y is 2^-4

If we assume a fixed binary point, range of 6-bit representation is 0 to 3.97375 (almost 4).

Addition and multiplication can also be represented with fixed point.

Fixed binary points face limitations with size, floating point is much more flexible and an efficient use of limited bits.

With floating point representation, each numeral carries an exponent field recording the locations of its binary point. The binary point can be outside the stored bits, so very large and small numbers can be represented.

Normalized form of decimal notation means there are no leading zeros. The four components of scientific decimal and binary notation are the mantissa, binary point, the radix, and the exponent.

With floating point notation there is a sign, an exponent, and a significand.

If the result is too large, it causes overflow where the exponent is larger than the represented exponent field. If the result is too small (close to zero), it causes underflow where the negative exponent is larger than the represented 8-bit exponent field.

IEEE 754 Floating Point Standard uses a biased exponent representation. Designers want to used fixed point numbers even without fixed point hardware. With this standard, number are ordered exactly the same as in sign-magnitude.

Biased notation is where bias is a number subtracted to get a real number. IEEE 754 uses bias of 127 for single precision. Double precision identical except with an exponent bias of 1023 (half, quad similar).

### Special Numbers

To represent infinity, use the most positive exponent (255) with significands of all zeros.

Zero is represented with an exponent and significands of all zeros.

If exponent is 255 and significand is nonzero, it is a NaN (Not a number). These are useful for debugging.

If exponent is zero, and significand is nonzero, it is a denorm. There is gap among representable numbers when they get exceedingly close to zero. There is a limit at smallest representable positive number. Normalization and implicit 1 is to blame. A denormalized number has no implied leading 1, with an implicit exponent of -126.

Example of decimal equivalents of floating point notation...

### Issues with Floating Point

* Floating Point is not associative because it only approximates the real result.
* With floating point hardware, two extra bits are included for rounding.
* IEEE FP Rounding Modes
   * Round toward positive or negative infinity
   * Truncate - round to 0
   * Unbiased rounding - round to even in binary

FP addition with floats is more difficult than with integers because the significands cannot simply be added to each other. First the values must be denormalized to match exponents, then the significands must be added to get the result. Keep the same exponent and then normalize it.

Casting floats to ints and vice versa uses truncation in C.