
---

## 📘 Flutter Basics – Part 6: State Management

---

### 1. **Why State Management?**

As your app grows, managing the UI and data flow gets harder. State management helps:
- Avoid messy `setState()` everywhere
- Keep your UI in sync with data
- Share data between widgets/screens cleanly

---

### 2. **Types of State Management in Flutter**

| Approach       | Best for                     | Complexity |
|----------------|------------------------------|------------|
| `setState()`   | Small widgets                 | 🟢 Easy     |
| `Provider`     | Mid to large apps             | 🟡 Medium   |
| `Riverpod`     | Robust apps, testing          | 🔵 Advanced |
| `Bloc`         | Enterprise apps, events       | 🔴 Complex  |
| `GetX`, `MobX`, `Redux` | Varies               | Varies     |

---

### 3. **`setState()` – For Local State**

```dart
int counter = 0;

ElevatedButton(
  onPressed: () {
    setState(() {
      counter++;
    });
  },
  child: Text('Count: $counter'),
)
```

Good for: Buttons, toggles, simple screens.

---

### 4. **`Provider` – Global State**

#### Step 1: Add package
```yaml
dependencies:
  provider: ^6.1.2
```

#### Step 2: Create a ChangeNotifier class
```dart
class CounterProvider with ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }
}
```

#### Step 3: Wrap your app with Provider
```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => CounterProvider(),
      child: MyApp(),
    ),
  );
}
```

#### Step 4: Use the provider in widgets
```dart
Consumer<CounterProvider>(
  builder: (context, counter, _) => Text('Count: ${counter.count}'),
)

ElevatedButton(
  onPressed: () => context.read<CounterProvider>().increment(),
  child: Text('Add'),
)
```

---

### 5. **`Riverpod` – Safer & Cleaner**

Install:
```yaml
dependencies:
  flutter_riverpod: ^2.5.1
```

Example:
```dart
final counterProvider = StateProvider((ref) => 0);

Consumer(
  builder: (context, ref, _) {
    final count = ref.watch(counterProvider);
    return Text('$count');
  },
)

onPressed: () => ref.read(counterProvider.notifier).state++,
```

Riverpod:
- No need for `BuildContext`
- Works well with async, testing
- Cleaner syntax with large apps

---

### 6. **Cleaning Up UI with Custom Widgets**

Break big UIs into components:

```dart
// In counter_button.dart
class CounterButton extends StatelessWidget {
  final VoidCallback onPressed;
  const CounterButton({required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(onPressed: onPressed, child: Text('Add'));
  }
}
```

Use it in main UI:
```dart
CounterButton(onPressed: () => context.read<CounterProvider>().increment())
```

---

### 7. **Testing State Logic**

Use `flutter_test` and `mockito` to test your providers or BLoC logic.

```dart
void main() {
  test('Counter increments', () {
    final counter = CounterProvider();
    counter.increment();
    expect(counter.count, 1);
  });
}
```

---

### 8. **Async State – Loading, Error, Data**

Use `FutureBuilder` or `AsyncNotifier` (Riverpod):

```dart
final userProvider = FutureProvider((ref) async {
  await Future.delayed(Duration(seconds: 1));
  return 'John Doe';
});
```

---

### 9. **Sharing State Between Screens**

Using `Provider` or `Riverpod`, you can access and modify state globally without passing props manually.

Example:
```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (_) => SecondScreen()),
);

// In SecondScreen, you can still access the same provider
final counter = context.watch<CounterProvider>().count;
```

---

### 10. **Other Popular State Tools (Optional)**

| Package   | Description                      |
|-----------|----------------------------------|
| `GetX`    | Lightweight, reactive, quick     |
| `Bloc`    | Event-based, structured, scalable|
| `Redux`   | Functional, predictable          |
| `MobX`    | Reactive, uses observables       |

---

