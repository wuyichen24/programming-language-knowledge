# Naming Convention

## Rules
### File
- File name are all lower case, multiple words are separated by underscore.
  ```
  plan_transition_repository.go
  ```
- Test file name should have a suffix `_test`.
  ```
  service.go
  service_test.go
  ```

### Constant
- Constants are upper camel case.
  ```go
  const StatusNotFound = "404"
  ```

### Variable
- Variables are lower camel case.
  ```go
  var statusNotFound = "404"
  ```

### Function
- Unexported function names are lower camel case.
  ```go
  func updateRecord(record Record) {}
  ```
- Exported function names are upper camel case.
  ```go
  func UpdateRecord(record Record) {}
  ```

### Inteface
- Prefer short but meaningful names.
- Avoid I-prefix.
   - For instance, prefer `Reader` over `IReader`.
 
## References
- [Practical Go: Real world advice for writing maintainable Go programs](https://dave.cheney.net/practical-go/presentations/qcon-china.html#_simplicity)
