# Swift Interview Prepration for iOS Developer

## 1. Execution states of app:

### a. Not running:

This means the app has not launched yet. Or the app has been terminated by the user. Also there is a possibility to the app has been terminated by the OS.

### b. Inactive:

Inactive app state can be occurred while running the app if a phone call is received or activated siri.

### c. Active:

This is the main execution state of an iOS application. The app is running on foreground and the UI is accessible. It receives any events and executes the code.

### d. Background:

Once the app moved to background it will be transited to the Background state. In this state, still the app can execute the code and receive the events (Not UI events). But in a limited time period the OS automatically moves the app to the Suspended state and stops execution of code.

### e. Suspended:

At this app state, your app remains on the memory. But not executing any code.
Since the app is not executing the code, the battery life is not affected by the application. But if any running app needs memory and the system is low on memory, the OS will terminate some suspended apps to free the memory.

## 2. AppDelegate Life Cycle Methods:

### Launch

### application:didFinishLaunchingWithOptions

This function is called by the OS when the launch process is almost done. (To launch the app, this function execution is need to be completed.) You can use this function and do the configurations you need, when the app is running.
Like,

- Create a Sqlite database for local data storage.
- Configure for data analytics.
- Configure for Firebase.

### applicationWillEnterForeground

This method is called after application: didFinishLaunchingWithOptions: or if your app becomes active again after receiving a phone call or other system interruption.

### applicationDidBecomeActive

This method is called after applicationWillEnterForeground: to finish up the transition to the foreground.

### Termination

### applicationWillResignActive

This method is called when the app is about to become inactive (for example, when the phone receives a call or the user hits the Home button).

### applicationDidEnterBackground

This method is called when your app enters in background state after becoming inactive. You have approximately five seconds to run any tasks you need to back things up in case the app gets terminated later or right after that.

### applicationWillTerminate

this method is called when your app is about to be purged from memory. Call any final cleanups here.

## 3. View Controller Life Cycle Methods:

### viewDidLoad:

This Method is loaded once in view controller life cycle. Its Called When all the view are loaded. It's called When content view is created in memory. You Can do Some common task in this method:
1.Network call which need Once.
2.User Interface
3.Others Task Those are Need to do Once
Note: This Method Call before the bound are defined and rotation happen.So its Risky to work view size in this method.

### viewWillAppear:

This Method is called every time before the view are visible to and before any animation are configured.

### viewDidAppear:

This Method is called after the view present on the screen. Usually save data to core data or start animation or start playing a video or a sound, or to start collecting data from the network This type of task good for this method.

### viewWillDisappear:

This method called before the view are remove from the view hierarchy. The View are Still on view hierarchy but not removed yet . any unload animations haven’t been configured yet. Add code here to handle timers, hide the keyboard, cancel network requests, revert any changes to the parent UI. Also, this is an ideal place to save state.

### viewDidDisappear:

This method is called after the VC’s view has been removed from the view hierarchy. Use this method to stop listening for notifications or device sensors.

## 4. Class and Struct:

### Similarities:

- Defines properties to store values.
- Defines methods to provide functionality.
- Defines initializers to set up their initial state.
- Conforms to protocols.

### Differences:

- Inheritance enables one class to inherit the characteristics of another.
- Deinitializer enables an instance of a class.
- Reference counting allows more than one reference to a class instance.
- Class is reference type but struct is value type.
- Instance of a class is stored in heap memory and instance of struct is stored in stack memory.
- Struct is faster than class.
- Struct is thread safe as each thread has it's own stack. But class is not thread safe as global memory space is needed for reference type.
- In struct, the amount of memory required can be calculated at compile time. But in class the amount of memory required can not be calculated at compile time.
- In struct, cost of allocation/deallocation is less, because stack pointer is to be moved. (Last in first out)

#### When should you use class?

- Objective-C interobrability.
- Copying doesn’t make sense.
- Shared mutable state is needed. - Ex. AppDelegate
- Need to control the identity.
- Inheritance is needed (but if you can use POP, as an alternative, go for Struct.)

#### When struct contains a property of class type, it's value will be copied, or it will refer to the same object?

