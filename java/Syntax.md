# Syntax

## Statements
### for loop
- Format 1
  ```java
  for (int i = 0; i < n; i++) {
      // do something
  }
  ```
- Format 2
  ```java
  for (String s : list) {
      // do something
  }
  ```

### while loop
- while loop
  ```java
  while (i < n) {
      // do something
  }
  ```
- do-while loop
  ```
  do {

  } while (i < n)
  ```

### if statement
- if
  ```java
  if (a == b) {
      // do something
  }
  ```
- if-else
  ```java
  if (a == b) {
      // do something
  } else {
      // do something
  }
  ```
- else if
  ```java
  if (a == b) {
      // do something
  } else if (a > b) {
      // do something
  } else {
      // do something
  }
  ```

### switch statement
```java
switch (str) {
    case "aaa":
        // do something
        break;
    case "bbb":
        // do something
        break;
    default:
        // do something
        break;
}
```
- **Note**
   - Without `break`, program will still go through each case even if it already executed the matched case.
