# SwiftUI InterviewPrepration

## 1. SwiftUI vs UIKit (Major Differences)

| Point                         | SwiftUI                                                                      | UIKit                                                                   |
| ----------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **1. Programming Style**      | Declarative framework — describes _what_ UI should look like                 | Imperative framework — describes _how_ UI should be created and updated |
| **2. Introduced**             | Introduced in 2019 (iOS 13)                                                  | Introduced in 2008                                                      |
| **3. Code Structure**         | Requires less boilerplate code and is more concise                           | Requires more code for UI setup and management                          |
| **4. UI Updates**             | Automatically updates UI when state changes                                  | Developers manually update UI components                                |
| **5. State Management**       | Built-in state management using `@State`, `@Binding`, `@ObservedObject` etc. | No built-in reactive state management                                   |
| **6. Layout System**          | Uses `VStack`, `HStack`, `ZStack` for layouts                                | Uses AutoLayout and constraints                                         |
| **7. Preview Support**        | Supports live preview in Xcode Canvas                                        | No live preview support                                                 |
| **8. Navigation**             | Uses `NavigationStack` and `NavigationLink`                                  | Uses `UINavigationController` and `pushViewController`                  |
| **9. Backward Compatibility** | Supports iOS 13 and above                                                    | Supports much older iOS versions                                        |
| **10. Industry Usage**        | Preferred for modern apps and new features                                   | Widely used in legacy and enterprise applications                       |

---

## 2. SwiftUI App Life Cycle

SwiftUI lifecycle defines:

- How the app starts
- How views are created
- How app state changes are managed
- How app responds to foreground/background events

Before SwiftUI, UIKit apps used:

- AppDelegate
- SceneDelegate

SwiftUI simplified this using:

- `App` protocol
- `Scene`
- State-driven lifecycle

---

### What is SwiftUI App Lifecycle?

SwiftUI app lifecycle is the modern application entry and management system based on the `App` protocol.

Instead of manually configuring:

- Window
- Root ViewController
- Scene management

SwiftUI automatically handles them.

---

### Entry Point of SwiftUI App

The app starts from a structure conforming to `App`.

## Example

```ruby
import SwiftUI

@main
struct MyApp: App {

    var body: some Scene {

        WindowGroup {
            ContentView()
        }
    }
}
```

---

### Understanding Each Part

---

#### @main

```ruby
@main
```

Marks the starting point of the application.

Equivalent to:

- `main.swift`
- App launch entry point

---

#### App Protocol

```ruby
struct MyApp: App
```

The `App` protocol manages:

- App initialization
- Scene creation
- App lifecycle events

Equivalent to:

- AppDelegate responsibilities

---

#### Scene

```ruby
var body: some Scene
```

A Scene represents:

- One instance of UI
- App window/interface

---

#### WindowGroup

```ruby
WindowGroup {
    ContentView()
}
```

Creates a window containing the root view.

Equivalent to:

- UIWindow
- RootViewController setup

---

### UIKit Lifecycle vs SwiftUI Lifecycle

| UIKit              | SwiftUI      |
| ------------------ | ------------ |
| AppDelegate        | App protocol |
| SceneDelegate      | Scene        |
| UIWindow           | WindowGroup  |
| RootViewController | Root View    |
| viewDidLoad        | onAppear     |
| viewWillDisappear  | onDisappear  |

---

### SwiftUI View Lifecycle

Each SwiftUI View has its own lifecycle.

Main lifecycle methods:

- `init()`
- `body`
- `onAppear`
- `onDisappear`

---

### init() in SwiftUI

Called when view is initialized.

#### Example

```ruby
struct ContentView: View {

    init() {
        print("View Initialized")
    }

    var body: some View {
        Text("Hello")
    }
}
```

---

### body Property

```ruby
var body: some View
```

Defines UI layout.

Important:

- Recomputed whenever state changes
- Not called only once

SwiftUI rebuilds UI frequently.

---

### onAppear()

Called when view appears on screen.

Equivalent to:

- `viewDidAppear`
- `viewWillAppear`

#### Example

