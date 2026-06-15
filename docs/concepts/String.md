# String interpolation in Flutter (more accurately, in Dart) is a way to embed variables or expressions directly inside a string

## Basic idea

Instead of concatenating strings like this:

```dart
String name = "Amsh";
String msg = "Hello " + name + "!";
```

You write:

```dart
String name = "Amsh";
String msg = "Hello $name!";
```

## With expressions

If you need to compute something:

```dart
int a = 5;
int b = 10;

print("Sum is ${a + b}");
```

Output:

```terminal
Sum is 15
```

## Why it exists

* Cleaner than string concatenation
* Less error-prone
* Easier to read in UI-heavy code (which Flutter is full of)

## In Flutter UI (common usage)

```dart
Text("Welcome, $name")
```

or:

```dart
Text("Total: \$${price * quantity}")
```

Note the `\$` — that’s how you print a literal dollar sign.

## So is it a “concept”?

Yes:

* It’s a **Dart language feature**
* Part of **string handling / syntax**
* Not Flutter-specific, but heavily used in Flutter apps

If you want, I can also show how it compares to template strings in JavaScript or Kotlin — it maps pretty closely.
