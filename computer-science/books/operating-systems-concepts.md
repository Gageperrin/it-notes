# Operating System Concepts
Abraham Silberschatz, Peter Baer Galvin, Greg Gagne
Ninth Edition. Wiley (2013)

## Part One: Overview
### Chapter 1: Introduction

An **operating system** is a program that manages a computer's hardware. It serves to provide two contrasting objectives of *convenience* and *efficiency*. The operating system is software that manages computer hardware. The hardware must provide appropriate mechanisms to ensure its correct operation.

#### 1.1 What Operating Systems Do ####

A computer system can be divided into four components: hardware, operating system, application programs, and users.

The hardware along with the central processing unit (CPU), memory, and input/output (I/O) devices, provide basic computing resources for the system. Application programs define the ways in which resources are used to solve user problems. The operating system controls the hardwrae and coordinates its use among various application programs for users.

Taken differently, a computer can be divided into hardware, software, and data. The operating system provides for an environment within which programs are executed.

##### 1.1.1 User View #####

The user's view of the computer varies according to the interface being used. In most cases, when the user operates an independent system. The objective is to maximize the efficiency of the computer's performance for the sake of the user. Operating systems are designed primarily for ease of use, secondarily for performance, with little attention to resource utilization.

In a connected system, a user may sit at a terminal connected to a mainframe or minicomputer. Sometimes users sit at workstations connected to networks of workstations and servers. Mobile computers may also act as standalone units for individual users.

##### 1.1.2 System View #####

For a computer, the opertaing system is a resource allocator. It can also be seen as a control program.

##### 1.1.3 Defining Operating Systems #####

The operating system can also be defined as a kernel running at all times. Alongside the kernel are system programs and application programs. Mobile operating systems also include middleware frameworks that provide additional services to developers.

#### 1.2 Computer-System Organization ####
##### 1.2.1 Computer-System Operation #####

A general purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus which provides access to the shared memory. A device controller manages a specific kind of driver. CPU and device controllers can execute in parallel and compete for memory. The memory controller orchestrates memory access.

For a computer to start running it needs to initialize through a bootstrap program. It is typically stored within the hardware in read-only memory (ROM) or electrically erasable programmable read-only memory (EEPROM) better known as firmware.

Once the kernel is loaded it can provide services to the system and users. Some service are provided outside the kernel as system processes or systems daemons that run the entire time the kernel is running. Once booted, the system waits for some event to occur. An event is signalled by an interrupt from the hardware or software. Software does this via a system call (i.e. monitor call). An interrupt stops the CPU and transfers execution to a fixed location.

Interrupt routines are called indirectly through  a table of pointers stored in low memory values. The table holds the addresses of the interrupt service routines for the various devices. The interrupt vector of addresses is indexed by a unique device number to provide the address of the interrupt service routine for the interrupting device. The interrupt architecture must also save the address of the interrupted instruction.

##### 1.2.2 Storage Structure #####

The CPU can only load instructions from memory. General purpose computers run most of their programs from main memory. Main memory commonly is implemented in a semiconductor technology called dynamic random-access memory.

The standard instruction-execution cycle (e.g. a von Neumann architecture) fetches an instruction from memory and stores that instruction in the instruction register. The instruction is decoded and may cause operands to be fetched from memory and stored in some internal register. Ideally, programs and data reside in main memory permanently, but this is typically not possible for two reasons: (1) main memory is too small to permanently store all programs and data; (2) main memory is a volatile storage device that loses its contents when power is turned off.

Most computer systems provide secondary storage as an extension of main memory. Most commonly, this is done through a magnetic disk which provides storage for both programs and data. Other types include cache memory, CD-ROM, magnetic tapes, etc. In essence, a storage system stores a datum and holds it for retrieval at a later time. Storage systems often face contrasting emphases on speed versus cost. From fastest to slowest, these are some storage device types: registers, caching, main memory, solid-state disk, magnetic disk, optical disk, magnetic tapes.

Solid-state disks have several variations. One stores data in a DRAM array and also contains a magnetic hard disk for backup in case of power loss. Flash memory is often used in cameras and PDAs. Flash is slower than DRAM but requires no power. NVRAM is DRAM with battery backup power.

##### 1.2.3 I/O Structure #####

### Chapter 2: Operating-System Structures

## Part Two: Process Management
### Chapter 3: Processes
### Chapter 4: Threads
### Chapter 5: Process Synchronization
### Chapter 6: CPU Scheduling
### Chapter 7: Deadlocks

## Part Three: Memory Management
### Chapter 8: Main Memory
### Chapter 9: Virtual Memory

## Part Four: Storage Management
### Chapter 10: Mass-Storage Structure 
### Chapter 11: File-System Interface
### Chapter 12: File-System Implementation
### Chapter 13: I/O Systems

## Part Five: Protection and Security
### Chapter 14: Protection
### Chapter 15: Security

## Part Six: Advanced Topics
### Chapter 16: Virtual Machines
### Chapter 17: Distributed Systems

## Part Seven: Case Studies
### Chapter 18: The Linux System
### Chapter 19: Windows 7 
### Chapter 20: Influential Operating Systems

## Part Eight: Appendices
### Appendix A: BSD Unix
### Appendix B: The Mach System
