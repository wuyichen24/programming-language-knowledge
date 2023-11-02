## Concurrency
- [**goroutine**](#goroutine)
- [**Channel**](#channel)
- [**select**](#select)
- [**sync Package**](#sync-package)
   - [Simple lock](#simple-lock)
   - [Read-write lock](#read-write-lock)
   - [#wait-groups](#wait-groups)
- [**Concurrency patterns**](#concurrency-patterns)
   - [The for-select Loop](#the-for-select-loop)
   - [Fan-in](#fan-in)
- [**Code example**](#code-example)
   - [Process tasks concurrently](#process-tasks-concurrently)

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
  
## Channel
- **Concepts**
   - Used for communication and synchronization between goroutines.
   - A channel is a first-class value that can be allocated and passed around like any other. 
- **Create channel**
   - Example
     ```go
     myChannel1 := make(chan int)           // The type of messages is int
     myChannel2 := make(chan string)        // The type of messages is string
     myChannel3 := make(chan string, 2)     // The type of messages is string, the channel can buffer up to 2 messages
     myChannels := make([]<-chan string, 3) // Create 3 channels in a slice
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

## select
### Concepts
- The select statement provides a control structure to handle multiple channels.
- It's like a switch, but each case is a communication:
   - All channels are evaluated.
   - Selection blocks until one communication can proceed, which then does.
   - If multiple can proceed, select chooses pseudo-randomly.
   - A default clause, if present, executes immediately if no channel is ready.

### Special channels
- `done` channel
   - Signal the completion or termination of a goroutine or a specific task.
     ```go
     done := make(chan interface{})
     defer close(done)                  // Close the 'done' channel when the task is complete

     // Wait for the task to complete or for a timeout
     select {
     case <-done:
         fmt.Println("Task completed.")
     case <-time.After(2 * time.Second):
         fmt.Println("Timeout: Task took too long.")
     }
     ```
- `time.After()` channel
   - The `time.After()` function returns a channel that blocks for the specified duration.
   - Local timeout
     ```go
     for {
         select {
         case s := <-c:
             fmt.Println(s)
         case <-time.After(1 * time.Second):
             fmt.Println("You're too slow.")
             return
         }
     }
     ```
   - Global timeout
     ```go
     timeout := time.After(5 * time.Second)
     for {
         select {
         case s := <-c:
             fmt.Println(s)
         case <-timeout:
             fmt.Println("You talk too much.")
             return
         }
     }
     ```

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
### Generator
- **Concepts**
   - Converts a set of discrete values into a stream of values on a channel.
- **Examples**
   - Template
     ```go
     func generator(done <-chan interface{}, values ...interface{},) <-chan interface{} {
         valueStream := make(chan interface{})
         go func() {
             defer close(valueStream)
             for {
                 for _, v := range values {
                    select {
                    case <-done:
                        return
                    case valueStream <- v:
                    }
                }
             }
         }()
         return valueStream
     }
     ```

### Pipeline
- **Concepts**
   - Process a stream of values from a channel and return a stream of new values to another channel.
- **Examples**
   - Template
     ```go
     func pipeline(done <-chan interface{}, valueStream <-chan interface{}) <-chan interface{} {
         takeStream := make(chan interface{})
         go func() {
             defer close(takeStream)
             for value := range valueStream {
                 select {
                 case <-done:
                     return
                 case takeStream <- {{operation on value}}:
                 }
             }
         }()
         return takeStream
     }
     ```

### Filter
- **Concepts**
   - Based on pipeline, filter some values from a channel and only return the values satisfying the criteria to another channel.
- **Examples**
   - Template
     ```go
     func Filter(done <-chan interface{}, inputStream <-chan int, operator func(int)bool) <-chan int {
         filteredStream := make(chan int)
         go func() {
             defer close(filteredStream)
  
             for {
                 select {
                 case <-done:
                     return
                 case i, ok := <-inputStream:
                     if !ok {
                         return
                     }
        
                     if !operator(i) { break }     // if the current value doesn't satisfy the criteria, it will ignore that value.
                     select {
                     case <-done:
                         return
                     case filteredStream <- i:
                     }
                 }
             }
         }()
         return filteredStream
     }
     ```

### Fan-in
- **Concepts**
   - Combine multiple results into one channel.
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
   - Template
     ```go
     func fanIn(done <-chan interface{}, channels ...<-chan interface{}, ) <-chan interface{} {
         var wg sync.WaitGroup                         // Wait until all channels have been drained.
         multiplexedStream := make(chan interface{})

         multiplex := func(c <-chan interface{}) {
             defer wg.Done()
             for i := range c {
                 select {
                 case <-done:
                     return
                 case multiplexedStream <- i:
                 }
             }
         }

         // Select from all the channels
         wg.Add(len(channels))
         for _, c := range channels {
             go multiplex(c)
         }

         // Wait for all the reads to complete
         go func() {
             wg.Wait()
             close(multiplexedStream)
         }()

         return multiplexedStream
     }
     ```
### Fan-out
- **Concept**
   - Start multiple goroutines to handle input from the pipeline.
- **Examples**
   - Template
     ```go
     numCPUs := runtime.NumCPU()
     outputChannels := make([]<-chan int, numCPUs)
     for i := 0; i < numCPUs; i++ {
         outputChannels[i] = doSomething(done, inputChannel)
     }
     ```

### or-done channel
- **Examples**
   - Template
     ```go
     func(done, c <-chan interface{}) <-chan interface{} {
         valStream := make(chan interface{})
         go func() {
             defer close(valStream)
             for {
                 select {
                 case <-done:
                     return
                 case v, ok := <-c:
                     if ok == false {
                         return
                     }
                     select {
                     case valStream <- v:
                     case <-done:
                     }
                 }
             }
         }()
         return valStream
     }
     ```
### tee channel
- **Concepts**
   - Get values from one channel and it will return two separate channels that will get the same value
- **Examples**
   - Template
     ```go
     func tee (done <-chan interface{}, in <-chan interface{},) (_, _ <-chan interface{}) { <-chan interface{}) {
         out1 := make(chan interface{})
         out2 := make(chan interface{})
         go func() {
             defer close(out1)
             defer close(out2)
             for val := range orDone(done, in) {
                 var out1, out2 = out1, out2
                 for i := 0; i < 2; i++ {
                     select {
                     case <-done:
                     case out1<-val:   // send value to first output channel
                         out1 = nil    // Once we’ve written to a channel,
                                       // we set its shadowed copy to nil so that further writes will block and the other channel may continue.
                     case out2<-val:   // send value to second output channel
                         out2 = nil
                     }
                 }
             }
         }()
         return out1, out2
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
    

    chTaskInputs := make(chan TaskInput, g)                // create channel for sending task inputs to multiple goroutines
    chTaskOutput := make(chan TaskOutput)                  // create channel for collecting task outputs from multiple goroutines

    for i := 0; i < g; i++ {
        wg.Add(1)
        go func() {                                        // create a single goroutine
            defer wg.Done()                                 // when this goroutine is finished, decrease the counter by 1

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

## References
- [Go Concurrency Patterns](https://go.dev/talks/2012/concurrency.slide#1)
- [5 Concurrency Patterns in GO to Enhance your next Project](https://blog.devgenius.io/5-useful-concurrency-patterns-in-golang-8dc90ad1ea61)
- [Go Concurrency Patterns: Pipelines and cancellation](https://go.dev/blog/pipelines)
