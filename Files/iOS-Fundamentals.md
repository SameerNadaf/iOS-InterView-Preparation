# iOS Fundamentals

#### 1. What are the two native frameworks used to create user interfaces in iOS?

<details>
  <summary>Solution</summary>

UIKit and SwiftUI.

**Detailed explanation & when to use each**

* **UIKit** (UIKit.framework)

  * Imperative, view-controller based API introduced with iPhone OS.
  * Uses `UIView`/`UIViewController`, `UITableView`/`UICollectionView`, storyboards/xibs or programmatic layout (Auto Layout).
  * Mature, battle-tested; full control over lifecycle, animations, drawing, accessibility and fine-grained view behaviour.
  * Required if you need some older APIs only available via UIKit, or for complex legacy projects.
  * Interop: SwiftUI views can be hosted inside UIKit using `UIHostingController`. UIKit views can be wrapped for SwiftUI via `UIViewRepresentable`/`UIViewControllerRepresentable`.

* **SwiftUI** (SwiftUI.framework)

  * Declarative UI introduced in iOS 13+. Views are value-types (`struct`) and describe UI state.
  * Strongly integrates with Combine/async-await and is highly productive for many UIs.
  * Pros: concise code, live previews, automatic state-driven updates.
  * Cons: some platform holes early on (less mature), API changes across iOS versions, not all UIKit features are directly available.
  * Interop: `UIHostingController` lets you embed SwiftUI in UIKit; conversely `UIViewRepresentable` / `UIViewControllerRepresentable` lets you show UIKit inside SwiftUI.

**Practical tip:** For new projects consider SwiftUI for most screens and use UIKit selectively for features not yet supported, or when you need backward compatibility. For complex enterprise apps with heavy custom controls, UIKit is still commonly used.

</details>

#### 2. What does the `IB` in `IBOutlet` or `IBAction` stand for?

<details>
  <summary>Solution</summary>

`IB` stands for **Interface Builder** — the visual UI editor that ships with Xcode.

**Details**

* `@IBOutlet` marks a property that can be connected to a UI element in Interface Builder (storyboard or xib). It allows code to hold a reference to that UI element.

  ```swift
  @IBOutlet weak var titleLabel: UILabel!
  ```

  Note: `weak` is often used to avoid retain cycles when outlets point to views owned by the view hierarchy.

* `@IBAction` marks a method that can be connected to a control action (button tap, slider value changed) from Interface Builder.

  ```swift
  @IBAction func didTapButton(_ sender: UIButton) { ... }
  ```

**Extra context**

* Interface Builder evolved from NeXT/OpenStep-era tooling and is integrated into Xcode. `IBInspectable` and `IBDesignable` allow runtime/storyboard previewable customizations.

</details>

#### 3. What are the two required methods of a `UITableViewDataSource`?

<details>
  <summary>Solution</summary>

The two required methods are:

1. `tableView(_:numberOfRowsInSection:)` — returns the number of rows in the given section.

   ```swift
   func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int
   ```

2. `tableView(_:cellForRowAt:)` — returns the configured `UITableViewCell` for the given indexPath.

   ```swift
   func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
   ```

**Details & example**

* Optional methods include `numberOfSections(in:)`, `tableView(_:titleForHeaderInSection:)`, etc.
* Typical implementation:

  ```swift
  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return items.count
  }

  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    cell.textLabel?.text = items[indexPath.row]
    return cell
  }
  ```
* `dequeueReusableCell(withIdentifier:for:)` ensures efficient reuse; register cell classes or nibs ahead of time.

**Notes**

* For `UICollectionViewDataSource`, required methods are analogous: `numberOfItemsInSection` and `cellForItemAt`.

</details>

#### 4. What's the difference Push notifications and Local Notifications?

<details>
  <summary>Solution</summary>

**Push (Remote) Notifications**

* Delivered by Apple Push Notification service (**APNs**) from a remote server to the device.
* Flow: server → APNs → device; the app receives the notification even when not running.
* Requires: app server, APNs certificate/key (or JWT token with provider token), device token, and user permission (`UNUserNotificationCenter`).
* Use cases: server-driven updates, messaging, remote alerts, silent background updates (`content-available`).
* Can include payload fields for badge, sound, alert, custom data and support mutable content for rich notifications via Notification Service Extension.

