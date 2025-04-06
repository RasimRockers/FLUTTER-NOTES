Perfect! Let’s level up with **Flutter Basics – Part 18: Custom Animations using AnimationController & Tween** — where *you control every frame* 🎮

---

## 🎯 Flutter Basics – Part 18: Custom Animation (Explicit Animations)

---

### 💡 1. Why Custom Animations?

Use **custom (explicit) animations** when:
- You need full control (e.g., loop, pause, reverse)
- You want to animate multiple properties
- You need to sync animations with events

---

## 🔧 2. Core Concepts

| Term               | Description                                        |
|--------------------|----------------------------------------------------|
| `AnimationController` | Controls the animation timing                     |
| `Tween`            | Defines the range of values (e.g., 0.0 → 1.0)      |
| `Animation`        | Combines controller + tween                        |
| `AnimatedBuilder`  | Rebuilds widget as animation progresses            |
| `TickerProvider`   | Required to sync frames (like a clock tick)        |

---

## 🔁 3. Basic Setup

```dart
class MyAnimationPage extends StatefulWidget {
  @override
  _MyAnimationPageState createState() => _MyAnimationPageState();
}

class _MyAnimationPageState extends State<MyAnimationPage> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this, // very important!
    );

    _animation = Tween<double>(begin: 0.0, end: 300.0).animate(_controller);

    _controller.forward(); // Start animation
  }

  @override
  void dispose() {
    _controller.dispose(); // Always dispose!
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: AnimatedBuilder(
          animation: _animation,
          builder: (context, child) {
            return Container(
              width: _animation.value,
              height: _animation.value,
              color: Colors.purple,
            );
          },
        ),
      ),
    );
  }
}
```

---

## 🔄 4. Add Controls: Repeat, Reverse, Status

```dart
_controller.repeat();          // Infinite loop
_controller.reverse();         // Reverse direction
_controller.stop();            // Stop it
_controller.addStatusListener((status) {
  if (status == AnimationStatus.completed) {
    print('✅ Done!');
  }
});
```

---

## ✨ 5. Combine Multiple Tweens

```dart
final opacity = Tween<double>(begin: 0, end: 1).animate(_controller);
final size = Tween<double>(begin: 50, end: 200).animate(_controller);
```

Then use both in the builder.

---

## 🌈6. Animate with Curves

```dart
_animation = CurvedAnimation(
  parent: _controller,
  curve: Curves.easeInOut,
);
```

Use other curves like:
- `bounceIn`, `elasticOut`
- `fastOutSlowIn`, `easeInCubic`

---

## 🎯 Bonus: Use `AnimatedWidget`

Subclass it to avoid writing `AnimatedBuilder` manually.

---
