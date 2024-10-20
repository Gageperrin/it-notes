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

## Week 2 Binary Search Trees

### 2.1 Tree Terminology

A tree is a linked structure with inheritance. Each element in a tree is a node and each connection between two nodes is an edge. Trees must always contain a root node with no incoming edges. Nodes that contain no outgoing edges are called leaf nodes. A node's children are the nodes with the node as its parent. 

A tree must have a root, directed edges, and not have a cycle. It is a rooted, directed, and acyclic structure.

### 2.2 Binary Trees

A binary tree is a tree where every node has no more than two children. The height of a binary tree is the number of edges in the longest path from the root to a leaf. 

A binary tree is full if and only if every node has either zero children or two children. A binary tree is perfect if and only if all interior nodes have two children and leaves are at the same level. A binary tree is complete if and only if the tree is perfect up until the last level and all leaf nodes on the last level are pushed to the left.

### 2.3 Tree Traversal

Tree traversal consists of shouting out the node to see if there is left and right branches, then traversing first the left branch and exhausting it before traversing the right branch. When both branches are exhausted it returns to the parent node.

### 2.4 Binary Search Tree

A binary search tree is an ordered binary tree capable of being used as a search structure. A binary tree is a BST if for every node in the tree nodes in the left subtree are less than itself. Nodes in the right subtree are greater than itself.

Dictionary ADT:
* `find` finds the data associated with a key in the dictionary
* `insert` adds a key/data pair to the dictionary
* `remove` removes a key from the dictionary
* `empty` returns true if the dictionary is empty

A BST used to implement a dictionary will store both a key and value in every node.

The in-order predecessor (IOP) of a node will always be the right most node in the node's left sub-tree.

To remove from a BST:
* Zero children: delete the node
* One child: Remove like a linked list
* Two children: Find the IOP of the node to be removed, swap with the IOP and the remove the node in its new position

### 2.5 BST Analysis

BST can take on many forms and structures, even containing the same data.


Action | BST avg. case| BST worst case | Sorted Array | Sorted List
------ | ------|----------| ------ | ------- |
find |    O(lg(n))      | O(n)  |  O(lg(n))   | O(n)
insert |  O(lg(n))      | O(n)  |  O(n)  | O(n)
remove |  O(lg(n))      | O(n)  |  O(n)  |  O(n)

The height balance factor (b) of a node is the difference in height between two trees. A balnaced BST is a BST where every node's balance factor has a magniture of 0 or 1.

There are n! different ways to create BSTs with the same data. The worst case BST will have a height proportional to the number of nodes while an avergae BST will have a height proportional to the logarithm of the number of nodes.

## Week 3: AVL Trees and B-Trees

### 3.1 AVL Trees

Balanced BSTs are height-balanced trees that ensure nearly half of the data is located in each subtree. BST sub-structures include a mountain with both a right and a left child and a stick that has two children only in one direction. Sticks should be converted into mountains. 

A balance factor is the absolute difference between heights of left and right subtrees. A higher balance factor means less balance.

To do this, first identify the node that is deepest into the tree that makes the tree out of balance. Identify the stick, break it in half, and raise it up as a mountain and re-attach the children. For example, a stick slanted to the right should be broken in half and everything rotated to the left. This is a generic left rotation.

An elbow occurs when a stick is bent. It is first necessary to unbend the elbow with a rotation about the end so that it becomes a straightened stick. 

There are four possible rotations: L, R, LR, and RL. Rotations are local operations that do not impact the broader tree. Rotations run in O(1) time. 

AVL trees are self-balancing BSTs like the above. They are named after Adelson-Velsky and Landis. Everything about a BST remains true in an AVL tree, but there is extra work on insert and remove.

For an AVL insert, insert at the proper place, check for imbalance, rotate if necessary and then update height.

For an AVL remove, find the node, swap it with the IOP, check for imbalance, rotate if necessary and then update height.

Again, an AVL tree is a BST implementation that also maintains the height at each node and the balance factor.

### 3.2 B-Trees

B-Trees are designed to perform well both in main memory and on-disk memory. The order of a B-Tree refers to the size of the nodes, it is the maximum number of keys that a given node can have plus one. A single B-tree should minimize the number of network packets, disk seeks, or whatever operation is being conducted. Again, the goal is to minimize the seeks to reach the data.

