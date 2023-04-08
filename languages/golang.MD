# Golang Notes

Notes for an intermediate to advanced level understanding of Go.

## Interfaces
Interfaces are named collections of method signatures. They are used to specify the behavior of an object. If something can do *this*, then it can be used *here*.

One type can implement multiple interfaces.

Example:
```
type Sequence []int

// Methods required by sort.Interface.
func (s Sequence) Len() int {
    return len(s)
}
func (s Sequence) Less(i, j int) bool {
    return s[i] < s[j]
}
func (s Sequence) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

// Copy returns a copy of the Sequence.
func (s Sequence) Copy() Sequence {
    copy := make(Sequence, 0, len(s))
    return append(copy, s...)
}

// Method for printing - sorts the elements before printing.
func (s Sequence) String() string {
    s = s.Copy() // Make a copy; don't overwrite argument.
    sort.Sort(s)
    str := "["
    for i, elem := range s { // Loop is O(N²); will fix that in next example.
        if i > 0 {
            str += " "
        }
        str += fmt.Sprint(elem)
    }
    return str + "]"
}
```

## Structs

Structs are typed collections of fields that can group data together to form records. 

```
type User struct {
    Name  string
    Email string
    Status bool
}
```

## Type Assertion
Type assertions check the type of the concrete value. For testing, a type assertion can return a second boolean value to see if there is a match.

Example:
```
t := i.(T)
t, ok := i.(T)
```

## Modules

Go modules were introduced in 1.11 and replaces earlier Go package management systems like `dep` and `Godeps.json`.

Modules are created with `go mod init [name]`.
Dependencies can be installed with `go get`.
Unused packages can be cleaned up with `go mod tidy`.

Module paths are declared in `go.mod`.

### Publishing Releases

Every required module in `go.mod` has a semantic version (vMAJOR.MINOR.PATCH).

Pre-release versions can be specified by appending a hyphen and dot separated identifiers.

v1 major versions and beyond requires backwards compatibility within that major version. Minor versions are for backwards-compatible feature changes and patches are for bug fixes.

### V2 Releases and Beyond
It is recommended to develop v2+ modules in a directory named after the major version suffix. For example `example.com/hello` -> `example.com/hello/v2`.

Copy the v1 `go.mod` into the new directory. Then run  `go mod edit [module] v2/go.mod`. This allows multiple major versions to co-exist in the same build. Update v2 packages to use the new import path. This can be done with `sed`:
```find . -type f \
    -name '*.go' \
    -exec sed -i -e 's,github.com/my/project,github.com/my/project/v2,g' {} \;
```

### Maintaining Compatibility

Breaking changes are often introduced by changing or adding arguments to functions. Variadic parameters do not solve for this. Do not modify a function's signature but instead create a new function.

For interfaces, a new interface should be defined with a new method. If the old interface is used, a dynamic check should identify if the provided type is older or newer.

Configuration methods can also be used to allow users to opt-in to new changes without punishing existing users.

Structs can be added to directly without introducing breakages (mostly). An exception comes when all field types are comparable, and therefore the struct type itself is also comparable. If a non-comparable field is added, any downstream code that uses the struct via comparables would also break. Testing can identify and prevent these scenarios.


## Contexts

The `context` package is used to gather information about the execution environment.

```
package main

import (
	"context"
	"fmt"
)

func doSomething(ctx context.Context) {
	fmt.Printf("doSomething: myKey's value is %s\n", ctx.Value("myKey"))
}

func main() {
	ctx := context.TODO()
	doSomething(ctx)
}
```

`context.TODO()` creates an empty, starting context. `context.Background()` creates an empty context but is intended to signal the context is known.

Best practice: use contexts for data that is not required for a function to run (e.g. logging).

### Completion and Cancelation

Contexts can signal to downstream functions that it has finished execution. Example:
```
ctx := context.Background()
resultsCh := make(chan *WorkResult)

for {
	select {
	case <- ctx.Done():
		// The context is over, stop processing results
		return
	case result := <- resultsCh:
		// Process the results received
	}
}
```

This could also be done using `ctx, cancelCtx := context.withCancel(ctx)`.

