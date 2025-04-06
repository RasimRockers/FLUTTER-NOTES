

## 🛠️ Flutter Basics – Part 8: Full Project (Login + Notes + Firestore)

---

### 1. **Project Plan: Notes App**
- Login (Email/Password)
- Add/Edit/Delete notes
- Firestore backend
- State management with Provider

---

### 2. **Folder Structure**

```
/lib
  /screens
    login_screen.dart
    home_screen.dart
    note_detail.dart
  /models
    note.dart
  /services
    auth_service.dart
    firestore_service.dart
  /widgets
    note_tile.dart
```

---

### 3. **Note Model**

```dart
class Note {
  final String id;
  final String title;
  final String content;

  Note({required this.id, required this.title, required this.content});

  factory Note.fromMap(Map<String, dynamic> data, String id) {
    return Note(
      id: id,
      title: data['title'],
      content: data['content'],
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'title': title,
      'content': content,
    };
  }
}
```

---

### 4. **AuthService (Firebase Auth)**

```dart
class AuthService {
  final _auth = FirebaseAuth.instance;

  Future<User?> signIn(String email, String pass) async {
    final result = await _auth.signInWithEmailAndPassword(email: email, password: pass);
    return result.user;
  }

  Future<void> logout() async => await _auth.signOut();
}
```

---

### 5. **FirestoreService**

```dart
class FirestoreService {
  final _notes = FirebaseFirestore.instance.collection('notes');

  Stream<List<Note>> getNotes() {
    return _notes.snapshots().map((snap) => snap.docs.map((doc) => Note.fromMap(doc.data(), doc.id)).toList());
  }

  Future<void> addNote(Note note) {
    return _notes.add(note.toMap());
  }

  Future<void> deleteNote(String id) {
    return _notes.doc(id).delete();
  }
}
```

---

### 6. **Home Screen**

```dart
@override
Widget build(BuildContext context) {
  return StreamBuilder<List<Note>>(
    stream: firestoreService.getNotes(),
    builder: (context, snapshot) {
      if (!snapshot.hasData) return CircularProgressIndicator();
      final notes = snapshot.data!;
      return ListView(
        children: notes.map((note) => NoteTile(note: note)).toList(),
      );
    },
  );
}
```

---

### 7. **Provider for User State**

```dart
ChangeNotifierProvider(
  create: (_) => AuthProvider(),
  child: MaterialApp(...),
)
```

Then access user state globally across screens.

---

### 8. **Final Touches**
- Add input validation
- Use `TextEditingController`
- Add `SnackBar` on add/delete
- Style with `GoogleFonts`, `Colors`, spacing

---

