# Swift Fundamentals

---

#### 1. What's the difference between mutable and immutable ?

<details> 
  <summary>Solution</summary>

A **mutable** object allows for changes after it has been created. An **immutable** object, once created, cannot be changed.

In Swift:

- `var` → mutable (can change value).
- `let` → immutable (constant, cannot be reassigned).

Mutable example:

```swift
var currentYear = 2020
currentYear = 2021 // ✅ value changed
```

Immutable example:

```swift
let usIndependenceDay = "July 4th"
usIndependenceDay = "February 22nd" // ❌ compile-time error
```

Immutability is important because it makes your code safer and easier to reason about, especially in concurrent or multi-threaded programming.

</details>

---

#### 2. What is a property observer?

<details> 
  <summary>Solution</summary>

Property observers let you run custom logic when a property’s value changes. There are two types:

- `willSet` → called right before the value is set.
- `didSet` → called immediately after the value is set.

```swift
var age = 20 {
  willSet {
    print("it's about to get fun") // before value changes
  }
  didSet {
    print("with great power comes great responsibility") // after value changes
  }
}

age = 21
```

Use cases: updating UI, validating inputs, saving state, or triggering dependent changes automatically.

</details>

---

#### 3. What is a computed property ?

<details> 
  <summary>Solution</summary>

A **computed property** does not store a value directly. Instead, it calculates (or computes) a value every time it’s accessed. It can have:

- **Getter** → returns a computed value.
- **Setter** → optionally modifies related properties.

```swift
struct Rectangle {
  var width: Double
  var height: Double

  var area: Double {
    return width * height
  }
}
```

This avoids storing redundant data and ensures values are always consistent.

</details>

---

#### 4. What are higher order functions?

<details> 
  <summary>Solution</summary>

A **higher-order function** is any function that either:

- Accepts another function as a parameter, or
- Returns a function as its result.

They are heavily used in **functional programming**.

Examples in Swift:

- `map`, `filter`, `reduce`, `compactMap`, `forEach`.

```swift
let numbers = [1, 2, 3, 4, 5]
let squares = numbers.map { $0 * $0 } // [1, 4, 9, 16, 25]
```

</details>

---

#### 5. What is recursion?

<details> 
  <summary>Solution</summary>

**Recursion** is when a function calls itself to solve a problem.
Every recursive function has:

1. **Base case** → condition to stop recursion.
2. **Recursive case** → function calling itself with a smaller input.

Example:

```swift
func factorial(_ n: Int) -> Int {
  if n == 0 { return 1 } // base case
  return n * factorial(n - 1) // recursive call
}

print(factorial(5)) // 120
```

⚠️ Be careful with recursion depth (can cause stack overflow).

</details>

---

#### 6. What are access control / modifiers and give three examples?

<details> 
  <summary>Solution</summary>

Access control restricts how code is accessed across modules and files. Swift has **5 levels**:

1. **open** → most permissive, can subclass and override outside the module.
2. **public** → can access outside the module, but subclassing/overriding not allowed.
3. **internal** (default) → available within the same module.
4. **fileprivate** → only within the same file.
5. **private** → only within the same type (struct/class/extension).

Example:

```swift
class Car {
  private var engine = "V8"
  internal var wheels = 4
  public func drive() { print("driving...") }
}
```

</details>

---

#### 7. Name three built-in protocols in Swift and their use cases?

<details> 
  <summary>Solution</summary>

- **Hashable** → allows type to be used as a dictionary key or in a `Set`.
- **CaseIterable** → lets you iterate over all cases in an enum.
- **CustomStringConvertible** → provides a custom string when printing an object.

Example:

```swift
enum Direction: CaseIterable {
  case north, south, east, west
}

print(Direction.allCases) // [north, south, east, west]
```

</details>

---

#### 8. What's the benefit of an inout function?

<details> 
  <summary>Solution</summary>

Normally, Swift function parameters are **passed by value**. Using `inout`, you can pass a reference and allow the function to **mutate** the original variable.