**Local Notifications**

* Scheduled and delivered on the device by the app itself (no external server).
* Triggers: time interval (`UNTimeIntervalNotificationTrigger`), calendar (`UNCalendarNotificationTrigger`), or region (`UNLocationNotificationTrigger`).
* Use cases: reminders, alarms, scheduled local alerts, location-based reminders.
* Managed via `UNUserNotificationCenter` and `UNNotificationRequest`.

**Common points**

* Both require user permission to display alerts, badges or play sounds.
* Both can be handled while the app is in the foreground using `UNUserNotificationCenterDelegate`.
* Rich interactive features (actions, attachments, custom UIs) are available for both via extensions.

**Example (scheduling a local notification)**

```swift
let content = UNMutableNotificationContent()
content.title = "Reminder"
content.body = "Time to check the app"

let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 3600, repeats: false)
let request = UNNotificationRequest(identifier: "reminder1", content: content, trigger: trigger)
UNUserNotificationCenter.current().add(request)
```

</details>

#### 5. Name the ways to persist data in iOS?

<details>
  <summary>Solution</summary>

Multiple persistence options exist; choose based on data size, structure, security and sync requirements:

* **UserDefaults**

  * Key-value storage for small amounts of user preferences and lightweight settings.
  * Not for large or sensitive data.

* **File system (Documents/Caches/Temporary)**

  * Save files (JSON, images, plist, custom binary) in app sandbox (`FileManager`).
  * Use Documents for user-generated files, Caches for nonessential/derivable data.

* **Keychain**

  * Secure storage for small secrets (passwords, tokens). Encrypted and survives app reinstalls if configured with keychain access groups.

* **Core Data**

  * Apple’s object graph persistence framework with features like relationships, change tracking, undo, and migration.
  * Backed by SQLite by default but exposes a high-level API (`NSManagedObject`, `NSPersistentContainer`).

* **SQLite**

  * Low-level relational DB access (via `sqlite3` or wrappers). Good for optimized queries and control.

* **Third-party DBs**

  * Realm, GRDB, FMDB — alternatives that provide simpler APIs or additional features.

* **CloudKit / iCloud**

  * Persist data in the user’s iCloud account and optionally sync across devices.

* **NSCoding / Codable + files**

  * Serialize objects with `Codable` to JSON/Plist and write to disk.

* **URLSession / Remote Servers**

  * Persist data on the server-side (backend). Combine with local caching or sync strategies.

**Selection guidelines**

* Small settings → `UserDefaults`.
* Sensitive credentials → `Keychain`.
* Complex object graphs / relationships → `Core Data`.
* Cross-device sync → `CloudKit` or custom server + sync logic.

</details>

#### 6. What is `Result` type?

<details>
  <summary>Solution</summary>

`Result` is a generic enum used to represent either success or failure of an operation:

```swift
enum Result<Success, Failure: Error> {
  case success(Success)
  case failure(Failure)
}
```

**Usage & benefits**

* Encapsulates an operation’s outcome in a single value, making asynchronous code and callbacks easier to reason about.
* Avoids separate success/error parameters and provides type-safety for the error type.
* Works well with completion handlers before `async/await` and still useful for internal APIs.

**Example**

```swift
enum NetworkError: Error { case badResponse }

func fetchData(completion: @escaping (Result<String, NetworkError>) -> Void) {
  let success = Bool.random()
  if success {
    completion(.success("Hello"))
  } else {
    completion(.failure(.badResponse))
  }
}

fetchData { result in
  switch result {
  case .success(let str): print("Got: \(str)")
  case .failure(let err): print("Error: \(err)")
  }
}
```

**Convenience**

* `Result` has methods like `map`, `flatMap`, and `get()` which throw if the result is a failure.
* For modern code using `async/await`, many APIs can simply `throw` instead of returning `Result`, but `Result` is still useful for completion-based patterns, combining multiple results, and functional transformations.

</details>

#### 7. Describe the ways in which a view can be created?

<details>
  <summary>Solution</summary>

Common approaches for creating views on iOS:

