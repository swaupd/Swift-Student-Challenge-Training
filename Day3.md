# **Step-by-Step Guide to Creating a macOS Calculator UI in SwiftUI**  

1. **Set up the base structure** (containers for layout).  
2. **Define the `CalcButton` struct** (to style buttons).  
3. **Add calculator buttons using `VStack` and `HStack`**.  
4. **Create the display section for showing calculations**.  

---

## **Step 1: Setting Up the Base Structure**  
1. Open Xcode and create a **new macOS SwiftUI project**.  
2. Open `ContentView.swift`.  
3. Define the main `ZStack` and `VStack` for structuring the UI.  

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
    }
}
```
### **Breakdown:**  
- `ZStack`: **Background layer** for overall styling.  
- `Color.black.opacity(0.2)`: **Dark semi-transparent background** for contrast.  
- `VStack()`: **Holds all UI elements vertically** (calculator screen + buttons).  

---

## **Step 2: Creating the `CalcButton` Struct**  
Before adding calculator buttons, **define a custom button style** for uniformity.

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
- `ButtonStyle`: **Creates a consistent style** for calculator buttons.  
- `color`: **Sets button color** (gray for function buttons, orange for operators).  
- `isWide`: **Handles the larger "0" button** by adjusting width.  
- `.font(.title)`: **Applies uniform text styling**.  
- `.background(color)`: **Defines button background color**.  
- `.clipShape(RoundedRectangle(cornerRadius: 30))`: **Rounds button edges**.  
- `.scaleEffect(configuration.isPressed ? 0.9 : 1)`: **Shrinks button slightly when pressed** for visual feedback.

---

## **Step 3: Adding the Display Section**  
Now, add the **calculator display** inside `VStack`.

```swift
HStack {
    Spacer()
    Text(displayValue)
        .font(.largeTitle)
        .foregroundColor(.white)
}
.padding()
```
### **Breakdown:**  
- `HStack`: **Aligns the text to the right** like a calculator screen.  
- `Spacer()`: **Pushes the text towards the right side**.  
- `Text(displayValue)`: **Displays the current calculation input/output**.  
- `.font(.largeTitle)`: **Sets text size** for readability.  
- `.foregroundColor(.white)`: **White text for contrast**.  

---

## **Step 4: Creating Calculator Buttons**  
Now, add the **button rows inside `VStack`** using `HStack` for proper alignment.

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
        Button("0") { }
            .buttonStyle(CalcButton(color: .secondary, isWide: true))
        Button(".") { }.buttonStyle(CalcButton(color: .secondary))
        Button("=") { }.buttonStyle(CalcButton(color: .orange))
    }
}
.padding()
```
### **Breakdown:**  
- `VStack(spacing: 10)`: **Organizes buttons into rows**.  
- `HStack(spacing: 10)`: **Aligns buttons horizontally** within each row.  
- `Button(action:)`: **Defines the button’s behavior** (functionality can be added later).  
- `.buttonStyle(CalcButton(color: .gray))`: **Applies the custom `CalcButton` style**.  
- **Row Arrangement:**
  - **Row 1:** `"AC"`, `"±"`, `"%"`, `"÷"`
  - **Row 2:** `"7"`, `"8"`, `"9"`, `"×"`
  - **Row 3:** `"4"`, `"5"`, `"6"`, `"-"`
  - **Row 4:** `"1"`, `"2"`, `"3"`, `"+"`
  - **Row 5:** `"0"` (wide button), `"."`, `"="`

---

## **Step 5: Finalizing the Layout**  
Now, insert everything into the `VStack` inside `ZStack`:

```swift
struct ContentView: View {
    @State private var displayValue = "0"

    var body: some View {
        ZStack {
            Color.black.opacity(0.2)
                .ignoresSafeArea()
            
            VStack() {
                HStack {
                    Spacer()
                    Text(displayValue)
                        .font(.largeTitle)
                        .foregroundColor(.white)
                }
                .padding()
                
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
                        Button("0") { }.buttonStyle(CalcButton(color: .secondary, isWide: true))
                        Button(".") { }.buttonStyle(CalcButton(color: .secondary))
                        Button("=") { }.buttonStyle(CalcButton(color: .orange))
                    }
                }
                .padding()
            }
        }
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
