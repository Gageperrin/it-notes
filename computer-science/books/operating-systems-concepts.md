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

Storage is only one type of I/O device. A general-purpose computer system consists of CPUs and multiple device controllers. A SCSI (small computer-systems interface) controller can have seven or more devices attached. The device controller maintains some local buffer storage. Operating systems have a device driver for each device controller. An I/O operation is started when the device driver loads the appropriate registers within the device controller. The device controller acts according to the specification of the register, transferring data from the device ot the local buffer. The device controller interrupts the device driver, and the device driver returns control to the operating system.

Interrupt-driven I/O can cause issues for bulk data movement. Direct memory access (DMA) is used to solve this. In this case, the device controller transfers an entire block of data directly to or from its buffer storage to memory. Only one interrupt is generated per block rather than one interrupt per byte. Some high-end systems use switch architecture where multiple components talk to other components concurrently.

#### 1.3 Computer-System Architecture ####
##### 1.3.1 Single-Processor Systems #####

Until recently, most computers had a single processor with one main CPU for general-purpose instructions. Single-processor systems usually include other special-purpose processors as well. The special-purpose processors do not run user processes. Microprocessors are also used to alleviate the CPU such as in a PC keyboard.

##### 1.3.2 Multiprocessor Systems #####

Multiprocessor systems (i.e. parallel systems, multicore systems) are common now. A multiprocessor system has two or more processes in communication, sharing a computer bus, clock, memory, and peripheral devices. The three advantages of a multiprocessor are: (1) increased throughput, (2) economy of scale, and (3) increased reliability. (3) is particularly crucial for graceful degradation and fault tolerance.

There are two types of multiprocessor systems. Asymmetric multiprocessing includes a boss processor delegating tasks to worker processors. More common though is symmetric multiprocessing (SMP) where each processor performs all tasks in an operating system. All processors are peers. 

Multiprocessing can cause a system to change its memory access model from uniform memory access (UMA) to non-uniform memory access (NUMA). UMA is when memory access from any CPU takes an equal amount of time. NUMA by contrast may take an uneqal amount of time.

CPU design now features multiple computing cores on a single chip--multicore chips. These are more efficient with faster communication rates.

Blade servers feature multiple processor boards, I/O boards, and networking boards within the same chassis.
 
##### 1.3.3 Clustered Systems #####

A clustered system gathers multiple CPUs because they have multiple nodes (individual systems) joined together. This is a loosely coupled system where each node may be either single or multiprocessor in nature. Clustering provides high-availability for when one node system may fail, and they are also used for high-performance computing environmnets which depend on parallelization. 

Clustering may be structured asymmetrically or symmetrically. Asymmetric clustering has one machine in hot-standby mode where it monitors an active server but does nothing else. In symmetric clustering, two or more hosts are running applications while monitoring each other. Symmetric is more efficient.

Parallel clusters or wide-area network (WAN) clusters can also be implemented but require a distributed lock manager (DLM) for effective version control and system locking to prevent conflicting operations.

Storage-area networks (SANs) can also be used to distribute storage across various geographic zones.

#### 1.4 Operating-System Structure ####

Multiprogramming is a core component of operating systems. Several jobs are kept in memory simultaneously within a job pool. The pool consists of all processes residing on disk awaiting an allocation of the main memory. The CPU switches between jobs. Time sharing systems are where the CPU executes multiple jobs by switching between them. This depends on an interactive computer system for the user to instruct a program directly. A program loaded into memory and which is executing is a process. When there are multiple jobs but not enough room in memory for all of them, job scheduling is required. 

CPU scheduling determines the sequence of several jobs to be run. In a time sharing system, the operating system ensures reasonable response times through swapping. More commonly, virtual memory is used to run a process that is not entirely in memory. This abstracts logical memory from physical memory.

#### 1.5 Operating-System Operations ####

Modern operating systems are interrupt driven. Events are signalled by an interrupt or a trap. A trap (i.e. exception) is a software generated interrupt caused by an error.

##### 1.5.1 Dual-Mode and Multimode Operations #####

Operating system code and user-defined code are separated into kernel and user mode respectively. A mode bit is added to the computer hardware to indicate the current mode. `0` is kernel and `1` is user. At system boot time, the hardware starts in kernel mode. This allows for privileged instructions to separate out user permissions. CPUs with virtualization can have a virtual machine manager (VMM) to be in control of the system. The VMM has more privileges than a user but less than the kernel.

##### 1.5.2 Timer #####

A variable timer is implemented by a fixed-rate clock and a timer. This is used to implement proper controls over user programs and interrupt after a certain period of time if it gets stuck in an infinite loop.

#### 1.6 Process Management ####

For process management, the operating system is responsible for: 
* Scheduling processes and threads on the CPUs
* Creating and deleting both user and system processes
* Suspending and resuming processes
* Providing mechanisms for process synchronization
* Providing mechanisms for process communication

#### 1.7 Memory Management ####

For memory management, the operating system is responsible for: 
* Keeping track of which parts of memory are currently being used and who is using them
* Deciding which processses and data to move into and out of memory
* Allocating and deallocating memory space as needed

#### 1.8 Storage Management ####

The operating system abstracts from the physical properties of storage devices to define a logical storage unit: the file. 

##### 1.8.1 File-System Management #####

File management is one of the most user-visible components of an operating system. A file is a collection of related information defined by its creator. They represent programs and data. A file is an umbrella concept for many kinds of entities.

For file management, the operating system is responsible for: 
* Creating and deleting files
* Creating and deleting directories to organize files
* Supporting primitives for manipulating files and directories
* Mapping files onto secondary storage
* Backing up files on stable (nonvolatile) storage media

##### 1.8.2 Mass-Storage Management #####

Most computer systems use disk memory as the principal on-line storage medium. Secondary storage is used frequently and therefore also must be efficient. Storage may also be slower and lower in cost in tertiary storage devices such as magnetic tape drives. Media can vary between WORM (write-once, ready-many) and RW (read-write) formats. Tertiary storage must also be managed.

For disk management, the operating system is responsible for: 
* Free-space management
* Storage allocation
* Disk scheduling

##### 1.8.3 Caching #####

Cache systems are fast short-term storage. A system will first check if a particular piece of information is stored in-cache before checking the memory. If it is not in-cache, then a copy is stored in-cache temporarily. Internal index registers provide a high-speed cache for main memory. Caches are also implemented completely in hardware.

The size of the cache is important to manage for performance. Cache coherency is also important in multiprocessor environments which have multiple local caches. This becomes more evident in distributed environments.

##### 1.8.4 I/O Systems #####

The I/O subsystem hides the peculiarity of I/O devices from both the operating system and the user. An I/O subsystem contains (1) a memory management component (buffering, caching, and spooling), (2) a general device-driver interface, and (3) drivers for specific hardware devices.


#### 1.9 Protection and Security ####


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
