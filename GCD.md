# Grand Central Dispatch (GCD) in Swift

Grand Central Dispatch (GCD) is Apple’s low-level concurrency framework used for:

- Multithreading
- Background tasks
- Parallel execution
- Keeping UI responsive

It helps perform heavy tasks asynchronously without freezing the UI.

---

# Why GCD is Needed?

iOS apps run on:

- Main Thread (UI Thread)

If heavy work runs on main thread:

- UI freezes
- App becomes unresponsive

Example:

- API calls
- Image processing
- Database operations

GCD moves heavy tasks to background threads.

---

# Main Concepts of GCD

| Concept          | Purpose                       |
| ---------------- | ----------------------------- |
| Queue            | Executes tasks                |
| Main Queue       | UI updates                    |
| Global Queue     | Background tasks              |
| Serial Queue     | One task at a time            |
| Concurrent Queue | Multiple tasks simultaneously |
| sync             | Synchronous execution         |
| async            | Asynchronous execution        |

---

# 1. Main Queue

Main queue handles:

- UI updates
- User interactions

## Important Rule

UIKit/SwiftUI UI updates MUST happen on main thread.

---

# Example

```swift id="zjlwm1"
DispatchQueue.main.async {

    self.label.text = "Updated"
}
```

---

# 2. Global Queue

Used for:

- Background tasks
- Heavy operations

---

# Example

```swift id="zjlwm2"
DispatchQueue.global().async {

    print("Background Task")
}
```

---

# 3. Serial Queue

Executes:

- One task at a time
- In sequence

---

# Example

```swift id="zjlwm3"
let serialQueue = DispatchQueue(
    label: "com.app.serial"
)

serialQueue.async {

    print("Task 1")
}

serialQueue.async {

    print("Task 2")
}
```

Output order guaranteed:

```text id="zjlwm4"
Task 1
Task 2
```

---

# 4. Concurrent Queue

Executes:

- Multiple tasks simultaneously

Order NOT guaranteed.

---

# Example

```swift id="zjlwm5"
let concurrentQueue = DispatchQueue(
    label: "com.app.concurrent",
    attributes: .concurrent
)

concurrentQueue.async {

    print("Task 1")
}

concurrentQueue.async {

    print("Task 2")
}
```

---

# 5. Difference Between Serial and Concurrent Queue

| Serial Queue       | Concurrent Queue              |
| ------------------ | ----------------------------- |
| One task at a time | Multiple tasks simultaneously |
| Predictable order  | Unpredictable order           |
| Slower             | Faster                        |

---

# 6. async in GCD

Asynchronous execution:

- Does NOT block current thread

---

# Example

```swift id="zjlwm6"
DispatchQueue.global().async {

    print("Background Work")
}
```

Main thread continues running.

---

# 7. sync in GCD

Synchronous execution:

- Blocks current thread until task finishes

---

# Example

```swift id="zjlwm7"
DispatchQueue.global().sync {

    print("Task")
}
```

---

# Important Interview Point

`sync` blocks execution.

`async` does NOT block execution.

---

# 8. Difference Between sync and async

| sync                 | async                 |
| -------------------- | --------------------- |
| Blocking             | Non-blocking          |
| Waits for completion | Continues immediately |
| Can freeze UI        | Keeps UI responsive   |

---

# 9. Real API Call Example

---

# Wrong Way (UI Freeze)

```swift id="zjlwm8"
func fetchData() {

    let data = try? Data(contentsOf: url)
}
```

Runs on main thread → freezes UI.

---

# Correct Way

```swift id="zjlwm9"
DispatchQueue.global().async {

    let data = try? Data(contentsOf: url)

    DispatchQueue.main.async {

        self.imageView.image = UIImage(data: data!)
    }
}
```

---

# Flow

```text id="zjlwm10"
Background Queue
      ↓
Heavy Work
      ↓
Main Queue
      ↓
UI Update
```

---

# 10. Quality of Service (QoS)

QoS defines task priority.

---

# Types of QoS

| QoS             | Use                |
| --------------- | ------------------ |
| userInteractive | UI animations      |
| userInitiated   | Immediate tasks    |
| utility         | Long-running tasks |
| background      | Non-urgent work    |

---

# Example

