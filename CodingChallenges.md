# 🥷🏻 Coding Challenges

## 1 - Write a program for below array whose sum of two numbers = 10

```swift
let array = [3, 6, 2, 7,4]

for i in 0..<array.count {
  for j in (i+1)..<array.count {
    if array[i] + array[j] == 10 {
      print("\(array[i]) + \(array[j]) == 10")
    }
  }
}

---- OR ----
let array = [3, 6, 2, 7, 4]
var seen = Set<Int>()
for num in array {
    let complement = 10 - num
    if seen.contains(complement) {
        print("Element \(complement) + Element \(num) == 10")
    }
    seen.insert(num)
}

```

**Output**

3 + 7 == 10  
6 + 4 == 1

---

## 2(a) - Remove duplicates from Array [Approch Sorted]

```swift
let array = [2,3,4,5,6,1,9,1,3,6,8,9]

var resultArray = [Int]()

for element in array {
    if !resultArray.contains(element) {
        resultArray.append(element)
    }
}
// alter the actaul array
print(resultArray.sorted())
```

**Output**

[1, 2, 3, 4, 5, 6, 8, 9]

---

## 2(b) - Remove duplicates [Approch - Extension]

```swift
extension Array where Element == Int {
  func removeDuplicates() -> [Int] {
    var temparoryArray = [Int]()
     for element in self {
        if !temparoryArray.contains(element){
             temparoryArray.append(element)
          }
    }
    return temparoryArray
  }
}

var array = [3,5,3,4,2,3,4,5,6,7,8,9,5,4]
print(array.removeDuplicates())
```

**Output**

[3, 5, 4, 2, 6, 7, 8, 9]

---

## 2(c) - Remove duplicates [Generics]

```swift
extension Array where Element: Equatable {
  func removeDuplicates() -> [Element] {
    var temparoryArray = [Element]()
     for element in self {
        if !temparoryArray.contains(element){
             temparoryArray.append(element)
          }
    }
    return temparoryArray
  }
}
var array = [1,3,2,4,5,6,4,3,7,8]
//var array = ["a","b","a","c","d","e"]
print(array.removeDuplicates())

```

**Output**

[1, 3, 2, 4, 5, 6, 7, 8] - For numbers  
["a", "b", "c", "d", "e"] - For Strings

---

## 2(d) - Remove duplicates of an array [Approch - Mutating]

```swift
extension Array where Element: Equatable {
 mutating func removeDuplicates() {
    var temparoryArray = [Element]()
     for element in self {
        if !temparoryArray.contains(element){
             temparoryArray.append(element)
          }
    }
    // Assigning to itself
    self = temparoryArray
  }
}
var array = [1,3,2,4,5,6,4,3,7,8]
//var array = ["a","b","a","c","d","e"]
array.removeDuplicates()
print(array)
```

**Output**

[1, 3, 2, 4, 5, 6, 7, 8]

---

## 3(a) - Swap two number's [Approch 1 Math trick]

```swift
var a = 10
var b = 20
print("Before Swap A: \(a) B : \(b)")
a = a + b
b = a - b
a = a - b
print("After Swap A: \(a) B : \(b)")
```

**Output**

Before Swap A: 10 B : 20  
After Swap A: 20 B : 10

---

## 3(b) - Swap two number's [Approch 2 Tuples]

```swift
var a = 10
var b = 20
print("Before Swap A: \(a) B : \(b)")
(a,b) = (b,a)
print("After Swap A: \(a) B : \(b)")
```

**Output**

Before Swap A: 10 B : 20  
After Swap A: 20 B : 10

---

## 3(c) - Swap two number's [Approch 3 Generic's]

```swift
var a = 10.0
var b = 20.0
func swapTwoNumbers<T>( _ a:inout T, _ b: inout T) {
 (a,b) = (b,a)
}
print("Before Swap : A \(a) B : \(b)")
print(swap(&a, &b))
print("After Swap : A \(a) B : \(b)")
```

