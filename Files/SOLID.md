# SOLID Principles

## ✅ **S – Single Responsibility Principle (SRP)**

### **Definition:**

A class should have only **one reason to change**. It means that each class or module should focus on a single piece of functionality or responsibility.

### **Why it's important:**

* Keeps code organized and easier to maintain.
* Reduces complexity, making debugging and testing easier.
* Avoids tightly coupled code where changing one feature affects others.

### **Detailed Explanation:**

When a class does too much, it becomes fragile. Any change in one function might break unrelated parts. Following SRP ensures that each class has one job and does it well.

### **Common Mistake:**

People often lump unrelated functionality together for convenience, like a `UserManager` class that fetches data, processes it, saves it, and logs activities. Over time, this class becomes difficult to maintain.

### **Real-world Example (Swift):**

```swift
// Violation of SRP
class UserManager {
    func fetchUser() {
        print("Fetching user from API")
    }
    
    func saveUser() {
        print("Saving user to database")
    }
    
    func logActivity() {
        print("Logging user activity")
    }
}

// Applying SRP – Separate concerns
class UserFetcher {
    func fetchUser() {
        print("Fetching user from API")
    }
}

class UserSaver {
    func saveUser() {
        print("Saving user to database")
    }
}

class UserLogger {
    func logActivity() {
        print("Logging user activity")
    }
}
```

### ✅ Real-world iOS Use Case:

* A **ViewModel** handles business logic, while a **NetworkService** manages API requests, and a **PersistenceService** manages CoreData operations.

---

## ✅ **O – Open/Closed Principle (OCP)**

### **Definition:**

Software entities (classes, modules, functions) should be **open for extension** but **closed for modification**. You should be able to add new functionality without changing existing code.

### **Why it's important:**

* Enhances scalability.
* Prevents introducing bugs when adding new features.
* Supports easier maintenance and testing.

### **Detailed Explanation:**

You design your system in a way that you can extend it by adding new classes or functions rather than altering existing ones. This reduces regression issues and makes the code more flexible.

### **Common Mistake:**

Hardcoding logic with `if-else` or `switch` statements that must be updated whenever you add a new feature.

### **Real-world Example (Swift):**

```swift
protocol Notification {
    func send(message: String)
}

class EmailNotification: Notification {
    func send(message: String) {
        print("Sending Email: \(message)")
    }
}

class SMSNotification: Notification {
    func send(message: String) {
        print("Sending SMS: \(message)")
    }
}

func notify(notification: Notification, message: String) {
    notification.send(message: message)
}

// Later, you can add a new PushNotification without changing `notify()`
class PushNotification: Notification {
    func send(message: String) {
        print("Sending Push Notification: \(message)")
    }
}
```

### ✅ Real-world iOS Use Case:

* Adding support for new payment methods (credit card, PayPal, Apple Pay) without modifying core payment logic.

---

## ✅ **L – Liskov Substitution Principle (LSP)**

### **Definition:**

Objects of a superclass should be **replaceable with objects of a subclass without altering the correctness** of the program.

### **Why it's important:**

* Ensures that subclass implementations are consistent.
* Prevents unexpected behavior when extending functionality.
* Enables polymorphism to be used effectively.

### **Detailed Explanation:**

A subclass should behave in a way that doesn't surprise users of the superclass. Overriding methods should enhance or conform to the behavior expected from the base class.

### **Common Mistake:**

Overriding a method in a way that breaks its expected behavior or interface. For example, throwing errors or changing side effects unpredictably.

### **Real-world Example (Swift):**

```swift
class Vehicle {
    func start() {
        print("Vehicle is starting")
    }
}

class Car: Vehicle {
    override func start() {
        print("Car is starting")
    }
}

class Bike: Vehicle {
    override func start() {
        print("Bike is starting")
    }
}

func testDrive(vehicle: Vehicle) {
    vehicle.start()
}

let car = Car()
let bike = Bike()

testDrive(vehicle: car)  // Works as expected
testDrive(vehicle: bike) // Works as expected
```

### ✅ Real-world iOS Use Case:

* A `UIViewController` subclass should not override lifecycle methods in ways that break standard navigation or UI behavior.

---

## ✅ **I – Interface Segregation Principle (ISP)**

### **Definition:**

Clients should not be forced to implement interfaces they don't use. Interfaces (or protocols in Swift) should be broken down into smaller, more specific ones.

### **Why it's important:**

* Reduces bloated or unused code.
* Encourages flexibility and modularity.
* Makes protocols easier to understand and implement.

### **Detailed Explanation:**

When interfaces contain too many unrelated methods, they force conforming classes to implement unnecessary functionality. ISP encourages designing smaller, focused protocols.

### **Common Mistake:**

Creating large protocols that mix unrelated methods, forcing unrelated classes to implement them.

### **Real-world Example (Swift):**

```swift
protocol AllDeviceFeatures {
    func printDocument()
    func scanDocument()
    func faxDocument()
}

// Better Approach – segregate interfaces
protocol Printer {
    func printDocument()
}

protocol Scanner {
    func scanDocument()
}

protocol Fax {
    func faxDocument()
}

class SimplePrinter: Printer {
    func printDocument() {
        print("Printing document")
    }
}
```

### ✅ Real-world iOS Use Case:

* Separate protocols for data sources and delegates (e.g., `UITableViewDataSource` and `UITableViewDelegate`) instead of combining them.

---

## ✅ **D – Dependency Inversion Principle (DIP)**

### **Definition:**

High-level modules should not depend on low-level modules. Both should depend on abstractions (like protocols in Swift). Also, abstractions should not depend on details; details should depend on abstractions.

### **Why it's important:**

* Promotes loose coupling.
* Makes components easier to swap or test.
* Encourages reusable and flexible designs.

### **Detailed Explanation:**

Instead of hardcoding dependencies, your classes should depend on protocols or abstract interfaces. Concrete implementations are injected at runtime.

### **Common Mistake:**

Directly instantiating low-level objects inside high-level modules, making them hard to test or extend.

### **Real-world Example (Swift):**

```swift
protocol DataService {
    func fetchData()
}

class APIService: DataService {
    func fetchData() {
        print("Fetching data from API")
    }
}

class LocalService: DataService {
    func fetchData() {
        print("Fetching data from Local Database")
    }
}

class ViewModel {
    private let service: DataService
    
    init(service: DataService) {
        self.service = service
    }
    
    func loadData() {
        service.fetchData()
    }
}

let apiService = APIService()
let viewModel = ViewModel(service: apiService)
viewModel.loadData()
```

### ✅ Real-world iOS Use Case:

* Using dependency injection for services like network managers or database handlers to easily swap implementations for testing or different environments.

---

## ✅ Final Thoughts

* **SRP** keeps your code clean and focused.
* **OCP** ensures you can grow your codebase without breaking it.
* **LSP** makes sure your subclasses behave predictably.
* **ISP** prevents unnecessary code requirements.
* **DIP** ensures that your modules are loosely coupled and easily testable.