A B-tree of order m is a tree that contains nodes of order m. Each node can contain no more than m-1 keys. Each internal node can have at most m children, so a B-tree of order m is like an m-way tree. To insert into a b-tree, throw up the value into a new node. The inserted key becomes a root node, splitting the B-tree into a child node on the left and right. 

A B-tree recursive insert will continuously throw up middle values to create deeper and deeper root nodes to maintain two value nodes.

B-tree properties:
* All keys within a node are in sorted order.
* Each node contains no more than m-1 keys.
* Each internal node has exactly one more child than key. A root node can be a leaf or have `[2,m]` children. Each non-root, internal node has `[ceil(m/2),m]` children.
* All leaves are on the same level.

With searching a B-Tree, the height of the B-Tree determines the maximum number of seeks possible in search data and the height of the structure is log<sub>m</sub>(n). Therefore the number of seeks can be no more than this number.

## Week 4: Heaps

### 4.1 Heap Introduction

A complete binary tree T is a min-heap if the tree is empty or has both a left and a right node that are larger than the original node. Every descendant must be larger than its ancestor. The memory representation of a heap is an array and is easily accessible.

### 4.2 Heap Insert

To insert a value into a heap, it should be inserted at the end of the array. This value should be compared with the represented ancestor to make sure it is larger than its parent. If it is not, it should be swapped with its parent node, and this should be done recursively until it is greater than its parent or is at root. Doubling an array adds a new layer on the bottom of the heap tree.

### 4.3 Heap - RemoveMIN

It is only possible to remove the most minimum element from a heap through a `heapifyDown`. The value to be removed should be swapped with the value at the end of the array and then removed. The value that was swapped should then be moved down the tree by comparing it with the left and right child nodes until it is given a new location on the tree.

### 4.4 Heap - buildHeap

Three possible ways to insert into an unsorted array:
(1) Sort the array.
(2) Call insert a number of times with `heapifyUp`
(3) Sort the array to have only the first half of the array have its heap properties restored then apply `heapifyDown` on the rest.

Given any sort of data, buildHeap can be completed in O(n) time.

### 4.5 Heap - Runtime Analysis

A heap can be built in linear O(n) time.

A heap sort consists of (1) building a heap, (2) call `n` number of removeMn, and (3) swap elements to order. Running time at worst will be `n * log(n)`. A heap sort is an ideal sorting algorithm.

# Unordered Data Structures

These are my notes to University of Illinois' Unordered Data Structures course on Coursera offered as part 3 of their Computer Science Accelerated Fundamentals Specialization track.

## Week 1: Hashing

### 1.1 Hashing Introduction

Hashing defines a keyspace (a mathematical description of the keys for a set of data) to use a function to map the keyspace into a small set of integers.

A hash table based dictionary consists of (1) a hash function, (2) an array, and (3) collision handling (what happens when a hash matches two different values).

### 1.2 Hashing - Hash Function

A perfect hash function has an onto function where there is a unique bilateral match for each hashed key and value.

A hash function has two parts: a hash and a compression. A hash transforms an input into an integer. A compression functoin takes a fixed lenght input and returns a fixed-length output.

A good hash function runs in O(1), is deterministic so that the output reliably comes out the same, and satisifes the SUHA requirement. SUHA is the simple uniform hashing assumption and says that the result of a hash algorithm must be uniform across an entire key space.

### 1.3 Hash Function Examples

Examples provided in lecture.

### 1.4 Collision Handling I: Separate Chaining

Sometimes the hashing function returns the same value for two different inputs. This often happens with weak hashing algorithms. This can be resolved with separate chaining which puts the clashing values in a linked list. For example if 29 % 7 and 8 % 7 both match 1. Then at index "1" in the array, it can point to 29 which points to 8.

The main cost is that the load factor will grow as linked lists accumulate inside arrays.

### 1.5 Collision Handling II: Probing and Double Hashing

Probe-based hashing will probe ahead in the array to find the next index without a corresponding value and insert it there, even if it means looping to the beginning of the array. Linear probing has problematic runtimes because of primary clustering. Primary clustering will have a set of values disproportionately grouped in a single block.

