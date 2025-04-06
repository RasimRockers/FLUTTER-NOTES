
---

## 🎞️ Flutter Basics – Part 7: Animations & Transitions

---

### 1. **Why Animations Matter**
- Makes UI feel responsive and alive  
- Guides user attention  
- Makes transitions less jarring

---

### 2. **Implicit Animations**

Use when you just want something to smoothly change.

#### Example: `AnimatedContainer`

```dart
bool selected = false;

AnimatedContainer(
  duration: Duration(seconds: 1),
  width: selected ? 200 : 100,
  height: selected ? 200 : 100,
  color: selected ? Colors.blue : Colors.red,
)

onPressed: () {
  setState(() {
    selected = !selected;
  });
}
```

---

### 3. **Explicit Animation with AnimationController**

```dart
late AnimationController _controller;
late Animation<double> _animation;

@override
void initState() {
  super.initState();
  _controller = AnimationController(
    duration: Duration(seconds: 2),
    vsync: this,
  );
  _animation = Tween<double>(begin: 0, end: 300).animate(_controller);
  _controller.forward();
}
```

Use `AnimatedBuilder` to animate a widget:
```dart
AnimatedBuilder(
  animation: _animation,
  builder: (context, child) {
    return Container(height: _animation.value);
  },
)
```

---

### 4. **Hero Animations (Screen Transitions)**

```dart
Hero(
  tag: 'profilePic',
  child: Image.asset('profile.jpg'),
)
```

Use same `tag` in second screen for smooth transition!

---

### 5. **AnimatedSwitcher**

```dart
AnimatedSwitcher(
  duration: Duration(milliseconds: 300),
  child: Text('$count', key: ValueKey(count)),
)
```

Great for counters, toggles, etc.

---

### 6. **Lottie Animations (JSON based)**

Install:
```yaml
lottie: ^2.6.0
```

```dart
Lottie.asset('assets/success.json')
```

---

