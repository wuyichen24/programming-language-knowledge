## Concurrency
- [goroutine](#goroutine)
- [Go Channel](#go-channel)
- [sync Package](#sync-package)
- [Concurrency patterns](#concurrency-patterns)
   - [The for-select Loop](#the-for-select-loop)

## goroutine
- **Concepts**
   - Lightweight, user-space threads that allow concurrent execution of functions.
   - It's not a real thread.
   - It has its own call stack, which grows and shrinks as required.
   - Goroutines are managed by the Go runtime, and they are multiplexed onto a smaller number of OS threads.
- **Creation**
   - Create a new goroutime: Append the “go” keyword before the function that you would like to run concurrently.
     ```go
     go somefunction()

     go func() {

     }
     ```
  
## Go Channel
- **Concepts**
   - Used for communication and synchronization between goroutines.
- **Create channel**
   - Example
     ```go
     myChannel1 := make(chan int)          // The type of messages is int
     myChannel2 := make(chan string)       // The type of messages is string
     myChannel3 := make(chan string, 2)    // The type of messages is string, the channel can buffer up to 2 messages
     ```
- **Send messages**
   - Send single message
     ```go
     myChannel1 <- "hello"                 // Send string message "hello" to the channel
     ```
   - Send multiple messages
     ```go
     var tasks []Task
     for _, task := range tasks {
         chTask <- task
     }
     ```
- **Receive messages**
   - Receive single message
     ```go
     message := <-myChannel1               // Receive a message from the channel
     ```
   - Receive multiple messages
     ```go
     var results []Result
     for result := range chResult {
         results = append(results, result)
     }
     ```
- **Channel direction**
   - Concepts
      - When using channels as function parameters, you can specify if a channel is meant to only send or receive values.
   - Examples
      - Only for sending message
        ```go
        func Func1(myChannel1 chan<- string) {    // myChannel1 is only for sending message
            myChannel1 <- "hello"
        }
        ```
      - Only for receving message
        ```go
        func Func2(myChannel2 <-chan string) {    // myChannel2 is only for receiving message
            message := <- myChannel2
        }
        ```
- **Close channel**
   - You need to close channel when you finish.
   - Example
     ```
     close(myChannel1)
     ```
- **time.After() function**
   - Generates a channel that only receives a value after the specified timeout, in effect producing a blocking channel for a predefined period of time.

## Select
### Concepts
- The select statement provides another way to handle multiple channels.
- It's like a switch, but each case is a communication:
   - All channels are evaluated.
   - Selection blocks until one communication can proceed, which then does.
   - If multiple can proceed, select chooses pseudo-randomly.
   - A default clause, if present, executes immediately if no channel is ready.
### Example
- Basic example
  ```go
  select {
  case v1 := <-c1:                                     // receive from channel c1 
      fmt.Printf("received %v from c1\n", v1)
  case v2 := <-c2:                                     // receive from channel c2
      fmt.Printf("received %v from c2\n", v1)
  case c3 <- 23:                                       // send 23 to channel c3
      fmt.Printf("sent %v to c3\n", 23)
  default:
      fmt.Printf("no one was ready to communicate\n")
  }
  ```

## sync Package
### Simple lock
```go
var myMutex = &sync.Mutex{}

myMutex.Lock()
myMap[1] = 100   // Protect myMap against multiple goroutines changing it at the same time.  
myMutex.Unlock()
```
### Read-write lock
- Concepts
   - If one goroutine performs read operations, other goroutines won't be blocked.
   - If one goroutine performs write operations, other read and write operations will be blocked until the write lock is released.
- Example
  ```go
  var myRWMutex = &sync.RWMutex{}

  // Read operation
  myRWMutex.RLock()
  fmt.Println(myMap[1])
  myRWMutex.RUnlock()

  // Write operation
  myRWMutex.Lock()
  myMap[2] = 200
  myRWMutex.Unlock()
  ```
### Wait Groups
- Concepts
   - Allows you to wait for multiple goroutines to finish before you proceed with the rest of your code.
- Example
  ```go
  var wg = &sync.WaitGroup{}     // Create a global waitgroup
  
  func main() {
      wg.Add(2)                  // Increment the wait group internal counter by 2
      go routineFunction1()      // Create 1st goroutine
      go routineFunction2()      // Create 2nd goroutine
      wg.Wait()                  // Wait till the wait group counter is 0
  }
  
  func routineFunction1() {
      defer wg.Done()            // When goroutine function finishes execution, decrement the internal wait group counter by one.
      // some other operations
  }
  
  func routineFunction2() {
      defer wg.Done()            // When goroutine function finishes execution, decrement the internal wait group counter by one.
      // some other operations
  }
  ```
- Common function
   - `wg.Add(int)`: Increase the `WaitGroup` counter by `int`.
   - `wg.Done()`: Decrease the `WaitGroup` counter by 1.
   - `wg.Wait()`: Wait till the `WaitGroup` counter is 0.

## Concurrency patterns
### The for-select Loop
- **Pattern**
  ```
  for { // Either loop infinitely or range over something
      select {
          // Do some work with channels
      }
  }
  ```
- **Example**
   - Sending iteration variables out on a channel
     ```
     for _, s := range []string{"a", "b", "c"} {
         select {
         case <-done:
             return
         case stringStream <- s:
         }
     }
     ```
   - Looping infinitely waiting to be stopped
     ```
     for {
         select {
         case <-done:
             return
         default:
         }

         // Do non-preemptable work
     }
     ```

### Fan-in
- **Examples**
   - Basic
     ```go
     func fanIn(input1, input2 <-chan string) <-chan string {
         c := make(chan string)
         go func() { for { c <- <-input1 } }()
         go func() { for { c <- <-input2 } }()
         return c
     }
     ```
   - Using `select`
     ```go
     func fanIn(input1, input2 <-chan string) <-chan string {
         c := make(chan string)
         go func() {
             for {
                 select {
                     case s := <-input1:  c <- s
                     case s := <-input2:  c <- s
                 }
             }
         }()
         return c
     }
     ```

## Code example
### Process tasks concurrently
```go
type TaskInput struct {}
type TaskOutput struct {}

func processTasks(tasks []TaskInput) {
    g := runtime.GOMAXPROCS(0)                             // get total number of CPUs 
    var wg sync.WaitGroup
    wg.Add(g)

    chTaskInputs := make(chan TaskInput, g)                // create channel for sending task inputs to multiple goroutines
    chTaskOutput := make(chan TaskOutput)                  // create channel for collecting task outputs from multiple goroutines

    for i := 0; i < g; i++ {
        go func() {                                        // create a single goroutine
            defer func() {                                 // when this goroutine is finished, decrease the counter by 1
                wg.Done()
            }()

            for taskInput := range chTaskInputs {
                taskOutput := processTask(taskInput)       // process a single task
                chTaskOutput <- taskOutput
            }
        }()
    }

    for _, task := range tasks {                           // distribute task inputs to multiple goroutines
        TaskInput <- task
    }

    close(chTaskInputs)
    wg.Wait()

    var taskOutputs []TaskOutput
    for taskOutput := range chTaskOutput {                 // collect task outputs from multiple goroutines
        taskOutputs = append(taskOutputs, taskOutput)
    }
	 
    close(chTaskOutput)
}
```
