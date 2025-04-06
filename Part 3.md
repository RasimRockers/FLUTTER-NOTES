
---

## 📘 Flutter Basics – Part 3

---

### 1. **Animations (Intro)**

#### Simple Animation using `AnimatedContainer`:

```dart
AnimatedContainer(
  duration: Duration(seconds: 1),
  width: isBig ? 200 : 100,
  height: isBig ? 200 : 100,
  color: isBig ? Colors.blue : Colors.red,
)
```

> Tip: Wrap it inside a `GestureDetector` to toggle the size on tap using `setState`.

#### Other animation widgets:
- `AnimatedOpacity`
- `AnimatedPositioned`
- `TweenAnimationBuilder`
- `Hero` – screen transition animation

---

### 2. **State Management (Slightly Advanced)**

#### Using `Provider` (officially recommended):

```yaml
dependencies:
  provider: ^6.0.5
```

```dart
class Counter with ChangeNotifier {
  int count = 0;
  void increment() {
    count++;
    notifyListeners();
  }
}
```

In `main.dart`:

```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => Counter(),
      child: MyApp(),
    ),
  );
}
```

Usage:

```dart
Text('${Provider.of<Counter>(context).count}')
```

---

### 3. **Local Storage using `shared_preferences`**

```yaml
dependencies:
  shared_preferences: ^2.2.1
```

Save data:
```dart
final prefs = await SharedPreferences.getInstance();
prefs.setString('username', 'flutter_user');
```

Read data:
```dart
String? name = prefs.getString('username');
```

---

### 4. **Calling APIs using `http`**

Add to `pubspec.yaml`:
```yaml
dependencies:
  http: ^0.14.0
```

Basic GET request:

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> fetchData() async {
  var response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
  var data = jsonDecode(response.body);
  print(data);
}
```

---

### 5. **Debugging Tips**

- Use **`debugPrint()`** instead of `print()` for better output.
- Use **Flutter DevTools** (inspect widget tree, memory, performance).
- Wrap widgets in `SafeArea`, `Padding`, `Expanded`, etc., to avoid layout overflow.
- Use **`flutter pub get`** and **`flutter clean`** if things break.
- Always check your **pubspec.yaml** spacing—it matters!

---

### 6. **Testing in Flutter**

Basic widget test:

```dart
testWidgets('MyWidget has a title', (WidgetTester tester) async {
  await tester.pumpWidget(MyWidget());
  expect(find.text('Hello'), findsOneWidget);
});
```

---

### 7. **Drawer Navigation**

```dart
Drawer(
  child: ListView(
    children: [
      DrawerHeader(child: Text('Menu')),
      ListTile(
        title: Text('Home'),
        onTap: () {
          Navigator.pushNamed(context, '/');
        },
      ),
    ],
  ),
)
```

Use it in `Scaffold`:
```dart
Scaffold(
  appBar: AppBar(title: Text('App')),
  drawer: Drawer(...),
  body: ...
)
```

---

### 8. **Custom Widgets**

Make reusable widgets for clean code:

```dart
class MyButton extends StatelessWidget {
  final String label;
  final VoidCallback onPressed;

  MyButton({required this.label, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(label),
    );
  }
}
```

---

### 9. **Responsive UI (Device-friendly)**

Use `MediaQuery`:

```dart
var width = MediaQuery.of(context).size.width;
```

Use `LayoutBuilder` or `Flexible` to adapt UI.

---

### 10. **Flutter DevTools Shortcuts**

| Command                | Action                  |
|------------------------|--------------------------|
| `r`                    | Hot reload              |
| `R`                    | Hot restart             |
| `flutter doctor`       | Check setup             |
| `flutter run`          | Run app on device       |
| `flutter clean`        | Clear build cache       |

---

