# **SwiftUI**

---

## **Step 1: Setting Up the Background**

The `ZStack` is used to layer elements, starting with a background color.

```swift
ZStack {
    Color.blue
        .edgesIgnoringSafeArea(.all)
}
```

### **Explanation:**
- **`ZStack`:** Layers views on top of one another.  
- **`Color.blue`:** Fills the screen with a blue background.  
- **`.edgesIgnoringSafeArea(.all)`:** Ensures the color covers the entire screen, including safe areas (e.g., around the notch).

---

## **Step 2: Adding a Profile Image**

Inside the `ZStack`, a profile image is displayed with a circular background.

```swift
ZStack {
    Circle()
        .fill(Color.white)
        .frame(width: 150, height: 150)
        .padding()

    Image(systemName: "person.fill")
        .resizable()
        .scaledToFit()
        .frame(width: 100, height: 100)
        .foregroundColor(.gray)
}
```

### **Explanation:**
1. **`Circle`:** A SwiftUI shape that creates a circle.  
   - **`.fill(Color.white)`:** Fills the circle with white.  
   - **`.frame(width: height:)`:** Sets the circle’s size.  
2. **`Image(systemName:)`:** Loads an icon from Apple’s SF Symbols library.  
   - **`resizable()`:** Allows the icon to resize.  
   - **`scaledToFit()`:** Maintains the icon's aspect ratio.  
   - **`foregroundColor(.gray)`:** Changes the icon’s color.

---

## **Step 3: Adding Profile Information**

Below the profile image, use a `VStack` to display the name and description.

```swift
VStack(spacing: 8) {
    Text("Swarit Upadhyay")
        .font(.title)
        .bold()
        .foregroundColor(.white)
    
    Text("iOS Developer | Swift Enthusiast")
        .font(.subheadline)
        .foregroundColor(.white.opacity(0.8))
}
```

### **Explanation:**
- **`VStack`:** Aligns its child views vertically.  
  - **`spacing:`** Adjusts spacing between elements.  
- **`Text`:** Displays a string of text.  
  - **Modifiers Used:**  
    - `.font()`: Changes the font size (`title`, `subheadline`, etc.).  
    - `.foregroundColor()`: Colors the text. `.opacity()` adjusts transparency.  
    - `.bold()`: Makes text bold.

---

## **Step 4: Adding Buttons for Interaction**

Use an `HStack` to align "Message" and "Follow" buttons horizontally.

```swift
HStack(spacing: 15) {
    Button(action: {
        print("Message tapped!")
    }) {
        Text("Message")
            .font(.headline)
            .padding()
            .frame(width: 120)
            .background(Color.white)
            .cornerRadius(10)
            .foregroundColor(.blue)
    }
    
    Button(action: {
        print("Follow tapped!")
    }) {
        Text("Follow")
            .font(.headline)
            .padding()
            .frame(width: 120)
            .background(Color.white)
            .cornerRadius(10)
            .foregroundColor(.blue)
    }
}
```

### **Explanation:**
- **`HStack`:** Aligns child views horizontally.  
  - **`spacing:`** Specifies the gap between the buttons.  
- **`Button(action:label:)`:** Creates an interactive button.  
  - **`action:`** Closure that runs when the button is tapped.  
  - **`label:`** Defines the button’s content (e.g., `Text`).  
- **Modifiers Used:**  
  1. **`font(.headline)`**: Styles the button text.  
  2. **`padding()`**: Adds internal spacing around the text.  
  3. **`frame(width:height:)`**: Sets the button’s dimensions.  
  4. **`background(Color.white)`**: Adds a white background to the button.  
  5. **`cornerRadius(10)`**: Rounds the corners of the button.  
  6. **`foregroundColor(.blue)`**: Changes the button text color to blue.

---

## **Step 5: Adding Padding and Alignment**

Enhance spacing and alignment using `Spacer()` and `.padding()` modifiers.

### Spacer and Padding:
```swift
Spacer() // Pushes buttons downward
.padding(.bottom, 40) // Adds space from the bottom edge
```

### **Explanation:**
1. **`Spacer()`:** A flexible space that pushes other views to the edges of the containing stack.  
2. **`.padding(.bottom, 40)`:** Adds extra space below the `HStack` for visual balance.

---

## **Full Code**
Here is the complete code integrating everything step-by-step:

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        ZStack {
            
            Color.blue
                .edgesIgnoringSafeArea(.all)
            
            VStack(spacing: 20) {
                
                // Profile Image
                ZStack {
                    Circle()
                        .fill(Color.white)
                        .frame(width: 150, height: 150)
                        .padding()
                    
                    Image(systemName: "person.fill")
                        .resizable()
                        .scaledToFit()
                        .frame(width: 100, height: 100)
                        .foregroundColor(.gray)
                }
                
                // Profile Details
                VStack(spacing: 8) {
                    Text("Swarit Upadhyay")
                        .font(.title)
                        .bold()
                        .foregroundColor(.white)
                    
                    Text("iOS Developer | Swift Enthusiast")
                        .font(.subheadline)
                        .foregroundColor(.white.opacity(0.8))
                }
                
                Spacer()
                
                // Buttons
                HStack(spacing: 15) {
                    Button(action: {
                        print("Message tapped!")
                    }) {
                        Text("Message")
                            .font(.headline)
                            .padding()
                            .frame(width: 120)
                            .background(Color.white)
                            .cornerRadius(10)
                            .foregroundColor(.blue)
                    }
                    
                    Button(action: {
                        print("Follow tapped!")
                    }) {
                        Text("Follow")
                            .font(.headline)
                            .padding()
                            .frame(width: 120)
                            .background(Color.white)
                            .cornerRadius(10)
                            .foregroundColor(.blue)
                    }
                }
                .padding(.bottom, 40)
            }
            .padding()
        }
    }
}

```

---

## **Key Takeaways**

1. **ZStack:** Layers background and content together.  
2. **VStack:** Aligns profile picture, name, and description vertically.  
3. **HStack:** Positions buttons side by side.  
4. **Modifiers:** Enhance views with styling (e.g., padding, font, color).  
5. **Reusable Patterns:** Combining stacks and modifiers makes UI design intuitive and efficient.  


