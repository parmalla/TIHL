#### Pointers from Building First Flutter App
---
**lib/main.dart**
```Dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());   //The main() method uses arrow (=>) notation. Use arrow notation for one-line functions or methods.

class MyApp extends StatelessWidget {   //The app extends StatelessWidget which makes the app itself a widget. In Flutter, almost everything is a widget, including alignment, padding, and layout.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(               //The Scaffold widget, from the Material library, provides a default app bar, title, and a body property that holds the widget tree for the home screen. The widget subtree can be quite complex.
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```
