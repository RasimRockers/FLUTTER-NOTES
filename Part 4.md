
---

## 📘 Flutter Basics – Part 4: Advanced Concepts

---

### 1. **Checkboxes & Switches**

#### Checkbox:

```dart
bool isChecked = false;

Checkbox(
  value: isChecked,
  onChanged: (value) {
    setState(() {
      isChecked = value!;
    });
  },
)
```

#### Switch:

```dart
bool isDark = false;

Switch(
  value: isDark,
  onChanged: (value) {
    setState(() {
      isDark = value;
    });
  },
)
```

---

### 2. **Login UI Example**

```dart
TextField(
  decoration: InputDecoration(labelText: 'Email'),
)

TextField(
  obscureText: true,
  decoration: InputDecoration(labelText: 'Password'),
)

ElevatedButton(
  onPressed: () {
    // Validate & login
  },
  child: Text('Login'),
)
```

Use `TextEditingController` to get user input.

---

### 3. **DatePicker & TimePicker**

#### Date Picker:

```dart
DateTime? pickedDate;

ElevatedButton(
  onPressed: () async {
    pickedDate = await showDatePicker(
      context: context,
      initialDate: DateTime.now(),
      firstDate: DateTime(2000),
      lastDate: DateTime(2100),
    );
    setState(() {});
  },
  child: Text('Pick Date'),
)
```

#### Time Picker:

```dart
TimeOfDay? pickedTime;

ElevatedButton(
  onPressed: () async {
    pickedTime = await showTimePicker(
      context: context,
      initialTime: TimeOfDay.now(),
    );
    setState(() {});
  },
  child: Text('Pick Time'),
)
```

---

### 4. **Multi-Screen Navigation (Send/Receive Data)**

#### Send data to another screen:

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => SecondScreen(data: 'Hello'),
  ),
);
```

#### Receive it in `SecondScreen`:

```dart
class SecondScreen extends StatelessWidget {
  final String data;
  SecondScreen({required this.data});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second')),
      body: Center(child: Text(data)),
    );
  }
}
```

---

### 5. **Working with JSON**

#### Sample JSON:
```json
{
  "id": 1,
  "title": "Flutter is awesome"
}
```

#### Dart Model:
```dart
class Post {
  final int id;
  final String title;

  Post({required this.id, required this.title});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(id: json['id'], title: json['title']);
  }
}
```

Use with HTTP response:

```dart
final jsonData = jsonDecode(response.body);
Post post = Post.fromJson(jsonData[0]);
```

---

### 6. **MediaQuery – Responsive UI**

```dart
double width = MediaQuery.of(context).size.width;
double height = MediaQuery.of(context).size.height;

if (width < 600) {
  // Mobile layout
} else {
  // Tablet/Desktop layout
}
```

---

### 7. **SnackBar & AlertDialog**

#### SnackBar:
```dart
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(content: Text('Item added!')),
);
```

#### AlertDialog:
```dart
showDialog(
  context: context,
  builder: (context) => AlertDialog(
    title: Text('Are you sure?'),
    content: Text('Do you want to delete this?'),
    actions: [
      TextButton(onPressed: () => Navigator.pop(context), child: Text('Cancel')),
      TextButton(onPressed: () {
        // Do delete
        Navigator.pop(context);
      }, child: Text('Yes')),
    ],
  ),
);
```

---

### 8. **Image Picker**

```yaml
dependencies:
  image_picker: ^1.0.0
```

```dart
final picker = ImagePicker();

Future<void> pickImage() async {
  final image = await picker.pickImage(source: ImageSource.gallery);
  if (image != null) {
    setState(() {
      _imageFile = File(image.path);
    });
  }
}
```

---

### 9. **Clean Code Tips**

- ✅ Use custom widgets to avoid long build methods
- 🔄 Avoid repeated `setState` logic – extract into methods
- 📁 Use folders like `screens`, `widgets`, `models`
- 💬 Use meaningful widget names and variable names

---

### 10. **Publishing Your App**

To build a release APK:
```bash
flutter build apk --release
```

To run on real device:
```bash
flutter run
```

To build for web:
```bash
flutter build web
```

To upload to Play Store: [Flutter docs - Android Release](https://docs.flutter.dev/deployment/android)

---

