
---

##  Flutter Basics – Part 12: Push Notifications (Firebase Messaging)

---

### 1. **What Are Push Notifications?**

Push notifications allow your app to receive alerts from **Firebase Cloud Messaging (FCM)** — even if the app is:
- In the background  
- Closed  
- Or running

Used for:
- Reminders  
- Chat messages  
- Promotions  
- App updates

---

### 2. **Setup Firebase for Notifications**

#### ✅ Step 1: Add Firebase to Flutter
> Already done in Part 4 (Firebase Auth)? If not:

- Use the [Firebase Console](https://console.firebase.google.com/)  
- Add project ➜ Add Android/iOS App  
- Download `google-services.json` (for Android) or `GoogleService-Info.plist` (for iOS)

#### ✅ Step 2: Add Required Packages

```yaml
dependencies:
  firebase_core: ^2.20.0
  firebase_messaging: ^14.6.5
```

Then run:
```bash
flutter pub get
```

---

### 3. **Initialize Firebase & Messaging**

In your `main.dart`:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
  runApp(MyApp());
}

Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  await Firebase.initializeApp();
  print('🔙 Message in background: ${message.notification?.title}');
}
```

---

### 4. **Request Notification Permission (iOS & Android 13+)**

```dart
FirebaseMessaging messaging = FirebaseMessaging.instance;

NotificationSettings settings = await messaging.requestPermission(
  alert: true,
  badge: true,
  sound: true,
);
```

---

### 5. **Handle Incoming Notifications**

```dart
FirebaseMessaging.onMessage.listen((RemoteMessage message) {
  print('📨 New notification: ${message.notification?.title}');
});

FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
  print('📲 Notification clicked!');
});
```

---

### 6. **Get FCM Token (Device ID)**

This token is unique per device — send it to your backend if needed.

```dart
String? token = await FirebaseMessaging.instance.getToken();
print('FCM Token: $token');
```

Use this token in tools like [Postman](https://www.postman.com/) or Firebase Console to **test notifications**.

---

### 7. **Send Notification via Firebase Console**

1. Go to Firebase Console  
2. Cloud Messaging → Send Your First Message  
3. Add Title & Body  
4. Select your app and target device token  
5. Done ✅

---

### 8. **Customize Notification UI (Optional)**

Use [flutter_local_notifications](https://pub.dev/packages/flutter_local_notifications) to:
- Show custom dialogs
- Display images
- Handle tap events better

---

### 9. **Android Setup Notes**

- Ensure these permissions are in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```

- And in `<application>` tag:
```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_channel_id"
    android:value="high_importance_channel"/>
```

---