**Output**

Before Swap : A 10.0 B : 20.0  
After Swap : A 20.0 B : 10.0

---

## 4 - Factorial of given number

```swift
var a = 5
var result = 1

if a < 1 {
  print("A should be greater than 0")
} else {
    while a > 0 {
    result = result * a
    a = a - 1
  }
}
```

**Output**

120

## Recusrion Approch

```swift
func factorialRecursive(_ n: Int) -> Int {
    guard n >= 0 else {
        fatalError("Factorial is not defined for negative numbers")
    }
    if n <= 1 {
        return 1
    } else {
        return n * factorialRecursive(n - 1)
    }
}
```

---

## 5 - Swap Index of an array of position 0 & 2

```swift
var numbers = [10,20,30]

func swapIndexOfAnArray<T>(_ array: inout [T], i: Int, j: Int) {
array.swapAt(i, j)
}

print("Before Swap: \(numbers)")
swapIndexOfAnArray(&numbers, i: 0, j: 2)
print("After Swap: \(numbers)")
```

**Output**

Before Swap: [10, 20, 30]  
After Swap: [30, 20, 10]

---

## 6(a) - Find the capital leeters in a given string [Approch 1]

```swift
//Find capital letters

let nameStr = "Sameer S Nadaf"

func removeDuplicateCharacters(str: String) -> [Character] {
var characters = [Character]()

for char in str {
  if char >= "A" && char <= "Z" {
    characters.append(char)
  }
}
return characters
}

let x = removeDuplicateCharacters(str: nameStr)
print(x)
```

**Output**

["S", "S", "N"]

---

## 6(b) - Find the capital leeters in a given string [Approch 2 Higher order function]

```swift
let nameStr = "Sameer S Nadaf"

let xx = nameStr.filter({
  ("A"..."Z").contains($0)
})

print(xx)
```

```swift
let name = "Sameer S Nadaf"
let x = name.filter({$0.isUppercase})
print(x)
```

**Output**

SSN

---

## 7 - Fing how many time the character repeats in a give string

```swift
let name = "hello world"
var dict = [Character: Int]()

for char in name {
  if let i =  dict[char] {
    dict[char] = (i + 1)
  } else {
    dict[char] = 1
  }
}

for (char, count) in dict {
  print("Char \(char) repeats \(count) times")
}
```

**Output**

Char r repeats 1 times  
Char l repeats 3 times  
Char o repeats 2 times  
Char h repeats 1 times  
Char e repeats 1 times  
Char w repeats 1 times  
Char d repeats 1 times  
Char repeats 1 times

---

## 8: Reverse String

```swift
import Foundation
let name = "Nadaf"
let reversed = String(name.reversed())
print(reversed)
```

**Output**

## fadaN

## 10: Sum of a given number from 1

```swift
import Foundation

func getSumOfNumberFromOne( number:  Int) -> Int {

//    var result = 0
//    var num = number
//
//    while num > 0 {
//        result = result + num
//        num -= 1
//    }
//
//    return result

    return number * (number + 1) / 2
}

var num = 5
print(getSumOfNumberFromOne(number: num))
```

**Output**

## 15

## 11: Fibonanci of a given number

```swift
func fibonacci(upTo n: Int) -> [Int] {
    guard n > 1 else { return [0] }

    var sequence = [0, 1]
    while sequence.count < n {
        let next = sequence[sequence.count - 1] + sequence[sequence.count - 2]
        sequence.append(next)
    }
    return sequence
}

print(fibonacci(upTo: 10))
```

---

## 12: Reverse String without reversed() built in function

```swift
func reverseString(_ input: String) -> String {
    var reversed = ""
    for char in input {
        reversed = String(char) + reversed
        print(reversed)
    }
    return reversed
}

// Test
let original = "hello"
let reversed = reverseString(original)
print("Original: \(original)")
print("Reversed: \(reversed)")
```

---- O/P----  
Original: hello  
Reversed: olleh

