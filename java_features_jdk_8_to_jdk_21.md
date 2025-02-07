# Java Features from JDK 8 to JDK 21

## JDK 8 Features

### 1. Lambda Expressions
Lambda expressions allow you to write more concise and readable code by treating functionality as method arguments.

```java
interface MathOperation {
    int operate(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        MathOperation addition = (a, b) -> a + b;
        System.out.println(addition.operate(5, 3));  // Output: 8
    }
}
```

### 2. Streams API
The Streams API provides a new abstraction for working with sequences of elements in a functional style.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        numbers.stream()
               .filter(n -> n % 2 == 0)
               .forEach(System.out::println);  // Output: 2 4
    }
}
```

### 3. Default Methods
Interface methods can have implementations.

```java
interface MyInterface {
    default void defaultMethod() {
        System.out.println("Default Method");
    }
}

public class Main implements MyInterface {
    public static void main(String[] args) {
        Main obj = new Main();
        obj.defaultMethod();  // Output: Default Method
    }
}
```

## JDK 9 Features

### 1. Modules (Project Jigsaw)
Modularizes the JDK, allowing for better organization of code.

```java
module mymodule {
    requires java.base;
    exports com.myapp;
}
```

### 2. JShell (REPL)
A command-line tool for interactive Java programming.

Run `jshell` from the command line and experiment with Java expressions interactively.

## JDK 10 Features

### 1. Local-Variable Type Inference
The `var` keyword is introduced for local variable declarations.

```java
public class Main {
    public static void main(String[] args) {
        var message = "Hello, World!";
        System.out.println(message);  // Output: Hello, World!
    }
}
```

## JDK 11 Features

### 1. String Methods
New methods in the `String` class, such as `isBlank()`, `lines()`, and `strip()`.

```java
public class Main {
    public static void main(String[] args) {
        String str = "   Hello, Java!   ";
        System.out.println(str.strip());  // Output: "Hello, Java!"
    }
}
```

### 2. HTTP Client API
A new API for HTTP requests and responses.

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class Main {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.github.com"))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
    }
}
```

## JDK 12 Features

### 1. Switch Expressions
More powerful and flexible `switch` statements.

```java
public class Main {
    public static void main(String[] args) {
        int day = 3;
        String dayType = switch (day) {
            case 1, 2, 3, 4, 5 -> "Weekday";
            case 6, 7 -> "Weekend";
            default -> throw new IllegalArgumentException("Invalid day: " + day);
        };
        System.out.println(dayType);  // Output: Weekday
    }
}
```

## JDK 13 Features

### 1. Text Blocks
Multi-line string literals.

```java
public class Main {
    public static void main(String[] args) {
        String text = """
                      This is a
                      multi-line
                      text block
                      """;
        System.out.println(text);
    }
}
```

## JDK 14 Features

### 1. Helpful NullPointerExceptions
More detailed exception messages.

```java
public class Main {
    public static void main(String[] args) {
        String str = null;
        System.out.println(str.length());  // Throws: NullPointerException with detailed message
    }
}
```

## JDK 15 Features

### 1. Sealed Classes
Restrict which classes or interfaces can extend or implement them.

```java
public sealed class Animal permits Dog, Cat {}
public final class Dog extends Animal {}
public final class Cat extends Animal {}
```

## JDK 16 Features

### 1. Records
A new kind of class for immutable data.

```java
public record Person(String name, int age) {}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Alice", 30);
        System.out.println(p);  // Output: Person[name=Alice, age=30]
    }
}
```

## JDK 17 Features

### 1. Pattern Matching for `switch` (Preview)
Enhances `switch` with pattern matching.

```java
public class Main {
    public static void main(String[] args) {
        Object obj = "Hello";

        switch (obj) {
            case Integer i -> System.out.println("Integer: " + i);
            case String s -> System.out.println("String: " + s);  // Output: String: Hello
            default -> System.out.println("Other");
        }
    }
}
```

## JDK 18 Features

### 1. UTF-8 by Default
The default charset for the platform is now UTF-8.

## JDK 19 Features

### 1. Virtual Threads (Preview)
Provides lightweight threads for better concurrency.

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread.ofVirtual().start(() -> System.out.println("Virtual Thread"));
    }
}
```

## JDK 20 Features

### 1. Foreign Function & Memory API (Incubator)
A new API for interacting with native code and memory.

## JDK 21 Features

### 1. Record Patterns (Preview)
Allows more powerful destructuring of records in patterns.

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Alice", 30);

        if (p instanceof Person(String name, int age)) {
            System.out.println(name + " is " + age + " years old.");
        }
    }
}
```