* **Programmatically**

  * Create `UIView`/`UIViewController` subclasses and layout with Auto Layout constraints (`NSLayoutConstraint`, anchors) or frame-based layout.
  * Best for dynamic UIs, fully code-driven projects, or when you want version control-friendly diffs.

* **Storyboard**

  * Visual, scene-based UI editor. Allows wiring segues and outlets for multiple screens in one file.
  * Good for simple flows but can become heavy for large teams/large storyboards.

* **XIB / Nib**

  * Individual view files representing a single view or cell. Easier to reuse across multiple view controllers.
  * Often used for custom table/collection view cells and isolated components.

* **SwiftUI**

  * Declarative `View` types (`struct`) written in Swift (e.g., `Text`, `VStack`, `List`). Previews available in Xcode canvas.
  * Use `UIHostingController` to host SwiftUI in UIKit, or `UIViewRepresentable` to wrap UIKit inside SwiftUI.

**Example — programmatic view**

```swift
let label = UILabel()
label.translatesAutoresizingMaskIntoConstraints = false
label.text = "Hello"
view.addSubview(label)
NSLayoutConstraint.activate([
  label.centerXAnchor.constraint(equalTo: view.centerXAnchor),
  label.centerYAnchor.constraint(equalTo: view.centerYAnchor)
])
```

**Notes**

* Choose technique based on team preference, project constraints, reusability, and maintainability.

</details>

#### 8. What is ARC?

<details>
  <summary>Solution</summary>

**ARC (Automatic Reference Counting)** is the memory management system used by Swift (and modern Objective-C) to track and manage object lifetimes.

**How it works**

* Each reference-counted object has a retain count. When a strong reference to the object is created, the count increases; when released, it decreases. When the count reaches zero, the object is deallocated.
* The compiler inserts retain/release calls automatically (you don’t call them manually).

**Reference types in ARC**

* `strong` (default): increases retain count.
* `weak`: does not increase retain count and becomes `nil` when the object is deallocated (must be optional).
* `unowned`: does not increase retain count and assumes the referenced object will outlive the holder (non-optional). Accessing an `unowned` reference after deallocation will crash.

**Retain cycles**

* Occur when two objects hold strong references to each other (directly or via closures), preventing deallocation.
* Example: `A` owns `B`, `B` owns `A` → neither is released.
* Closures capture `self` strongly by default; use `[weak self]` or `[unowned self]` capture lists to break cycles.

**Detecting & debugging**

* Instruments (Leaks/Allocations) and Xcode memory graph reveal retained objects and cycles.
* Use `po` and memory graph in debugger to inspect references.

**Example**

```swift
class Owner {
  var child: Child?
}
class Child {
  weak var owner: Owner? // weak to avoid retain cycle
}
```

</details>

#### 9. What is MVC?

<details>
  <summary>Solution</summary>

**MVC (Model–View–Controller)** is an architectural pattern dividing responsibilities:

* **Model**

  * Holds app data, business logic and state — e.g., model objects, data persistence.
* **View**

  * UI elements: controls, views, layouts that render model data.
* **Controller**

  * Mediates between Model and View: receives UI events from the view, updates model, and updates the view accordingly. In iOS, `UIViewController` often acts as the controller.

**Typical flow**

1. View sends user interaction to Controller.
2. Controller updates the Model.
3. Model changes are reflected back to the Controller which updates Views.

**Pros**

* Clear separation of concerns at a conceptual level.
* Works well for small to medium projects.

**Cons (in iOS context)**

* “Massive View Controller” problem: controllers often accumulate UI logic, data formatting, networking and become hard to test/maintain.
* Leads teams to adopt patterns like **MVVM**, **MVP**, **Coordinator** pattern, or to use **SwiftUI** (state-driven) for clearer separation.

**Example**

```swift
class ArticleModel { var title: String }
class ArticleView: UIView { func display(_ title: String) { ... } }
class ArticleViewController: UIViewController {
  var model: ArticleModel!
  @IBOutlet var titleLabel: UILabel!
  override func viewDidLoad() {
    super.viewDidLoad()
    titleLabel.text = model.title
  }
}
```

</details>

