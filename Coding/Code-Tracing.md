# Popular & Tricky Swift Interview Coding Questions

### **1. Closure Capture (Value vs Reference)**

```swift
var a = 11

let x = { [a] in
   return a
}

a = 5
print(x())
```

**Output:**
`11`

**🧠 What’s Happening?**

* `[a]` in the capture list takes a **copy** of the value at the moment of closure creation.
* So even if `a` changes later, the closure has stored `11`.

---

### **2. Closure Without Capture List**

```swift
var a = 11

let x = { return a }

a = 5
print(x())
```

**Output:**
`5`

**🧠 What’s Happening?**

* Here closure captures `a` by **reference** (default).
* When `a` is modified later, closure reads the updated value.

---

### **3. Optional Chaining Trap**

```swift
var str: String? = "Swift"
print(str?.count ?? 0)

str = nil
print(str?.count ?? 0)
```

**Output:**

```
5
0
```

**🧠 What’s Happening?**

* When `str` is `"Swift"`, `str?.count` is `5`.
* When `str = nil`, optional chaining returns `nil`, so `?? 0` gives `0`.

---

### **4. Arrays – Value Type Behavior**

```swift
var arr1 = [1, 2, 3]
var arr2 = arr1
arr2.append(4)

print(arr1)
print(arr2)
```

**Output:**

```
[1, 2, 3]
[1, 2, 3, 4]
```

**🧠 What’s Happening?**

* `Array` in Swift is a **struct** (value type).
* Copy-on-write ensures `arr1` remains unaffected when `arr2` changes.

---

### **5. Reference Type Behavior**

```swift
class Person {
    var name: String
    init(name: String) { self.name = name }
}

var p1 = Person(name: "A")
var p2 = p1
p2.name = "B"

print(p1.name)
```

**Output:**
`B`

**🧠 What’s Happening?**

* `Person` is a **class** (reference type).
* Both `p1` and `p2` refer to the same memory object.

---

### **6. Lazy Property**

```swift
struct Test {
    lazy var value: Int = {
        print("Calculating...")
        return 42
    }()
}

var t = Test()
print("Before accessing")
print(t.value)
print(t.value)
```

**Output:**

```
Before accessing
Calculating...
42
42
```

**🧠 What’s Happening?**

* Lazy property isn’t initialized until first access.
* After first calculation, value is cached.

---

### **7. Defer Execution Order**

```swift
func test() {
    defer { print("1") }
    defer { print("2") }
    print("3")
}

test()
```

**Output:**

```
3
2
1
```

**🧠 What’s Happening?**

* `defer` blocks run **in reverse order** of declaration when scope ends.

---

### **8. Strong Reference Cycle**

```swift
class A {
    var b: B?
    deinit { print("A deinit") }
}
class B {
    var a: A?
    deinit { print("B deinit") }
}

var obj1: A? = A()
var obj2: B? = B()
obj1?.b = obj2
obj2?.a = obj1
obj1 = nil
obj2 = nil
```

**Output:**
Nothing (no deinit called)

**🧠 What’s Happening?**

* `A` and `B` strongly reference each other → retain cycle → memory leak.
* Use `weak` to break the cycle.

---

### **9. Higher Order Function Trick**

```swift
let numbers = [1, 2, 3, 4]
let result = numbers.map { $0 * 2 }.filter { $0 > 4 }.reduce(0, +)
print(result)
```

**Output:**
`14`

**🧠 What’s Happening?**

* `map` → `[2,4,6,8]`
* `filter` (>4) → `[6,8]`
* `reduce` sum → `14` (oops check carefully).
  Correction:
  `2*1=2,2*2=4,2*3=6,2*4=8` → `[2,4,6,8]`
  Filter (>4) → `[6,8]`
  Sum → `14`. ✅

---

### **10. Nil-Coalescing & Optional Binding**

```swift
var name: String? = nil
let finalName = name ?? "Guest"
print(finalName)
```

**Output:**
`Guest`

**🧠 What’s Happening?**

* `??` operator unwraps optionals.
* If `name` is nil, fallback value is used.

---

### **11. Struct Mutability**

```swift
struct Point {
    var x: Int
    mutating func move() {
        x += 1
    }
}

var p = Point(x: 5)
p.move()
print(p.x)
```

**Output:**
`6`

**🧠 What’s Happening?**

* Struct methods need `mutating` keyword to modify properties.

---

### **12. Async Execution Order**

```swift
print("1")
DispatchQueue.main.async {
    print("2")
}
print("3")
```

**Output:**

```
1
3
2
```

**🧠 What’s Happening?**

* `DispatchQueue.main.async` schedules block asynchronously.
* So `"3"` prints before `"2"`.

---
