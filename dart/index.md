# Dart

## conditional item in the list

Nice and clean
```dart
  final foo = [
    "foo",
    if (_flag)  "bar",
  ]

```

## .. operator

Assuming you are chaining functions as 
```dart
foo().bar().baz()
```
but unfortunately `baz` is returning `void` but you need same type as returned by `bar`. You can use `..` instead of having temporary variable.
```dart
foo().bar()..baz() // chain returns object returned by bar
```