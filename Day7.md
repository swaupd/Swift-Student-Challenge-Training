# **Implementing Login, Registration, and User Management with SwiftData**  

This guide provides step-by-step instructions for integrating **SwiftData** into an existing SwiftUI app, covering:  
1. **Modifying `MyApp.swift` to enable SwiftData**  
2. **Setting up `LoginView.swift` for authentication**  
3. **Implementing `RegisterView.swift` for new user registration**  
4. **Adding user management and logout functionality in `HomeView.swift`**  

---

## **1️⃣ Modifying `MyApp.swift` to Enable SwiftData**  

### **Step 1: Import SwiftData**  
Add the import statement at the top of `MyApp.swift`:  
```swift
import SwiftData
```

### **Step 2: Define the User Model**  
Create a `User` model using SwiftData:  
```swift
@Model
class User {
    var email: String
    var password: String

    init(email: String, password: String) {
        self.email = email
        self.password = password
    }
}
```
- `@Model` allows SwiftData to manage user storage automatically.  
- Stores `email` and `password` for authentication.  

### **Step 3: Attach SwiftData to the App**  
Modify `@main` to **initialize SwiftData storage**:  
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .modelContainer(for: User.self)
        }
    }
}
```
- `modelContainer(for: User.self)` enables **data persistence for user authentication**.  

---

## **2️⃣ Implementing Login in `LoginView.swift`**  

### **Step 4: Import SwiftData and Define State Variables**  
```swift
import SwiftData
```
```swift
@Query var users: [User]  // Fetch users from SwiftData
@State private var email = ""
@State private var password = ""
@State private var errorMessage = ""
@State private var navigateToHome = false
```
- `@Query var users: [User]` loads stored users.  
- `@State` variables store user input and error messages.  

### **Step 5: Create the Login Button Action**  
```swift
Button("Login") {
    if users.first(where: { $0.email == email && $0.password == password }) != nil {
        navigateToHome = true
    } else {
        errorMessage = "Invalid email or password"
    }
}
```
- **Checks if input credentials match a stored user**.  
- **Sets `navigateToHome = true` to navigate to `HomeView`** if login is successful.  

### **Step 6: Handle Navigation to HomeView**  
```swift
.navigationDestination(isPresented: $navigateToHome) {
    HomeView()
}
```
- Navigates to `HomeView` upon successful login.  

---

## **3️⃣ Implementing Registration in `RegisterView.swift`**  

### **Step 7: Import SwiftData and Define State Variables**  
```swift
import SwiftData
```
```swift
@Environment(\.modelContext) var modelContext
@State private var email = ""
@State private var password = ""
@State private var confirmPassword = ""
@State private var errorMessage = ""
```
- `@Environment(\.modelContext)` accesses the **SwiftData storage context**.  

### **Step 8: Validate Input Before Registration**  
```swift
guard email.contains("@") && email.contains(".") else {
    errorMessage = "Invalid email format"
    return
}
guard password == confirmPassword, !password.isEmpty else {
    errorMessage = "Passwords do not match"
    return
}
```
- **Ensures the email format is correct (`user@example.com`)**.  
- **Checks that passwords match before storing the user**.  

### **Step 9: Store New User in SwiftData**  
```swift
let newUser = User(email: email, password: password)
modelContext.insert(newUser)
dismiss()
```
- **Inserts the new user into SwiftData storage**.  
- **Dismisses `RegisterView`, returning to `LoginView`**.  

---

## **4️⃣ Implementing User Management and Logout in `HomeView.swift`**  

### **Step 10: Import SwiftData and Define State Variables**  
```swift
import SwiftData
```
```swift
@Environment(\.dismiss) var dismiss
@Environment(\.modelContext) var modelContext
@Query var users: [User]
@State private var email = ""
@State var errorText = ""
```
- `@Query var users: [User]` fetches stored users.  
- `@State var email` stores the input for user removal.  
- `@State var errorText` displays error messages.  

### **Step 11: Create the "Remove User" Feature**  
```swift
Button("Remove user") {
    if email.isEmpty {
        errorText = "Please input an email"
    }
    else if let user = users.first(where: { $0.email == email }) {
        modelContext.delete(user)
        errorText = "User removed successfully"
    } else {
        errorText = "User not found"
    }
}
```
- **Deletes the user from SwiftData if the email exists**.  
- **Displays success or error messages accordingly**.  

### **Step 12: Replace Back Button with "Logout"**  
```swift
.toolbar {
    ToolbarItem(placement: .navigationBarLeading) {
        Button("Logout") {
            dismiss()
        }
    }
}
```
- **Replaces the default back button with "Logout"**.  
- **Pressing "Logout" returns the user to `LoginView`**.  

---
## **MyApp.swift**
```swift
import SwiftUI
import SwiftData

