# 🚀 Java Evolution - A Tour of New Features in Java 9-21

Welcome to this comprehensive guide on the exciting new features introduced in Java versions 9 through 21! 🎉 Each section covers a major Java release, providing code examples to illustrate key innovations.

## Java 9 📦

Java 9 introduced the Java Platform Module System (JPMS), revolutionizing how Java SE organizes its components for enhanced performance, security, and maintainability.

```java
module com.example.mymodule {
    requires java.base;
    exports com.example.mypackage;
}
```

## Java 10 🔍

Java 10 brought us "local-variable type inference" with the `var` keyword:

```java
var list = new ArrayList<String>(); // Type inferred as ArrayList<String>
```

## Java 11 📜

Java 11 added the `String` class method `lines()`:

```java
Path path = Paths.get("example.txt");
String text = Files.readString(path);
Stream<String> lines = text.lines();
```

## Java 12 🔀

Java 12 introduced switch expressions for more concise switch statements:

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

## Java 13 📝

Java 13 brought us Text Blocks for multi-line string literals:

```java
String html = """
    <html>
        <body>
            <p>Hello, world!</p>
        </body>
    </html>
""";
```

## Java 14 🧐

Java 14 introduced pattern matching for instanceof:

```java
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

## Java 15 🔒

Java 15 introduced sealed classes to restrict class hierarchies:

```java
sealed interface Shape permits Circle, Square, Triangle {
    // interface methods
}

final class Circle implements Shape { /* ... */ }
final class Square implements Shape { /* ... */ }
final class Triangle implements Shape { /* ... */ }
```

## Java 16 📊

Java 16 introduced Records for compact data classes:

```java
record Person(String name, int age) {}
```

## Java 17 (LTS) ❗

Java 17 brought "new" improved NullPointerExceptions:

```java
String name = null;
try {
    System.out.println(name.length());
} catch (NullPointerException e) {
    System.out.println(e.getMessage()); // Prints "Cannot invoke length() on null object"
}
```

## Java 18 🧵

Java 18 introduced Simple Web Server for testing and prototyping:

```java
var server = SimpleFileServer.createFileServer(new InetSocketAddress(8080), 
    Path.of("/path/to/directory"), OutputLevel.VERBOSE);
server.start();
```

## Java 19 🏗️

Java 19 introduced Virtual Threads (preview) for lightweight concurrency:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 10_000).forEach(i -> {
        executor.submit(() -> {
            Thread.sleep(Duration.ofSeconds(1));
            return i;
        });
    });
}
```

## Java 20 🔢

Java 20 introduced Scoped Values (preview) for thread-local variables:

```java
final ScopedValue<String> USER = ScopedValue.newInstance();

void process() {
    ScopedValue.where(USER, "Alice").run(() -> {
        System.out.println("User: " + USER.get());
    });
}
```

## Java 21 (LTS) 🎭

Java 21 introduced Pattern Matching for switch (standard):

```java
static String formatter(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}
```

That's it! 🎓 This guide covers the exciting journey of Java from version 9 to 21. Remember to consult the official Java documentation for more detailed information and examples.

Note: Features marked as "preview" may change in future releases. Always check the latest Java documentation for the most up-to-date information. Happy coding! 💻🌟
