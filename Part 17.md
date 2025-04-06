## 🌀 Flutter Basics – Part 17: Animations & Hero Transitions

---

### 💡 1. Why Use Animations in Flutter?

Animations:
- Make UI feel smooth and modern
- Help users understand transitions
- Add visual delight 🤩

Flutter provides **powerful animation APIs** that are easy to use!

---

## ✨ 2. Implicit Animations (Super Easy!)

Just change a value and Flutter animates it.

### 🧪 Example: `AnimatedContainer`

```dart
bool isLarge = false;

AnimatedContainer(
  duration: Duration(seconds: 1),
  width: isLarge ? 200 : 100,
  height: isLarge ? 200 : 100,
  color: isLarge ? Colors.teal : Colors.orange,
)
```

Toggle size on tap:

```dart
onTap: () {
  setState(() {
    isLarge = !isLarge;
  });
}
```

---

### 🔄 3. More Implicit Widgets

| Widget                 | What it animates              |
|------------------------|-------------------------------|
| `AnimatedOpacity`      | Fade in/out                   |
| `AnimatedAlign`        | Animate position              |
| `AnimatedPadding`      | Animate spacing               |
| `AnimatedCrossFade`    | Swap between two widgets      |
| `AnimatedPositioned`   | For positioned widgets in a `Stack` |

---

## 🚀 133. Hero Animation (Screen-to-Screen)

### 🧪 Simple Example:

**Screen 1:**

```dart
Hero(
  tag: 'myHero',
  child: Image.asset('assets/photo.jpg', width: 100),
)
```

**Screen 2:**

```dart
Hero(
  tag: 'myHero',
  child: Image.asset('assets/photo.jpg', width: 300),
)
```

> Transition is smooth and automatic ✨  
> Use this for profile pics, product previews, etc.

---

## 🧠 4. Best Practices

- Use **unique Hero `tag`** for each transition
- Wrap `GestureDetector` or `InkWell` around Hero if needed
- Don’t animate large file images (optimize first)