---

## 13: Reverse Array without reversed() built in function

```swift
func reverseArray(_ arr: [Int]) -> [Int] {
    var reversed: [Int] = []
    for element in arr {
        reversed.insert(element, at: 0)
    }
    return reversed
}

// Test
let original = [1, 2, 3, 4, 5]
let reversed = reverseArray(original)
print("Original: \(original)")
print("Reversed: \(reversed)")
```

----O/P----  
Original: [1, 2, 3, 4, 5]  
Reversed: [5, 4, 3, 2, 1]

---

## 14: Reverse a give number

```swift
func reverseNumber(_ num: Int) -> Int {
    var number = num
    var reversed = 0

    while number != 0 {
        let digit = number % 10          // Get the last digit
        reversed = reversed * 10 + digit // Append digit to reversed number
        number /= 10                     // Remove the last digit
    }

    return reversed
}

// Test
let original = 12345
let reversed = reverseNumber(original)
print("Original: \(original)")
print("Reversed: \(reversed)")
```

---

## 15: Quick sort using swift

```swift
func quickSort<T: Comparable>(_ array: [T]) -> [T] {
    guard array.count > 1 else { return array }

    let pivot = array[array.count / 2]
    let less = array.filter { $0 < pivot }
    let equal = array.filter { $0 == pivot }
    let greater = array.filter { $0 > pivot }

    return quickSort(less) + equal + quickSort(greater)
}
```

------Output-----  
[0, 1, 2, 3, 4, 5, 6, 9]

---

## 16: Below compiles or not?

**Ans: No**

```swift
protocol Drawable{
  var edges: Int {get set}
var shape: String {
    return edges == 3 ? "Traingle" : "Quadrtotral"
  }
}
```

**It should be**

```Swift
extension Drawable {
var shape: String {
    return edges == 3 ? "Traingle" : "Quadrtotral"
  }
}
```

---

## 17: Write a logic to find repeated numbers in an array more than once?

```swift
import Foundation

func getRepeatedNumbers(array: [Int]) -> [Int: Int] {
  var result = [Int: Int]()

for element in array {
  result[element, default: 0] += 1
}
  return result.filter({$0.value > 1})
}

let array = [2,4,3,1,6,7,5,9,0,3,9,9,2,4]

print(getRepeatedNumbers(array: array))
```

------**Output**-----  
[4: 2, 3: 2, 2: 2, 9: 3]

---

## 18: Write a logic to convert 3[a]2[ccc] ?

```swift
import Foundation

func decodeString(_ s: String) -> String {
    var countStack: [Int] = []
    var stringStack: [String] = []
    var currentNum = 0
    var currentStr = ""

    for ch in s {
        if ch.isNumber {
            // build the repeat count (may be multi-digit)
            if let digit = ch.wholeNumberValue {
                currentNum = currentNum * 10 + digit
            }
        } else if ch == "[" {
            // push current state and reset for substring
            countStack.append(currentNum)
            stringStack.append(currentStr)
            currentNum = 0
            currentStr = ""
        } else if ch == "]" {
            // pop and build expanded string
            let repeatCount = countStack.popLast() ?? 1
            let prevStr = stringStack.popLast() ?? ""
            let expanded = String(repeating: currentStr, count: repeatCount)
            currentStr = prevStr + expanded
        } else {
            // regular character, append to current substring
            currentStr.append(ch)
        }
    }

    return currentStr
}

// Example usage:
let input = "3[a]2[bcc]"
let output = decodeString(input)
print(output)  // prints: aaabccbcc
```

---Output---  
aaabccbcc

---

## 19: Check if a String is Palindrome

```swift
func isPalindrome(_ str: String) -> Bool {
    let reversed = String(str.reversed())
    return str == reversed
}

// Test
print(isPalindrome("madam"))   // true
print(isPalindrome("hello"))   // false
```

---

## 20: Find Maximum and Minimum in Array

