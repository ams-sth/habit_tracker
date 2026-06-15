# Flutter concepts you need to know

## 1. Everything is a Widget

In Flutter, the entire UI is built from widgets — buttons, text, padding, layout, even the app itself.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("Habit Tracker")),
        body: Text("Hello!"),
      ),
    );
  }
}
```

Widgets are just Dart classes that describe what the UI should look like. Flutter reads your widget tree and draws it on screen.

## 2. StatelessWidget vs StatefulWidget

### StatelessWidget

No internal state. The UI is driven entirely by what you pass in:

```dart
class HabitCard extends StatelessWidget {
  final String title;

  const HabitCard({required this.title});

  @override
  Widget build(BuildContext context) {
    return Text(title);
  }
}
```

Use this when the widget doesn't need to change after it's built.

### StatefulWidget

Has internal state that can change, causing the widget to rebuild:

```dart
class CounterWidget extends StatefulWidget {
  @override
  State<CounterWidget> createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text("Count: $count"),
        ElevatedButton(
          onPressed: () => setState(() { count++; }),
          child: Text("Increment"),
        ),
      ],
    );
  }
}
```

Use this when the widget needs to track and update its own data.

## 3. setState

`setState` is how you tell Flutter "something changed, please redraw this widget":

```dart
setState(() {
  isDone = true;
});
```

**Important:** only state changes *inside* `setState` trigger a rebuild. Changing a variable outside of it does nothing to the UI:

```dart
// This does NOT update the UI:
count = count + 1;

// This DOES:
setState(() {
  count = count + 1;
});
```

## 4. The Widget Tree and BuildContext

Flutter builds your UI as a tree of widgets. Every widget has a `BuildContext` — it's basically the widget's position in that tree.

``` dart
MaterialApp
  └── Scaffold
        ├── AppBar
        └── Column
              ├── Text
              └── ElevatedButton
```

You mostly use `context` to:

- Navigate to other screens
- Access inherited data (like Theme or MediaQuery)

```dart
// Get screen width
double width = MediaQuery.of(context).size.width;

// Navigate
Navigator.of(context).push(...);
```

## 5. Layout Widgets

Flutter has no CSS. Layout is done with widgets.

### Column and Row

```dart
Column(
  children: [
    Text("Item 1"),
    Text("Item 2"),
  ],
)
```

```dart
Row(
  children: [
    Icon(Icons.check),
    Text("Exercise"),
  ],
)
```

### Container

The all-purpose box — size, color, padding, margin, border:

```dart
Container(
  width: 200,
  height: 100,
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.symmetric(vertical: 8),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(12),
  ),
  child: Text("Hello"),
)
```

### Expanded and Flexible

Inside a Column or Row, `Expanded` makes a widget fill the remaining space:

```dart
Row(
  children: [
    Expanded(child: Text("Takes all remaining width")),
    Icon(Icons.arrow_forward),
  ],
)
```

### SizedBox

A simple spacer or fixed-size box:

```dart
SizedBox(height: 16)   // vertical gap
SizedBox(width: 8)     // horizontal gap
```

## 6. Common Widgets

| Widget | What it does |
| --- | --- |
| `Text` | Displays a string |
| `Icon` | Material icon |
| `Image.asset` | Image from assets folder |
| `ElevatedButton` | A button with elevation |
| `TextButton` | A flat button |
| `IconButton` | A tappable icon |
| `TextField` | Text input |
| `Checkbox` | A checkbox |
| `Switch` | A toggle switch |
| `ListView` | Scrollable list |
| `Padding` | Adds padding around a child |
| `Center` | Centers its child |
| `GestureDetector` | Detects taps, swipes, etc. |

## 7. Scaffold

`Scaffold` gives you the basic Material Design page structure:

```dart
Scaffold(
  appBar: AppBar(title: Text("My Habits")),
  body: Center(child: Text("Content here")),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
  bottomNavigationBar: BottomNavigationBar(items: [...]),
  drawer: Drawer(...),
)
```

## 8. Navigation

Flutter uses a stack of screens (called routes). You push to go forward, pop to go back.

```dart
// Go to a new screen
Navigator.of(context).push(
  MaterialPageRoute(builder: (context) => HabitDetailScreen()),
);

// Go back
Navigator.of(context).pop();

// Go back and send data
Navigator.of(context).pop(result);
```

### Named routes (cleaner for larger apps)

```dart
// Define in MaterialApp
routes: {
  '/home': (context) => HomeScreen(),
  '/detail': (context) => DetailScreen(),
}

// Navigate
Navigator.pushNamed(context, '/detail');
```

## 9. Lifecycle Methods (StatefulWidget)

When using a `StatefulWidget`, these methods run at different points:

```dart
@override
void initState() {
  super.initState();
  // Runs once when the widget is first created
  // Good place to load data, start timers, etc.
}

@override
void dispose() {
  // Runs when the widget is removed from the tree
  // Always clean up: controllers, streams, timers
  super.dispose();
}

@override
void didChangeDependencies() {
  super.didChangeDependencies();
  // Runs when an inherited widget (like Theme) changes
}
```

Example — always dispose a TextEditingController:

```dart
final _controller = TextEditingController();

@override
void dispose() {
  _controller.dispose();  // prevents memory leaks
  super.dispose();
}
```

## 10. pubspec.yaml — Adding Packages

Flutter packages live on [pub.dev](https://pub.dev). To add one:

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^2.2.2   # add your package here
```

Then run:

```bash
flutter pub get
```

Common packages for a habit tracker:

| Package | Purpose |
| --- | --- |
| `shared_preferences` | Save simple data locally |
| `sqflite` | SQLite database |
| `provider` | Simple state management |
| `riverpod` | More powerful state management |
| `intl` | Date formatting |
| `uuid` | Generate unique IDs |

## 11. Hot Reload vs Hot Restart

| | Hot Reload | Hot Restart |
| --- | --- | --- |
| Shortcut | `r` in terminal | `R` in terminal |
| What resets | Nothing — state is preserved | Everything — app restarts from scratch |
| When to use | UI tweaks, layout changes | Logic changes, new dependencies |
| Speed | Instant | A few seconds |

If your change isn't showing up, try a hot restart.

## Quick mental model

``` dart
Your app
  = a tree of widgets
  = each widget describes a piece of UI
  = StatefulWidgets hold state that can change
  = setState() triggers a rebuild
  = Flutter redraws only what changed
```