```ruby
Text("Welcome")
    .onAppear {
        print("View Appeared")
    }
```

---

### Common Uses

- API calls
- Analytics
- Start animations
- Load data

---

### onDisappear()

Called when view disappears.

Equivalent to:

- `viewDidDisappear`

#### Example

```ruby
Text("Details")
    .onDisappear {
        print("View Disappeared")
    }
```

---

### Common Uses

- Stop timers
- Cancel API requests
- Cleanup resources

---

### Complete Lifecycle Example

```ruby
struct ContentView: View {

    init() {
        print("Init Called")
    }

    var body: some View {

        Text("SwiftUI Lifecycle")
            .onAppear {
                print("onAppear Called")
            }
            .onDisappear {
                print("onDisappear Called")
            }
    }
}
```

---

### App States in SwiftUI

Apps can move through states:

| State      | Description                  |
| ---------- | ---------------------------- |
| Active     | App is running in foreground |
| Inactive   | Temporary interruption       |
| Background | App running in background    |
| Suspended  | App frozen by system         |

---

### ScenePhase in SwiftUI

SwiftUI provides `scenePhase` to observe app state changes.

#### Example

```ruby
@Environment(\.scenePhase)
private var scenePhase
```

---

### Detect App State Changes

#### Example

```ruby
import SwiftUI

@main
struct DemoApp: App {

    @Environment(\.scenePhase)
    private var scenePhase

    var body: some Scene {

        WindowGroup {
            ContentView()
        }
        .onChange(of: scenePhase) { newPhase in

            switch newPhase {

            case .active:
                print("App Active")

            case .inactive:
                print("App Inactive")

            case .background:
                print("App Background")

            @unknown default:
                break
            }
        }
    }
}
```

---

### ScenePhase States

---

#### .active

App is:

- Open
- Interactive
- Foreground

Example:

- User actively using app

---

#### .inactive

Temporary interruption.

Example:

- Phone call
- Notification popup

---

#### .background

App moved to background.

Example:

- User pressed Home button

---

### Multiple Scenes Support

SwiftUI supports multiple windows/scenes.

Especially useful in:

- iPadOS
- macOS

Each scene manages independent UI state.

---

### AppDelegate in SwiftUI

SwiftUI can still use AppDelegate if needed.

#### Example

```ruby
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions:
        [UIApplication.LaunchOptionsKey : Any]? = nil
    ) -> Bool {
        print("App Launched")
        return true
    }
}
```

Use in SwiftUI:

```ruby
@UIApplicationDelegateAdaptor(AppDelegate.self)
var appDelegate
```

---

### When AppDelegate is Still Needed

Sometimes needed for:

- Push Notifications
- Firebase setup
- Deep Linking
- Third-party SDKs
- App launch handling

---

### View Recreation in SwiftUI

Important interview concept.

SwiftUI Views are:

- Structs
- Lightweight
- Frequently recreated

Therefore:

- Never rely heavily on init()
- Use state properly

---

### SwiftUI Lifecycle Flow

#### App Launch Flow

```text
@main
   ↓
App Protocol
   ↓
Scene Creation
   ↓
WindowGroup
   ↓
Root View
   ↓
body Rendered
   ↓
onAppear()
```

---

### UIKit vs SwiftUI Lifecycle Comparison

| UIKit Lifecycle     | SwiftUI Lifecycle |
| ------------------- | ----------------- |
| AppDelegate         | App               |
| SceneDelegate       | Scene             |
| viewDidLoad         | init              |
| viewWillAppear      | onAppear          |
| viewDidDisappear    | onDisappear       |
| UIApplication State | ScenePhase        |

---

## 3. Property Wrappers in SwiftUI

Property wrappers are one of the MOST IMPORTANT topics in SwiftUI interviews.

They help SwiftUI:

- Manage state
- Share data
- Observe changes
- Automatically refresh UI

SwiftUI is completely state-driven, and property wrappers are the foundation of that system.

---

### What is a Property Wrapper?

A property wrapper adds special behavior to a variable.

In SwiftUI, property wrappers are mainly used for:

