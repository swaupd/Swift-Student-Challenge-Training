### **Swift Concepts**

#### **1. Variables and Constants**
Introduce variables and constants by creating a simple structure for the to-do list.  

**Code:**  
```swift
let appName: String = "Swift To-Do List" // A constant for the app name
var tasks: [String] = []                // A variable to store tasks
```

**Explanation:**  
- **`let`:** Declares a constant that cannot change.  
- **`var`:** Declares a variable that can be modified.  
- **Integration:** `appName` is a constant, while `tasks` will store the to-do items dynamically.

---

#### **2. Functions**
Write reusable functions to manage tasks, such as adding and viewing tasks.

**Code:**  
```swift
func addTask(_ task: String) {
    tasks.append(task)
    print("Task added: \(task)")
}

func viewTasks() {
    if tasks.isEmpty {
        print("No tasks available.")
    } else {
        print("Your tasks:")
        for (index, task) in tasks.enumerated() {
            print("\(index + 1): \(task)")
        }
    }
}
```

**Explanation:**  
- **`addTask(_:)`:** Adds a new task to the list.  
- **`viewTasks()`:** Displays the list of tasks with an index.  
- **Integration:** These functions provide the core functionality of the to-do list.

---

#### **3. Optionals**
Use optionals for safely handling task retrieval.

**Code:**  
```swift
func getTask(at index: Int) -> String? {
    guard index >= 0 && index < tasks.count else {
        return nil // Returns nil if the index is invalid
    }
    return tasks[index]
}
```

**Explanation:**  
- **`String?`:** Declares an optional that may contain `nil`.  
- **`guard`:** Ensures the index is within bounds before returning the task.  
- **Integration:** Ensures safety when accessing specific tasks.

---

#### **4. Control Flow**
Use control flow to let users interactively manage tasks.

**Code:**  
```swift
func manageTasks() {
    print("""
    What would you like to do?
    1: Add Task
    2: View Tasks
    3: Exit
    """)

    if let choice = Int(readLine() ?? "") {
        switch choice {
        case 1:
            print("Enter a task:")
            if let task = readLine() {
                addTask(task)
            }
        case 2:
            viewTasks()
        case 3:
            print("Goodbye!")
            return
        default:
            print("Invalid choice.")
        }
        manageTasks() // Recurse for the next action
    } else {
        print("Invalid input.")
        manageTasks()
    }
}
```

**Explanation:**  
- **`if let`:** Safely unpacks optional user input.  
- **`switch`:** Handles different user choices.  
- **Integration:** Builds interactivity for task management.

---

#### **5. Structs**
Create a struct to encapsulate task data and provide additional functionality.

**Code:**  
```swift
struct Task {
    let name: String
    var isCompleted: Bool = false

    mutating func markComplete() {
        isCompleted = true
    }

    func display() {
        print("\(name) - \(isCompleted ? "Completed" : "Pending")")
    }
}
```

**Explanation:**  
- **Struct:** Groups related data (`name` and `isCompleted`).  
- **`mutating`:** Allows modification of properties in a struct method.  
- **Integration:** Can replace the current `tasks` array with an array of `Task`.

---

#### **6. Arrays and Loops**
Replace the plain `String` tasks array with the `Task` struct and loop through tasks for display.

**Code:**  
```swift
var taskList: [Task] = []

func addNewTask(_ taskName: String) {
    let task = Task(name: taskName)
    taskList.append(task)
    print("Task added: \(taskName)")
}

func displayAllTasks() {
    if taskList.isEmpty {
        print("No tasks to show.")
    } else {
        print("All tasks:")
        for (index, task) in taskList.enumerated() {
            print("\(index + 1). \(task.name) - \(task.isCompleted ? "Completed" : "Pending")")
        }
    }
}
```

**Integration:** Replace `tasks` with `taskList`, introducing better task management.

---

#### **7. Error Handling**
Handle errors for invalid user input or operations.

**Code:**  
```swift
enum TaskError: Error {
    case invalidIndex
}

func completeTask(at index: Int) throws {
    guard index >= 0 && index < taskList.count else {
        throw TaskError.invalidIndex
    }
    taskList[index].markComplete()
    print("Task \(index + 1) marked as complete.")
}
```

**Explanation:**  
- **`enum`:** Defines error types.  
- **`throws`:** Declares that the function can throw an error.  
- **Integration:** Adds robust error handling to task management.

---

#### **Final Program**
The final to-do list program integrates all concepts:

```swift
import Foundation

struct Task {
    let name: String
    var isCompleted: Bool = false

    mutating func markComplete() {
        isCompleted = true
    }
}

var taskList: [Task] = []

func addNewTask(_ taskName: String) {
    let task = Task(name: taskName)
    taskList.append(task)
    print("Task added: \(taskName)")
}

func displayAllTasks() {
    if taskList.isEmpty {
        print("No tasks to show.")
    } else {
        print("All tasks:")
        for (index, task) in taskList.enumerated() {
            print("\(index + 1). \(task.name) - \(task.isCompleted ? "Completed" : "Pending")")
        }
    }
}

func completeTask(at index: Int) {
    guard index >= 0 && index < taskList.count else {
        print("Invalid index.")
        return
    }
    taskList[index].markComplete()
    print("Task \(index + 1) marked as complete.")
}

func manageTasks() {
    print("""
    Choose an option:
    1: Add Task
    2: View Tasks
    3: Complete Task
    4: Exit
    """)
    if let choice = Int(readLine() ?? "") {
        switch choice {
        case 1:
            print("Enter task name:")
            if let taskName = readLine() {
                addNewTask(taskName)
            }
        case 2:
            displayAllTasks()
        case 3:
            print("Enter the task number to complete:")
            if let taskNumber = Int(readLine() ?? "") {
                completeTask(at: taskNumber - 1)
            }
        case 4:
            print("Exiting...")
            return
        default:
            print("Invalid choice.")
        }
        manageTasks()
    } else {
        print("Invalid input.")
        manageTasks()
    }
}

print("Welcome to \(appName)")
manageTasks()
```