It will refer to same object.

```ruby
class Employee {
    var name: String

    init(name: String) {
        self.name = name
    }
}

struct Company {
    var salary: String
    var employee: Employee
}

let company1 = Company(salary: "25,00,000", employee: Employee(name: "Ayush"))
var company2 = company1

company2.salary = "30,00,000"
company2.employee.name = "Neelima"

print(company1.salary) ---> 25,00,000
print(company1.employee.name) ---> Neelima

print(company2.salary) ---> 30,00,000
print(company2.employee.name) ---> Neelima
```

## 5. Optional:

A type that represents either a wrapped value or nil, the absence of a value. Enum is optional type, it has 2 values: some and none.

```ruby
var value: String?
```

### Optional Binding: (Conditional Unwrapping)

To conditionally bind the wrapped value of an Optional instance to a new variable, use one of the optional binding control structures, including if let, guard let, and switch.

```ruby
if let starPath = imagePaths["star"] {
    print("The star image is at '\(starPath)'")
} else {
    print("Couldn't find the star image")
}
// Prints "The star image is at '/glyphs/star.png'"
```

```ruby
 guard let starPath = imagePaths["star"] else {
     return
  }
 print("The star image is at '\(starPath)'")
```

### Optional Chaining:

To safely access the properties and methods of a wrapped instance, use the postfix optional chaining operator (postfix ?). The following example uses optional chaining:

```ruby
var name: String?
let myName = name?.uppercased()
// let myName = name?.uppercased().someOptionalValue?.someOtherOptionalValue?.whatever

//or

if imagePaths["star"]?.hasSuffix(".png") == true {
    print("The star image is in PNG format")
}
// Prints "The star image is in PNG format"
```

### Using the Nil-Coalescing Operator:

Use the nil-coalescing operator (??) to supply a default value in case the Optional instance is nil. Here a default path is supplied for an image that is missing from imagePaths.

```ruby
let defaultImagePath = "/images/default.png"
let heartPath = imagePaths["heart"] ?? defaultImagePath
print(heartPath)
// Prints "/images/default.png"
```

The ?? operator also works with another Optional instance on the right-hand side. As a result, you can chain multiple ?? operators together.

```ruby
let shapePath = imagePaths["cir"] ?? imagePaths["squ"] ?? defaultImagePath
print(shapePath)
// Prints "/images/default.png"
```

### Unconditional Unwrapping: (Implicitly Unwrapping)

When you’re certain that an instance of Optional contains a value, you can unconditionally unwrap the value by using the forced unwrap operator (postfix !). For example, the result of the failable Int initializer is unconditionally unwrapped in the example below.

```ruby
let age: Int! = 20
print(age)
// Prints 20
```

You can also perform unconditional optional chaining by using the postfix ! operator.

```ruby
let isPNG = imagePaths["star"]!.hasSuffix(".png")
print(isPNG)
// Prints "true"
```

Unconditionally unwrapping a nil instance with ! triggers a runtime error.

### Unwrapping via Type Casting:

In this way, Sometimes different types of values ​​can be defined during the runtime of the application. If you don’t be careful, your application may be crash.

```ruby
var userData: Any? = 1

if userData as? String != nil {
    print("userData is defined as a String.")
} else if userData as? Int != nil {
    print("userData is defined as an Integer.")
}
```

### Forced Unwrapping:

You can unwrap your Optional variable with “!” operator but it’s a less safe wrapping way, you have to be sure of variable contains value or not. If you do not use the forcibly unwrapping method in the right place, your application may be crash.

```ruby
var moviesCount: Int?
print(moviesCount!)
// Fatal error: Unexpectedly found nil while unwrapping an Optional value
```

## 6. Delegate:

A delegate is an object that acts on behalf of, or in coordination with, another object when that object encounters an event in a program.

## 7. Protocol:

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

```ruby
protocol SomeProtocol {
    var name: String { get set }
    var email: String { get }
    static var address: String { get }
    static func someTypeMethod()
    func anyMethod()
}

class MyClass: SomeProtocol {
    var email: String {
        return "ayush@gmail.com"
    }

    func anyMethod() {
        // write some code
    }
}
```

