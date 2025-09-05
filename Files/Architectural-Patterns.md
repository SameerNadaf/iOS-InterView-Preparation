# iOS Design Patterns and Architecture

#### 1. What is the **Model-View-Controller (MVC)** design pattern? What are its pros and cons in the context of iOS?

<details>
  <summary>Answer</summary>

**MVC** is one of the oldest and most widely used architectural patterns in iOS development. It separates an application into three interconnected parts:

- **Model:** The application's data and business logic. It should be independent of the UI.  
- **View:** The UI components that display the data from the Model and handle user interactions.  
- **Controller:** The intermediary that connects the Model and the View. It updates the View when the Model changes and updates the Model when the user interacts with the View.  

**Pros:**

- **Simplicity:** Easy to understand and is the default pattern used by Apple's UIKit framework.  
- **Separation of Concerns:** Separates data from the UI, making code easier to manage.  

**Cons:**

- **Massive View Controller:** Controllers often become bloated with logic (networking, data management, UI logic), making them hard to maintain and test.  
- **Tight Coupling:** View and Controller are tightly coupled, making it difficult to reuse a View without its specific Controller.  

</details>


#### 2. Explain the **Model-View-ViewModel (MVVM)** pattern. How does it improve upon MVC?

<details>
  <summary>Answer</summary>

**MVVM** separates the presentation logic from the View. It consists of three parts:

- **Model:** Represents the data and business logic (same as in MVC).  
- **View:** UI components that display data from the ViewModel and send user actions to it. The View is passive and contains no business logic.  
- **ViewModel:** Exposes data and commands for the View. Fetches data from the Model and transforms it into a format the View can display. Does not reference the View, making it independently testable.  

**How it improves upon MVC:**

- **Separation of Concerns:** Moves presentation logic out of the View Controller, avoiding "Massive View Controllers."  
- **Testability:** ViewModels can be unit tested without a UI.  
- **Reusability:** ViewModels can be reused across multiple Views, and Views can work with different ViewModels.  

</details>

#### 3. What is **Protocol-Oriented Programming (POP)**? How does it differ from traditional Object-Oriented Programming (OOP)?

<details>
  <summary>Answer</summary>

**POP** emphasizes the use of **protocols** and **protocol extensions** to model behavior and composition. Instead of class hierarchies with inheritance, behavior is defined via protocols, which structs and classes can conform to.

**Difference from OOP:**

- **Inheritance vs. Composition:** OOP uses class inheritance; POP favors **composition over inheritance**, allowing a type to conform to multiple protocols.  
- **Value Types:** OOP centers on reference types (`class`), while POP works with both value types (`struct`, `enum`) and reference types.  
- **Flexibility:** POP avoids the "fragile base class" problem of deep class hierarchies where changes to a superclass can break subclasses.  

</details>

#### 4. What is the **Singleton** pattern, and why is it often considered an "anti-pattern" in modern iOS development?

<details>
  <summary>Answer</summary>

**Singleton** ensures a class has only one instance and provides a global point of access, typically via a static `shared` property.  

**Why it's considered an anti-pattern:**

- **Hidden Dependencies:** Makes a class's dependencies implicit, reducing clarity.  
- **Difficult to Test:** Hard to mock or replace singletons, making unit testing challenging.  
- **Global State:** Introduces global mutable state, causing potential side effects.  
- **Memory Management:** Singleton instances live for the app's lifetime, which can waste memory unnecessarily.  

</details>

#### 5. What is **Dependency Injection**? Why is it a valuable practice in iOS development?

<details>
  <summary>Answer</summary>

**Dependency Injection (DI)** is a design principle where an object receives its dependencies from an external source rather than creating them itself.

**Example:**

```swift
// Without DI
class PostListViewModel {
    private let networkManager = NetworkManager() // Tightly coupled

    func fetchPosts() {
        networkManager.getPosts()
    }
}

// With DI
class PostListViewModel {
    private let networkManager: NetworkManager

    // Dependency is "injected" through the initializer
    init(networkManager: NetworkManager) {
        self.networkManager = networkManager
    }

    func fetchPosts() {
        networkManager.getPosts()
    }
}