```swift
func doubleNumber(_ number: inout Int) {
  number *= 2
}

var x = 5
doubleNumber(&x)
print(x) // 10
```

This avoids copying large data and lets functions modify external state.

</details>

---

#### 9. Write code to access the last element of an array ?

<details> 
  <summary>Solution</summary>

Option 1: Manual indexing

```swift
let arr = [1, 2, 3, 4]
print(arr[arr.count - 1]) // 4
```

⚠️ Will crash if array is empty.

Option 2: Using `last` (safe)

```swift
print(arr.last ?? -1) // 4
```

`last` returns an **optional**.

</details>

---

#### 10. What is an optional ?

<details> 
  <summary>Solution</summary>

An **optional** is a type that can hold either:

- a value, or
- `nil` (no value).

Syntax: `?`

```swift
var name: String? = "Alice"
name = nil
```

You need to safely unwrap it before use (`if let`, `guard let`, `??`, or optional chaining).

</details>

---

#### 11. What are Closures ?

<details> 
  <summary>Solution</summary>

A **closure** is a self-contained block of functionality that can be passed around and used in your code.
Closures can capture values from their surrounding context (important difference from functions).

```swift
func someFunc(action: (Int, Bool) -> ()) {
  let value = 20
  action(8 + value, Bool.random())
}

someFunc { intValue, boolValue in
  print("Captured: \(intValue), \(boolValue)")
}
```

Closures are used heavily in Swift for callbacks, completion handlers, animations, etc.

</details>

---

#### 12. What is GCD?

<details> 
  <summary>Solution</summary>

**GCD (Grand Central Dispatch)** is Apple’s concurrency framework that helps you run tasks asynchronously and concurrently.

It provides:

- **Dispatch queues** (main, global, custom).
- **Async / Sync execution**.

Example:

```swift
DispatchQueue.global().async {
  print("Background task")
  DispatchQueue.main.async {
    print("Update UI")
  }
}
```

</details>

---

#### 13. Name the types of loops available in Swift ?

<details> 
  <summary>Solution</summary>

Swift has three loop types:

- `for-in` → iterate over ranges, arrays, dictionaries.
- `while` → runs while condition is true.
- `repeat-while` → runs at least once, then checks condition.

</details>

---

#### 14. If using a Command-line macOS application what's the function used for taking user input ?

<details> 
  <summary>Solution</summary>

You use `readLine()` to read from **standard input (stdin)**.

```swift
print("Enter your name:")
if let name = readLine() {
  print("Hello, \(name)")
}
```

</details>

---

#### 15. What is the restriction on a dictionary ?

<details> 
  <summary>Solution</summary>

In Swift, dictionary **keys must conform to `Hashable`**.
This ensures each key is unique and can be compared quickly.

```swift
let dict: [String: Int] = ["A": 1, "B": 2]
```

</details>

---

#### 16. What is Object Oriented Programming ?

<details> 
  <summary>Solution</summary>

**OOP (Object-Oriented Programming)** is a paradigm where code is structured around **objects** that contain **properties (data)** and **methods (behavior)**.

Key benefits:

- Encapsulation
- Reuse via inheritance
- Modularity

```swift
class Person {
  var name: String
  var age: Int

  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }

  func info() {
    print("Hi, my name is \(name)")
  }
}
```

</details>

---

#### 17. Name three principles of OOP ?

<details> 
  <summary>Solution</summary>

1. **Encapsulation** → bundling data + methods.
2. **Inheritance** → reuse code by subclassing.
3. **Polymorphism** → same method name, different implementations.

(Abstraction is sometimes counted as the 4th.)

</details>

---

#### 18. What is Protocol Oriented Programming ?

<details> 
  <summary>Solution</summary>

**POP (Protocol-Oriented Programming)** is a Swift paradigm where behavior is defined using **protocols**, not inheritance.