@Model
class User {
    var email: String
    var password: String
    
    init(email: String, password: String) {
        self.email = email
        self.password = password
    }
}

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .modelContainer(for: User.self)
        }
    }
}
```
---
## **ContentView.swift**
```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        NavigationStack {
            LoginView()
        }
    }
}
```
---
## **LoginView.swift**
```swift
import SwiftUI
import SwiftData

struct LoginView: View {
    @State private var navigateToHome = false
    @Environment(\.modelContext) var modelContext
    @Query var users: [User]
    @State private var email = ""
    @State private var password = ""
    @State private var errorMessage = ""

    var body: some View {
        NavigationStack {
            VStack(spacing: 20) {
                Text("Login")
                    .font(.largeTitle)
                    .bold()
                VStack(alignment: .leading, spacing: 8) {
                    Text("Email")
                        .font(.headline)
                    TextField("Enter your username", text: $email)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                    Text("Password")
                    SecureField("Enter your password", text: $password)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                    if !errorMessage.isEmpty {
                        Text(errorMessage).foregroundColor(.red).font(.subheadline)
                    }
                }
                .padding()
                
                Button(action: {
                    if users.first(where: { $0.email == email && $0.password == password }) != nil {
                        navigateToHome = true
                    } else {
                        errorMessage = "Invalid email or password"
                    }
                }) {
                    Text("Login")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background(Color.blue)
                        .foregroundStyle(.white)
                        .cornerRadius(10)
                }
                .padding(.horizontal)
                
                HStack {
                    Text("Don't have an account?")
                    NavigationLink("Register", destination: RegisterView())
                }
                .font(.subheadline)
                .padding(.top, 10)
                
             }
            .padding()
            .navigationDestination(isPresented: $navigateToHome) {
                HomeView()
            }
        }
    }
}
```
---
## **RegisterView.swift**
```swift
import SwiftUI
import SwiftData

struct RegisterView: View {
    @Environment(\.dismiss) var dismiss
    @Environment(\.modelContext) var modelContext
    @State private var email = ""
    @State private var password = ""
    @State private var confirmPassword = ""
    @State private var errorMessage = ""
    
    var body: some View {
        VStack(spacing: 20) {
            Text("Register")
                .font(.largeTitle)
                .bold()
            
            VStack(alignment: .leading, spacing: 8) {
                Text("Email")
                    .font(.headline)
                TextField("Enter your email", text: $email)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                
                Text("Password")
                    .font(.headline)
                SecureField("Enter your password", text: $password)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                
                Text("Confirm Password")
                    .font(.headline)
                SecureField("Re-enter your password", text: $confirmPassword)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
            }
            .padding()
            
            Button(action: {
                guard email.contains("@") && email.contains(".") else {
                    errorMessage = "Invalid email format"
                    return
                }
                guard password == confirmPassword, !password.isEmpty else {
                    errorMessage = "Passwords do not match"
                    return
                }
                let newUser = User(email: email, password: password)
                modelContext.insert(newUser)
                dismiss()

            }) {
                Text("Register")
                    .frame(maxWidth: .infinity)
                    .padding()
                    .background(Color.green)
                    .foregroundColor(.white)
                    .cornerRadius(10)
            }
            .padding(.horizontal)
            
            HStack {
                Text("Already have an account?")
                Button("Login") {
                    dismiss()
                }
            }
            .font(.subheadline)
            .padding(.top, 10)
        }
        .padding()
    }
}
```
---
## **HomeView.swift**
```swift
import SwiftUI
import SwiftData

struct HomeView: View {
    
    @Environment(\.dismiss) var dismiss
    @Environment(\.modelContext) var modelContext
    @Query var users: [User]
    @State private var email = ""
    @State var errorText = ""

    var body: some View {
        VStack {
            Text("Home Screen")
                .font(.largeTitle)
                .bold()
            TextField("Insert the email of the user you'd like to remove", text: $email)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
            Text(errorText)
            Button("Remove user") {
                if email.isEmpty {
                    errorText = "Please input an email"
                }
                else if let user = users.first(where: {$0.email == email}) {
                    modelContext.delete(user)
                    errorText = "User removed successfully"
                }
                else {
                    errorText = "User not found"
                }
            }
        }
        .toolbar {
            ToolbarItem(placement: .navigationBarLeading) {
                Button("Logout") {
                    dismiss()
                }
            }
        }
    }
}
```
