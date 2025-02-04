## **1️⃣ Setting Up Navigation with `NavigationStack`**  

Navigation is managed by `NavigationStack`, which allows pushing and popping views in a structured way.  

### **Implementation in `ContentView.swift`**
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
### **Explanation:**  
✅ `NavigationStack` **wraps `LoginView`**, making navigation possible.  
✅ **Ensures smooth transitions between Login, Register, and Home screens.**  

---

## **2️⃣ Login Screen (`LoginView.swift`)**  

The **Login screen** allows users to:  
- **Navigate to the Register screen using `NavigationLink`.**  
- **Navigate to the Home screen upon pressing the Login button (`navigationDestination`).**  

### **Implementation:**
```swift
import SwiftUI

struct LoginView: View {
    @State private var navigateToHome = false

    var body: some View {
        NavigationStack {
            VStack(spacing: 20) {
                Text("Login")
                    .font(.largeTitle)
                    .bold()

                VStack(alignment: .leading, spacing: 8) {
                    Text("Email")
                        .font(.headline)
                    TextField("Enter your email", text: .constant(""))
                        .textFieldStyle(RoundedBorderTextFieldStyle())

                    Text("Password")
                        .font(.headline)
                    SecureField("Enter your password", text: .constant(""))
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                }
                .padding()

                Button(action: {
                    navigateToHome = true
                }) {
                    Text("Login")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background(Color.blue)
                        .foregroundColor(.white)
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
### **Explanation:**  
✅ **Login button updates `navigateToHome = true`**, triggering navigation.  
✅ **`navigationDestination(isPresented:)` pushes `HomeView`.**  
✅ **`NavigationLink` allows navigation to the Register screen.**  

---

## **3️⃣ Register Screen (`RegisterView.swift`)**  

The **Register screen** allows users to:  
- **Enter an email, password, and confirm password.**  
- **Navigate back to Login by dismissing the current view (`@Environment(\.dismiss)`).**  

### **Implementation:**
```swift
import SwiftUI

struct RegisterView: View {
    @Environment(\.dismiss) var dismiss

    var body: some View {
        VStack(spacing: 20) {
            Text("Register")
                .font(.largeTitle)
                .bold()

            VStack(alignment: .leading, spacing: 8) {
                Text("Email")
                    .font(.headline)
                TextField("Enter your email", text: .constant(""))
                    .textFieldStyle(RoundedBorderTextFieldStyle())

                Text("Password")
                    .font(.headline)
                SecureField("Enter your password", text: .constant(""))
                    .textFieldStyle(RoundedBorderTextFieldStyle())

                Text("Confirm Password")
                    .font(.headline)
                SecureField("Re-enter your password", text: .constant(""))
                    .textFieldStyle(RoundedBorderTextFieldStyle())
            }
            .padding()

            Button(action: {}) {
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
### **Explanation:**  
✅ **Users can return to `LoginView` using `@Environment(\.dismiss)`.**  
✅ **TextFields for email and password ensure a structured UI.**  

---

## **4️⃣ Home Screen (`HomeView.swift`)**  

The **Home screen** is the final destination after login.

### **Implementation:**
```swift
import SwiftUI

struct HomeView: View {
    var body: some View {
        VStack {
            Text("Home Screen")
                .font(.largeTitle)
                .bold()
        }
    }
}
```
---

## **5️⃣ Summary of Navigation Flow**  

### **Navigation Paths:**
- **Login → Register** (`NavigationLink`)  
- **Register → Login** (`dismiss()`)  
- **Login → Home** (`navigationDestination`)  

### **Navigation Flow Diagram:**  
```
📂 Project Folder
 ├── ContentView.swift  → Manages navigation stack
 ├── LoginView.swift    → First screen, navigates to Home or Register
 ├── RegisterView.swift → Allows registration, dismisses to Login
 ├── HomeView.swift     → Destination after login
```

---
