# Syntax

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

## Constant
### Declaration**
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