```swift id="zjlwm11"
DispatchQueue.global(qos: .background).async {

    print("Background Task")
}
```

---

# 11. DispatchGroup

Used to track multiple async tasks.

---

# Example

```swift id="zjlwm12"
let group = DispatchGroup()

group.enter()

DispatchQueue.global().async {

    print("API 1")
    group.leave()
}

group.enter()

DispatchQueue.global().async {

    print("API 2")
    group.leave()
}

group.notify(queue: .main) {

    print("All Tasks Completed")
}
```

---

# Use Cases

- Multiple API calls
- Parallel downloads
- Waiting for all tasks

---

# 12. DispatchSemaphore

Controls access to resources.

Limits concurrent execution.

---

# Example

```swift id="zjlwm13"
let semaphore = DispatchSemaphore(value: 2)
```

Allows only 2 tasks simultaneously.

---

# 13. Deadlock in GCD

Deadlock occurs when:

- Two tasks wait for each other forever

---

# Example

```swift id="zjlwm14"
DispatchQueue.main.sync {

    print("Deadlock")
}
```

Main queue waiting on itself.

App freezes.

---

# Interview Point

Never call:

```swift id="zjlwm15"
DispatchQueue.main.sync
```

from main thread.

---

# 14. DispatchWorkItem

Encapsulates work to execute later.

---

# Example

```swift id="zjlwm16"
let workItem = DispatchWorkItem {

    print("Task")
}

DispatchQueue.global().async(execute: workItem)
```

---

# 15. Barrier in Concurrent Queue

Barrier ensures:

- One task executes exclusively

---

# Example

```swift id="zjlwm17"
queue.async(flags: .barrier) {

    print("Safe Write")
}
```

Useful for:

- Thread-safe writes

---

# 16. Thread Safety

Thread safety prevents:

- Race conditions
- Data corruption

---

# Example Problem

Multiple threads modifying same array.

---

# Solution

Use:

- Serial queue
- Barrier
- Locks

---

# 17. Race Condition

Occurs when:

- Multiple threads access shared data simultaneously

---

# Example

```swift id="zjlwm18"
count += 1
```

Multiple threads may corrupt value.

---

# 18. GCD vs OperationQueue

| GCD           | OperationQueue   |
| ------------- | ---------------- |
| Low-level API | Higher-level API |
| Lightweight   | More features    |
| Faster        | More control     |

---

# OperationQueue Features

- Dependencies
- Cancellation
- Priorities

---

# 19. GCD vs async/await

| GCD                    | async/await        |
| ---------------------- | ------------------ |
| Old concurrency system | Modern concurrency |
| Callback-based         | Sequential style   |
| Harder to read         | Cleaner syntax     |

---

# Example

## GCD

```swift id="zjlwm19"
DispatchQueue.global().async {

}
```

---

## async/await

```swift id="zjlwm20"
Task {

    await fetchData()
}
```

---

# 20. Common Interview Questions

---

# Q1. What is GCD?

Answer:
Apple’s framework for managing concurrent tasks and multithreading.

---

# Q2. Difference between serial and concurrent queue?

| Serial             | Concurrent     |
| ------------------ | -------------- |
| One task at a time | Multiple tasks |

---

# Q3. Difference between sync and async?

| sync     | async        |
| -------- | ------------ |
| Blocking | Non-blocking |

---

# Q4. Which queue is used for UI updates?

Answer:
Main queue.

---

# Q5. What is deadlock?

Answer:
A situation where tasks wait indefinitely for each other.

---

# Q6. Why use background queue?

Answer:
To avoid blocking UI thread.

---

# Q7. What is DispatchGroup used for?

Answer:
Waiting for multiple async tasks to complete.

---

# Most Important GCD Interview Topics

Focus strongly on:

- Main thread
- Background thread
- sync vs async
- Serial vs concurrent
- Deadlock
- DispatchGroup
- QoS
- Thread safety

---

# Real Interview Summary

> Grand Central Dispatch (GCD) is Apple’s concurrency framework used for performing background and parallel tasks efficiently. It uses dispatch queues to execute tasks asynchronously or synchronously. Main queue handles UI updates, while global queues handle background work. GCD improves app responsiveness and performance by preventing heavy operations from blocking the main thread.
