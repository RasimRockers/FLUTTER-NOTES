
---

## 🌐 Flutter Basics – Part 9: REST API Integration

---

### 1. **What Is a REST API?**

A REST API lets your Flutter app **talk to a server**.  
You send HTTP requests (GET, POST, PUT, DELETE) and get responses, usually in JSON.

Example API:  
`https://jsonplaceholder.typicode.com/posts`

---

### 2. **Add HTTP Package**

In your `pubspec.yaml`:

```yaml
dependencies:
  http: ^0.13.6
```

Then run:

```bash
flutter pub get
```

---

### 3. **Basic HTTP GET Request**

```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

Future<List<dynamic>> fetchPosts() async {
  final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

  if (response.statusCode == 200) {
    return jsonDecode(response.body);
  } else {
    throw Exception('Failed to load posts');
  }
}
```

---

### 4. **Create a Post Model**

```dart
class Post {
  final int id;
  final String title;
  final String body;

  Post({required this.id, required this.title, required this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}
```

---

### 5. **Display API Data in UI**

```dart
class PostsScreen extends StatelessWidget {
  final Future<List<dynamic>> postsFuture = fetchPosts();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Posts')),
      body: FutureBuilder(
        future: postsFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting)
            return Center(child: CircularProgressIndicator());

          if (snapshot.hasError)
            return Center(child: Text('Error: ${snapshot.error}'));

          final posts = snapshot.data as List;
          return ListView.builder(
            itemCount: posts.length,
            itemBuilder: (context, index) {
              final post = Post.fromJson(posts[index]);
              return ListTile(
                title: Text(post.title),
                subtitle: Text(post.body),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### 6. **POST (Send Data to API)**

```dart
Future<void> createPost(String title, String body) async {
  final response = await http.post(
    Uri.parse('https://jsonplaceholder.typicode.com/posts'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({
      'title': title,
      'body': body,
      'userId': 1,
    }),
  );

  if (response.statusCode == 201) {
    print('Post created!');
  } else {
    throw Exception('Failed to create post');
  }
}
```

---

### 7. **Error Handling & Loading States**

- Use `try-catch` for network calls.
- Show `CircularProgressIndicator` during `loading`.
- Display error message on failure.

```dart
try {
  final data = await fetchPosts();
} catch (e) {
  print('Error: $e');
}
```

---

### 8. **Best Practices**

✅ Use models (`fromJson`, `toJson`)  
✅ Organize API calls in `/services/api_service.dart`  
✅ Avoid calling APIs in `build()` directly  
✅ Use `Provider` or `Riverpod` for state management

---

### 9. **Bonus: Call Auth API**

```dart
Future<void> login(String email, String password) async {
  final response = await http.post(
    Uri.parse('https://reqres.in/api/login'),
    body: {'email': email, 'password': password},
  );

  if (response.statusCode == 200) {
    final token = jsonDecode(response.body)['token'];
    print('Logged in with token: $token');
  } else {
    print('Login failed');
  }
}
```

---

