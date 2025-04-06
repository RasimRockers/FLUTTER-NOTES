

---

## 📘 Flutter Basics 

### 📌 What is Flutter?
- Flutter is an **open-source UI toolkit** by Google.
- Used for **building cross-platform apps** (Android, iOS, Web, Desktop) using a **single codebase**.
- Uses the **Dart** programming language.

---

### 🛠️ Flutter Installation (Quick Steps)
1. Install Flutter SDK: [https://flutter.dev/docs/get-started/install](https://flutter.dev/docs/get-started/install)
2. Install Android Studio / VS Code.
3. Set up an emulator or connect a real device.
4. Run:
   ```bash
   flutter doctor
   ```
   to check everything is set up properly.

---

### 🧱 Flutter App Structure

```plaintext
/lib
  └── main.dart      --> Entry point of the app
```

- `main()` function runs the app.
- `runApp(MyApp())` is the root widget of your app.

---

### 📄 Basic Flutter Code Example

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My App',
      home: Scaffold(
        appBar: AppBar(title: Text('Hello Flutter')),
        body: Center(child: Text('Welcome to Flutter!')),
      ),
    );
  }
}
```

---

### 🔤 Common Widgets

| Widget        | Description                         |
|---------------|-------------------------------------|
| `Text`        | Display text                        |
| `Container`   | Box with padding, margin, etc.      |
| `Row`/`Column`| Layout children horizontally/vert.  |
| `Center`      | Centers its child                   |
| `Scaffold`    | Basic app layout (appBar, body)     |
| `AppBar`      | Top bar of the app                  |
| `TextField`   | User input field                    |
| `ElevatedButton` | Button with elevation           |

---

### ⚙️ Hot Reload vs Hot Restart
- **Hot Reload**: Updates UI while keeping app state.
- **Hot Restart**: Rebuilds app from scratch.

---

###  Stateful vs Stateless Widget

- **StatelessWidget**: UI does **not** change.
- **StatefulWidget**: UI **can change** dynamically.

```dart
class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  int counter = 0;

  void increment() {
    setState(() {
      counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Text('Count: $counter');
  }
}
```

---

### 🎨 Styling Widgets

Use `TextStyle`, `BoxDecoration`, and other properties:

```dart
Text(
  'Styled Text',
  style: TextStyle(fontSize: 20, color: Colors.blue),
)
```

---

### 🔗 Navigation (Basic)

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondPage()),
);
```

---

Let me know if you'd like a downloadable version (like PDF), or if you want to dive into more topics like Firebase, APIs, or animations!