Double hashing can solve this. Double hasing applies an alternative shifted hash function if the value returned for the first one is full.

### 1.6 Hashing Analysis

Separate chaining works better for big records while double hashing works better for structure speed. Hash tables replace a dictionary. Hashing does not have any nearest neighbor approach like BSTs do. So for range finding or nearest neighbor, a tree structure is best. Hash tables are best if the exact key is known and is used only for lookups.

### 1.7 Hash Tables in C++

```
std::map
  ::operator[]
  ::insert
  ::erase
  ::lower_bound(key) -> Iterator to first element <= key
  ::upper_bound(key) -> Iterator to first element > key
```

An unordered map has the first three but no bound approach. Instead it has:
* `::load_factor()`
* `::max_load_factor(ml)`


## Week 2: Disjoint Sets

### 2.1 Disjoint Sets Introduction

A disjoint set (also known as union-find or merge-find) stores a collection of non-overlapping sets which are held to be equivalent regardless of the elements contained inside them. Each set has a unique identity and a representative member.

In C++, union is a reserved word and cannot be used as a name for a union find function.

### 2.2 Naive Implementation

If there is enough room in the array, each value across disjoint sets (including its identity value) can be assigned array values in sequence. But this only works well under ideal circumstances.

### 2.3 UpTrees

This implementation continues to use an array where the index is the key. The value of the array is `-1` if the representative element has been located or the index of the parent if it is not the representative element. This is called an UpTree. It is a tree where each node except the root has a single pointer that points to the parent. This makes it easy to update a data structure with disjoint sets.

### 2.4 UpTrees: Simple Running Time

The running time of a disjoint sets find is proportional to the height of the algorithm O(h). The ideal UpTree is extremely flat with a height of one. Otherwise, it can have a very bad worst case runtime.

### 2.5 UpTrees: Smart Union and Path Compression

To keep track of the height of the tree, it is good practice to replace a `-1` root node with the negative height of the tree minus one. For example a node of tree with height 1 would be `-2`.

Union by height should keep the height of the tree as small as possible. Union by size should minimize the number of nodes that increase in height.

Path compression can be used to point intermediate tree nodes to the root node itself reducing path traversal.

The iterated log function is the number of times you can take a log of a number before reaching 1.

In a disjoint set implemented with smart unions and path compression on find, any sequence of m union and find operations result in the worst case running time of `O(m*log(n))` where n is the number of items in the Disjoint set.

## Week 3: Graph Data Structures

### 3.1 Graphs Introduction

A graph is perhaps the most powerful data structure in this course. A graph can label every node with different color or shape, so that no colors or shapes are touching.

### 3.2 Graph Vocabulary

* `G` is a collection of vertices and edges where every node is connected.
* Node and vertex can be used interchangeably.
* Edge is the connection between two nodes.
* Degree is the count of number of incident edges in a collection.
* A path is a sequence of vertices connected by edges.
* A cycle is a path with a common beginning and end vertex.
* A simple graph is a graph with no self loops or multi-edges.
* A subgraph contains all the vertices and edges of a particular subgraph. Subgraphs can be complete, connected, a connected component, an acyclic subgraph, and spanning tree.

Run times are reported by the number of vertices but can often depend on the number of edges.

### 3.3 Graphs: Edge List Implementation

Graph ADT functions: (edgeless implementation)
* `insertVertex(K key)`
* `insertEdge (Vertex v1, Vertex v2, K key)`
* `removeVertex (Vertex v)`
* `removeEdge(Vertex v1, Vertex v2)`
* `incidentEdges(Vertex v)`
* `areAdjacent(Vertex v1, Vertex v2)`
* `origin(Edge e)`
* `destination(Edge e)`

### 3.4 Graphs: Adjacency Matrix Implementation

An adjacency matrix stores a false value if an edge between two vertices does not exist, and true if there is. An index on a vertex list is matched with the possible connections on an edge list. Inserting a vertex has between O(1)* -> O(n) and removing a vertex has O(n). Checking adjacency has O(1) runtime. Checking incident edges is O(n).

### 3.5 Adjacency List Implementation

The third implementation is the adjacency list implementation. This combines some features of each.

