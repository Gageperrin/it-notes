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