- State management
- Data flow
- Dependency injection
- Data persistence

They start with `@`.

Example:

```ruby
@State private var count = 0
```

Here:

- `@State` is a property wrapper
- It tells SwiftUI to observe this value

---

### Most Important SwiftUI Property Wrappers

| Property Wrapper     | Purpose                          |
| -------------------- | -------------------------------- |
| `@State`             | Local state management           |
| `@Binding`           | Two-way data binding             |
| `@ObservedObject`    | Observe external object          |
| `@StateObject`       | Create and own observable object |
| `@EnvironmentObject` | Share data globally              |
| `@Environment`       | Access system environment values |
| `@Published`         | Notify object changes            |
| `@AppStorage`        | Store data in UserDefaults       |
| `@SceneStorage`      | Preserve scene state             |
| `@FocusState`        | Manage keyboard focus            |

---

### @State

#### Purpose

Used for local mutable state inside a View.

When value changes:

- SwiftUI automatically refreshes UI.

---

#### Example

```ruby
struct CounterView: View {

    @State private var count = 0

    var body: some View {

        VStack {

            Text("Count: \(count)")

            Button("Increment") {
                count += 1
            }
        }
    }
}
```

---

#### Important Points

| Point               | Description                    |
| ------------------- | ------------------------------ |
| Local state         | Owned by same view             |
| Value type          | Usually structs, strings, ints |
| UI refresh          | Automatic                      |
| Private recommended | Yes                            |

---

#### When to use @State?

Use `@State` when:

- Data belongs to the current view
- Small local UI state is needed

Example:

- Toggle state
- Counter
- TextField input

---

### @Binding

#### Purpose

Creates two-way connection to data owned by another view.

Child view can:

- Read
- Modify parent data

---

#### Example

---

#### Parent View

```ruby
struct ParentView: View {

    @State private var isOn = false

    var body: some View {
        ChildView(isOn: $isOn)
    }
}
```

---

#### Child View

```ruby
struct ChildView: View {

    @Binding var isOn: Bool

    var body: some View {
        Toggle("Switch", isOn: $isOn)
    }
}
```

---

#### Difference between @State and @Binding?

| @State             | @Binding            |
| ------------------ | ------------------- |
| Owns data          | References data     |
| Source of truth    | Connected to source |
| Parent mostly uses | Child mostly uses   |

---

### @ObservedObject

#### Purpose

Observes external class object conforming to `ObservableObject`.

UI refreshes when published properties change.

---

#### Example

---

#### ViewModel

```ruby
class UserViewModel: ObservableObject {

    @Published var name = "Ayush"
}
```

---

#### View

```ruby
struct ContentView: View {

    @ObservedObject var vm = UserViewModel()

    var body: some View {
        Text(vm.name)
    }
}
```

---

#### Important Points

| Point                  | Description |
| ---------------------- | ----------- |
| For reference types    | Classes     |
| Needs ObservableObject | Yes         |
| Uses @Published        | Usually     |
| External ownership     | Yes         |

---

#### Problem with @ObservedObject

If view reloads:

- Object may recreate

Solution:

- Use `@StateObject`

---

### @StateObject

#### Purpose

Creates and owns ObservableObject.

Object survives view redraws.

---

#### Example

```ruby
struct ContentView: View {

    @StateObject private var vm = UserViewModel()

    var body: some View {
        Text(vm.name)
    }
}
```

---

#### Difference between @ObservedObject and @StateObject

| @ObservedObject     | @StateObject   |
| ------------------- | -------------- |
| Does not own object | Owns object    |
| Recreated sometimes | Persistent     |
| Injected object     | Created object |

---

#### Use:

- `@StateObject` → when view CREATES ViewModel
- `@ObservedObject` → when ViewModel comes from parent

---

### @EnvironmentObject

#### Purpose

Shares data globally across many views.

Avoids manual dependency passing.

---

#### Example

---

#### Theme Manager

```ruby
class ThemeManager: ObservableObject {

    @Published var darkMode = false
}
```

---

#### Inject

