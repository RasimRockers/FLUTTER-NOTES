
---

## 📘 Flutter Basics – Part 5: Firebase (Auth, Firestore, Realtime DB)

---

### 1. **Setting Up Firebase with Flutter**

#### Step-by-Step:
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Add Android/iOS app (add package name, download `google-services.json`)
4. Place `google-services.json` into `android/app/`
5. Update `android/build.gradle`:
```gradle
classpath 'com.google.gms:google-services:4.3.15'
```
6. Update `android/app/build.gradle`:
```gradle
apply plugin: 'com.google.gms.google-services'
```

---

### 2. **Add Firebase Packages**

In `pubspec.yaml`:

```yaml
dependencies:
  firebase_core: ^2.24.0
  firebase_auth: ^4.17.6
  cloud_firestore: ^4.13.4
```

Then run:

```bash
flutter pub get
```

---

### 3. **Initialize Firebase**

In `main.dart`:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

---

### 4. **Firebase Authentication (Email/Password)**

#### Sign Up:

```dart
await FirebaseAuth.instance.createUserWithEmailAndPassword(
  email: 'test@example.com',
  password: 'password123',
);
```

#### Login:

```dart
await FirebaseAuth.instance.signInWithEmailAndPassword(
  email: 'test@example.com',
  password: 'password123',
);
```

#### Logout:

```dart
await FirebaseAuth.instance.signOut();
```

---

### 5. **Check Login State**

```dart
User? user = FirebaseAuth.instance.currentUser;

if (user != null) {
  print('Logged in as: ${user.email}');
} else {
  print('Not logged in');
}
```

---

### 6. **Cloud Firestore (NoSQL Database)**

#### Add data:

```dart
await FirebaseFirestore.instance.collection('notes').add({
  'title': 'First Note',
  'content': 'This is stored in Firestore',
  'timestamp': Timestamp.now(),
});
```

#### Read data:

```dart
StreamBuilder(
  stream: FirebaseFirestore.instance.collection('notes').snapshots(),
  builder: (context, snapshot) {
    if (!snapshot.hasData) return CircularProgressIndicator();
    final docs = snapshot.data!.docs;
    return ListView.builder(
      itemCount: docs.length,
      itemBuilder: (context, index) => ListTile(
        title: Text(docs[index]['title']),
      ),
    );
  },
)
```

#### Delete data:

```dart
await FirebaseFirestore.instance.collection('notes').doc(docId).delete();
```

---

### 7. **Realtime Database (Alternative to Firestore)**

Use only **if you prefer structured JSON-style data.**  
Enable Realtime DB from Firebase Console.

```yaml
dependencies:
  firebase_database: ^10.4.4
```

Write data:
```dart
DatabaseReference ref = FirebaseDatabase.instance.ref('messages/');
await ref.set({'user': 'test', 'message': 'Hello Firebase'});
```

Read data:
```dart
ref.onValue.listen((event) {
  final data = event.snapshot.value;
  print(data);
});
```

---

### 8. **Firebase Rules (Important!)**

Default Firestore rule (dev mode):
```js
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true; // Not for production!
    }
  }
}
```

Set proper rules before deploying!

---

### 9. **Securing Auth Screens**

Show login or home based on user state:

```dart
Widget build(BuildContext context) {
  return StreamBuilder<User?>(
    stream: FirebaseAuth.instance.authStateChanges(),
    builder: (context, snapshot) {
      if (snapshot.connectionState == ConnectionState.waiting)
        return CircularProgressIndicator();
      if (snapshot.hasData)
        return HomeScreen();
      else
        return LoginScreen();
    },
  );
}
```

---

### 10. **Common Firebase Errors**

| Error                                | Reason                               |
|-------------------------------------|--------------------------------------|
| `MissingPluginException`            | Forgot `await Firebase.initializeApp()` |
| `FirebaseException: PERMISSION_DENIED` | Firestore rules block access         |
| `PlatformException: network_error`  | No internet connection               |
| `duplicate plugin` error            | Version mismatch – check Gradle/dependencies |

---

