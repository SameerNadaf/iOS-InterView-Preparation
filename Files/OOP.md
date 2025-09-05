# OOP Concept

#### 1\. What are the four main pillars of OOP? Explain each with an example.

<details> 
  <summary>Solution</summary> 
  The four main pillars of OOP are:

  * **Encapsulation:** The bundling of data (properties) and methods (functions) that operate on the data into a single unit, or class. It hides the internal state and requires all interaction to be through the object's public interface. This protects the data from accidental modification. **Example:** A `Car` class might have a private `fuelLevel` property, and you can only change it using a public `refuel(gallons: Double)` method.
  * **Inheritance:** A mechanism where one class (subclass or child class) can inherit properties and behaviors from another class (superclass or parent class). This promotes code reusability. **Example:** A `Sedan` class can inherit from a `Car` class, gaining properties like `color` and `make` and methods like `startEngine()`.
  * **Abstraction:** The concept of hiding complex implementation details and showing only the essential features of an object. It focuses on what the object does rather than how it does it. **Example:** When you use a `UIButton` in iOS, you don't need to know the complex drawing and event handling code behind it; you just use its `setTitle` and `addTarget` methods. You are interacting with its abstraction.
  * **Polymorphism:** The ability of an object to take on many forms. It allows objects of different classes to be treated as objects of a common superclass. It's often achieved through method overriding or using protocols. **Example:** A `Car` class has a `drive()` method. A `Sedan` and a `Truck` (both subclasses of `Car`) can override the `drive()` method to have their own specific implementations, but you can still call `drive()` on an array of `Car` objects, and each will execute its specific implementation.

</details> 

#### 2\. Differentiate between a `class` and a `struct` in Swift. When would you use one over the other?

<details> 
  <summary>Solution</summary> 

  * **Class:** A reference type. When you pass an instance of a class, you are passing a reference to the same object in memory. They support inheritance, deinitializers, and type casting. Classes are suitable for modeling complex data, shared resources, or when you need inheritance.
  * **Struct:** A value type. When you pass an instance of a struct, you are passing a copy of the data. They do not support inheritance. They are suitable for simple data models, small objects, or when you want to avoid shared state and potential side effects.

**Use cases:**

  * **Use a `struct` when:** The primary purpose is to encapsulate a few related values, you want to ensure the data is not modified unexpectedly (no shared state), and the type is small. Examples include geometric shapes (e.g., `Point`, `Size`), or simple data models like a `User` struct with `id` and `name`.
  * **Use a `class` when:** You need inheritance, you need to work with shared resources (like a file manager or a network client), or you need Objective-C interoperability. Examples include `UIViewController` or a `Vehicle` class that needs to be subclassed.

</details> 

#### 3\. What is a protocol in Swift? How does it relate to OOP?

<details> 
  <summary>Solution</summary> 
  A protocol defines a blueprint of methods, properties, and other requirements that a class, struct, or enum must conform to. It allows for a level of abstraction and provides a way to define an interface without implementing it.

**Relation to OOP:** Protocols are a fundamental part of Swift's approach to polymorphism and abstraction. While traditional OOP languages use class inheritance for polymorphism, Swift often uses **Protocol-Oriented Programming (POP)**. You can treat different types (classes, structs, enums) that conform to the same protocol as a single type, allowing for flexible and reusable code. This is known as **protocol-based polymorphism**.

</details> 

#### 4\. Explain the difference between `strong`, `weak`, and `unowned` references. How do they prevent retain cycles?

<details> 
  <summary>Solution</summary> 
  These are reference modifiers used for Automatic Reference Counting (ARC) to manage memory.

  * **Strong:** The default reference type. A strong reference keeps a firm hold on an instance, preventing it from being deallocated.
  * **Weak:** A reference that does not keep a strong hold on the instance it refers to. It's an optional and automatically becomes `nil` when the referenced instance is deallocated. It's used to break retain cycles.
  * **Unowned:** Similar to a weak reference, it does not keep a strong hold. However, it is a non-optional type and is guaranteed to always have a value. If the instance it refers to is deallocated, accessing an unowned reference will cause a runtime crash. It's used when the other instance has the same or a longer lifetime.

**Retain cycles:** A retain cycle occurs when two objects hold strong references to each other, preventing either from being deallocated. This leads to a memory leak. **Weak** and **unowned** references break this cycle by allowing one of the objects to be deallocated, which in turn allows the other to be deallocated.

</details> 

#### 5\. What is the difference between `self` and `Self` in Swift?

<details> 
  <summary>Solution</summary> 

  * `self` (lowercase) refers to an instance of a type. It's used to access properties or methods on the current instance, especially to disambiguate between a local variable and a property with the same name.
  * `Self` (uppercase) refers to the type itself, not an instance. It's used in protocol definitions to refer to the conforming type or in static/class methods to refer to the type the method is called on.

</details> 

#### 6\. What is the purpose of the `init` and `deinit` methods?
<details> 
  <summary>Solution</summary> 

  * **`init` (initializer):** A special method called to create and initialize a new instance of a class, struct, or enum. Its purpose is to ensure that all stored properties have an initial value before the instance is used.
  * **`deinit` (deinitializer):** A special method for classes that is called just before a class instance is deallocated. It's used to perform any clean-up tasks, such as closing file handles or releasing resources. Structs and enums do not have deinitializers.
</details> 

#### 7\. Explain `mutating` in the context of structs. Why is it needed?
<details> 
  <summary>Solution</summary> 
  Since structs are value types, their properties cannot be modified by default within an instance method. If you try to modify a property of a struct, you will get a compiler error. The `mutating` keyword is required for any method that modifies the properties of the struct it belongs to. It signifies that the method will change the struct itself.

