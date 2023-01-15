
# Java Evolution - A Tour of the New Features in Java 9-17

This repository is a guide to the new features introduced in Java 9-17. Each section will cover a new version of Java and provide code examples of the new features.

## Java 9

Java 9 introduced the Java Platform Module System (JPMS), which organizes the Java SE Platform into a set of modules for improved performance, security, and maintainability. Here is an example of how to use the JPMS to define a module and its dependencies:

```
module com.example.mymodule {
    requires java.base;
    exports com.example.mypackage;
}
```

## Java 10

Java 10 introduced "local-variable type inference" which is a way to infer the type of a local variable by using the `var` keyword, like this:

```
var list = new ArrayList<String>();
```

## Java 11

Java 11 introduced the `String` class method `lines()` which returns a `Stream<String>` of lines read from a file. Here's an example of how to use it:

```
Path path = Paths.get("example.txt");
String text = new String(Files.readAllBytes(path));
Stream<String> lines = text.lines();
```

## Java 12

Java 12 introduced the switch expression which allows you to use a more concise and expressive syntax for switch statements. Here's an example of how to use it:

```
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

## Java 13

Java 13 introduced the `Text Blocks` which allows you to write multi-line string literals in a more readable way. Here's an example of how to use it:

```
String html = """
    <html>
        <body>
            <p>Hello, world!</p>
        </body>
    </html>
""";
```

## Java 14

Java 14 introduced the `pattern matching for instanceof` which allows you to check the type of an object and extract a value from it in a more concise and expressive way. Here's an example of how to use it:

```
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

## Java 15

Java 15 introduced the `sealed classes` which allow you to restrict the types that can extend a class or implement an interface. Here's an example of how to use it:

```
sealed interface Shape permits Circle, Square, Triangle {
    // interface methods
}

final class Circle implements Shape {
    // class methods
}

final class Square implements Shape {
    // class methods
}

final class Triangle implements Shape {
    // class methods
}
```

## Java 16

Java 16 introduced the `Record` keyword which allows you to create a simple and compact class for storing a fixed set of values. Here's an example of how to use it:

```
record Person(String name, int age) {}
```

## Java 17

Java 17 introduced the `Helpful NullPointerExceptions` which allows you to get more detailed information about what went wrong when you get a `NullPointerException`. Here's an example of how it can be used:

```
String name = null;
try {
    System.out.println(name.length());
} catch (NullPointerException e) {
    System.out.println(e.getMessage()); // Prints "Cannot invoke length() on null object"
}
```

That's it! I hope this guide has helped you to understand the new features introduced in Java 9-17. As always, be sure to consult the official Java documentation for more detailed information and examples.

Please note that these are just examples of some of the new features in these versions of Java, and there may be other new features and changes as well.
