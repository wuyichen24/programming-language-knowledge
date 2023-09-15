# Basics

## Constants
### Define a single constants
```java
public static final String GREETING = "Hello, World!";
```
### Define a `enum` 
```java
public enum Status {
     ORDERED,
     READY, 
     DELIVERED; 
}
```

## Statements
### `for` loop
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

### `while` loop
- `while` loop
  ```java
  while (i < n) {
      // do something
  }
  ```
- `do-while` loop
  ```
  do {
      // do something
  } while (i < n)
  ```

### `if` statement
- `if`
  ```java
  if (a == b) {
      // do something
  }
  ```
- `if-else`
  ```java
  if (a == b) {
      // do something
  } else {
      // do something
  }
  ```
- `else if`
  ```java
  if (a == b) {
      // do something
  } else if (a > b) {
      // do something
  } else {
      // do something
  }
  ```

### `switch` statement
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

### `try` statement
- `try-catch`
  ```java
  try {
      // do something
  } catch (Exception e) {
      // exception handling
  }
  ```
- `try-catch-finally`
   - `finally` block contains code that should always be executed, whether or not an exception is thrown.
  ```java
  try {
      // do something
  } catch (Exception e) {
      // exception handling
  } finally {
      
  }
  ```
- `try-with-resources`
   - Any closeable class (e.g., file, stream, socket) can be automatically closed, no need to call the `close()` function manually.
  ```java
  try (CloseableClass obj = new CloseableClass()) {
      // do something
  } catch (Exception e) {
      // exception handling
  }
  ```

## Class
### Constructor
- **Concepts**
   - A special type of method to initialize the newly created object.
- **Default constructor**
   - If a class does not explicitly define any constructors, Java provides a default constructor with no arguments.
- **Constructor overloading**
   - Constructors can be overloaded, which means a class can have multiple constructors with different parameter lists.
- **Superclass constructor**
   - A subclass can call its superclass constructor explicitly by using the `super` keyword.
   - If a subclass does not explicitly call a superclass constructor, the compiler automatically inserts a call to the default constructor of the superclass.
     ```java
     public class Vehicle {
        public Vehicle(String name) {

        }
     }

     public class Car extends Vehicle {
         public Child(String name) {
             super(name)          // call superclass's contructor
         }
     }
     ```
- **Best practices**
   - *Static class should have a final keyword and a private constructor*
      - `final` keyword means the class will be inherited.
      - A private constructor is to prevent it from creating a new object**
     ```java
     public final class UtilityClass {
         private UtilityClass() {
             throw new AssertionError("This class should not be instantiated.");
         }

         // Static methods and members can be defined here
     }
     ```
   
### Inheritance
### Interface
### Abstract