The index in the vertex list stores pointers to two separate values and two separate rows in the edge list.

The runtimes are:
* insertVertex: O(1)*
* removeVertex: O(deg(v))
* areAdjacent: min(deg(v1), deg(v2))
* incidentEdges: deg(v)

Expressed as O(f) | Edge List | Adjacency Matrix | Adjacency List
------ | ----------| ------ | ------- |
Space |  n + m | n<sup>L<sup> | n+m | 
insertVertex(v) | 1 | n | 1 | 
removeVertex(v) | m | n | deg(v) | 
insertEdge(v, w, k) | 1 | 1 | 1 |
removeEdge(v, w) | 1 | 1 | 1 |
incidentEdges(v) | m | n | deg(v) |
areAdjacent(v, w) | m | 1 |  min(deg(v), deg(w)) | 
  
### 4.1 Graph Traversal

#### 4.1.1 Graphs: BFS Traversal

A traversal visits every vertex and edge in the graph. BST traversals are ordered, have an obvious start, and a notion of completeness.

Graph traversal is different because it is not sequential, not clear where it starts, and must be designed to traverse every node exactly once. It starts with one node, visits all of its children, then all of their children.

#### 4.1.2 Graphs: BFS Analysis

A well-designed BFS traversal can handle disjoint graphs and count components, it can detect a cycle, and have a running time of O(n+m).

#### 4.1.3 Graphs: DFS Traversal

In a depth-first traversal, it goes deep very quickly instead of visiting each generation of children at a time.

### 4.2 Minimum Spanning Trees (MST)

#### 4.2.1 Minimum Spanning Tree

A minimum spanning tree takes in a connected, undirected graph G with edge weights, and it outputs a spanning graph that is a connected, acyclic tree with a minimal total weight among all spanning trees.

#### 4.2.2 MST: Kruskal's Algorithm

Kruskal's algorithm helps find the shortest route on the entire map. It requires an organized list (mini-heap) of edge weight for every vertex in the graph. Every vertex needs to be provided with its own uptree.

It removes the minimum element from the heap and check if the two vertices in the element are in the same disjoint set. If they are not, then they are unioned together so that they are in the same set. If they are in the exact same est, nothing needs to be done.

Building a priority queue in a heap takes O(m) time while for a sorted array takes O(m * lg(m)). Each `removeMin` in a heap takes log(m) runtime and for a sorted array O(1).

#### 4.2.(3) MST: Prim's Algorithm

Prim's algorithm offers a different approach. An arbitrary partition of the vertices on G into two subsets U and V will have an edge of minimum weight across the partition. This edge will be part of some minimum spanning tree.

The partition property itself suggests an algorithm and finds the minimum spanning tree.

The input is a graph with a starting vertex and return a spanning tree as output.

Runtime for a heap is O(n<sup>2<sup> + m log (n)) for adjusting the matrix and O(n log(n) + m (log(n)) for adjusting the list. For an unsorted array it is O(n<sup>2<sup>) for both the matrix and the list. Prim's algorithm is m * log m for a sparse graph but n<sup>2<sup>log(n) for a dense graph.
  
### 4.3 Shortest Path Algorithms

#### 4.3.1 Dijkstra's Algorithm

Instead of finding the minimum spanning tree, Dijkstra's Algorithm finds the shortest path on a graph. It goes through every vertex on a graph and labels the distance to that vertex to infinity and the previous pointer as null, just like Prim's algorithm. The initial vertex is set to 0 with hte same priority Q.

Dijkstra's Algorithm gives the shortest path from a single source path to every connected vertex.


#### 4.3.2 Dijkstra's Algorithm: Edge Cases

In cases of a single heavy-weight path versus multiple light-weight paths, Dijkstra's Algorithm still effectively finds the shortest path. With negative weight cycles, Dijkstra's algorithm fails. Shortest path algorithms always fail with negative weight cycles in general as shortest paths in these cases are infinite.


#### 4.3.3 Dijkstra's Running Time

The Fibonacci heap optimizes Dijkstra's algorithm to make it the smallest time in any shortest path algorithm with a run time of O(m + n * log(n)). It is just slightly more than a traversal but still very good.