```ruby
@main
struct DemoApp: App {

    @StateObject var theme = ThemeManager()

    var body: some Scene {

        WindowGroup {
            ContentView()
                .environmentObject(theme)
        }
    }
}
```

---

#### Access

```ruby
@EnvironmentObject var theme: ThemeManager
```

---

#### Common Use Cases

- Theme management
- User session
- Authentication
- App settings

---

### @Published

#### Purpose

Marks properties that notify observers when changed.

Used inside ObservableObject.

---

#### Example

```ruby
class CounterVM: ObservableObject {

    @Published var count = 0
}
```

Whenever `count` changes:

- SwiftUI updates UI

---

#### Important Points

| Point                     | Description |
| ------------------------- | ----------- |
| Used in class             | Yes         |
| Requires ObservableObject | Yes         |
| Triggers updates          | Yes         |

---

### @Environment

#### Purpose

Access system-provided values.

---

#### Example

```ruby
@Environment(\.colorScheme)
var colorScheme
```

---

#### Common Environment Values

| Environment Value | Purpose                |
| ----------------- | ---------------------- |
| colorScheme       | Light/Dark mode        |
| presentationMode  | Dismiss screen         |
| locale            | Current language       |
| dismiss           | Close sheet/navigation |

---

#### Example

```ruby
@Environment(\.dismiss)
var dismiss
```

---

### @AppStorage

#### Purpose

Store simple data in UserDefaults.

---

#### Example

```ruby
@AppStorage("isLoggedIn")
var isLoggedIn = false
```

---

#### Benefits

| Benefit            | Description |
| ------------------ | ----------- |
| Persistent storage | Yes         |
| Automatic sync     | Yes         |
| Simple syntax      | Yes         |

---

#### Common Use Cases

- Login flag
- Theme preference
- Settings

---

### @SceneStorage

#### Purpose

Preserves state for a scene/window.

---

#### Example

```ruby
@SceneStorage("username")
var username = ""
```

---

#### Difference

| @AppStorage         | @SceneStorage         |
| ------------------- | --------------------- |
| Persistent app-wide | Scene-specific        |
| UserDefaults        | Temporary scene state |

---

### @FocusState

#### Purpose

Manages keyboard focus.

---

#### Example

```ruby
@FocusState private var isFocused: Bool
```

---

#### Example Usage

```ruby
TextField("Name", text: $name)
    .focused($isFocused)
```

---

#### Complete Data Flow Hierarchy

```text
@State
   ↓
@Binding
   ↓
@ObservedObject
   ↓
@StateObject
   ↓
@EnvironmentObject
```

---

## 4. How to Integrate SwiftUI in UIKit Code and vice-versa?

This is a VERY IMPORTANT real-world interview topic because most existing iOS apps are UIKit-based and companies gradually add SwiftUI screens into them.

There are mainly 2 ways:

| Integration Type     | Purpose     |
| -------------------- | ----------- |
| SwiftUI inside UIKit | Most common |
| UIKit inside SwiftUI | Also common |

---

### SwiftUI Inside UIKit (MOST IMPORTANT)

UIKit app can display SwiftUI view using:

#### `UIHostingController`

`UIHostingController` acts as a bridge between UIKit and SwiftUI.

---

#### Architecture

```text
UIKit ViewController
        ↓
UIHostingController
        ↓
SwiftUI View
```

---

#### Example: Show SwiftUI Screen from UIKit

---

#### Step 1: Create SwiftUI View

```ruby
import SwiftUI

struct ProfileView: View {

    var body: some View {

        VStack {
            Text("SwiftUI Screen")
                .font(.largeTitle)

            Text("Integrated inside UIKit")
        }
    }
}
```

---

#### Step 2: Open in UIKit

```ruby
import UIKit
import SwiftUI

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        let swiftUIView = ProfileView()

        let hostingController = UIHostingController(
            rootView: swiftUIView
        )

        navigationController?.pushViewController(
            hostingController,
            animated: true
        )
    }
}
```

---

### What Happens Internally?

```text
UIKit Controller
      ↓
UIHostingController
      ↓
SwiftUI View Rendering
```

---

#### 2. Add SwiftUI View as Child View in UIKit