Properties in protocol are not computed properties, to acheive computed property for protocol we have to use extension for that protocol.

```ruby
protocol Employee {
    var experience: Int { get set }
}

extension Employee {
    var salary: String {
        return experience > 5 ? "20,00,000" : "15,00,000"
    }
}
```

### Optional Protocol Methods:

It is not necessary to implement that method if we were not going to use it. Swift protocols on their side do not allow optional methods. But if you are making an app for macOS, iOS, tvOS or watchOS you can add the @objc keyword at the beginning of the implementation of your protocol and add @objc follow by optional keyword before each methods you want to be optional.

```ruby
@objc protocol SomeProtocol {
    @objc optional var age: Int { get }
    @objc optional func getValue()
    func anyMethod()
}

class MyClass: SomeProtocol {
    func anyMethod() {
        // write some code
    }
}
```

## 8. Tuples:

Tuples enable you to create and pass around groupings of values. You can use a tuple to return multiple values from a function as a single compound value.

There are two types of Tuple:

1. Named Tuple:
   In Named tuple we assign individual names to each elements.

```ruby
// Define it like:

let nameAndAge = (name:"Midhun", age:7)

// Access the values like:

nameAndAge.name
nameAndAge.age
```

2. Unnamed Tuple:
   In unnamed tuple we don't specify the name for it's elements.

```ruby
// Define it like:

let nameAndAge = ("Midhun", 7)

// Access the values like:

nameAndAge.0
nameAndAge.1
```

### Difference between Tuples & Dictionary:

- If you need to return multiple values from a method you can use tuple.
- Tuple won't need any key value pairs like Dictionary.
- A tuple can contain only the predefined number of values, in dictionary there is no such limitation.
- A tuple can contain different values with different datatype while a dictionary can contain only one datatype value at a time.
- Tuples are particularly useful for returning multiple values from a function. A dictionary can be used as a model object.

## 9. Generics:

Generic code enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define. You can write code that avoids duplication and expresses its intent in a clear, abstracted manner.

Example:

```ruby
func getValue<T>(arr: [T]) -> T {
    return arr[0]
}

print(getValue(arr: [1, 2, 3])) // prints 1
print(getValue(arr: ["Car", "Bat", "Mobile"])) // prints Car
print(getValue(arr: [1.1, 2.1, 3.1])) // prints 1.1
```

## 10. Dependency Injection:

- Dependency Injection is often used with the intention of writing code that is loosely coupled, and thus, easier to test.

- Separation of concerns:
- Dependency injection allows us to understand our code in a much clearer way and separates our concerns. When we use dependency injection, we can see that our objects are responsible for managing and handling the given dependency.

- Testing:
- Dependency injection is great for testing.

- Loosely Coupled:
- In computing and systems design a loosely coupled system is one in which each of its components has, or makes use of, little or no knowledge of the definitions of other separate components.

## 11. Access Control:

Access control restricts access to parts of your code from code in other source files and modules. This feature enables you to hide the implementation details of your code, and to specify a preferred interface through which that code can be accessed and used.

- Open access and public access enable entities to be used within any source file from their defining module, and also in a source file from another module that imports the defining module. You typically use open or public access when specifying the public interface to a framework.
- Internal access enables entities to be used within any source file from their defining module, but not in any source file outside of that module. You typically use internal access when defining an app’s or a framework’s internal structure.
- File-private access restricts the use of an entity to its own defining source file. Use file-private access to hide the implementation details of a specific piece of functionality when those details are used within an entire file.
- Private access restricts the use of an entity to the enclosing declaration, and to extensions of that declaration that are in the same file. Use private access to hide the implementation details of a specific piece of functionality when those details are used only within a single declaration.

### Difference between Open and Public:

- An open class is accessible and subclassable outside of the defining module.
- An open class member is accessible and overridable outside of the defining module.

## 12. Properties:

Properties associate values with a particular class, structure, or enumeration.

### Stored Properties:

In its simplest form, a stored property is a constant or variable that is stored as part of an instance of a particular class or structure. Stored properties can be either variable stored properties (introduced by the var keyword) or constant stored properties (introduced by the let keyword).

```ruby
struct Rectangle {
    var width: Int?
    var height: Int?
}
```

#### Lazy Stored Properties:

- A lazy stored property is a property whose initial value is not calculated until the first time it is used. You indicate a lazy stored property by writing the lazy modifier before its declaration.
- You can’t use lazy with let.
- You can’t use it with computed properties. Because, a computed property returns the value every time we try to access it after executing the code inside the computation block.
- You can use lazy only with members of struct and class.
- Lazy variables are not initialised atomically and so is not thread safe.

```ruby
class Test {
    var name: String
    lazy var greeting : String = {
        return “Hello \(self.name)”
    }()
}
```

### Computed Properties:

In addition to stored properties, classes, structures, and enumerations can define computed properties, which do not actually store a value. Instead, they provide a getter and an optional setter to retrieve and set other properties and values indirectly.

```ruby
struct Rectangle {
    var width: Int?
    var height: Int?
    var area: Int {
        get {
            return width * height
        }
        set {
            width = 10
            height = 20
        }
    }
}
```

### Property Observers:

Property observers observe and respond to changes in a property’s value. Property observers are called every time a property’s value is set, even if the new value is the same as the property’s current value.

- willSet is called just before the value is stored.
- didSet is called immediately after the new value is stored.

```ruby
class Account {
    var balance: Int = 0 {
        willSet(newbalance) {
            print("About to set totalBalance to \(newbalance).")
        }
        didSet {
            print("Send message to user for balance updated.")
        }
    }
}
```

## 13. Mutating:

In order to modify the properties of a value type, you have to use the mutating keyword in the instance method.

```ruby
struck Account {
    var balance: Double

    mutating func addMoney(amount: Double) {
        balance += amount
    }
}
```

## 14. Subscripts:

- Classes, structures, and enumerations can define subscripts, which are shortcuts for accessing the member elements of a collection, list, or sequence.
- You use subscripts to set and retrieve values by index without needing separate methods for setting and retrieval.

```ruby
class Colors {
    var arrColor = ["Red", "Blue", "yellow"]
    subscript(_ index: Int) -> {
        get {
            return arrColor[index]
        }
        set(newValue) {
            self.arrColor[index] = newValue
        }
    }
}

var color = Colors()
print(color[0]) // prints Red

color[0] = "Brown"
print(color[0]) // prints Brown
```

## 15. Memory Management: (ARC)

Memory management in Swift is primarily handled by Automatic Reference Counting (ARC), which automatically manages the allocation and deallocation of memory for your objects. However, it's essential to understand how ARC works and how to manage memory effectively to avoid issues like retain cycles and memory leaks.

Here are some key concepts related to memory management in Swift:

1.  **Automatic Reference Counting (ARC):**
    - Swift uses ARC to track and manage your app's memory usage.
    - ARC keeps track of how many references exist to each instance of a class. When the number of references becomes zero, ARC deallocates the instance's memory.

2.  **Strong References:**
    - By default, Swift uses strong references when you create instances of classes.
    - A strong reference means that an object remains in memory as long as at least one strong reference to it exists.
    - Example:

      ```swift
      class Person {
          var name: String
          init(name: String) {
              self.name = name
          }
      }

      var person1: Person? = Person(name: "Alice")
      var person2 = person1
      person1 = nil // person1's reference is set to nil, but person2 still holds a strong reference
      ```

3.  **Weak References:**
    - Weak references are used to avoid retain cycles, which can lead to memory leaks.
    - Weak references don't keep a strong hold on the instance they reference. If the instance is deallocated, a weak reference automatically becomes nil.
    - Example:

      ```swift
      class Person {
          var name: String
          weak var friend: Person?
          init(name: String) {
              self.name = name
          }
      }

      var alice: Person? = Person(name: "Alice")
      var bob: Person? = Person(name: "Bob")

      alice?.friend = bob
      bob?.friend = alice // Without weak references, this would create a retain cycle
      alice = nil // Both alice and bob are deallocated because of the weak reference
      bob = nil
      ```

