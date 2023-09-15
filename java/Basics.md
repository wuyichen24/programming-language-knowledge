# Basics

- [**Class**](#class)
   - [Constructor](#constructor)
   - [Inheritance](#inheritance)
   - [Interface](#interface)
   - [Abstract](#abstract)

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
- **Concept**
   - The subclass inherits data and methods from the super class.
- **Rules**
   - The subclass can only inherits data and methods which are `public` and `protected`, `private` data and methods will not be visible from subclass.
   - The `final` class can not be inherited.
   - A class can inherit from only one class (In C++, a class can inherit multiple classes).

### Interface
- **Concept**
   - An interface is a set of abstract methods (signature) that a class implementing the interface must provide.
- **Rules**
   - A class can implement multiple interfaces.
   - A interface can have default methods (Java8+).
   - A interface can have static methods (Java8+).
- **Code example**
  ```java
  public interface MyInterface {
      // Abstract method (to be implemented by classes that implement this interface)
      void regularMethod();

      // Default method (with a default implementation)
      default void defaultMethod() {
          System.out.println("This is a default method.");
      }

      // Static method (a method belonging to the interface itself)
      static void staticMethod() {
          System.out.println("This is a static method.");
      }
  }
  ```
  
### Abstract class and abstract function
- **Concepts**
   - Abstract class
      - Abstract classes cannot be instantiatedand are often used as base classes for other classes.
   - Abstract method
      - An abstract method is a method declared in an abstract class or an interface but does not contain a method body.
  
