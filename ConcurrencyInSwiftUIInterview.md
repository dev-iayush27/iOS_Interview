# Concurrency in SwiftUI

Concurrency is one of the MOST IMPORTANT modern iOS interview topics.

SwiftUI uses Swift Concurrency features to:

- Perform background tasks
- Call APIs
- Avoid UI freezing
- Update UI safely

Main concepts:

- `async`
- `await`
- `Task`
- `MainActor`
- Structured concurrency

---

# 1. What is Concurrency?

Concurrency means:

> Performing multiple tasks without blocking the UI.

Example:

- API calls
- Image downloading
- Database operations
- Heavy calculations

Without concurrency:

- UI freezes
- App becomes unresponsive

---

# 2. Why Concurrency is Important in SwiftUI?

SwiftUI is state-driven.

If heavy work runs on main thread:

- UI lag happens
- Animations stop
- Scrolling becomes poor

Concurrency allows:

- Background execution
- Smooth UI updates

---

# 3. Old Way vs Modern Way

| Old Approach        | Modern Approach        |
| ------------------- | ---------------------- |
| GCD (DispatchQueue) | async/await            |
| Completion handlers | Structured concurrency |
| Callback nesting    | Cleaner code           |

---

# Old GCD Example

```ruby
DispatchQueue.global().async {

    let data = fetchData()

    DispatchQueue.main.async {
        self.label.text = data
    }
}
```

---

# Modern Swift Concurrency

```ruby
func fetchData() async {

}
```

Cleaner and easier to read.

---

# 4. async Keyword

Marks a function as asynchronous.

---

# Example

```ruby
func fetchUsers() async {

}
```

Means:

- Function may take time
- Can suspend execution

---

# 5. await Keyword

Used to wait for async operation completion.

---

# Example

```ruby
await fetchUsers()
```

Important:

- Does NOT block thread
- Suspends task temporarily

---

# 6. Simple SwiftUI API Example

---

# ViewModel

```ruby
class UserViewModel: ObservableObject {

    @Published var users: [String] = []

    func fetchUsers() async {

        try? await Task.sleep(for: .seconds(2))

        let response = ["Ayush", "Rahul", "Amit"]

        await MainActor.run {
            self.users = response
        }
    }
}
```

---

# View

```ruby
struct ContentView: View {

    @StateObject private var vm = UserViewModel()

    var body: some View {

        List(vm.users, id: \.self) { user in
            Text(user)
        }
        .task {
            await vm.fetchUsers()
        }
    }
}
```

---

# 7. What is Task?

`Task` creates asynchronous work.

---

# Example

```ruby
Task {

    await fetchData()
}
```

---

# Why Use Task?

Because SwiftUI views are synchronous.

To call async functions:

- Use `Task`

---

# Example with Button

```ruby
Button("Load Data") {

    Task {
        await fetchUsers()
    }
}
```

---

# 8. .task Modifier in SwiftUI

Very important interview topic.

Runs async task when view appears.

---

# Example

```ruby
.task {

    await fetchUsers()
}
```

Equivalent to:

- `onAppear`
- But async-friendly

---

# Benefits

| Benefit                | Description |
| ---------------------- | ----------- |
| Automatic cancellation | Yes         |
| Async support          | Yes         |
| Cleaner syntax         | Yes         |

---

# 9. MainActor

UI updates MUST happen on main thread.

Swift Concurrency provides:

- `MainActor`

---

# Example

```ruby
await MainActor.run {

    self.users = response
}
```

---

# Why Needed?

SwiftUI UI state updates should happen safely on main thread.

---

# Alternative

Annotate entire class:

```ruby
@MainActor
class UserViewModel: ObservableObject {

}
```

Now all UI updates happen on main thread automatically.

---

# 10. Task.sleep()

Suspends task temporarily.

---

# Example

```ruby
try await Task.sleep(for: .seconds(2))
```

Useful for:

- Delays
- Testing loading states

---

