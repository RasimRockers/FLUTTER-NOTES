## ☁️ Flutter Basics – Part 15: File Upload + Download (Firebase Storage)

---

### 💡 1. Why Firebase Storage?

Use Firebase Storage when:
- You want to store images, PDFs, or videos
- You need **secure cloud storage**
- You want users to **upload and retrieve media**
- Works great with Firebase Auth too

---

## 🔧 2. Setup Firebase Storage

### ✅ Step 1: Add to `pubspec.yaml`

```yaml
dependencies:
  firebase_core: ^2.20.0
  firebase_storage: ^11.6.5
  image_picker: ^1.0.4
```

```bash
flutter pub get
```

---

### ✅ Step 2: Initialize Firebase

Make sure this is in your `main.dart`:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

---

### 🔐 Step 3: Set Firebase Storage Rules (for testing)

In Firebase Console → Storage → Rules:

```js
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true; // ⚠️ Only for dev!
    }
  }
}
```

---

## 📤 3. Uploading Files to Firebase Storage

```dart
import 'dart:io';
import 'package:firebase_storage/firebase_storage.dart';
import 'package:image_picker/image_picker.dart';

Future<void> uploadImage() async {
  final picker = ImagePicker();
  final XFile? pickedImage = await picker.pickImage(source: ImageSource.gallery);

  if (pickedImage != null) {
    File file = File(pickedImage.path);

    // Unique path
    String fileName = 'images/${DateTime.now().millisecondsSinceEpoch}.jpg';

    // Upload
    try {
      final storageRef = FirebaseStorage.instance.ref().child(fileName);
      await storageRef.putFile(file);
      print('✅ Upload successful');
    } catch (e) {
      print('❌ Upload failed: $e');
    }
  }
}
```

---

## 📥 4. Get Download URL

After uploading:

```dart
final downloadUrl = await storageRef.getDownloadURL();
print('🌐 File URL: $downloadUrl');
```

Use this URL to:
- Show image in `Image.network(url)`
- Save in Firestore
- Share or preview the file

---

## 🖼️ 117. Display Image from Firebase Storage

```dart
Image.network(downloadUrl)
```

---

## 📄 5. Upload PDFs or Any Files

```dart
final result = await FilePicker.platform.pickFiles(
  type: FileType.custom,
  allowedExtensions: ['pdf'],
);

if (result != null) {
  File file = File(result.files.single.path!);
  final ref = FirebaseStorage.instance.ref('docs/${file.path.split('/').last}');
  await ref.putFile(file);
}
```

> Use the `file_picker` package for any files like `.pdf`, `.mp4`, `.docx`, etc.

---

## 📥6. Download Files

```dart
final ref = FirebaseStorage.instance.ref('images/example.jpg');
final data = await ref.getData(); // Returns Uint8List
```

Or use `getDownloadURL()` and download manually with `http` package if needed.

---

## 🧪 7. Tips & Best Practices

- ✅ Always compress images before upload (`image` package)
- ✅ Use folder paths like `users/userId/images/...`
- ✅ Set max upload file size in Firebase settings
- ❌ Don't allow open `read, write` in production