#### 10. What is `URLSession`?

<details>
  <summary>Solution</summary>

`URLSession` is Apple’s API for network data transfer (part of Foundation). It handles HTTP/HTTPS requests, downloads, uploads, and streaming.

**Key concepts**

* **Tasks**

  * `dataTask` — fetches data into memory (typical REST calls).
  * `downloadTask` — downloads to a temporary file (good for large files).
  * `uploadTask` — uploads data or files.
  * `streamTask` — bi-directional streaming.
* **Configurations**

  * `URLSessionConfiguration.default`, `.ephemeral`, `.background` (background sessions allow transfers to continue when app is suspended).
* **Delegates vs Completion Handlers**

  * Delegate (`URLSessionDelegate` and friends) handles authentication, redirects, progress, and background session events.
  * Completion handler API is simpler and common for data tasks.

**Example (data task)**

```swift
let url = URL(string: "https://api.example.com/data")!
let task = URLSession.shared.dataTask(with: url) { data, response, error in
  // parse data, handle error
}
task.resume()
```

**Modern integration**

* `URLSession` integrates with Combine and supports `async/await`:

  ```swift
  let (data, response) = try await URLSession.shared.data(from: url)
  ```

**Notes**

* Be mindful of caching, timeouts, HTTP headers, security (TLS pinning if needed), reachability handling, and handling background session delegate callbacks in AppDelegate/SceneDelegate.

</details>

#### 11. Name three types of gesture recognizers?

<details>
  <summary>Solution</summary>

Common `UIGestureRecognizer` subclasses:

* `UITapGestureRecognizer` — detects taps (single, double-tap).
* `UISwipeGestureRecognizer` — detects swipes in a given direction.
* `UILongPressGestureRecognizer` — detects long-press gestures.

**Other useful ones**

* `UIPanGestureRecognizer` — dragging/panning.
* `UIPinchGestureRecognizer` — pinch/zoom.
* `UIRotationGestureRecognizer` — rotation gestures.

**Example (adding tap recognizer)**

```swift
let tap = UITapGestureRecognizer(target: self, action: #selector(handleTap(_:)))
view.addGestureRecognizer(tap)

@objc func handleTap(_ recognizer: UITapGestureRecognizer) {
  // handle tap
}
```

**SwiftUI**

* Gestures are handled declaratively with modifiers: `.onTapGesture`, `.gesture(DragGesture())`, `.simultaneousGesture`, etc.

</details>

#### 12. Which built-in tool do we use to test performance of our application?

<details>
  <summary>Solution</summary>

**Instruments** (part of Xcode) is the primary profiling tool.

**Key Instruments**

* **Time Profiler** — CPU sampling to find hot code paths.
* **Allocations** — shows object allocation and memory footprint.
* **Leaks** — detects memory leaks (objects that are never freed).
* **Core Animation** — frames, dropped frames, rendering time.
* **Energy Log** — measure battery/energy impact.
* **Network** — inspect network calls and timings.
* **Zombies** — detect over-released objects if using Obj-C (helps find use-after-free).

**How to use**

* Open Product → Profile in Xcode (or run Instruments directly).
* Attach to process or launch app from Instruments.
* Record, analyze call stacks, examine allocation snapshots, and use the memory graph and backtraces to find root causes.

**Notes**

* Combine Instruments with unit tests or UI tests to reproduce scenarios.
* Use `measure` APIs in XCTest for micro-benchmarking.

</details>

#### 13. What is Core Data?

<details>
  <summary>Solution</summary>

**Core Data** is Apple’s framework for managing an object graph and optionally persisting it to disk (commonly backed by SQLite). It provides more than just DB storage — it manages relationships, change tracking, faulting, validation, and undo/redo.

**Key components**

* `NSManagedObject` — represents an entity instance.
* `NSManagedObjectModel` — the schema (entities, attributes, relationships).
* `NSPersistentStoreCoordinator` / `NSPersistentContainer` — coordinates stores and contexts.
* `NSManagedObjectContext` — in-memory "scratchpad" for fetch/create/update/delete; contexts handle concurrency.

**Typical usage**

