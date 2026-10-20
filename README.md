# week-8

week-2(a)
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Form Validation Demo',
      debugShowCheckedModeBanner: false,
      home: FormScreen(),
    );
  }
}

class FormScreen extends StatefulWidget {
  const FormScreen({super.key});

  @override
  State<FormScreen> createState() => _FormScreenState();
}

class _FormScreenState extends State<FormScreen> {
  final GlobalKey<FormState> _formKey = GlobalKey<FormState>();

  final TextEditingController _nameController =
      TextEditingController();

  final TextEditingController _emailController =
      TextEditingController();

  final TextEditingController _passwordController =
      TextEditingController();

  void _submitForm() {
    final form = _formKey.currentState;

    if (form == null) {
      return;
    }

    if (form.validate()) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Form submitted successfully!'),
          backgroundColor: Colors.green,
        ),
      );

      debugPrint('Name: ${_nameController.text.trim()}');
      debugPrint('Email: ${_emailController.text.trim()}');
      debugPrint('Password: ${_passwordController.text}');
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Please correct the errors in the form'),
          backgroundColor: Colors.red,
        ),
      );
    }
  }

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Form Validation & Error Handling',
        ),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),

      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16),

          child: Form(
            key: _formKey,

            child: Column(
              children: [
                // Full Name
                TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'Full Name',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    final name = value?.trim() ?? '';

                    if (name.isEmpty) {
                      return 'Name is required';
                    }

                    if (name.length < 3) {
                      return 'Name must be at least 3 characters long';
                    }

                    return null;
                  },
                ),

                const SizedBox(height: 16),

                // Email
                TextFormField(
                  controller: _emailController,
                  keyboardType: TextInputType.emailAddress,
                  decoration: const InputDecoration(
                    labelText: 'Email',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    final email = value?.trim() ?? '';

                    if (email.isEmpty) {
                      return 'Email is required';
                    }

                    final emailPattern = RegExp(
                      r'^[^@\s]+@[^@\s]+\.[^@\s]+$',
                    );

                    if (!emailPattern.hasMatch(email)) {
                      return 'Enter a valid email';
                    }

                    return null;
                  },
                ),

                const SizedBox(height: 16),

                // Password
                TextFormField(
                  controller: _passwordController,
                  obscureText: true,
                  decoration: const InputDecoration(
                    labelText: 'Password',
                    border: OutlineInputBorder(),
                  ),
                  validator: (value) {
                    final password = value ?? '';

                    if (password.isEmpty) {
                      return 'Password is required';
                    }

                    if (password.length < 6) {
                      return 'Password must be at least 6 characters long';
                    }

                    return null;
                  },
                ),

                const SizedBox(height: 24),

                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: _submitForm,
                    child: const Text('Submit'),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}









import 'dart:math' as math;
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Flutter Animation Demo',
      debugShowCheckedModeBanner: false,
      home: AnimatedHome(),
    );
  }
}

class AnimatedHome extends StatefulWidget {
  const AnimatedHome({super.key});

  @override
  State<AnimatedHome> createState() => _AnimatedHomeState();
}

class _AnimatedHomeState extends State<AnimatedHome>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;

  late final Animation<double> _sizeAnimation;
  late final Animation<Color?> _colorAnimation;
  late final Animation<double> _rotationAnimation;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    _sizeAnimation = Tween<double>(
      begin: 100,
      end: 200,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeInOut,
      ),
    );

    _colorAnimation = ColorTween(
      begin: Colors.blue,
      end: Colors.orange,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.linear,
      ),
    );

    _rotationAnimation = Tween<double>(
      begin: 0,
      end: 2 * math.pi,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeInOutCubic,
      ),
    );
  }

  void _startAnimation() {
    if (_controller.status == AnimationStatus.completed) {
      _controller.reverse();
    } else if (_controller.status == AnimationStatus.dismissed) {
      _controller.forward();
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Flutter Animation Framework'),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),
      body: Center(
        child: AnimatedBuilder(
          animation: _controller,
          builder: (context, child) {
            return Transform.rotate(
              angle: _rotationAnimation.value,
              child: Container(
                width: _sizeAnimation.value,
                height: _sizeAnimation.value,
                decoration: BoxDecoration(
                  color: _colorAnimation.value,
                  borderRadius: BorderRadius.circular(20),
                ),
              ),
            );
          },
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _startAnimation,
        child: const Icon(Icons.play_arrow),
      ),
    );
  }
}






week-8(b)
import 'package:flutter/material.dart';

void main() {
  runApp(const AnimationDemoApp());
}

class AnimationDemoApp extends StatelessWidget {
  const AnimationDemoApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: AnimationTypesDemo(),
    );
  }
}

class AnimationTypesDemo extends StatefulWidget {
  const AnimationTypesDemo({super.key});

  @override
  State<AnimationTypesDemo> createState() => _AnimationTypesDemoState();
}

