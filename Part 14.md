
---

## 📷 Flutter Basics – Part 14: Camera & Image Picker

---

### 1. Why Use Camera or Image Picker?

You can:
- Let users **capture photos or videos**
- Pick media from **gallery**
- Use for profiles, document uploads, selfie apps, etc.

---

## 2. Add Required Package

In `pubspec.yaml`:

```yaml
dependencies:
  image_picker: ^1.0.4
```

Then:

```bash
flutter pub get
```

---

## 3. Android & iOS Permissions

### 📱 Android

Add this in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

Also add in `<application>`:

```xml
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="${applicationId}.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/provider_paths"/>
</provider>
```

And create `android/app/src/main/res/xml/provider_paths.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
  <external-path name="my_images" path="."/>
</paths>
```

---

### 🍏 iOS

Add to `Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>This app needs camera access</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>This app needs photo library access</string>
```

---

## 4. Basic Usage (Camera & Gallery)

```dart
import 'package:image_picker/image_picker.dart';

final ImagePicker picker = ImagePicker();

Future<void> pickImageFromGallery() async {
  final XFile? image = await picker.pickImage(source: ImageSource.gallery);
  if (image != null) {
    print('📁 Picked: ${image.path}');
  }
}

Future<void> pickImageFromCamera() async {
  final XFile? image = await picker.pickImage(source: ImageSource.camera);
  if (image != null) {
    print('📷 Captured: ${image.path}');
  }
}
```

---

## 5. Display Image in App

```dart
File? _imageFile;

void _pickFromGallery() async {
  final XFile? picked = await picker.pickImage(source: ImageSource.gallery);
  if (picked != null) {
    setState(() => _imageFile = File(picked.path));
  }
}

...

_imageFile != null
  ? Image.file(_imageFile!)
  : Text("No image selected.")
```

---

## 6. Other Features

| Feature           | Code Example                                        |
|------------------|-----------------------------------------------------|
| 📽️ Pick Video     | `picker.pickVideo(source: ImageSource.camera)`     |
| 📏 Limit Size     | `maxWidth`, `maxHeight` in `pickImage()`           |
| 🗂️ Multiple Files | Use [multi_image_picker](https://pub.dev/packages/multi_image_picker2) |
| 🔁 Crop Image     | Use `image_cropper` package                         |

---

