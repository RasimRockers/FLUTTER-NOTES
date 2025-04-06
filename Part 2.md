
---

## 📘 Flutter Basics – Part 2

### 🧾 1. **Input & Forms**

To handle user input:

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Username',
    border: OutlineInputBorder(),
  ),
)
```

For forms:
```dart
final _formKey = GlobalKey<FormState>();

Form(
  key: _formKey,
  child: Column(
    children: [
      TextFormField(
        validator: (value) {
          if (value == null || value.isEmpty) {
            return 'Please enter text';
          }
          return null;
        },
      ),
      ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            // Process data
          }
        },
        child: Text('Submit'),
      ),
    ],
  ),
)
```

---

### 📋 2. **Lists and ListView**

To show a scrollable list:

```dart
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
  ],
)
```

Dynamic list:
```dart
List<String> items = ['One', 'Two', 'Three'];

ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(items[index]),
    );
  },
)
```

---

### 🖼️ 3. **Images**

```dart
Image.asset('assets/images/logo.png') // From assets

Image.network('https://example.com/image.jpg') // From URL
```

Add asset in `pubspec.yaml`:
```yaml
flutter:
  assets:
    - assets/images/logo.png
```

---

### 🧭 4. **Navigation and Routes**

**Named Routes:**

```dart
MaterialApp(
  initialRoute: '/',
  routes: {
    '/': (context) => HomePage(),
    '/second': (context) => SecondPage(),
  },
)
```

To navigate:
```dart
Navigator.pushNamed(context, '/second');
```

To go back:
```dart
Navigator.pop(context);
```

---

### 🎨 5. **Themes and Styling**

```dart
MaterialApp(
  theme: ThemeData(
    primarySwatch: Colors.teal,
    textTheme: TextTheme(
      bodyLarge: TextStyle(fontSize: 18),
    ),
  ),
)
```

Use colors:
```dart
Container(
  color: Colors.amber,
)
```

---

### ⏱️ 6. **Buttons**

```dart
ElevatedButton(
  onPressed: () {},
  child: Text('Click Me'),
)

TextButton(
  onPressed: () {},
  child: Text('Flat Button'),
)

IconButton(
  onPressed: () {},
  icon: Icon(Icons.favorite),
)
```

---

### 🔄 7. **State Management (Intro)**

Simple counter using `setState`:

```dart
int count = 0;

ElevatedButton(
  onPressed: () {
    setState(() {
      count++;
    });
  },
  child: Text('Increment'),
)
```

Popular state management options:
- `Provider` (official recommendation)
- `Riverpod`
- `GetX`
- `Bloc`

---

### ⌨️ 8. **User Input Handling**

```dart
TextEditingController controller = TextEditingController();

TextField(
  controller: controller,
)

ElevatedButton(
  onPressed: () {
    print(controller.text);
  },
  child: Text("Submit"),
)
```

---

### 🔁 9. **FutureBuilder & Async**

Used to handle asynchronous data like APIs:

```dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Data loaded!';
}

FutureBuilder<String>(
  future: fetchData(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator();
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text(snapshot.data!);
    }
  },
)
```

---

### 🧩 10. **Packages (pub.dev)**

Add dependencies in `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^0.14.0
```

Import and use:
```dart
import 'package:http/http.dart' as http;
```

Popular packages:
- `http` – for APIs
- `provider` – for state management
- `shared_preferences` – for local storage
- `image_picker` – for picking images

---