class _AnimationTypesDemoState extends State<AnimationTypesDemo>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;

  late final Animation<double> _fadeAnimation;
  late final Animation<Offset> _slideAnimation;
  late final Animation<double> _scaleAnimation;
  late final Animation<double> _rotationAnimation;
  late final Animation<double> _sizeAnimation;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    // Fade Animation
    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeIn,
      ),
    );

    // Slide Animation
    _slideAnimation = Tween<Offset>(
      begin: const Offset(-1.0, 0.0),
      end: Offset.zero,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeInOut,
      ),
    );

    // Scale Animation
    _scaleAnimation = Tween<double>(
      begin: 0.5,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.decelerate,
      ),
    );

    // Rotation Animation
    // 0.0 to 1.0 = one complete rotation
    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.linear,
      ),
    );

    // Size Animation
    _sizeAnimation = Tween<double>(
      begin: 100.0,
      end: 200.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.fastOutSlowIn,
      ),
    );
  }

  void _toggleAnimation() {
    if (_controller.status == AnimationStatus.completed) {
      _controller.reverse();
    } else {
      _controller.forward();
    }
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Different Types of Animations'),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),

      body: Center(
        child: SingleChildScrollView(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              // Fade Animation
              FadeTransition(
                opacity: _fadeAnimation,
                child: const Text(
                  'Fade Animation',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),

              const SizedBox(height: 25),

              // Slide Animation
              SlideTransition(
                position: _slideAnimation,
                child: const Text(
                  'Slide Animation',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),

              const SizedBox(height: 25),

              // Scale Animation
              ScaleTransition(
                scale: _scaleAnimation,
                child: const Text(
                  'Scale Animation',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),

              const SizedBox(height: 25),

              // Rotation Animation
              RotationTransition(
                turns: _rotationAnimation,
                child: const Icon(
                  Icons.refresh,
                  size: 50,
                  color: Colors.deepPurple,
                ),
              ),

              const SizedBox(height: 25),

              // Size Animation
              AnimatedBuilder(
                animation: _sizeAnimation,
                builder: (context, child) {
                  return Container(
                    width: 200,
                    height: _sizeAnimation.value,
                    color: Colors.amber,
                    alignment: Alignment.center,
                    child: const Text(
                      'Size Animation',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  );
                },
              ),
            ],
          ),
        ),
      ),

      floatingActionButton: FloatingActionButton(
        onPressed: _toggleAnimation,
        child: const Icon(Icons.play_arrow),
      ),
    );
  }
}




week-9(a)
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

// ---------------- MAIN APP ----------------

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: UsersPage(),
    );
  }
}

// ---------------- USER MODEL ----------------

class User {
  final int id;
  final String name;
  final String email;

  const User({
    required this.id,
    required this.name,
    required this.email,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] is int
          ? json['id'] as int
          : int.tryParse(json['id'].toString()) ?? 0,
      name: json['name']?.toString() ?? 'N/A',
      email: json['email']?.toString() ?? 'N/A',
    );
  }
}

// ---------------- USERS PAGE ----------------

class UsersPage extends StatefulWidget {
  const UsersPage({super.key});

  @override
  State<UsersPage> createState() => _UsersPageState();
}

class _UsersPageState extends State<UsersPage> {
  late Future<List<User>> _futureUsers;

  @override
  void initState() {
    super.initState();

    _futureUsers = fetchUsers();
  }

  // Fetch users from API
  Future<List<User>> fetchUsers() async {
    const String url =
        'https://jsonplaceholder.typicode.com/users';

    try {
      final response = await http.get(
        Uri.parse(url),
      );

      if (response.statusCode == 200) {
        final dynamic decodedData =
            jsonDecode(response.body);

        if (decodedData is List) {
          return decodedData
              .map(
                (item) => User.fromJson(
                  item as Map<String, dynamic>,
                ),
              )
              .toList();
        }

        throw Exception('Invalid API response');
      } else {
        throw Exception(
          'Failed to load users. '
          'Status code: ${response.statusCode}',
        );
      }
    } catch (e) {
      throw Exception(
        'Unable to fetch users: $e',
      );
    }
  }

  // Retry API request
  void _retry() {
    setState(() {
      _futureUsers = fetchUsers();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('User List'),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),

      body: FutureBuilder<List<User>>(
        future: _futureUsers,

        builder: (context, snapshot) {
          // Loading
          if (snapshot.connectionState ==
              ConnectionState.waiting) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          // Error
          if (snapshot.hasError) {
            return Center(
              child: Padding(
                padding: const EdgeInsets.all(20),
                child: Column(
                  mainAxisAlignment:
                      MainAxisAlignment.center,
                  children: [
                    const Icon(
                      Icons.error_outline,
                      size: 50,
                      color: Colors.red,
                    ),

                    const SizedBox(height: 15),

                    Text(
                      'Error: ${snapshot.error}',
                      textAlign: TextAlign.center,
                    ),

                    const SizedBox(height: 20),

                    ElevatedButton(
                      onPressed: _retry,
                      child: const Text('Retry'),
                    ),
                  ],
                ),
              ),
            );
          }

          // No data
          if (!snapshot.hasData ||
              snapshot.data!.isEmpty) {
            return const Center(
              child: Text(
                'No users found',
              ),
            );
          }

          final List<User> users =
              snapshot.data!;

          // Display users
          return ListView.separated(
            itemCount: users.length,

            separatorBuilder: (context, index) {
              return const Divider(
                height: 1,
              );
            },

            itemBuilder: (context, index) {
              final User user = users[index];

              return ListTile(
                leading: CircleAvatar(
                  backgroundColor:
                      Colors.teal.shade100,
                  child: Text(
                    user.id.toString(),
                  ),
                ),

                title: Text(
                  user.name,
                  style: const TextStyle(
                    fontWeight: FontWeight.bold,
                  ),
                ),

                subtitle: Text(
                  user.email,
                ),

                trailing: const Icon(
                  Icons.person,
                ),
              );
            },
          );
        },
      ),
    );
  }
}






