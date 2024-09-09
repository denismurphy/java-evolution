# 🚀 A Tour of New Features in Java 9-21

A comprehensive overview of significant Java enhancements from versions 9 through 21, tailored for experienced Java developers.

## Java 9: Module System (JPMS) 📦

The Java Platform Module System (JPMS) introduced modular programming at the language and JVM level.

```java
module com.example.mymodule {
    requires java.base;  // Implicit, but good practice to declare
    requires transitive com.example.othermodule;  // Transitive dependency
    exports com.example.mypackage;
    provides com.example.spi.MyService with com.example.impl.MyServiceImpl;
}
```

Key points:
- Use `requires transitive` for API dependencies
- `exports ... to` for fine-grained access control
- `opens` for reflection access
- `uses` and `provides` for service loading

## Java 10: Local-Variable Type Inference 🔍

Type inference with `var` improves code readability without sacrificing type safety.

```java
var list = new ArrayList<String>();  // Inferred as ArrayList<String>
var stream = list.stream().map(String::length);  // Stream<Integer>

// Complex generic types become more readable
var map = new HashMap<String, List<Map<String, Object>>>();
```

Best practices:
- Use descriptive variable names to compensate for the lack of explicit types
- Avoid `var` with diamond operator or when the type is not clear from the right-hand side

## Java 11: New String and File Methods 📜

Enhanced String and File APIs for more efficient text processing.

```java
String multiline = "line1\nline2\nline3";
multiline.lines().forEach(System.out::println);

Path path = Path.of("example.txt");
String content = Files.readString(path);
Files.writeString(path, "New content");

// HTTP Client (standardized from Java 9)
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.example.com"))
        .build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
```

## Java 12: Switch Expressions 🔀

More concise and less error-prone switch statements.

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
    default -> throw new IllegalArgumentException("Invalid day: " + day);
};

// With block expression
String result = switch (status) {
    case SUCCESS -> {
        logger.info("Operation successful");
        yield "Process completed";
    }
    case ERROR -> {
        logger.error("Operation failed");
        yield "Error occurred";
    }
};
```

## Java 13: Text Blocks 📝

Multi-line string literals with improved readability and fewer escape sequences.

```java
String json = """
    {
        "name": "John Doe",
        "age": 30,
        "city": "New York"
    }
    """;

String query = """
    SELECT id, name, email
    FROM users
    WHERE status = 'ACTIVE'
      AND last_login > ?
    ORDER BY last_login DESC
    """;
```

Best practice: Use `\` for line continuation without newline in the resulting string.

## Java 14: Pattern Matching for instanceof 🧐

Simplifies type checking and casting in a single step.

```java
if (obj instanceof String s && s.length() > 5) {
    System.out.println("Long string: " + s.toUpperCase());
} else if (obj instanceof List<?> list && !list.isEmpty()) {
    System.out.println("Non-empty list: " + list.get(0));
} else if (obj instanceof Map<?, ?> map) {
    map.forEach((k, v) -> System.out.println(k + " = " + v));
}
```

## Java 15: Sealed Classes 🔒

Precisely control class hierarchies for better domain modeling and API design.

```java
public sealed interface Shape
    permits Circle, Rectangle, Polygon {
    double area();
}

public final class Circle implements Shape {
    private final double radius;
    // constructor and area() implementation
}

public non-sealed class Rectangle implements Shape {
    // allows further subclassing
}

public sealed class Polygon implements Shape
    permits Triangle, Quadrilateral {
    // restricted subclassing
}
```

## Java 16: Records 📊

Concise immutable data classes with built-in equals, hashCode, and toString.

```java
public record Person(String name, int age) {
    // Compact constructor for validation
    public Person {
        if (age < 0) throw new IllegalArgumentException("Age must be non-negative");
    }

    // Additional methods
    public boolean isAdult() {
        return age >= 18;
    }
}

// Usage
var person = new Person("Alice", 30);
System.out.println(person.name()); // Getter
System.out.println(person); // toString
```

## Java 17 (LTS): Enhanced Pseudo-Random Number Generators ❗

Improved random number generation with new interfaces and implementations.

```java
RandomGenerator generator = RandomGenerator.of("L64X128MixRandom");
DoubleStream doubles = generator.doubles(1000);
IntStream ints = generator.ints(10, 1, 100); // 10 numbers between 1 and 100

// Jumpable and SplittableRandomGenerator for parallel streams
SplittableRandomGenerator splittable = SplittableRandomGenerator.of("L64X128MixRandom");
SplittableRandomGenerator split1 = splittable.split();
SplittableRandomGenerator split2 = splittable.split();
```

## Java 18: Simple Web Server 🧵

Lightweight HTTP server for testing and prototyping.

```java
var server = SimpleFileServer.createFileServer(
    new InetSocketAddress(8080),
    Path.of("/path/to/directory"),
    OutputLevel.VERBOSE
);
server.start();

// Custom handlers
HttpHandlers.createOrModify("/api", exchange -> {
    exchange.sendResponseHeaders(200, 0);
    try (var writer = new PrintWriter(exchange.getResponseBody())) {
        writer.println("API response");
    }
}).start(InetSocketAddress.createUnresolved("localhost", 8080));
```

## Java 19: Virtual Threads (Preview) 🏗️

Lightweight, high-throughput threads for concurrent applications.

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 1_000_000).forEach(i -> {
        executor.submit(() -> {
            Thread.sleep(Duration.ofSeconds(1));
            return i;
        });
    });
}

// Structured concurrency (preview)
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Future<String> user = scope.fork(() -> fetchUser());
    Future<Integer> order = scope.fork(() -> fetchOrder());
    scope.join(); // Wait for both forks
    scope.throwIfFailed(); // Propagate errors
    processData(user.resultNow(), order.resultNow());
}
```

## Java 20: Scoped Values (Preview) 🔢

Thread-local variables with improved performance and scoping.

```java
final ScopedValue<String> TRANSACTION_ID = ScopedValue.newInstance();

void processTransaction(String txId) {
    ScopedValue.where(TRANSACTION_ID, txId).run(() -> {
        // All code in this block can access TRANSACTION_ID
        logOperation();
        updateDatabase();
    });
}

void logOperation() {
    logger.info("Operation in transaction: " + TRANSACTION_ID.get());
}
```

## Java 21 (LTS): Pattern Matching for switch 🎭

Powerful type-based dispatching with data extraction.

```java
static String formatValue(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int: %d", i);
        case Long l    -> String.format("long: %d", l);
        case Double d  -> String.format("double: %.2f", d);
        case String s  -> String.format("String: \"%s\"", s);
        case Point p   -> String.format("Point: (%d, %d)", p.x(), p.y());
        case ColoredPoint(Point p, Color c) -> 
            String.format("ColoredPoint: (%d, %d, %s)", p.x(), p.y(), c);
        case null      -> "null";
        default        -> obj.toString();
    };
}

record Point(int x, int y) {}
record ColoredPoint(Point p, Color c) {}
```

Note: Features marked as "preview" are subject to change. Always refer to the latest Java SE specifications and JEPs for the most current information and best practices.
