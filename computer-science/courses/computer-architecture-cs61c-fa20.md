# Great Ideas in Computer Architecture

These are my notes for UC Berkeley's CS61C FA20 course "Great Ideas in Computer Architecture" presented by Dan Garcia and Borivoje Nikolic. It is provided here on [YouTube](https://www.youtube.com/playlist?list=PL0j-r-omG7i0-mnsxN5T4UcVS1Di0isqf).

## Table of Contents

1. [Introduction](#introduction)
2. [Number Representation](#number-representation)
3. Introduction to C
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