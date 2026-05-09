# Memory Management Interview Questions in Swift (with Examples)

Memory Management is one of the MOST IMPORTANT topics in iOS interviews.

Main concepts:

- ARC
- Strong/Weak references
- Retain cycle
- Closure capture
- Deinitialization

---

# 1. What is ARC in Swift?

## Answer

ARC (Automatic Reference Counting) automatically manages memory in Swift by tracking and releasing objects when they are no longer needed.

Swift automatically:

- Increases reference count
- Decreases reference count
- Frees memory when count becomes 0

---

# Example

```ruby
class Person {

    let name: String

    init(name: String) {
        self.name = name
        print("Person Initialized")
    }

    deinit {
        print("Person Deallocated")
    }
}

var person: Person? = Person(name: "Ayush")

person = nil
```

---

# Interview Point

When:

```ruby
person = nil
```

Reference count becomes 0 → object removed from memory.

---

# 2. What is Strong Reference?

## Answer

By default, all references in Swift are strong references.

A strong reference:

- Increases reference count
- Keeps object alive

---

# Example

```ruby
class Car {

}

var car1: Car? = Car()
var car2 = car1
```

Now object has:

- 2 strong references

Memory deallocates only when both become nil.

---

# 3. What is Weak Reference?

## Answer

Weak reference:

- Does NOT increase reference count
- Used to avoid retain cycles
- Must be optional

---

# Example

```ruby
class Employee {

    weak var manager: Manager?
}
```

---

# Important Interview Points

| Weak Reference            | Description |
| ------------------------- | ----------- |
| Does not retain object    | Yes         |
| Automatically becomes nil | Yes         |
| Must be optional          | Yes         |

---

# 4. What is Unowned Reference?

## Answer

Unowned reference:

- Does NOT increase reference count
- Assumes object will never become nil
- Not optional usually

---

# Example

```ruby
class Customer {

    var card: CreditCard?
}

class CreditCard {

    unowned let customer: Customer

    init(customer: Customer) {
        self.customer = customer
    }
}
```

---

# Weak vs Unowned

| Weak           | Unowned               |
| -------------- | --------------------- |
| Optional       | Non-optional          |
| Can become nil | Assumes always exists |
| Safer          | Faster but risky      |

---

# 5. What is Retain Cycle?

## Answer

A retain cycle occurs when two objects strongly reference each other, preventing memory deallocation.

---

# Example

```ruby
class Person {

    var apartment: Apartment?
}

class Apartment {

    var tenant: Person?
}
```

---

# Problem

```ruby
person.apartment = apartment
apartment.tenant = person
```

Both strongly hold each other.

Memory leak occurs.

---

# Solution

Use weak reference.

```ruby
weak var tenant: Person?
```

---

# 6. What is Memory Leak?

## Answer

Memory leak happens when memory is allocated but never released.

Common reason:

- Retain cycles

---

# Symptoms

- App memory increases continuously
- Performance issues
- App crash due to high memory

---

# 7. How to Detect Memory Leaks?

## Answer

Using:

- Xcode Memory Graph
- Instruments
- Leaks tool

---

# Steps

```text id="jlwm79"
Xcode
  ↓
Product
  ↓
Profile
  ↓
Leaks / Allocations
```

---

# 8. What is deinit?

## Answer

`deinit` is called before object deallocation.

Used for:

- Cleanup
- Removing observers
- Closing resources

---

# Example

```ruby
class User {

    deinit {
        print("Object Removed")
    }
}
```

---

# 9. What is Capture List in Closures?

## Answer

Closures capture variables strongly by default.

This can create retain cycles.

Capture list helps avoid it.

---

# Example

```ruby
class HomeViewController: UIViewController {

    var completion: (() -> Void)?

    func setupClosure() {

        completion = { [weak self] in

            self?.view.backgroundColor = .red
        }
    }
}
```

---

# Why weak self?

Without `[weak self]`:

- Closure strongly captures self
- Retain cycle possible

---

# 10. Difference Between weak and unowned in Closures?

| weak          | unowned                        |
| ------------- | ------------------------------ |
| Optional self | Non-optional self              |
| Safer         | Risky if nil                   |
| Used commonly | Used when guaranteed existence |