```swift
let numbers = [10, 20, 5, 40, 15]

if let max = numbers.max(), let min = numbers.min() {
    print("Max: \(max), Min: \(min)")
}
```

---

## 21: Find Missing Number in Sequence

```swift
// Array 1...n with one missing
let numbers = [1,2,4,5,6]
let n = 6
let expectedSum = n * (n + 1) / 2
let actualSum = numbers.reduce(0, +)
let missing = expectedSum - actualSum
print("Missing number is: \(missing)")
```

---

## 22: Anagram Check

```swift
func areAnagrams(_ s1: String, _ s2: String) -> Bool {
    return s1.lowercased().sorted() == s2.lowercased().sorted()
}

print(areAnagrams("listen", "silent")) // true
print(areAnagrams("hello", "world"))   // false
```

---

## 23: Check Prime Number

```swift
func isPrime(_ num: Int) -> Bool {
    if num < 2 { return false }
    for i in 2..<num {
        if num % i == 0 { return false }
    }
    return true
}

print(isPrime(7))  // true
print(isPrime(10)) // false
```

---

## 24: Find Second Largest Number in Array

```swift
let array = [10, 5, 30, 25, 40]
let sorted = Array(Set(array)).sorted()
if sorted.count > 1 {
    print("Second Largest: \(sorted[sorted.count - 2])")
}
```

---

## 25: Count Vowels in String

```swift
func countVowels(_ str: String) -> Int {
    let vowels: Set<Character> = ["a", "e", "i", "o", "u"]
    return str.lowercased().filter { vowels.contains($0) }.count
}

print(countVowels("Hello World")) // 3
```

---

## 26: Reverse Words in Sentence

```swift
let sentence = "I love Swift"
let reversedWords = sentence.split(separator: " ").reversed().joined(separator: " ")
print(reversedWords)  // Swift love I
```

---

## 27: Print Numbers Without Using Loop (Recursion)

```swift
func printNumbers(_ n: Int) {
    if n == 0 { return }
    printNumbers(n - 1)
    print(n)
}

printNumbers(5)
// 1 2 3 4 5
```

---

## 28: Find Common Elements Between Two Arrays

```swift
let a1 = [1,2,3,4,5]
let a2 = [3,4,5,6,7]
let common = Array(Set(a1).intersection(a2))
print(common)  // [3,4,5]
```

---

## 29: Flatten a Nested Array

```swift
let nested: [Any] = [1, [2, 3], [4, [5, 6]]]

func flatten(_ array: [Any]) -> [Int] {
    var result = [Int]()
    for element in array {
        if let num = element as? Int {
            result.append(num)
        } else if let subArray = element as? [Any] {
            result += flatten(subArray)
        }
    }
    return result
}

print(flatten(nested))  // [1,2,3,4,5,6]
```

---

## 30: Print Pattern (Star Pyramid)

```swift
func printPyramid(rows: Int) {
    for i in 1...rows {
        let spaces = String(repeating: " ", count: rows - i)
        let stars = String(repeating: "*", count: (2 * i - 1))
        print(spaces + stars)
    }
}

printPyramid(rows: 4)
/*
   *
  ***
 *****
*******
*/
```

---

## 31: Check Strong Password

```swift
func isStrongPassword(_ password: String) -> Bool {
    let capital = password.contains { $0.isUppercase }
    let small = password.contains { $0.isLowercase }
    let digit = password.contains { $0.isNumber }
    let special = password.contains { "!@#$%^&*".contains($0) }
    return password.count >= 8 && capital && small && digit && special
}

print(isStrongPassword("Hello@123")) // true
print(isStrongPassword("weak"))      // false
```

---

## 32: Print Occurrence of Each Word in String

```swift
let text = "Swift is great and Swift is fast"
var dict = [String: Int]()

for word in text.split(separator: " ") {
    dict[String(word), default: 0] += 1
}

print(dict)
// ["Swift": 2, "is": 2, "great": 1, "and": 1, "fast": 1]
```

---