* Use `NSPersistentContainer` (iOS 10+) to simplify setup:

  ```swift
  let container = NSPersistentContainer(name: "ModelName")
  container.loadPersistentStores { _, error in /* handle error */ }
  let context = container.viewContext
  ```
* Fetch with `NSFetchRequest` and `NSPredicate`.
* Save changes by calling `context.save()`.

**Concurrency**

* Use main context for UI work and background contexts for heavy operations (`newBackgroundContext()`).
* Perform background work using `perform` / `performAndWait`.

**Pros**

* Tight integration with Cocoa, change notifications, and undo management.
* Efficient memory handling (faulting) for large datasets.

**Cons**

* Learning curve and migration complexity for schema changes.
* Consider alternatives (Realm, SQLite, simple files) depending on needs.

</details>

#### 14. What is TestFlight and describe its process?

<details>
  <summary>Solution</summary>

**TestFlight** is Apple’s official beta distribution service for iOS, tvOS and watchOS apps. It allows developers to distribute pre-release builds to testers.

**Process**

1. **Archive & upload**

   * Create an archive in Xcode (Product → Archive) and upload to App Store Connect.
2. **Processing**

   * The build is processed by Apple’s servers (status visible in App Store Connect).
3. **Internal testing**

   * Add team members (App Store Connect users with roles) as Internal Testers — they can install builds without App Review.
   * Internal testers can access builds immediately after processing.
4. **External testing**

   * For external testers (up to many thousands), the build must pass Apple’s beta app review (a lighter review than App Store release).
   * Once approved, invite testers via email or provide a public TestFlight link. Testers install TestFlight app and can download the build.
5. **Testing & feedback**

   * Testers can send feedback, screenshots, and crash reports. Developers receive this feedback in App Store Connect.
6. **Expiration**

   * Test builds expire (typically 90 days).
7. **Promote to App Store**

   * Once ready, submit for App Store review for public release.

**Notes**

* TestFlight provides crash logs and tester feedback integrated into App Store Connect.
* Use internal testing for rapid iterative testing; external testing for broader QA or client testing.

</details>

#### 15. What are the types of Local Notifications?

<details>
  <summary>Solution</summary>

There are three main types of local notification triggers:

1. **Time interval trigger** (`UNTimeIntervalNotificationTrigger`)

   * Fires after a specified time interval (one-off or repeating).
   * Example: reminder after 3600 seconds.

2. **Calendar trigger** (`UNCalendarNotificationTrigger`)

   * Fires at a specific date/time or repeating calendar components (hour, minute, weekday).
   * Example: every day at 8:00 AM.

3. **Location trigger** (`UNLocationNotificationTrigger`)

   * Fires when the user enters or exits a geographic region (`CLCircularRegion`).
   * Requires location permission.

**Building a notification**

* Use `UNMutableNotificationContent` for title/body/sound/badge and `UNNotificationRequest` to schedule.

**Example**

```swift
let content = UNMutableNotificationContent()
content.title = "Check-in"
content.body = "Time for your daily check-in"
let trigger = UNCalendarNotificationTrigger(dateMatching: DateComponents(hour: 20), repeats: true)
let request = UNNotificationRequest(identifier: "dailyCheck", content: content, trigger: trigger)
UNUserNotificationCenter.current().add(request)
```

</details>

#### 16. What is the default http method when making a request?

<details>
  <summary>Solution</summary>

**GET** is the default HTTP method for typical `URLSession` requests when you create a request with just a `URL` (e.g., `dataTask(with: URL)`). In `URLRequest`, the `httpMethod` property is `nil` by default, and the URL loading system treats it as a GET request if no body is provided and the method is unspecified.

**Notes**

* If you need to send a request body (e.g., for form data or JSON), explicitly set `httpMethod = "POST"` (or `"PUT"`, etc.) and set `httpBody`.
* Example setting a POST request:

  ```swift
  var request = URLRequest(url: URL(string: "https://api.example.com/send")!)
  request.httpMethod = "POST"
  request.httpBody = try? JSONEncoder().encode(payload)
  request.setValue("application/json", forHTTPHeaderField: "Content-Type")
  URLSession.shared.dataTask(with: request) { data, resp, err in ... }.resume()
  ```

</details>
