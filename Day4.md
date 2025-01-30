# **Implementing Calculation Functions in the macOS Calculator**  

---

## **1. Storing the Full Expression**  

### **State Variables**
```swift
@State private var expression: String = "0"
```
- `expression`: Stores the complete mathematical input as a string.

---

## **2. Handling Number Input**  

Users can tap digits (`0-9`) to **append them** to the expression.  

### **Appending Numbers**
```swift
func appendCharacter(_ char: String) {
    if expression == "0" {
        expression = char
    } else {
        expression += char
    }
}
```
- If the current expression is `"0"`, it is replaced with the new number.  
- Otherwise, the number is appended to the existing input.

---

## **3. Handling Operators**  

When users tap an operator (`+`, `-`, `×`, `÷`), it should only be added **if the last character is not already an operator**.  

### **Appending Operators**
```swift
func appendOperator(_ op: String) {
    guard let lastChar = expression.last, !"+-×÷.".contains(lastChar) else { return }
    expression += op
}
```
- Prevents **duplicate operators** from being entered consecutively.  
- Ensures that operators cannot follow a decimal point directly.  

---

## **4. Handling Decimal Points (`.`) Correctly**  

The calculator must support **floating-point numbers** while preventing **multiple decimal points** in a single number.  

### **Appending a Decimal Point**
```swift
func appendDecimal() {
    let components = expression.split(whereSeparator: { "+-×÷".contains($0) })
    if let lastComponent = components.last, !lastComponent.contains(".") {
        expression += "."
    }
}
```
- Splits the expression into individual numbers using `+`, `-`, `×`, `÷` as separators.  
- Checks if the **current number** already contains a decimal before adding another.  

---

## **5. Evaluating the Expression**  

When the `=` button is pressed, the calculator **computes the final result**.  

### **Expression Evaluation**
```swift
func evaluateExpression() {
    let formattedExpression = expression
        .replacingOccurrences(of: "×", with: "*")
        .replacingOccurrences(of: "÷", with: "/")

    let expressionNS = NSExpression(format: formattedExpression)
    if let result = expressionNS.expressionValue(with: nil, context: nil) as? NSNumber {
        expression = formatResult(result.doubleValue)
    }
}
```
- Replaces **SwiftUI symbols** (`×` → `*`, `÷` → `/`) to make the expression compatible with `NSExpression`.  
- Uses `NSExpression` to evaluate the **entire mathematical expression**.  
- Ensures that **division always produces floating-point results**.  
- Calls `formatResult()` to clean up the output.

---

## **6. Formatting the Result**  

The result must be **formatted to a maximum of 4 decimal places** while removing unnecessary `.0` values.  

### **Formatting Function**
```swift
func formatResult(_ result: Double) -> String {
    let formatter = NumberFormatter()
    formatter.minimumFractionDigits = 0
    formatter.maximumFractionDigits = 4
    return formatter.string(from: NSNumber(value: result)) ?? "\(result)"
}
```
- **Limits decimals to 4 places** (e.g., `5.123456` → `5.1234`).  
- **Removes trailing `.0`** (e.g., `4.0` → `4`).  

---

## **7. Implementing Backspace (`delete.left`)**  

Users should be able to **delete characters** from the expression.  

### **Backspace Function**
```swift
func backspace() {
    expression = expression.count > 1 ? String(expression.dropLast()) : "0"
}
```
- Removes the **last character** from the expression.  
- If the expression becomes empty, it resets to `"0"`.  

---

# **Final Code**
```swift
import SwiftUI

struct ContentView: View {
    @State private var expression: String = "0"

    var body: some View {
        ZStack {
            Color.black.opacity(0.2).ignoresSafeArea()
            VStack {
                Text(expression)
                    .font(.largeTitle)
                    .foregroundColor(.white)

                VStack(spacing: 10) {
                    HStack(spacing: 10) {
                        Button(action: { backspace() }) {
                            Image(systemName: "delete.left")
                        }.buttonStyle(CalcButton(color: .gray))
                        Button("+/-") { appendOperator("+/-") }.buttonStyle(CalcButton(color: .gray))
                        Button("%") { appendOperator("%") }.buttonStyle(CalcButton(color: .gray))
                        Button("÷") { appendOperator("÷") }.buttonStyle(CalcButton(color: .orange))
                    }
                    HStack(spacing: 10) {
                        Button("7") { appendCharacter("7") }.buttonStyle(CalcButton(color: .secondary))
                        Button("8") { appendCharacter("8") }.buttonStyle(CalcButton(color: .secondary))
                        Button("9") { appendCharacter("9") }.buttonStyle(CalcButton(color: .secondary))
                        Button("×") { appendOperator("×") }.buttonStyle(CalcButton(color: .orange))
                    }
                    HStack(spacing: 10) {
                        Button("4") { appendCharacter("4") }.buttonStyle(CalcButton(color: .secondary))
                        Button("5") { appendCharacter("5") }.buttonStyle(CalcButton(color: .secondary))
                        Button("6") { appendCharacter("6") }.buttonStyle(CalcButton(color: .secondary))
                        Button("-") { appendOperator("-") }.buttonStyle(CalcButton(color: .orange))
                    }
                    HStack(spacing: 10) {
                        Button("1") { appendCharacter("1") }.buttonStyle(CalcButton(color: .secondary))
                        Button("2") { appendCharacter("2") }.buttonStyle(CalcButton(color: .secondary))
                        Button("3") { appendCharacter("3") }.buttonStyle(CalcButton(color: .secondary))
                        Button("+") { appendOperator("+") }.buttonStyle(CalcButton(color: .orange))
                    }
                    HStack(spacing: 10) {
                        Button("0") { appendCharacter("0") }.buttonStyle(CalcButton(color: .secondary, isWide: true))
                        Button(".") { appendDecimal() }.buttonStyle(CalcButton(color: .secondary))
                        Button("=") { evaluateExpression() }.buttonStyle(CalcButton(color: .orange))
                    }
                }
            }
        }
    }
}
```

---
