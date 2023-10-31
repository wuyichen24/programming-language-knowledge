# Syntax

- [Variable](#variable)
   - [Declaration](#declaration)
   - [Pointer](#pointer)
- [Constant](#constant)
   - [Declaration](#declaration-1)
- [Slice](#slice)
   - [Declaration](#declaration-2)
   - [Initialization](#initialization)
   - [Operations](#operations)
   - [2D slice](#2d-slice)
- [Map](#map)
   - [Declaration](#declaration-3)
   - [Initialization](#initialization-1)
   - [Operations](#operations-1)
- [Statements](#statements)
   - [for](#for)
   - [if](#if)
   - [switch](#switch)
- [Function](#function)
   - [Parameters](#parameters)
   - [Return values](#return-values)
- [Struct & Interface](#struct--interface)
   - [Struct](#struct)
   - [Interface](#interface)
- [Other keywords](#other-keywords)
   - [defer](#defer)
  
## Variable
### Declaration
- `var`
   - Basic example
     ```go
     var name string
     ```
   - Variable can be declared within function or without function.
   - If multiple variables have the same type, you only need to define the type after the last variable
     ```go
     var firstName, lastName string
     ```
   - Variable can be initialized.
     ```go
     var i, j int = 1, 2
     ```
   - If the variable has been initialized, the variable type can be omitted and the variable will get the type from the initial value.
     ```go
     var name = "John";      // string
     var i    = 42           // int
     var f    = 3.142        // float64
     var c    = 0.867 + 0.5i // complex128
     ```
- `:=`
   - Basic example
     ```go
     name := "John"
     ```

### Pointer
- `*T`is a pointer to a value of type T
   ```go
   var p *int   // p points to type int
   ```
- `&` operator generates a pointer to the variable
   ```go
   i := 42
   p := &i   // p points to the address of variable i
   ```
- `*` operator accesses the content pointed to by the pointer
   ```go
   i := 42
   p := &i
   fmt.Println(*p)   // print 42
   ```
   
## Constant
### Declaration
- Declare a single constant
  ```go
  const PlanID = "xyz"
  ```
- Declare multiple constants
  ```go
  const (
      PortNumber = 23
      IpAddress = "123.234.345"
  )
  ```

## Slice
### Declaration
- `var`
  ```go
  var s1 []int
  var s2 [10]int
  ```
  
### Initialization
- `:=`
  ```go
  primes := []int{2, 3, 5, 7, 11, 13}
  primes := [6]int{2, 3, 5, 7, 11, 13}
  ```
- `make`
  ```go
  a := make([]int, 5)      // len=5
  b := make([]int, 0, 5)   // len=0, cap=5
  ```
  
### Operations
- Get length (`len()`)
  ```go
  primes := []int{2, 3, 5, 7, 11, 13}
  len(primes)
  ```
- Get capacity (`cap()`)
  ```go
  primes := []int{2, 3, 5, 7, 11, 13}
  cap(primes)
  ```
- Add element (`append`)
  ```go
  var s []int
  s = append(s, 0)         // {0}
  s = append(s, 1)         // {0,1}
  s = append(s, 2, 3, 4)   // {0,1,2,3,4}
  ```
- Modify element
  ```
  numbers := []int{1, 2, 3, 4, 5}
  numbers[2] = 999
  ```
- Traversal
   - `for-range`
      - `for <index>:<element> := range <slice>`
     ```go
     s := []int{2, 3, 5, 7, 11, 13}
     for i, v := range s {
         fmt.Printf("%d, %d\n", i, v)
     }
     ```
- Get sub-slice
  ```go
  primes := [6]int{2, 3, 5, 7, 11, 13}
  var s1 []int = primes[1:4]   // {3,5,7}
  var s2 []int = primes[:4]    // {2,3,5,7}
  var s3 []int = primes[1:]    // {3,5,7,11,13}
  var s4 []int = primes[:]     // {2,3,5,7,11,13}
  ```
  
### 2D slice
- Declaration
  ```go
  matrix = make([][]int, numRows)
  for i := range matrix {
      matrix[i] = make([]int, numCols)
  }
  ```
- Traversal
  ```go
  for row := 0; row < len(matrix); row++ {
      for col := 0; col < len(matrix[row]); col++ {
          element := matrix[row][col]
          fmt.Printf("matrix[%d][%d] = %d\n", row, col, element)
      }
  }
  ```

## Map
### Declaration
```go
var m map[string]int = make(map[string]int)
```

### Initialization
```go
var m = map[string]int{
    "aaa": 111,
    "bbb": 222,
}
```

### Operations
- Add
  ```go
  m["aaa"] = 111
  ```
- Access
  ```go
  fmt.Println(m["aaa"])
  ```
- Remove (`delete()`)
  ```go
  m["aaa"] = 111
  delete(m, "aaa")
  ```
- Check key existing (`elem, ok = m[key]`)
  ```go
  m["aaa"] = 111
  v1, ok1 := m["aaa"]    // v1 = "111", ok1 = true
  v2, ok2 := m["bbb"]    // v1 = 0,     ok2 = false
  ```
- Traversal
  ```go
  for key, value := range m {
      fmt.Println(key, ":", value)
  }
  ```

## Statements
### `for`
- Loop in a numeric range
  ```go
  for i := 0; i < 10; i++ {
      sum += i
  }
  ```
- Loop on slice (`for-range`)
  ```go
  s := []int{2, 3, 5, 7, 11, 13}
  for i, v := range s {
      fmt.Printf("%d, %d\n", i, v)
  }
  ```
- Loop on map (`for-range`)
  ```go
  var m map[string]int = make(map[string]int)
  for key, value := range m {
      fmt.Println(key, ":", value)
  }
  ```
- Go doesn't have `while` loop, but you can do the similar
  ```go
  sum := 1
  for sum < 1000 {
      sum += sum
  }
  ```
- Infinite loop
  ```go
  for {
  
  }
  ```

### `if`
- Basic example
  ```go
  if x < 0 {
      y := 6
  }
  ```
- You can add a simple statement to the `if` condition, and the scope is within all branches
  ```go
  if x := 5 ; y > x {
      return x + y
  }
  ```

### `switch`
- Basic example
  ```go
  switch day {
  case "Monday":
      fmt.Println("It's Monday. Get to work!")
  case "Tuesday":
      fmt.Println("It's Tuesday. Still working.")
  case "Wednesday":
      fmt.Println("It's Wednesday. Halfway there.")
  case "Thursday":
      fmt.Println("It's Thursday. Almost there.")
  case "Friday":
      fmt.Println("It's Friday. Time to celebrate!")
  default:
      fmt.Println("It's the weekend. Enjoy!")
  }
  ```
- You can use expressions in a switch statement
  ```go
  number := 7
  switch {
  case number < 5:
      fmt.Println("Number is less than 5.")
  case number >= 5 && number < 10:
      fmt.Println("Number is between 5 and 9.")
  default:
      fmt.Println("Number is 10 or greater.")
  }
  ```
- The `case` block in the `switch` statement comes with a `break`. But if you want to continue consider the next case, you can use `fallthrough` keyword.
  ```go
  score := 85
  switch {
  case score >= 90:
      fmt.Println("A")
      fallthrough // Continue to the next case
  case score >= 80:
      fmt.Println("B")
  case score >= 70:
      fmt.Println("C")
  case score >= 60:
      fmt.Println("D")
  default:
      fmt.Println("F")
  }
  ```
  
## Function
### Parameters
- The type of the parameter is defined after the parameter
  ```go
  func add(x int, y int)
  ```
- If multiple parameters have the same type, you only need to define the type after the last parameter
  ```go
  func add(x, y int)
  ```
- Function parameter can be a function
  ```go
  func negative(fn func(int) int) int {
      return fn(3)
  }
      
  func main() {
      fn := func(x int) int {
          return x + 2
      }
      fmt.Println(negative(fn))
  }
  ```
- Go does not support function overloading
  
### Return values
- Function can have multiple return values
  ```go
  func swap(x, y string) (string, string)
  ```
- The return value can be named
  ```go
  func split(sum int) (x, y int) {
      x = sum * 4 / 9
      y = sum - x
      return
  }
  ```

## Struct & Interface
### Struct
- Declaration
  ```go
  type ABC struct {
      Field1 int
      Field2 string
  }
  ```
- Construction
  ```go
  abc := new(ABC)
  
  abc := ABC{}

  abc := ABC {
      Field1: 3,
      Field2: "ABC"
  }
  ```
- Create member function
  ```go
  func (abc *ABC) Method1() {
      abc.Field1 + abc.Field2
  }
  ```
- Access
  ```go
  abc.Field1    // access data
  abc.Method1() // access function
  ```

### Interface
- Declaration
  ```go
  type IService interface {
      createInventory() boolean
      deleteInventory() boolean
  }
  ```
- Implementation (implement interface)
  ```go
  type CarService struct {}
  type TreeService struct {}

  func (c *CarService) createInventory() boolean {}
  func (c *CarService) deleteInventory() boolean {}

  func (t *TreeService) createInventory() boolean {}
  func (t *TreeService) deleteInventory() boolean {}
  ```
- Calling (use interface)
  ```go
  func MethodX(service IService) {
      service.createInventory()
      service.deleteInventory()
  }

  carService := CarService{}
  MethodX(carService)
  
  treeService := TreeService{}
  MethodX(treeService)
  ```
- Interface can also become a field in a struct
  ```go
  type Handler struct {
      service         IService
      repository      IRepository
      eventPublisher  IEventPublisher
      distLockManager ILockManager
  }
  ``` 

## Other keywords
### defer
- The deferred function is executed just before the surrounding function's return.
  ```go
  func main () {
      defer fmt.Println("world")
      fmt.Println("hello")
  }
  ```
- Multiple `defer` statements in a function will be pushed into a stack and executed in the order of FILO.
- `defer` is commonly used for tasks such as closing files, releasing resources, unlocking mutexes, and handling panics gracefully.