---

# Weak Example

```ruby
{ [weak self] in

    self?.fetchData()
}
```

---

# Unowned Example

```ruby
{ [unowned self] in

    fetchData()
}
```

---

# 11. Why are Delegates weak?

## Answer

Delegates are usually weak to avoid retain cycles.

---

# Example

```ruby
weak var delegate: UITableViewDelegate?
```

---

# Reason

```text id="jlwm2n"
Parent
  ↓ strong
Child
  ↓ weak
Parent
```

Avoids circular strong references.

---

# 12. What Happens if weak is not used in Delegates?

## Answer

Retain cycle occurs.

Objects never deallocate.

Memory leak happens.

---

# 13. Explain Reference Count

## Answer

ARC keeps count of strong references.

Example:

```ruby
var obj1 = Person()
```

Reference count = 1

```ruby
var obj2 = obj1
```

Reference count = 2

```ruby
obj1 = nil
```

Reference count = 1

```ruby
obj2 = nil
```

Reference count = 0 → memory released

---

# 14. Difference Between Value Type and Reference Type?

| Value Type             | Reference Type   |
| ---------------------- | ---------------- |
| Struct                 | Class            |
| Copied on assignment   | Shared reference |
| Stored on stack mostly | Stored on heap   |
| No ARC needed          | ARC needed       |

---

# Example

## Struct

```ruby
struct User {

    var name: String
}
```

---

## Class

```ruby
class User {

    var name = ""
}
```

---

# 15. Why Structs are Preferred in Swift?

## Answer

Structs:

- Avoid retain cycles
- Better performance
- Safer memory behavior
- Immutable support

SwiftUI heavily uses structs.

---

# 16. What is Heap Memory?

## Answer

Heap stores:

- Reference types (classes)
- Dynamic memory

Managed by ARC.

---

# 17. What is Stack Memory?

## Answer

Stack stores:

- Value types
- Local variables
- Function calls

Faster than heap.

---

# 18. What Causes Retain Cycles Most Commonly?

## Answer

Most common causes:

- Closures
- Delegates
- Parent-child references

---

# Example

```ruby
self.closure = {

    self.fetchData()
}
```

Strong capture of self.

---

# 19. What is Lazy Property?

## Answer

Lazy property initializes only when used first time.

---

# Example

```ruby
lazy var tableView = UITableView()
```

---

# Benefits

- Saves memory
- Improves startup performance

---

# 20. Explain Memory Graph Debugger

## Answer

Xcode tool for visualizing memory references.

Helps identify:

- Retain cycles
- Leaks
- Strong references

---

# Common Interview Rapid-Fire Questions

| Question                                | Answer             |
| --------------------------------------- | ------------------ |
| What manages memory in Swift?           | ARC                |
| Which references increase retain count? | Strong             |
| Which reference avoids retain cycle?    | Weak               |
| Which reference is non-optional?        | Unowned            |
| Which type uses ARC?                    | Class              |
| Which types avoid ARC mostly?           | Struct             |
| Why are delegates weak?                 | Avoid retain cycle |
| Which tool detects leaks?               | Instruments        |
| What is called before deallocation?     | deinit             |
| What causes memory leaks most commonly? | Retain cycles      |

---

# Most Important Practical Interview Topics

Focus strongly on:

- ARC
- Retain cycles
- weak vs unowned
- Closures
- Delegates
- deinit
- Memory leaks
- Struct vs Class

---

# Most Asked Real Interview Question

## Explain retain cycle with example.

### Answer

A retain cycle occurs when two objects strongly reference each other and prevent ARC from deallocating memory.

Example:

```ruby
class A {

    var b: B?
}

class B {

    var a: A?
}
```

Solution:
Use weak reference in one direction.

```ruby
weak var a: A?
```

---

# Best Interview Summary

> Swift uses ARC (Automatic Reference Counting) to manage memory automatically. ARC tracks strong references and deallocates objects when reference count becomes zero. Memory leaks mainly occur due to retain cycles, especially with delegates and closures. Weak and unowned references are used to break retain cycles and ensure proper memory management.