### Context Deadlines
Deadlines set constraints on when the context will automatically finish.
```

func doSomething(ctx context.Context) {
	deadline := time.Now().Add(1500 * time.Millisecond)
	ctx, cancelCtx := context.WithDeadline(ctx, deadline)
	defer cancelCtx()

	printCh := make(chan int)
	go doAnother(ctx, printCh)

	for num := 1; num <= 3; num++ {
		select {
		case printCh <- num:
			time.Sleep(1 * time.Second)
		case <-ctx.Done():
			break
		}
	}

	cancelCtx()

	time.Sleep(100 * time.Millisecond)

	fmt.Printf("doSomething: finished\n")
}
```

This can be paired with `context.WithTimeout` to specify a `time.Duration` instead of a `time.Time` when the context should expire.

## Goroutines

Goroutines are lightweight threads. They execute concurrently alongside other goroutines in the same address space. They are multiplexed onto multiple OS threads.

They are invoked by prefixing a function or method call with `go`.

`go list.Sort()`.

Function literals are useful for invoking goroutines. Function literals are closures, so the implementation makes sure the variables survive as long as they are active.

## Channels

Channels are a typed conduit for sending and receiving values via `<-`. `ch <- v` sends `v` to channel `ch`. Like maps and slices, channels must be initialized prior to use: `ch := make(chan int)`.

An optional integer parameter sets the buffer size for the channel. The default value is 0.

```
c := make(chan int)  // Allocate a channel.
// Start the sort in a goroutine; when it completes, signal on the channel.
go func() {
    list.Sort()
    c <- 1  // Send a signal; value does not matter.
}()
doSomethingForAWhile()
<-c   // Wait for sort to finish; discard sent value.
```

Receivers always block until there is data to receive. If the channel has a buffer, the sender blocks only until the value has been copied to the buffer.

A channel is a first-class value and can be passed in to other values. This can be used for safe, parallel demultiplexing.
```
type Request struct {
    args        []int
    f           func([]int) int
    resultChan  chan int
}
```

Calculations can be split and parallelized across multiple CPU cores.

## Defer
`defer` is used to defer a function or method being returned until the surrounding function returns. Its arguments are evaluated immediately, but it is not executed until later.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

## Buffer

Buffer is included inside the `bytes` package. It can be used to give buffer storage for accessing and storing data that is too large for normal variables.
```
package main
import (
	"fmt"
)
//importing the bytes package so that buffer can be used
import (
	"bytes"
)
func main() {
//Creating buffer variable to hold and manage the string data
	var strBuffer bytes.Buffer
	strBuffer.WriteString("Ranjan")
	strBuffer.WriteString("Kumar")
	fmt.Println("The string buffer output is",strBuffer.String())
}
```

## Select

`select` lets a goroutine wait on multiple communication operations through blocking. It will return the channel values as they are received.

Example across two channels:
```
package main

import (
    "fmt"
    "time"
)

func main() {

    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "two"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```

## Mutex

Mutexes are data structure provided by the `sync` package to lock different sections of data so that only one goroutine can access it at a time.

Example:
```
func main() {
	n := 0
	var m sync.Mutex

	// now, both goroutines call m.Lock() before accessing `n`
	// and call m.Unlock once they are done
	go func() {
		m.Lock()
		defer m.Unlock()
		nIsEven := isEven(n)
		time.Sleep(5 * time.Millisecond)
		if nIsEven {
			fmt.Println(n, " is even")
			return
		}
		fmt.Println(n, "is odd")
	}()

	go func() {
		m.Lock()
		n++
		m.Unlock()
	}()

	time.Sleep(time.Second)
}
```

Concurrent reads can be permitted (atomic writes) with `sync.RWMutex`.

Mutexes can be bundled into structs.

## JSON

The `json` package is used to read and write from JSON data in Go.

To encode JSON data, use the `Marshal` function

Example:
```
func Marshal(v interface{}) ([]byte, error)

type Message struct {
    Name string
    Body string
    Time int64
}

m := Message{"Alice", "Hello", 1294706395881547000}

b, err := json.Marshal(m)
```

Returns b == []byte(`{"Name":"Alice","Body":"Hello","Time":1294706395881547000}`)

To decode JSON data, use the `Unmarshal` function. JSON can be decoded into structs.

Example:
```
birdJson := `{"species": "pigeon","description": "likes to perch on rocks"}`
var bird Bird	
json.Unmarshal([]byte(birdJson), &bird)
fmt.Printf("Species: %s, Description: %s", bird.Species, bird.Description)
//Species: pigeon, Description: likes to perch on rocks
```
