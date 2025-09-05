# Swift Fundamentals

#### 1. What's the difference between mutable and immutable ?

<details> 
  <summary>Solution</summary> 

A **mutable** object allows for change after creation; an **immutable** object does not. In Swift:

* `var` creates a **mutable** binding (you can reassign it).
* `let` creates an **immutable** binding (you cannot reassign it).

Mutable object

```swift
var currentYear = 2020
currentYear = 2021 // ✅ value changed
```

Immutable object

```swift
let usIndependenceDay = "July 4th"
usIndependenceDay = "February 22nd" // ❌ compile-time error
```

Notes:

* Immutability improves safety (especially with concurrency) and helps the compiler optimize.
* With reference types (`class`), `let` makes the **reference** immutable (you can’t point it to another instance) but the **instance’s internal properties** can still change if they are `var`.

</details> 

#### 2. What is a property observer?

<details> 
  <summary>Solution</summary> 

Property observers run code when a stored property’s value changes:

* `willSet(newValue)` fires **before** the value is stored.
* `didSet(oldValue)` fires **after** the value is stored.

```swift
var age = 20 {
  willSet {
    print("it's about to get fun")
  }
  didSet {
    print("with great power comes great responsibility")
  }
}
age = 21
```

Notes:

* Observers run even if the new value equals the old value.
* Observers are for **stored** properties (not pure computed properties).
* Common uses: keep UI/model in sync, validation, logging, cache invalidation.

</details> 

#### 3. What is a computed property ?

<details> 
  <summary>Solution</summary> 

A **computed property** doesn’t store data; it computes a value on access. It may provide a **getter** and optionally a **setter** that updates other state.

```swift
struct Rectangle {
  var width: Double
  var height: Double
  var area: Double { width * height }             // get-only
  var perimeter: Double {                         // get-set
    get { 2 * (width + height) }
    set {                                         // assume square-ish for demo
      let side = newValue / 4
      width = side; height = side
    }
  }
}
```

Notes:

* Get-only computed properties can omit `get`.
* Computed properties are reevaluated on each access; avoid heavy work or cache it.

</details> 

#### 4. What are higher order functions?

<details> 
  <summary>Solution</summary> 

Functions that **take other functions as parameters** or **return functions**. Swift’s standard library provides many:

* `map`, `compactMap`, `flatMap` transform collections.
* `filter` selects elements by predicate.
* `reduce` combines to a single value.
* `sorted(by:)`, `forEach`, `contains(where:)`, `first(where:)`.

```swift
let nums = [1,2,3,4,5]
let squares = nums.map { $0 * $0 }
let evens = nums.filter { $0 % 2 == 0 }
let sum    = nums.reduce(0, +)
```

Benefits: concise, expressive, and composable code.

</details> 

#### 5. What is recursion?

<details> 
  <summary>Solution</summary> 

A function calling itself to solve a problem by breaking it into smaller subproblems. Requires:

* **Base case** to stop recursion.
* **Recursive step** to progress toward base.

```swift
func factorial(_ n: Int) -> Int {
  if n == 0 { return 1 }            // base
  return n * factorial(n - 1)       // recursive
}
```

Notes:

* Swift doesn’t guarantee tail-call optimization; deep recursion can overflow the stack. Consider iterative solutions for very deep problems.

</details> 

#### 6. What are access control / modifiers and give three examples?

<details> 
  <summary>Solution</summary> 

Swift access levels:

* **open**: visible **and** subclassable/overridable **outside** the module (classes/members only).
* **public**: visible outside the module, but **no** subclass/override outside.
* **internal** (default): visible within the same module.
* **fileprivate**: visible within the same file.
* **private**: visible within the enclosing declaration/scope.

```swift
open class A {}          // subclassable across modules
public class B {}        // visible across modules, not subclassable outside
internal class C {}      // default
fileprivate class D {}   // same file
private class E {}       // same scope
```

Design tip: Start restrictive (`internal`/`private`) and open up only as needed.

</details> 

#### 7. Name three built-in protocols in Swift and their use cases?

<details> 
  <summary>Solution</summary> 

* **Hashable**: enable use as `Dictionary` keys / in `Set`. Often auto-synthesized for structs.
* **Codable** (`Encodable & Decodable`): serialize/deserialize to JSON/PropertyList.
* **Identifiable**: provide stable `id` (handy in SwiftUI `List`/`ForEach`).

Bonus:

* **Equatable** for `==` comparisons (auto-synthesized frequently).
* **CustomStringConvertible** for custom `description` during printing.
* **CaseIterable** to iterate over all enum cases.

</details> 

#### 8. What's the benefit of an inout function?

<details> 
  <summary>Solution</summary> 

Allows a function to **mutate the caller’s variable**. Under the hood it’s **copy-in/copy-out** (value types) with exclusivity checks.

```swift
func double(_ n: inout Int) { n *= 2 }
var x = 5
double(&x)      // x becomes 10
```

