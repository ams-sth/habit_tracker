# Core Dart concepts you need to know for Flutter

## 1. Variables and Types

Dart is statically typed, but you can let it infer the type:

```dart
var name = "Amsh";       // Dart infers String
int age = 24;            // explicit type
double pi = 3.14;
bool isDone = false;
```

`var` = type inferred once at declaration. You cannot change the type after:

```dart
var x = 5;
x = "hello"; // ERROR — x is int, not String
```

## 2. final vs const

Both are immutable (can't be reassigned), but differ in *when* the value is set:

```dart
final name = "Amsh";       // set at runtime — computed once, never changed
const pi = 3.14159;        // set at compile time — must be a literal
```

**Rule of thumb:**

- Use `const` for values you know at write time (numbers, strings, colors)
- Use `final` for values computed at runtime (e.g., from an API, DateTime.now())

```dart
final now = DateTime.now();  // ok — runtime value
const now = DateTime.now();  // ERROR — not a compile-time constant
```

In Flutter, `const` widgets are a performance win because Flutter skips rebuilding them.

## 3. Null Safety

In Dart, variables are **non-nullable by default** — they can't be null unless you say so:

```dart
String name = "Amsh";   // can never be null
String? nickname;        // can be null (the ? makes it nullable)
```

### Handling nulls

```dart
String? input = getUserInput();

// Option 1: if-check
if (input != null) {
  print(input.length);
}

// Option 2: null-aware access (?.)
print(input?.length);    // returns null instead of crashing

// Option 3: fallback (??)
String display = input ?? "Anonymous";

// Option 4: force unwrap (!) — use only when you're 100% sure it's not null
print(input!.length);    // crashes if input is null
```

## 4. Functions

```dart
int add(int a, int b) {
  return a + b;
}
```

### Arrow syntax (single expression)

```dart
int add(int a, int b) => a + b;
```

### Named parameters (very common in Flutter)

```dart
void greet({required String name, int age = 0}) {
  print("Hello $name, age $age");
}

greet(name: "Amsh", age: 21);
greet(name: "Bob");  // age defaults to 0
```

`required` means the caller must pass it. Without it, the parameter is optional.

In Flutter, almost every widget uses named parameters:

```dart
Text(
  "Hello",
  style: TextStyle(fontSize: 16),
  textAlign: TextAlign.center,
)
```

## 5. Collections

### List (like an array)

```dart
List<String> habits = ["Read", "Exercise", "Meditate"];

habits.add("Sleep early");
habits.remove("Read");
print(habits.length);    // 3
print(habits[0]);        // Exercise
```

### Map (key-value pairs)

```dart
Map<String, bool> habitDone = {
  "Read": true,
  "Exercise": false,
};

habitDone["Meditate"] = true;
print(habitDone["Read"]);    // true
```

### Useful shortcuts

```dart
// Spread operator — merge two lists
List<String> all = [...list1, ...list2];

// Collection if — add conditionally
List<Widget> items = [
  Text("Always shown"),
  if (isLoggedIn) Text("Welcome back!"),
];

// Collection for — build a list from another
List<Widget> habitWidgets = [
  for (var h in habits) Text(h),
];
```

## 6. Classes

```dart
class Habit {
  String name;
  bool isDone;

  Habit(this.name, this.isDone);  // shorthand constructor
}

var h = Habit("Read", false);
print(h.name);   // Read
h.isDone = true;
```

### Named constructors

```dart
class Habit {
  String name;
  bool isDone;

  Habit(this.name, this.isDone);

  Habit.empty() : name = "", isDone = false;  // named constructor
}

var blank = Habit.empty();
```

### Named parameters in constructors (Flutter style)

```dart
class Habit {
  final String name;
  final bool isDone;

  const Habit({required this.name, this.isDone = false});
}

var h = Habit(name: "Read");
```

## 7. Async / Await

Most Flutter operations (API calls, file reads, database queries) are asynchronous — they take time and don't block the app.

```dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));  // simulates a delay
  return "Data loaded";
}

void main() async {
  String result = await fetchData();
  print(result);  // Data loaded (after 2 seconds)
}
```

- `Future<T>` = a value that will be available later
- `async` = marks a function as asynchronous
- `await` = pauses here until the Future completes (only inside `async` functions)

### In Flutter

```dart
void loadHabits() async {
  final data = await database.fetchHabits();
  setState(() {
    habits = data;
  });
}
```

## 8. The `??=` and Cascade (`..`) operators

### `??=` — assign only if null

```dart
String? name;
name ??= "Guest";  // assigns "Guest" only if name is null
print(name);       // Guest
```

### `..` — cascade: call multiple methods on the same object

```dart
// without cascade
var list = [];
list.add(1);
list.add(2);
list.sort();

// with cascade
var list = []
  ..add(1)
  ..add(2)
  ..sort();
```

## Quick reference

| Concept | Keyword / Syntax |
| --- | --- |
| Inferred type | `var` |
| Runtime immutable | `final` |
| Compile-time immutable | `const` |
| Nullable type | `String?` |
| Null fallback | `?? value` |
| Null-safe access | `?.property` |
| Force unwrap | `!` |
| Arrow function | `=> expr` |
| Named parameter | `{required Type name}` |
| Async function | `async` / `await` |
| Future type | `Future<T>` |