Instead of full screen navigation, you can embed SwiftUI inside a UIViewController.

---

#### Example

```ruby
import UIKit
import SwiftUI

class HomeViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        let swiftUIView = ProfileView()

        let hostingController = UIHostingController(
            rootView: swiftUIView
        )

        addChild(hostingController)

        hostingController.view.translatesAutoresizingMaskIntoConstraints = false

        view.addSubview(hostingController.view)

        NSLayoutConstraint.activate([
            hostingController.view.topAnchor.constraint(equalTo: view.topAnchor),
            hostingController.view.bottomAnchor.constraint(equalTo: view.bottomAnchor),
            hostingController.view.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            hostingController.view.trailingAnchor.constraint(equalTo: view.trailingAnchor)
        ])

        hostingController.didMove(toParent: self)
    }
}
```

---

#### Why Use This?

Useful when:

- Gradually migrating app to SwiftUI
- Replacing small UIKit components
- Embedding modern UI into legacy app

---

#### Pass Data from UIKit to SwiftUI

You can pass values normally.

---

#### SwiftUI View

```ruby
struct UserView: View {

    let username: String

    var body: some View {
        Text(username)
    }
}
```

---

#### UIKit

```ruby
let swiftUIView = UserView(username: "Ayush")
```

---

#### Pass ObservableObject to SwiftUI

---

#### ViewModel

```ruby
class UserViewModel: ObservableObject {

    @Published var name = "Ayush"
}
```

---

#### SwiftUI View

```ruby
struct UserView: View {

    @ObservedObject var vm: UserViewModel

    var body: some View {
        Text(vm.name)
    }
}
```

---

#### UIKit

```ruby
let vm = UserViewModel()

let swiftUIView = UserView(vm: vm)

let hostingController = UIHostingController(
    rootView: swiftUIView
)
```

---

#### UIKit Inside SwiftUI

Sometimes existing UIKit components need to be reused in SwiftUI.

Use:

- `UIViewRepresentable`
- `UIViewControllerRepresentable`

---

#### Example: UILabel in SwiftUI

---

#### Step 1

```ruby
import SwiftUI
import UIKit

struct CustomLabel: UIViewRepresentable {

    func makeUIView(context: Context) -> UILabel {

        let label = UILabel()
        label.text = "UIKit Label"

        return label
    }

    func updateUIView(
        _ uiView: UILabel,
        context: Context
    ) {

    }
}
```

---

#### Step 2

Use inside SwiftUI:

```ruby
struct ContentView: View {

    var body: some View {
        CustomLabel()
    }
}
```

---

#### UIViewControllerRepresentable

Used for UIKit ViewControllers.

---

#### Example

```ruby
struct CameraView: UIViewControllerRepresentable {

    func makeUIViewController(
        context: Context
    ) -> UIImagePickerController {

        UIImagePickerController()
    }

    func updateUIViewController(
        _ uiViewController: UIImagePickerController,
        context: Context
    ) {

    }
}
```

---

### Communication Between UIKit and SwiftUI

---

#### UIKit → SwiftUI

Pass:

- Variables
- Bindings
- ObservableObjects

---

#### SwiftUI → UIKit

Use:

- Closures
- Coordinators
- Delegates

---

#### Example Using Closure

---

#### SwiftUI View

```ruby
struct LoginView: View {

    var onLogin: (() -> Void)?

    var body: some View {

        Button("Login") {
            onLogin?()
        }
    }
}
```

---

#### UIKit

```ruby
let view = LoginView {

    print("Login tapped")
}
```

---

#### Coordinator Pattern

Important for:

- Delegates
- Navigation
- UIKit callbacks

Mostly used in:

- UIViewRepresentable
- UIViewControllerRepresentable

---

#### Benefits of SwiftUI + UIKit Integration

| Benefit             | Description               |
| ------------------- | ------------------------- |
| Gradual migration   | No need full rewrite      |
| Reuse legacy code   | Saves effort              |
| Modern UI adoption  | Use SwiftUI incrementally |
| Hybrid architecture | Most companies use this   |

---
