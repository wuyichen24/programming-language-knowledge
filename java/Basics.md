# Basics

- [Class](#class)
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
### Interface
### Abstract