4.  **Unowned References:**
    - Unowned references are similar to weak references but assume that the reference will always point to a valid instance.
    - Unlike weak references, unowned references don't become nil if the instance they reference is deallocated. This can lead to crashes if the referenced instance is accessed after deallocation.
    - Example:

      ```swift
      class Apartment {
          var number: Int
          var tenant: Person?

          init(number: Int) {
              self.number = number
          }
      }

      class Person {
          var name: String
          var apartment: Apartment?

          init(name: String) {
              self.name = name
          }
      }

      var alice: Person? = Person(name: "Alice")
      var apartment: Apartment? = Apartment(number: 123)

      alice?.apartment = apartment
      apartment?.tenant = alice

      alice = nil // apartment still has a reference to alice through the unowned reference
      ```

5.  **Capture Lists and Closures:**
    - When using closures that capture values from their surrounding context, you may need to use capture lists to avoid retain cycles.
    - Capture lists specify how values are captured by a closure, using `[weak self]` or `[unowned self]` to prevent strong reference cycles.
    - Example:
      ```swift
      class SomeClass {
      var closure: (() -> Void)?

               func setupClosure() {
                   closure = { [weak self] in
                       self?.doSomething()
                   }
               }

               func doSomething() {
                   print("Doing something")
               }

               deinit {
                   print("SomeClass instance deallocated")
               }
           }

           var someInstance: SomeClass? = SomeClass()
           someInstance?.setupClosure()
           someInstance?.closure?() // The closure is executed, and it doesn't create a strong reference cycle
           someInstance = nil // SomeClass instance deallocated
           ```

      By understanding these concepts and using them appropriately, you can effectively manage memory in your Swift code and avoid common memory-related issues.

## 16. Closure:

- Self contained blocks of functionality that can be passed around and used in code.
- It can capture and store references to any constants and variables from the current context in which they are defined.
- Used for event handling and callbacks.
- Can pass closure as completion handlers.
- Closure is a reference type.

```ruby
let add: (Int, Int) -> Int = { (value1, value2) in
    return (value1 + value2)
}
```

### Types Of Closure:

#### 1. @nonescaping Closure:

When passing a closure as the function argument, the closure gets execute with the function’s body and returns the compiler back. As the execution ends, the passed closure goes out of scope and have no more existence in memory.

##### Lifecycle of the @nonescaping closure:

- Pass the closure as function argument, during the function call.
- Do some additional work with function.
- Function runs the closure.
- Function returns the compiler back.

```ruby
    func getSumOf(array:[Int], handler: ((Int)->Void)) {
        //step 2
        var sum: Int = 0
        for value in array {
            sum += value
        }
        //step 3
        handler(sum)
    }

    func doSomething() {
        //setp 1
        self.getSumOf(array: [16,756,442,6,23]) { [weak self](sum) in
            print(sum)
            //step 4, finishing the execution
        }
    }
//It will print the sumof all the given numbers.
```

#### 2. @escaping Closure:

When passing a closure as the function argument, the closure is being preserve to be execute later and function’s body gets executed, returns the compiler back. As the execution ends, the scope of the passed closure exist and have existence in memory, till the closure gets executed.

##### Lifecycle of the @escaping closure:

- Pass the closure as function argument, during the function call.
- Do some additional work with function.
- Function executes the closure asynchronously or stored.
- Function returns the compiler back.

##### Storage:

When you need to preserve the closure in storage that exist in the memory, past of the calling function get executed and return the compiler back. (Like waiting for the API response)

```ruby
var complitionHandler: ((Int)->Void)?
func getSumOf(array:[Int], handler: @escaping ((Int)->Void)) {
    //step 2
    //here I'm taking for loop just for example, in real case it'll be something else like API call
    var sum: Int = 0
    for value in array {
        sum += value
    }
    //step 3
    self.complitionHandler = handler
}

func doSomething() {
    //setp 1
    self.getSumOf(array: [16,756,442,6,23]) { [weak self](sum) in
        print(sum)
        //step 4, finishing the execution
    }
}
//Here we are storing the closure for future use.
//It will print the sumof all the passed numbers.
```

