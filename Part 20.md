## 🎨 Flutter Basics – Part 20: Themes & Dark Mode

---

### 💡 1. Why Use Themes?

- Maintain **consistent UI design**
- Update **all styles** from one place
- Support **light & dark** modes easily

---

## 🛠️ 2. Define a Theme in `MaterialApp`

```dart
MaterialApp(
  title: 'Themed App',
  theme: ThemeData(
    primarySwatch: Colors.deepPurple,
    textTheme: TextTheme(
      bodyLarge: TextStyle(fontSize: 18),
      titleLarge: TextStyle(fontWeight: FontWeight.bold),
    ),
    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        backgroundColor: Colors.deepPurple,
        foregroundColor: Colors.white,
      ),
    ),
  ),
)
```

✅ You can theme **buttons, text, colors, app bars, cards**, etc.

---

## 🌙 3. Add Dark Mode Support

```dart
MaterialApp(
  theme: ThemeData.light(),   // Light mode
  darkTheme: ThemeData.dark(), // Dark mode
  themeMode: ThemeMode.system, // System-based
)
```

### 🔄 Options for `themeMode`

- `ThemeMode.system`: Follows device setting
- `ThemeMode.light`: Always light
- `ThemeMode.dark`: Always dark

---

## ⚙️ 4. Manually Toggle Theme

Use a `Provider`, `GetX`, or `setState`:

```dart
bool isDark = false;

MaterialApp(
  theme: ThemeData.light(),
  darkTheme: ThemeData.dark(),
  themeMode: isDark ? ThemeMode.dark : ThemeMode.light,
)
```

### 🧠 Save User Choice

```dart
final prefs = await SharedPreferences.getInstance();
prefs.setBool('isDark', true);
```

---

## 🧪 5. Themed Widgets in Action

**Custom text style from theme:**

```dart
Text("Welcome", style: Theme.of(context).textTheme.titleLarge)
```

**Button using global style:**

```dart
ElevatedButton(
  onPressed: () {},
  child: Text("Themed Button"),
)
```

> This will automatically use your app’s theme colors & styles.

---

## 🧱 6. Use Theme Extensions (Advanced)

If you want to extend the theme for your custom styles:

```dart
extension CustomColors on ThemeData {
  Color get fancyBlue => const Color(0xFF3366FF);
}
```

Then use:

```dart
Theme.of(context).fancyBlue
```

