
---

## 🗂️ Flutter Basics – Part 13: SQLite Database in Flutter

---

### 1. **Why SQLite?**

Use SQLite when:
- You need **structured tables** with rows & columns  
- You want **offline storage with querying power**  
- You need to **filter/search/sort** big data locally  
- You don’t need Firebase or the Internet

---

## 2. Add Dependencies

In your `pubspec.yaml`:

```yaml
dependencies:
  sqflite: ^2.3.0
  path: ^1.9.0
```

Then:

```bash
flutter pub get
```

---

## 3. Set Up Database

### ✅ Step 1: Create DB Helper Class

```dart
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

class DBHelper {
  static Database? _db;

  static Future<Database> get db async {
    if (_db != null) return _db!;
    _db = await initDb();
    return _db!;
  }

  static Future<Database> initDb() async {
    String path = join(await getDatabasesPath(), 'notes.db');
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE notes (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            title TEXT,
            content TEXT
          )
        ''');
      },
    );
  }
}
```

---

## 4. CRUD Operations (Create, Read, Update, Delete)

### ➕ Add Note

```dart
Future<void> addNote(String title, String content) async {
  final db = await DBHelper.db;
  await db.insert('notes', {'title': title, 'content': content});
}
```

### 📥 Read Notes

```dart
Future<List<Map<String, dynamic>>> getNotes() async {
  final db = await DBHelper.db;
  return await db.query('notes');
}
```

### 📝 Update Note

```dart
Future<void> updateNote(int id, String title, String content) async {
  final db = await DBHelper.db;
  await db.update(
    'notes',
    {'title': title, 'content': content},
    where: 'id = ?',
    whereArgs: [id],
  );
}
```

### ❌ Delete Note

```dart
Future<void> deleteNote(int id) async {
  final db = await DBHelper.db;
  await db.delete('notes', where: 'id = ?', whereArgs: [id]);
}
```

---

## 5. Model Class (Optional but Recommended)

```dart
class Note {
  int? id;
  String title;
  String content;

  Note({this.id, required this.title, required this.content});

  Map<String, dynamic> toMap() => {
        'id': id,
        'title': title,
        'content': content,
      };

  factory Note.fromMap(Map<String, dynamic> map) => Note(
        id: map['id'],
        title: map['title'],
        content: map['content'],
      );
}
```

---

## 6. UI Example to Display Notes

```dart
FutureBuilder(
  future: getNotes(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.done) {
      final notes = snapshot.data as List<Map<String, dynamic>>;
      return ListView.builder(
        itemCount: notes.length,
        itemBuilder: (_, index) {
          final note = notes[index];
          return ListTile(
            title: Text(note['title']),
            subtitle: Text(note['content']),
          );
        },
      );
    }
    return CircularProgressIndicator();
  },
)
```

---

### 7. Best Practices

- Use `model.toMap()` and `Note.fromMap()` for clean conversions  
- Keep your DB logic in one file (e.g. `db_helper.dart`)  
- Avoid large DB operations on main thread — use `async/await` always  
- Prefer `FutureBuilder` or `setState()` to update UI

---

