# iOS Design Patterns and Architecture
A comprehensive collection of **iOS design patterns, architecture concepts, and app fundamentals** to help developers prepare for interviews.

---

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

Dependency Injection (DI) is a design principle where an object receives its dependencies from an external source rather than creating them itself. This makes the code more flexible, testable, and maintainable.

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
```
</details>

#### 6. What are the major layers of the iOS software stack?

<details>
  <summary>Answer</summary>

The iOS software stack is composed of several layers, each building upon the one below it. The four main layers are:

- **Core OS Layer:** The lowest level, providing fundamental services such as the kernel (Darwin), device drivers, and low-level networking.  
- **Core Services Layer:** Provides essential system services and frameworks like **Core Foundation**, **Core Location**, and **Core Data** for networking, location, and data management.  
- **Media Layer:** Handles graphics, audio, and video technologies, including **Core Graphics**, **Metal**, and **AVFoundation**.  
- **Cocoa Touch Layer:** The highest layer for building the user interface and handling user interactions. Includes **UIKit**, **SwiftUI**, **MapKit**, and other UI frameworks.  

</details>

#### 7. What is the Human Interface Guidelines (HIG)? Why is it so important for iOS developers?

<details>
  <summary>Answer</summary>

The **Human Interface Guidelines (HIG)** is a set of principles and recommendations from Apple for designing consistent, intuitive, and visually appealing interfaces.

**Importance:**

- **Consistency:** Ensures predictable behaviors and visual cues across apps.  
- **Usability:** Makes apps easy to learn and use.  
- **Platform Integration:** Guides correct usage of gestures and system-wide controls.  

Following HIG ensures your app feels native and provides a smooth user experience.

</details>

#### 8. Explain the iOS App Lifecycle. What are the key states?

<details>
  <summary>Answer</summary>

The iOS App Lifecycle defines the states an app can be in from launch to termination. Correctly managing these states is essential for resource management and user experience.

**Key states:**

- **Not Running:** App is not launched.  
- **Inactive:** App is running but not receiving events (e.g., during interruptions like phone calls).  
- **Active:** App is in the foreground and receiving events.  
- **Background:** App is not in the foreground but executing code for a short time to finish tasks.  
- **Suspended:** App is in the background and not executing code; ready to be terminated if needed.  

</details>

#### 9. What is the role of the `AppDelegate` and `SceneDelegate`?

<details>
  <summary>Answer</summary>

- **`AppDelegate`:** Handles application-wide events like launch, termination, background/foreground transitions, push notifications, and URL openings.  
- **`SceneDelegate`:** Manages the lifecycle of a single UI "scene" (window), handling events like activation, entering background, or disconnection.  

Modern apps use `AppDelegate` for app-level events and `SceneDelegate` for UI-specific state changes per window.

</details>


#### 10. What is the iOS Sandbox? Why is it important?

<details>
  <summary>Answer</summary>

The **iOS Sandbox** isolates each app's data and resources from other apps and the OS, providing a private storage area for each app.

**Importance:**

- **Security:** Prevents malicious access to user data.  
- **Stability:** Protects other apps from crashes caused by misbehaving apps.  
- **Privacy:** Limits access to sensitive data until user permission is granted.  

Apps can only read/write within their designated directories (Documents, Library, Temp).

</details>


#### 11. What is the difference between synchronous and asynchronous operations?

<details>
  <summary>Answer</summary>

- **Synchronous:** Runs sequentially; the calling code waits until the operation completes. Can block the main thread.  
- **Asynchronous:** Runs concurrently; the calling code continues immediately. Completion is handled via a callback or closure.  

**Analogy:** Synchronous is waiting in line for coffee; asynchronous is ordering coffee and reading a book while waiting.

</details>


#### 12. Why is it important to perform UI updates on the main thread?

<details>
  <summary>Answer</summary>

The main thread handles user events and rendering. UIKit and SwiftUI are **not thread-safe**. Updating the UI from a background thread can cause race conditions, glitches, or crashes. Always use `DispatchQueue.main.async` for UI updates.

</details>

#### 13. What is Grand Central Dispatch (GCD)?

<details>
  <summary>Answer</summary>

**GCD** is a low-level API for managing concurrent operations in iOS. It simplifies asynchronous programming by letting you submit tasks to **dispatch queues**, which manage execution on threads.

**Key concepts:**

- **Dispatch Queues:** Serial (main) or concurrent (global) queues.  
- **Tasks:** Blocks of code submitted for execution.  
- **Quality of Service (QoS):** Assign priority levels like `.userInteractive` or `.background`.  

</details>


#### 14. What is the purpose of `.xcassets` in an Xcode project?

<details>
  <summary>Answer</summary>

`.xcassets` is a visual editor for managing app assets like images, colors, and app icons.

**Benefits:**

- Organizes all assets in one place.  
- Automatically provides the correct resolution (`@1x`, `@2x`, `@3x`).  
- Supports light/dark mode variations.  
- Manages all app icon sizes.  

</details>


#### 15. What is a property list (`.plist`) file?

<details>
  <summary>Answer</summary>

A **`.plist` file** is an XML-based file for storing structured data in a hierarchical format.

**Use cases:**

- **`Info.plist`:** App metadata like bundle identifier, version, display name, capabilities.  
- **UserDefaults:** Persists key-value pairs in a `.plist`.  
- **Settings Bundles:** Creates settings screens in the iOS Settings app.  

</details>

#### 16. What is the `Bundle`? How is it related to the app?

<details>
  <summary>Answer</summary>

A **Bundle** is a directory that packages an app's executable and resources (images, sounds, `.plist`) into a single unit. The `.app` bundle is created at build time. Resources can be accessed via `Bundle.main`.

</details>


#### 17. Explain the purpose of `UserDefaults`. What are its limitations?

<details>
  <summary>Answer</summary>

`UserDefaults` stores small key-value data like preferences or settings, persisted in a `.plist`.

**Limitations:**

- Not for large or complex data.  
- Frequent reads/writes can affect performance.  
- No data integrity guarantees.  
- Supports only property-list compatible types (`String`, `Int`, `Bool`, `Array`, `Dictionary`, etc.).  

</details>


#### 18. What is the difference between a `class` and a `struct` in Swift?

<details>
  <summary>Answer</summary>

- **Class (Reference Type):** Passed by reference. Supports inheritance, deinitializers, type casting. Use for shared data or complex models.  
- **Struct (Value Type):** Passed by value (copied). No inheritance. Use for simple data models, preventing unexpected state changes, and for better performance (allocated on stack). SwiftUI heavily relies on structs.  

</details>


#### 19. What are a `Delegate` and a `DataSource`?

<details>
  <summary>Answer</summary>

- **Delegate:** Lets an object hand off responsibilities to another object via a protocol. Used for event handling (e.g., `UITextFieldDelegate`).  
- **DataSource:** Provides data to another object. Used for populating UI components (e.g., `UITableViewDataSource`).  

</details>

#### 20. What is the `info.plist` file, and what kind of information does it contain?

<details>
  <summary>Answer</summary>

**`Info.plist`** is a structured XML configuration file containing app metadata.

**Key information:**

- **Bundle Identifier:** Unique app ID.  
- **Version & Build Number:** App versioning.  
- **Display Name:** Name under app icon.  
- **Required Device Capabilities:** Hardware/software features (GPS, camera).  
- **Permissions:** Privacy usage descriptions (camera, microphone).  
- **Supported Orientations:** Device orientations supported by the app.  

</details>