Rules & caveats:

* You must pass a variable (not a literal/constant/computed property).
* `inout` params can’t escape (e.g., be captured by closures).
* Avoid aliasing the same variable to multiple `inout` parameters in one call.

</details> 

#### 9. Write code to access the last element of an array ?

<details> 
  <summary>Solution</summary> 

Unsafe (crashes if empty):

```swift
let arr = [1,2,3,4]
print(arr[arr.count - 1])
```

Safe:

```swift
if let last = arr.last {
  print(last)
}
// or default
print(arr.last ?? -1)
```

Tip: Prefer `last` to avoid out-of-bounds access.

</details> 

#### 10. What is an optional ?

<details> 
  <summary>Solution</summary> 

A type that may hold a value **or** `nil`. Declared with `?`.

```swift
var name: String? = "Alice"
name = nil
```

Unwrapping:

* `if let` / `guard let`
* Nil-coalescing `??`
* Optional chaining `user?.profile?.email`
* ❌ Avoid force unwrap `!` unless you’re certain it’s non-nil.

</details> 

#### 11. What are Closures ?

<details> 
  <summary>Solution</summary> 

Self-contained blocks of functionality that **capture** values from their surrounding scope.

```swift
func fetch(completion: (Result<Data, Error>) -> Void) { /* ... */ }

fetch { result in
  print(result)
}
```

Important:

* **Trailing closure** syntax for readability.
* **Escaping** closures (`@escaping`) outlive the function call (e.g., async APIs).
* **Capture lists** (e.g., `[weak self]`) help avoid retain cycles with reference types.

</details> 

#### 12. What is GCD?

<details> 
  <summary>Solution</summary> 

**Grand Central Dispatch**: low-level concurrency API for scheduling work on **dispatch queues**.

Key concepts:

* **Main queue** for UI work.
* **Global queues** with QoS (`userInteractive`, `userInitiated`, `utility`, `background`).
* **Custom queues** (serial/concurrent).
* **DispatchGroup**, **DispatchSemaphore**, **barrier**, **asyncAfter**.

```swift
DispatchQueue.global(qos: .userInitiated).async {
  let data = compute()
  DispatchQueue.main.async {
    updateUI(with: data)
  }
}
```

