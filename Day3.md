# **Step-by-Step Guide to Creating a macOS Calculator UI in SwiftUI**  

## **Step 1: Setting Up the Base Structure**  
1. Open Xcode and create a **new macOS SwiftUI project**.  
2. Open `ContentView.swift`.  
3. Define the `ZStack` as the background container.  

```swift
import SwiftUI

struct ContentView: View {
    @State private var displayValue = "0"
    
    var body: some View {
        ZStack {
            Color.black.opacity(0.2)
                .ignoresSafeArea()
            
            VStack() {
                
            }
        }
        .background(.ultraThinMaterial)
    }
}
```

### **Breakdown:**  
- `ZStack`: Layers the background and UI elements.  
- `Color.black.opacity(0.2)`: A dark background with slight transparency.  
- `.ignoresSafeArea()`: Ensures the background extends to all edges.  
- `.background(.ultraThinMaterial)`: Provides a **blurred material effect**.  

---

## **Step 2: Creating the `CalcButton` Struct**  
Now, create the **button style** to ensure consistent button sizes and rounded edges.

### **Define a Custom Button Style**  

```swift
struct CalcButton: ButtonStyle {
    let color: Color
    var isWide: Bool = false
    
    func makeBody(configuration: Configuration) -> some View {
        configuration.label
            .font(.title)
            .frame(minWidth: isWide ? 130 : 60, minHeight: 60)
            .background(color)
            .foregroundColor(color == .gray ? .black : .white)
            .clipShape(RoundedRectangle(cornerRadius: 30))
            .scaleEffect(configuration.isPressed ? 0.9 : 1)
    }
}
```

### **Breakdown:**  
- **`ButtonStyle`**: Defines a **custom look** for buttons.  
- `.font(.title)`: Ensures text is large enough to be visible.  
- `.frame(minWidth: 130, minHeight: 60)`: 
  - **Standard buttons**: `60x60`  
  - **Wide buttons (like 0)**: `130x60`  
- `.background(color)`: Colors the button dynamically.  
- `.clipShape(RoundedRectangle(cornerRadius: 30))`: Rounds button edges.  
- `.scaleEffect(configuration.isPressed ? 0.9 : 1)`: Slightly shrinks buttons when clicked.

---

## **Step 3: Adding the Display Area**  
### **Insert the Calculator Display**  
Modify the `VStack` to include the **display section at the top**.

```swift
VStack {
    Text(displayValue)
        .font(.largeTitle)
        .foregroundColor(.white)
}
```

### **Breakdown:**  
- `Text(displayValue)`: Displays the current input or result.  
- `.font(.largeTitle)`: Increases text size for better visibility.  
- `.foregroundColor(.white)`: Ensures readability on a dark background.  

---

## **Step 4: Adding the Calculator Buttons**  
### **Structure the Buttons Using `VStack` and `HStack`**  

```swift
VStack(spacing: 10) {
    HStack(spacing: 10) {
        Button("AC") { }.buttonStyle(CalcButton(color: .gray))
        Button("+/-") { }.buttonStyle(CalcButton(color: .gray))
        Button("%") { }.buttonStyle(CalcButton(color: .gray))
        Button("÷") { }.buttonStyle(CalcButton(color: .orange))
    }

    HStack(spacing: 10) {
        Button("7") { }.buttonStyle(CalcButton(color: .secondary))
        Button("8") { }.buttonStyle(CalcButton(color: .secondary))
        Button("9") { }.buttonStyle(CalcButton(color: .secondary))
        Button("×") { }.buttonStyle(CalcButton(color: .orange))
    }

    HStack(spacing: 10) {
        Button("4") { }.buttonStyle(CalcButton(color: .secondary))
        Button("5") { }.buttonStyle(CalcButton(color: .secondary))
        Button("6") { }.buttonStyle(CalcButton(color: .secondary))
        Button("-") { }.buttonStyle(CalcButton(color: .orange))
    }

    HStack(spacing: 10) {
        Button("1") { }.buttonStyle(CalcButton(color: .secondary))
        Button("2") { }.buttonStyle(CalcButton(color: .secondary))
        Button("3") { }.buttonStyle(CalcButton(color: .secondary))
        Button("+") { }.buttonStyle(CalcButton(color: .orange))
    }

    HStack(spacing: 10) {
        Button("0") { }.buttonStyle(CalcButton(color: .secondary, isWide: true))
        Button(".") { }.buttonStyle(CalcButton(color: .secondary))
        Button("=") { }.buttonStyle(CalcButton(color: .orange))
    }
}
.padding()
```

### **Breakdown:**  
- **Rows Organized in `HStack`s:**  
  - **First row**: `"AC"`, `"+/-"`, `"%"`, `"÷"`  
  - **Second row**: `"7"`, `"8"`, `"9"`, `"×"`  
  - **Third row**: `"4"`, `"5"`, `"6"`, `"-"`  
  - **Fourth row**: `"1"`, `"2"`, `"3"`, `"+"`  
  - **Fifth row**: `"0"` (wide button), `"."`, `"="`  
- **Each button is styled using `CalcButton`** to ensure consistent design.  
- **Spacing ensures an aligned, structured layout.**  

---

## **Step 5: Putting Everything Together**  

### **Final Code**
```swift
import SwiftUI

struct ContentView: View {
    @State private var displayValue = "0"
    
    var body: some View {
        ZStack {
            Color.black.opacity(0.2)
                .ignoresSafeArea()
            
            VStack {
                Text(displayValue)
                    .font(.largeTitle)
                    .foregroundColor(.white)
                
                VStack(spacing: 10) {
                    HStack(spacing: 10) {
                        Button("AC") { }.buttonStyle(CalcButton(color: .gray))
                        Button("+/-") { }.buttonStyle(CalcButton(color: .gray))
                        Button("%") { }.buttonStyle(CalcButton(color: .gray))
                        Button("÷") { }.buttonStyle(CalcButton(color: .orange))
                    }
                    
                    HStack(spacing: 10) {
                        Button("7") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("8") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("9") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("×") { }.buttonStyle(CalcButton(color: .orange))
                    }
                    
                    HStack(spacing: 10) {
                        Button("4") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("5") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("6") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("-") { }.buttonStyle(CalcButton(color: .orange))
                    }
                    
                    HStack(spacing: 10) {
                        Button("1") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("2") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("3") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("+") { }.buttonStyle(CalcButton(color: .orange))
                    }
                    
                    HStack(spacing: 10) {
                        Button("0") { }.buttonStyle(CalcButton(color: .secondary, isWide: true))
                        Button(".") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("=") { }.buttonStyle(CalcButton(color: .orange))
                    }
                }
                .padding()
            }
        }
        .background(.ultraThinMaterial)
    }
}

struct CalcButton: ButtonStyle {
    let color: Color
    var isWide: Bool = false
    
    func makeBody(configuration: Configuration) -> some View {
        configuration.label
            .font(.title)
            .frame(minWidth: isWide ? 130 : 60, minHeight: 60)
            .background(color)
            .foregroundColor(color == .gray ? .black : .white)
            .clipShape(RoundedRectangle(cornerRadius: 30))
            .scaleEffect(configuration.isPressed ? 0.9 : 1)
    }
}
```