##### Asynchronous Execution:

When you are executing the closure asynchronously on dispatch queue, the queue will hold the closure in memory for you, to be used in future. In this case you have no idea when the closure will get executed.

```ruby
func getSumOf(array:[Int], handler: @escaping ((Int)->Void)) {
    //step 2
    var sum: Int = 0
    for value in array {
        sum += value
    }
    //step 3
    Globals.delay(0.3, closure: {
        handler(sum)
    })
}

func doSomething() {
    //setp 1
    self.getSumOf(array: [16,756,442,6,23]) { [weak self](sum) in
        print(sum)
        //step 4, finishing the execution
    }
}
//Here we are calling the closure with the delay of 0.3 seconds
//It will print the sumof all the passed numbers.
```

#### 3. Trailing closure:

- If you need to pass a closure expression to a function as the functions last argument.
- A trailing closure is written after the function call’s parentheses as a final parameter.

```ruby
func makeSquareOf(digit: Int, onCompletion: (Int) -> void) {
    let squareOfDigit = digit * digit
    onCompletion(squareOfDigit)
}
```

#### 4. Auto closure:

- Parameter in a function can be replaced with a closure that takes no parameter and returns back a value of the same type as the parameter.
- Such closure parameter can be marked with an @autoclosure attribute.

```ruby
func assert(_ condition: @autoclosure () -> Bool, _ message: @autoclosure () -> String) {
    if condition() {
        print(message())
    }
}

func conditionOne() -> Bool {
    return true
}

func phrase() -> String {
    return "Text"
}

assert(true, "Hello")
assert(false, "Good bye")
assert(conditionOne(), phrase())
```

## 17. Higher Order Functions:

Higher order functions are simply functions that can either accept functions or closures as arguments, or return a function/closure.

### Map:

Map can be used to loop over a collection and apply the same operation to each element in the collection.

```ruby
let arrName = ["Ayush", "Jhon", "Bob"]
let newArr = arrName.map { $0.lowerCased() }
print(newArr) // prints: [ayush, jhon, bob]
```

### CompactMap:

Returns an array containing the non-nil results of calling the given transformation with each element of this sequence.

```ruby
let possibleNumbers = ["1", "2", "three", "four", "5"]

let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]

let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```

### FlatMap:

Flatmap is used to flatten a collection of collections. But before flattening the collection, we can apply map to each elements.

```ruby
let name = "ayush"
let value = name.flatMap { $0 }
print(value) // prints: ["a", "y", "u", "s", "h"]
```

### Filter:

Use filter to loop over a collection and return an Array containing only those elements that match an include condition.

```ruby
let arrNumber = [1, 2, 3, 4, 5]
let arrEvenNumber = arrNumber.filter { $0 % 2 == 0 }
print(arrEvenNumber) // prints: [2, 4]
```

### Reduce:

Reduce is used to combine all items in a collection to create a single new value.

```ruby
let arrNumber = [1, 2, 3, 4, 5]
let numSum = arrNumber.reduce(0, { x, y in
    x + y
})
print(numSum) // prints: 15
```

## 18. Core Data:

Core data is used to manage the model layer object in our application. You can treat Core Data as a framework to save, track, modify and filter the data within iOS apps, however, Core Data is not a Database.

#### Object Graph:

An object graph is nothing more than a collection of objects that are connected with one another.
The Core Data framework takes care of managing the life cycle of the objects in the object graph.

#### NSManagedObjectContext:

The NSManagedObjectContext object manages a collection of model objects, instances of the NSManagedObject class.

#### Core Data Stack:

- Managed object model.
- Managed object context.
- Persistent store coordinator.
- Persistent store (storage).

#### Abstract Entity:

An Entity can be abstract, in which case it is never directly attached to a managed object.
An abstract object (in programming) or entity (in Core Data) is an object or entity that is never instantiated.

#### FetchedResultcontroller:

A controller that you use to manage the results of a Core Data fetch request and display data to the user in UITableView.

#### NSManagedObjectId:

A compact, universal identifier for a managed object.
