# **Implementing UserDefaults for data storage in apps**

This document details the **changes made to MyApp.swift, LoginView.swift, RegisterView.swift, and HomeView.swift**, explicitly specifying **what was changed, how it was implemented, and what each change does**.  

---

## **1️⃣ Changes in MyApp.swift**  

### **What Changed?**  
The latest version **added a `UserDefaultsManager` class** to manage user data, while the original version only contained the `@main` struct for launching `ContentView`.  

### **How It Was Implemented?**  
The `UserDefaultsManager` class was introduced at the top of `MyApp.swift`:  

```swift
class UserDefaultsManager {
    static let shared = UserDefaultsManager()
    
    private let userDefaults = UserDefaults.standard
    
    private init() {}
    
    func saveUser(email: String, password: String) {
        userDefaults.set(email, forKey: "userEmail")
        userDefaults.set(password, forKey: "userPassword")
    }
    
    func getUser() -> (email: String?, password: String?) {
        let email = userDefaults.string(forKey: "userEmail")
        let password = userDefaults.string(forKey: "userPassword")
        return (email, password)
    }
    
    func deleteUser() {
        userDefaults.removeObject(forKey: "userEmail")
        userDefaults.removeObject(forKey: "userPassword")
    }
}
```
- **This class provides helper methods** for saving, retrieving, and deleting user credentials in `UserDefaults`.  
- `saveUser(email:password:)` stores **user credentials**.  
- `getUser()` retrieves **saved credentials** for login verification.  
- `deleteUser()` removes the **stored user account** when logging out or deleting a user.  

**Why was this change made?**  
- The original version **did not handle user authentication**.  
- This ensures **persistent user data storage** without using databases like Core Data.  

---

## **2️⃣ Changes in LoginView.swift**  

### **What Changed?**  
1. **Added user authentication using `UserDefaultsManager`**.  
2. **Replaced placeholder text fields (`.constant("")`) with actual `@State` variables** to allow user input.  
3. **Added validation logic for email and password checking** before navigating to `HomeView`.  

### **How It Was Implemented?**  

#### **1. User Input is Now Stored in `@State` Variables**  
```swift
@State private var email = ""
@State private var password = ""
@State private var errorMessage = ""
```
- **Previous version used `.constant("")`** which did not allow text entry.  
- **Now, actual user input is stored in state variables**.  

#### **2. User Authentication Added**  
The login button now verifies user credentials against stored data in `UserDefaults`:  

```swift
Button(action: {
    let (savedEmail, savedPassword) = UserDefaultsManager.shared.getUser()
    
    if email == savedEmail && password == savedPassword {
        navigateToHome = true
    } else {
        errorMessage = "Invalid email or password"
    }
}) {
    Text("Login")
        .frame(maxWidth: .infinity)
        .padding()
        .background(Color.blue)
        .foregroundColor(.white)
        .cornerRadius(10)
}
```
- `getUser()` fetches **stored email and password**.  
- If the entered credentials **match the stored ones**, the user is **navigated to HomeView**.  
- If credentials **do not match**, an **error message is displayed**.  

**Why was this change made?**  
- The original version **did not check credentials** and simply allowed navigation.  
- This update **validates credentials before granting access**.  

---

## **3️⃣ Changes in RegisterView.swift**  

### **What Changed?**  
1. **Added email validation and password confirmation logic**.  
2. **Ensured the user does not already exist before registration**.  
3. **Stored new user credentials in `UserDefaults` when registration is successful**.  

### **How It Was Implemented?**  

#### **1. Email and Password Input Fields are Now Functional**  
```swift
@State private var email = ""
@State private var password = ""
@State private var confirmPassword = ""
@State private var errorMessage = ""
```
- Previously, the fields were using `.constant("")`, meaning user input **was not stored**.  
- Now, `@State` variables store user input properly.  

#### **2. User Registration Logic Added**  
```swift
Button(action: {
    let (savedEmail, _) = UserDefaultsManager.shared.getUser()
    guard email.contains("@") && email.contains(".") else {
        errorMessage = "Invalid email format"
        return
    }
    guard password == confirmPassword, !password.isEmpty else {
        errorMessage = "Passwords do not match"
        return
    }
    guard email != savedEmail else {
        errorMessage = "User Already Exists"
        return
    }
    UserDefaultsManager.shared.saveUser(email: email, password: password)
    errorMessage = "User registered successfully"
    dismiss()
}) {
    Text("Register")
        .frame(maxWidth: .infinity)
        .padding()
        .background(Color.green)
        .foregroundColor(.white)
        .cornerRadius(10)
}
```
- **Ensures the email contains "@"" and "."** for validation.  
- **Ensures the password and confirm password match** before saving the user.  
- **Prevents duplicate registrations** by checking if the email is already stored.  
- **Saves the user in `UserDefaults`** if validation is successful.  
- **Dismisses `RegisterView` upon successful registration**.  

**Why was this change made?**  
- The original version **did not store or validate user credentials**.  
- Now, it ensures **only valid and unique users can register**.  

---

## **4️⃣ Changes in HomeView.swift**  

### **What Changed?**  
1. **Added a "Delete User" feature** that allows a user to be removed from `UserDefaults`.  
2. **Displayed an error message when an invalid email is entered**.  
3. **Replaced the default back button with a "Logout" button**.  

### **How It Was Implemented?**  

#### **1. User Deletion Logic Added**  
```swift
Button(action: {
    let (savedEmail, _) = UserDefaultsManager.shared.getUser()
    if savedEmail == email {
        UserDefaultsManager.shared.deleteUser()
        errorText = "User removed successfully"
    } else {
        errorText = "User not found"
    }
}) {
    Text("Delete User")
        .frame(maxWidth: .infinity)
        .padding()
        .background(Color.blue)
        .foregroundColor(.white)
        .cornerRadius(10)
}
```
- **Fetches the stored email** from `UserDefaults`.  
- **If the email matches**, the user is **deleted from storage**.  
- **Displays success or failure messages accordingly**.  

#### **2. Logout Button Replaces Default Back Button**  
```swift
.toolbar {
    ToolbarItem(placement: .navigationBarLeading) {
        Button("Logout") {
            UserDefaultsManager.shared.deleteUser()  // Optional: Clears user data on logout
            dismiss()
        }
    }
}
```
- **Logs out the user and returns to `LoginView`**.  
- **Clears stored credentials if necessary**.  

**Why was this change made?**  
- The original version **did not allow user deletion or logout**.  
- Now, users can **remove their account or log out properly**.  

---

# **Summary of Key Changes**
| **File** | **Changes Made** | **Why It Was Implemented?** |
|----------|----------------|-----------------------------|
| **MyApp.swift** | Added `UserDefaultsManager` for managing user data | Provides persistent user storage without databases |
| **LoginView.swift** | Implemented authentication logic, replaced constant fields with `@State` | Ensures valid credentials before login |
| **RegisterView.swift** | Added validation for email/password, prevented duplicate registrations | Ensures correct user data before storing |
| **HomeView.swift** | Implemented user deletion and logout functionality | Allows account removal and session management |

---

This update **fully integrates UserDefaults for login, registration, and user management** while improving **security, validation, and user experience**.