# 11. Error Handling with async/await

Use:

- `throws`
- `try`
- `do-catch`

---

# Example

```ruby
func fetchData() async throws -> String {

    return "Success"
}
```

Usage:

```ruby
do {

    let result = try await fetchData()

} catch {

    print(error)
}
```

---

# 12. Real API Call Example

---

# Model

```ruby
struct User: Codable {

    let name: String
}
```

---

# ViewModel

```ruby
@MainActor
class UserViewModel: ObservableObject {

    @Published var users: [User] = []

    func fetchUsers() async {

        guard let url = URL(
            string: "https://jsonplaceholder.typicode.com/users"
        ) else {
            return
        }

        do {

            let (data, _) = try await URLSession.shared.data(from: url)

            users = try JSONDecoder().decode([User].self, from: data)

        } catch {

            print(error)
        }
    }
}
```

---

# View

```ruby
struct ContentView: View {

    @StateObject private var vm = UserViewModel()

    var body: some View {

        List(vm.users, id: \.name) { user in
            Text(user.name)
        }
        .task {
            await vm.fetchUsers()
        }
    }
}
```

---

# 13. Structured Concurrency

Tasks are organized hierarchically.

Benefits:

- Safer memory management
- Automatic cancellation
- Better control

---

# 14. async let

Used for parallel execution.

VERY IMPORTANT interview topic.

---

# Example

```ruby
async let users = fetchUsers()
async let posts = fetchPosts()

let resultUsers = await users
let resultPosts = await posts
```

Both APIs execute simultaneously.

---

# Benefits

| Benefit          | Description |
| ---------------- | ----------- |
| Faster execution | Yes         |
| Parallel tasks   | Yes         |
| Cleaner syntax   | Yes         |

---

# 15. Task Groups

Advanced concurrency feature.

Used for:

- Multiple dynamic parallel tasks

---

# Example

```ruby
await withTaskGroup(of: String.self) { group in

}
```

Mostly asked for experienced developers.

---

# 16. Task Cancellation

SwiftUI automatically cancels tasks when view disappears.

---

# Example

```ruby
.task {

    await fetchData()
}
```

If view disappears:

- Task may cancel automatically

---

# Manual Cancellation

```ruby
Task.isCancelled
```

---

# 17. Difference Between GCD and async/await

| GCD                      | async/await      |
| ------------------------ | ---------------- |
| Callback-based           | Sequential style |
| Harder to read           | Cleaner          |
| Manual thread management | Automatic        |
| Nested closures          | Flat code        |

---

# 18. Concurrency + MVVM

Very common architecture.

---

# Flow

```text
View
  ↓
ViewModel async function
  ↓
API Call
  ↓
Published property updated
  ↓
SwiftUI refreshes UI
```

---

# 19. Common SwiftUI Concurrency Interview Questions

---

# Q1. Why use async/await?

Answer:
To perform asynchronous operations without blocking UI.

---

# Q2. What does await do?

Answer:
Suspends task until async work finishes.

---

# Q3. Why use MainActor?

Answer:
To safely update UI on main thread.

---

# Q4. Difference between Task and DispatchQueue?

| Task                | DispatchQueue         |
| ------------------- | --------------------- |
| Modern concurrency  | Old concurrency       |
| Structured          | Unstructured          |
| async/await support | No native async/await |

---

# Q5. What is async let?

Answer:
Used for parallel async operations.

---

# 20. Most Important SwiftUI Concurrency Topics

Focus strongly on:

- async/await
- Task
- MainActor
- URLSession async API
- .task modifier
- async let
- Error handling
- MVVM integration

---

# Real Interview Summary

> Concurrency in SwiftUI uses Swift’s modern async/await system to perform asynchronous tasks like API calls without blocking the UI. `Task` is used to start asynchronous work, `await` waits for async operations, and `MainActor` ensures safe UI updates on the main thread. SwiftUI also provides the `.task` modifier for lifecycle-aware asynchronous execution.