week-9(b)
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

// ---------------------- DATA MODEL ----------------------

class User {
  final int id;
  final String name;
  final String email;
  final String phone;

  const User({
    required this.id,
    required this.name,
    required this.email,
    required this.phone,
  });

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'] is int
          ? json['id'] as int
          : int.tryParse(json['id'].toString()) ?? 0,
      name: json['name']?.toString() ?? 'N/A',
      email: json['email']?.toString() ?? 'N/A',
      phone: json['phone']?.toString() ?? 'N/A',
    );
  }
}

// ---------------------- FETCH FUNCTION ----------------------

Future<List<User>> fetchUsers() async {
  const String url =
      'https://jsonplaceholder.typicode.com/users';

  try {
    final response = await http.get(
      Uri.parse(url),
    );

    if (response.statusCode == 200) {
      final dynamic decodedData =
          jsonDecode(response.body);

      if (decodedData is List) {
        return decodedData
            .map(
              (item) => User.fromJson(
                item as Map<String, dynamic>,
              ),
            )
            .toList();
      }

      throw Exception('Invalid response format');
    } else {
      throw Exception(
        'Server returned status code: ${response.statusCode}',
      );
    }
  } catch (e) {
    throw Exception(
      'Failed to load users: $e',
    );
  }
}

// ---------------------- MAIN APP ----------------------

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: UserListScreen(),
    );
  }
}

// ---------------------- USER LIST SCREEN ----------------------

class UserListScreen extends StatefulWidget {
  const UserListScreen({super.key});

  @override
  State<UserListScreen> createState() =>
      _UserListScreenState();
}

class _UserListScreenState
    extends State<UserListScreen> {
  late Future<List<User>> _futureUsers;

  @override
  void initState() {
    super.initState();

    _futureUsers = fetchUsers();
  }

  void _retry() {
    setState(() {
      _futureUsers = fetchUsers();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('User List'),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),

      body: FutureBuilder<List<User>>(
        future: _futureUsers,

        builder: (context, snapshot) {
          if (snapshot.connectionState ==
              ConnectionState.waiting) {
            return const Center(
              child: CircularProgressIndicator(),
            );
          }

          if (snapshot.hasError) {
            return Center(
              child: Padding(
                padding: const EdgeInsets.all(20),
                child: Column(
                  mainAxisAlignment:
                      MainAxisAlignment.center,
                  children: [
                    const Icon(
                      Icons.error_outline,
                      size: 50,
                      color: Colors.red,
                    ),

                    const SizedBox(height: 10),

                    Text(
                      '${snapshot.error}',
                      textAlign: TextAlign.center,
                    ),

                    const SizedBox(height: 20),

                    ElevatedButton(
                      onPressed: _retry,
                      child: const Text('Retry'),
                    ),
                  ],
                ),
              ),
            );
          }

          if (!snapshot.hasData ||
              snapshot.data!.isEmpty) {
            return const Center(
              child: Text(
                'No users found',
              ),
            );
          }

          final List<User> users =
              snapshot.data!;

          return ListView.builder(
            itemCount: users.length,

            itemBuilder: (context, index) {
              final User user = users[index];

              return Card(
                margin:
                    const EdgeInsets.symmetric(
                  horizontal: 16,
                  vertical: 8,
                ),
                elevation: 3,

                child: ListTile(
                  leading: CircleAvatar(
                    backgroundColor:
                        Colors.teal.shade100,
                    child: Text(
                      user.id.toString(),
                    ),
                  ),

                  title: Text(
                    user.name,
                    style: const TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),

                  subtitle: Column(
                    crossAxisAlignment:
                        CrossAxisAlignment.start,
                    children: [
                      Text(user.email),
                      Text(user.phone),
                    ],
                  ),

                  trailing: IconButton(
                    icon: const Icon(
                      Icons.phone,
                    ),
                    onPressed: () {
                      ScaffoldMessenger.of(
                        context,
                      ).showSnackBar(
                        SnackBar(
                          content: Text(
                            'Call ${user.phone}',
                          ),
                        ),
                      );
                    },
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}