Alternative: **OperationQueue**/**Operation** add dependencies, cancellation, KVO.

</details> 

#### 13. Name the types of loops available in Swift ?

<details> 
  <summary>Solution</summary>   

* `for-in` over ranges/collections/dicts.
* `while` checks before each iteration.
* `repeat-while` runs once, then checks.

```swift
for i in 0..<3 { print(i) }
var n = 0
while n < 3 { n += 1 }
repeat { n -= 1 } while n > 0
```

</details> 

#### 14. If using a Command-line macOS application what's the function used for taking user input ?

<details> 
  <summary>Solution</summary>   

Use `readLine()` (returns `String?`):

```swift
print("Enter your name:")
if let name = readLine(), !name.isEmpty {
  print("Hello, \(name)!")
}
```

Notes:

* Blocks until newline.
* Convert strings to numbers with `Int(readLine() ?? "")`.

</details> 

#### 15. What is the restriction on a dictionary ?

<details> 
  <summary>Solution</summary> 

**Keys must conform to `Hashable`**:

```swift
let ages: [String: Int] = ["Ana": 30, "Lee": 25]
```

Why: hashing enables fast key lookups and uniqueness. Values have no restriction.

</details> 

#### 16. What is Object Oriented Programming ?

<details> 
  <summary>Solution</summary> 

A paradigm organizing code as **objects** that bundle **state** (properties) and **behavior** (methods).

Core ideas:

* **Encapsulation**: hide internal state.
* **Inheritance**: reuse via subclassing.
* **Polymorphism**: one interface, many forms.

```swift
class Person {
  var name: String
  init(name: String) { self.name = name }
  func speak() { print("Hi, I'm \(name)") }
}
```

Use OOP when you need identity, shared mutable state, or dynamic dispatch.

</details> 

#### 17. Name three principles of OOP ?

<details> 
  <summary>Solution</summary> 

* **Encapsulation**: restrict direct access to internals.
* **Inheritance**: form hierarchies; share/override behavior.
* **Polymorphism**: call the same method on different types with different results.

(**Abstraction** is often listed as a 4th: expose essentials, hide details.)

</details> 

#### 18. What is Protocol Oriented Programming ?

<details> 
  <summary>Solution</summary> 

A Swift-first paradigm emphasizing **protocols**, **value semantics**, and **protocol extensions** (default implementations) over deep class hierarchies.

```swift
protocol Vehicle { var wheels: Int { get } func drive() }
extension Vehicle { func drive() { print("Default drive") } }

struct Bike: Vehicle { let wheels = 2 } // gets default drive()
```

Benefits: composition over inheritance, testability, conditional conformance, and generic algorithms.

</details> 

#### 19. What is dependency injection?

<details> 
  <summary>Solution</summary> 

Supplying an object’s dependencies **from the outside** instead of creating them internally.

Forms:

* **Initializer injection** (preferred): enforce required dependencies.
* **Property injection**: set after init.
* **Method injection**: pass per call.

```swift
protocol API { func fetch() async throws -> Data }
final class ViewModel {
  private let api: API
  init(api: API) { self.api = api } // initializer injection
}
```

Benefits: loose coupling, easier testing (mocks), better separation of concerns.

</details> 

#### 20. What framework is used for writing Unit Test in iOS ?

<details> 
  <summary>Solution</summary> 

**XCTest**.

Highlights:

* `XCTestCase` subclasses with `test...()` methods.
* Assertions: `XCTAssertEqual`, `XCTAssertTrue`, etc.
* Async tests with `async/await` or expectations.
* Performance tests with `measure { }`.
* `@testable import` to test internal symbols.

</details> 

#### 21. What is a Singleton?

<details> 
  <summary>Solution</summary> 

A pattern guaranteeing **one shared instance**.

```swift
final class GameSession {
  static let shared = GameSession() // lazy & thread-safe
  private init() {}
}
```

Pros: convenient global state.
Cons: hidden dependencies, harder testing. Prefer DI when feasible.
UIKit examples: `UserDefaults.standard`, `FileManager.default`, `UIApplication.shared`.

</details> 

#### 22. Is `URLSession` part of `Foundation` or `UIKit`?

<details> 
  <summary>Solution</summary> 

`URLSession` is in **Foundation** (networking/core services). `UIKit` is UI-layer only.

</details> 

#### 23. What's the difference between a compile time error and a runtime error?

<details> 
  <summary>Solution</summary> 

* **Compile-time**: detected by the compiler (syntax, type mismatch, missing symbols). Prevents building.
* **Runtime**: occurs while executing (nil force-unwrap, out-of-bounds, failed I/O). Causes exceptions/crashes or incorrect behavior.

Aim to move as many errors as possible to compile-time via strong typing and optionals.

</details> 

#### 24. Is `Index out of range` error on an array an compile-time error or a runtime error?

<details> 
  <summary>Solution</summary> 

A **runtime error** (trap) because the index value is only known during execution.

Safer patterns:

```swift
if let last = arr.last { /* ... */ }
if arr.indices.contains(i) { print(arr[i]) }
```

</details> 

#### 25. What's the difference between Structs and Classes?

<details> 
  <summary>Solution</summary> 

**Structs (value types):**

* Copied on assignment/passing.
* No inheritance (can conform to protocols).
* Get a memberwise initializer by default.
* Use `mutating` for methods that change `self`.

**Classes (reference types):**

* Passed by reference (shared mutable state).
* Support inheritance & deinitializers.
* Managed by ARC; identity (`===`) matters.

Guideline: Prefer **structs** unless you specifically need reference semantics, subclassing, or Objective-C interop.

</details> 

#### 26. What is Type Annotation?

<details> 
  <summary>Solution</summary> 

Explicitly writing a variable/constant’s type:

```swift
let emojiCharacter: Character = "🚀"
let scores: [String: Int] = [:]
```

Use it for clarity, APIs, or when inference would pick an undesired type (e.g., `Double` vs `CGFloat`).

</details> 

#### 27. What is Type Inference?

<details> 
  <summary>Solution</summary> 

The compiler deduces types from context, reducing boilerplate:

```swift
let names = ["Bob", "Anne"]     // [String]
let answer = 42                 // Int
let average = 3.14              // Double
```

You can still annotate when needed for readability or to influence overload resolution.

</details> 

#### 28. Is `NSString` a class or a struct?

<details> 
  <summary>Solution</summary> 

`NSString` is an Objective-C **class** (reference type). Swift’s `String` is a **struct** (value type). They **bridge** seamlessly:

```swift
let s: String = "Hello"
let ns: NSString = s as NSString
let back: String = ns as String
```

Prefer Swift `String` for modern APIs; use bridging when interacting with Obj-C frameworks.

</details> 

#### 29. What's the difference between frames and bounds?

<details> 
  <summary>Solution</summary> 

(UIKit/Core Animation context)

* **frame**: the view’s **position and size in its superview’s coordinate system**.
* **bounds**: the view’s **position and size in its own coordinate system** (usually origin `(0,0)`).

Implications:

* Transforms (scale/rotate) affect `frame` but not necessarily `bounds`.
* Scrolling changes a scroll view’s `bounds.origin`, not its `frame`.

</details> 

#### 30. What modifier can we use to prevent a class from being subclassed?

<details> 
  <summary>Solution</summary> 

Use `final` on a class (or specific members) to forbid subclassing/overriding and enable devirtualization optimizations.

```swift
final class BlackJack { }
class MyBlackJack: BlackJack { } // ❌ cannot inherit from a final class
```

</details>
