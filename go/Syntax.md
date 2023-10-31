# Syntax

## Variable
- **Declaration**
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
- **Declaration**
   - Basic example
     ```go
     const PlanID = "xyz"

     const (
         PortNumber = 23
         IpAddress = "123.234.345"
     )
     ```

## Slice
- **Declaration**
   - `var`
     ```go
     var s1 []int
     var s2 [10]int
     ```
- **Initialization**
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
- **Operations**
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
   - Traverse
      - `for-range`
         - `for <index>:<element> := range <slice>`
        ```go
        s := []int{2, 3, 5, 7, 11, 13}
        for i, v := range s {
            fmt.Printf("%d, %d\n", i, v)
        }
        ```
     
## Function

## Statements

## Struct