```swift
protocol Vehicle {
  var wheels: Int { get }
  func drive()
}

struct Bike: Vehicle {
  let wheels = 2
  func drive() { print("Riding bike 🚴") }
}
```

This promotes composition and avoids deep class hierarchies.

</details>

---

#### 19. What is dependency injection?

<details> 
  <summary>Solution</summary>

**Dependency Injection (DI)** is the technique of supplying required dependencies from the outside, instead of creating them inside.

Benefits:

- Easier testing (mock dependencies).
- Loose coupling.

Example:

```swift
class Service {}
class ViewModel {
  let service: Service
  init(service: Service) {
    self.service = service
  }
}
```

</details>

---

#### 20. What framework is used for writing Unit Test in iOS ?

<details> 
  <summary>Solution</summary>

**XCTest** is Apple’s unit testing framework.
It supports assertions, performance tests, and async testing.

</details>

---

#### 21. What is a Singleton?

<details> 
  <summary>Solution</summary>

A **singleton** ensures only **one shared instance** of a class exists during app lifetime.

```swift
class GameSession {
  static let shared = GameSession()
  private init() {}
}

let session = GameSession.shared
```

Examples: `UserDefaults.standard`, `UIApplication.shared`.

</details>

---

#### 22. Is `URLSession` part of `Foundation` or `UIKit`?

<details> 
  <summary>Solution</summary>

`URLSession` belongs to the **Foundation** framework, not UIKit.
UIKit is UI-related, while Foundation provides core data structures and networking APIs.

</details>

---

#### 23. What's the difference between a compile time error and a runtime error?

<details> 
  <summary>Solution</summary>

- **Compile-time error** → detected by compiler before running (syntax errors, type mismatches).
- **Runtime error** → happens when the app runs (crashes, invalid inputs, nil force unwraps).

</details>

---

#### 24. Is `Index out of range` error on an array an compile-time error or a runtime error?

<details> 
  <summary>Solution</summary>

It is a **runtime error** because the compiler cannot know which index will be accessed at execution time.

```swift
let arr = [1,2,3]
print(arr[5]) // ❌ runtime crash
```

</details>

---

#### 25. What's the difference between Structs and Classes?

<details> 
  <summary>Solution</summary>

- **Structs** → Value types (copied when assigned).
- **Classes** → Reference types (multiple variables can point to same instance).

Other differences:

- Classes support inheritance; structs don’t.
- Structs are more memory efficient.
- Structs are preferred unless you need shared mutable state.

</details>

---

#### 26. What is Type Annotation?

<details> 
  <summary>Solution</summary>

Explicitly stating the type of a variable/constant.

```swift
let emojiCharacter: Character = "🚀"
```

Useful for clarity and preventing compiler inference mistakes.

</details>

---

#### 27. What is Type Inference?

<details> 
  <summary>Solution</summary>

Swift’s compiler can infer types from context.

```swift
let names = ["Bob", "Anne"] // inferred as [String]
let number = 42 // inferred as Int
```

This reduces boilerplate.

</details>

---

#### 28. Is `NSString` a class or a struct?

<details> 
  <summary>Solution</summary>

`NSString` is an **Objective-C class**.
Swift’s `String` is a **struct**. They are **bridged**, meaning you can convert between them easily.

```swift
let swiftString: String = "Hello"
let nsString: NSString = swiftString as NSString
```

</details>

---

#### 29. What's the difference between frames and bounds?

<details> 
  <summary>Solution</summary>

- **Frame** → position and size of a view in its **superview’s coordinate system**.
- **Bounds** → position and size in the view’s **own coordinate system**.

Use case: when applying transforms, bounds usually stay the same, but frame changes.

</details>

---

#### 30. What modifier can we use to prevent a class from being subclassed?

<details> 
  <summary>Solution</summary>

Use the `final` keyword.

```swift
final class BlackJack {}
class MyBlackJack: BlackJack {} // ❌ error
```

This improves performance because compiler can optimize method dispatch.

</details>

---
