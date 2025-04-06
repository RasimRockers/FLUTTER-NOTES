## 🧩 Flutter Basics – Part 19: Reusable Widgets

---

### 💡 1. Why Reusable Widgets?

- Keep UI **clean and modular**
- Avoid copy-paste code
- Easy to maintain and test
- Improve scalability of your app

> 🧠 If you're repeating the same layout in multiple places, it's a candidate for reuse!

---

## ⚒️ 2. Create a Custom Widget

Let’s build a reusable button:

```dart
class MyButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;

  MyButton({required this.text, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(text),
    );
  }
}
```

### ✅ Use It Like This:

```dart
MyButton(
  text: "Click Me",
  onPressed: () {
    print("Button clicked!");
  },
)
```

---

## 🧱 3. Reusable Card Widget

```dart
class InfoCard extends StatelessWidget {
  final String title;
  final IconData icon;

  InfoCard({required this.title, required this.icon});

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: EdgeInsets.all(12),
      child: ListTile(
        leading: Icon(icon),
        title: Text(title),
      ),
    );
  }
}
```

### ✅ Use:

```dart
InfoCard(title: "Profile", icon: Icons.person)
InfoCard(title: "Settings", icon: Icons.settings)
```

---

## 🔁 4. Combine Reusable Widgets + ListView

```dart
ListView(
  children: [
    InfoCard(title: "Home", icon: Icons.home),
    InfoCard(title: "Messages", icon: Icons.message),
    InfoCard(title: "Logout", icon: Icons.logout),
  ],
)
```

> This structure is perfect for dashboard menus, settings screens, etc.

---

## 🎯 5. Make Reusable Widgets Responsive

Use `MediaQuery` inside your widget:

```dart
double width = MediaQuery.of(context).size.width;

Container(
  width: width * 0.8,
  child: Text("Responsive"),
)
```

---

## 💡 6. Best Practices

- ✅ Break down large UI files into multiple widgets
- ✅ Follow naming conventions (e.g., `CustomButton`, `ProfileTile`)
- ✅ Group widgets into folders like `widgets/`, `components/`
- ✅ Keep logic separate — use Providers or Controllers

---
