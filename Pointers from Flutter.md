## Pointers from Building First Flutter App
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
- A widget’s main job is to provide a **build()** method that describes how to display the widget in terms of other, lower level widgets.
- The body for this example consists of a **Center** widget containing a **Text** child widget. The Center widget aligns its widget subtree to the center of the screen.
---
### [Use an external package](https://flutter.dev/docs/get-started/codelab#step-2-use-an-external-package)
1. Find and use packages to build Dart and Flutter apps in [pub.dev](https://pub.dev/).
2. The **pubspec.yaml** file manages the assets and dependencies for a Flutter app. While viewing the **pubspec.yaml** file in Android Studio’s editor view, click Packages get. This pulls the package into your project. Performing Packages get also auto-generates the **pubspec.lock** file with a list of all packages pulled into the project and their version numbers.
3. In lib/main.dart, import the new package.
---
### [Add a Stateful Widget](https://flutter.dev/docs/get-started/codelab#step-3-add-a-stateful-widget)
Stateless widgets are immutable, meaning that their properties can’t change—all values are final.
Stateful widgets maintain state that might change during the lifetime of the widget. Implementing a stateful widget requires at least two classes:
1. a **StatefulWidget** class that creates an instance of  
2. a **State** class. The **StatefulWidget** class is, itself, immutable, but the **State** class persists over the lifetime of the widget.
---
### [Tips](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2)
1.  In Flutter's reactive style framework, calling **setState()** triggers a call to the **build()** method for the State object, resulting in an update to the UI.
2.  Some widget properties take a single widget (**child**), and other properties, such as action, take an array of widgets (**children**), as indicated by the square brackets (**[]**).
---
[Curated list of Sample apps](https://github.com/flutter/samples/blob/master/INDEX.md)