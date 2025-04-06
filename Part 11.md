
---

## 💾 Flutter Basics – Part 11: Local Storage (Hive & Shared Preferences)

---

### 1. **Why Local Storage?**

Use local storage when you want to:
- Save user settings (theme, login, etc.)
- Store app data offline (like notes)
- Cache API data
- Save tokens or flags (`isLoggedIn = true`)

---

### 2. **Two Common Methods**

| Tool                | Use Case                            |
|---------------------|-------------------------------------|
| **SharedPreferences** | Small key-value pairs (e.g., login flag, theme) |
| **Hive**              | Structured offline storage (like a local database) |

---

## 3. Using SharedPreferences

### 📦 Add Dependency

```yaml
dependencies:
  shared_preferences: ^2.2.2
```

### 📥 Save & Read Data

```dart
import 'package:shared_preferences/shared_preferences.dart';

Future<void> saveLoginStatus(bool isLoggedIn) async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  await prefs.setBool('isLoggedIn', isLoggedIn);
}

Future<bool> getLoginStatus() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  return prefs.getBool('isLoggedIn') ?? false;
}
```

### 🔁 Other Data Types

- `prefs.setInt('counter', 1)`
- `prefs.setString('username', 'john')`
- `prefs.setDouble('volume', 0.8)`

---

## 4. Using Hive (Offline DB)

### 📦 Add Dependencies

```yaml
dependencies:
  hive: ^2.2.3
  hive_flutter: ^1.1.0

dev_dependencies:
  hive_generator: ^2.0.1
  build_runner: ^2.4.6
```

### 🧱 Initialize Hive

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Hive.initFlutter();
  await Hive.openBox('myBox');
  runApp(MyApp());
}
```

---

### 💽 Save & Read Data in Hive

```dart
var box = Hive.box('myBox');

// Save data
box.put('username', 'flutterDev');

// Read data
String? name = box.get('username');

// Delete data
box.delete('username');
```

---

### 5. Use Hive for List of Notes (Like Database)

You can create a **custom model**:

#### ✅ Step 1: Create model class
```dart
@HiveType(typeId: 0)
class Note extends HiveObject {
  @HiveField(0)
  String title;

  @HiveField(1)
  String content;

  Note({required this.title, required this.content});
}
```

#### ✅ Step 2: Generate adapter

```bash
flutter pub run build_runner build
```

#### ✅ Step 3: Register it

```dart
Hive.registerAdapter(NoteAdapter());
await Hive.openBox<Note>('notes');
```

#### ✅ Step 4: Save a note

```dart
final noteBox = Hive.box<Note>('notes');
noteBox.add(Note(title: 'Note 1', content: 'Hello Hive!'));
```

---

## 6. Best Practices

- ✅ Use `SharedPreferences` for small, fast config values  
- ✅ Use `Hive` for offline-first apps (like Notes, Todos)  
- ❌ Don't store sensitive data (like passwords) here — use secure storage instead

---

### 🧰 Bonus: Secure Storage

For passwords/tokens, use:

```yaml
flutter_secure_storage: ^9.0.0
```

---