**Example:**

```swift
struct Point {
    var x: Int
    var y: Int
    
    mutating func moveBy(dx: Int, dy: Int) {
        x += dx
        y += dy
    }
}
```

</details> 

#### 8\. What is method overriding? Provide a simple example in Swift.
<details> 
  <summary>Solution</summary> 
  Method overriding is a feature of inheritance where a subclass provides its own specific implementation of a method that is already defined in its superclass. The overridden method must have the same name, parameters, and return type as the superclass's method. You use the `override` keyword to indicate that you are overriding a superclass method.

**Example:**

```swift
class Animal {
    func makeSound() {
        print("Animal makes a sound")
    }
}

class Dog: Animal {
    override func makeSound() {
        print("Woof!")
    }
}
```
</details> 

#### 9\. What is a type property? How do you define it, and why would you use it?
<details> 
  <summary>Solution</summary> 
  A type property is a property that belongs to the type itself, not to an instance of the type. It's similar to static variables in other languages. You define them using the `static` keyword for both classes and structs.

**Use case:** Type properties are useful for storing values that are universal to all instances of a type. For example, a shared constant like a maximum value for a game score or a static ID counter.

**Example:**

```swift
struct Player {
    static var maxScore = 1000
}

print(Player.maxScore) // Accessing the type property
```

</details> 

#### 10\. How does Swift achieve polymorphism without class inheritance?
<details> 
  <summary>Solution</summary> 
  Swift achieves polymorphism through **protocols**. A function or variable can be defined to accept or hold a type that conforms to a specific protocol. Any class, struct, or enum that conforms to that protocol can be used, allowing for a single interface to interact with multiple different concrete types. This is a core concept of Protocol-Oriented Programming (POP).

**Example:**

```swift
protocol Drivable {
    func drive()
}

struct Car: Drivable {
    func drive() { print("Driving a car") }
}

struct Truck: Drivable {
    func drive() { print("Driving a truck") }
}

func startDriving(vehicle: Drivable) {
    vehicle.drive()
}

startDriving(vehicle: Car()) // "Driving a car"
startDriving(vehicle: Truck()) // "Driving a truck"
```

</details> 

#### 11\. What is an `extension` in Swift? How does it relate to OOP?
<details> 
  <summary>Solution</summary> 
  An `extension` allows you to add new functionality to an existing class, struct, enum, or protocol type. You can add new computed properties, instance methods, type methods, initializers, and nested types.

**Relation to OOP:** Extensions are a powerful way to add functionality to types without modifying their original source code. This promotes the **Open/Closed Principle**, a core OOP principle, which states that software entities should be open for extension but closed for modification.
</details> 

#### 12\. Explain the concept of `super` in a subclass.
<details> 
  <summary>Solution</summary> 
  The `super` keyword refers to the superclass of the current class. It's used to call methods or initializers that are defined in the superclass.

**Use cases:**

  * **Calling a superclass's method:** When you override a method, you might still want to execute the superclass's implementation before or after your own.
  * **Calling a superclass's initializer:** In a subclass's initializer, you must call a superclass initializer to ensure all properties inherited from the superclass are properly initialized. This is a crucial step in the initialization process.

</details> 

#### 13\. What is the difference between a `stored property` and a `computed property`?
<details> 
  <summary>Solution</summary> 

  * **Stored Property:** A variable or constant that is part of an instance of a class or struct. It stores a value in memory. For example, `var name: String`.
  * **Computed Property:** A property that does not store a value. Instead, it provides a `getter` and an optional `setter` to compute its value. It's used to calculate a value from other properties. For example, a `fullName` computed property that combines `firstName` and `lastName`.

</details> 

#### 14\. What is a `final` class in Swift? Why would you use it?
<details> 
  <summary>Solution</summary> 
  A `final` class is a class that cannot be subclassed. You declare a class as `final` by using the `final` keyword before the class declaration.

**Why use it?**

  * **Performance Optimization:** The compiler can make optimizations because it knows the class won't be subclassed. For example, it can dispatch method calls directly instead of using a dynamic lookup.
  * **Preventing Modification:** It prevents other developers from creating a subclass and overriding your methods, ensuring your class's behavior remains unchanged. This can be useful for security or API design.

</details> 

#### 15\. Describe the concept of **composition over inheritance** and provide a Swift example.

<details> 
  <summary>Solution</summary> 
  
  **Composition over inheritance** is a design principle that favors creating complex objects by combining simpler objects (composition) instead of extending the functionality of a superclass (inheritance).

**Advantages:**

  * **Flexibility:** It avoids the rigid hierarchy of inheritance.
  * **Reusability:** You can reuse components in various ways.
  * **Avoids the "fragile base class" problem:** Changes to a superclass don't unexpectedly break a subclass.

**Example:** Instead of a `FlyingCar` inheriting from a `Car` and a `Plane` (which isn't possible in Swift anyway), you can use composition:

```swift
// Components
protocol Engine {
    func start()
}

struct CarEngine: Engine {
    func start() { print("Car engine started") }
}

struct JetEngine: Engine {
    func start() { print("Jet engine started") }
}

// Composition
class Vehicle {
    private let engine: Engine

    init(engine: Engine) {
        self.engine = engine
    }

    func start() {
        engine.start()
    }
}

// Creating instances with different engines
let car = Vehicle(engine: CarEngine())
car.start() // "Car engine started"

let jet = Vehicle(engine: JetEngine())
jet.start() // "Jet engine started"
```
</details> 
