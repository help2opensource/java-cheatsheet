# Java Stream API Cheatsheet

The Java Stream API provides a functional approach to processing collections of data. It was introduced in **Java 8** and allows operations like filtering, mapping, and reducing.

## 1. Creating Streams

### From a Collection
```java
import java.util.List;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie");
        Stream<String> stream = names.stream();
        stream.forEach(System.out::println);
    }
}
```

### From an Array
```java
import java.util.Arrays;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        String[] names = {"Alice", "Bob", "Charlie"};
        Stream<String> stream = Arrays.stream(names);
        stream.forEach(System.out::println);
    }
}
```

### From `Stream.of()`
```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
```

### Infinite Streams
```java
Stream<Integer> infiniteStream = Stream.iterate(0, n -> n + 2);
```

## 2. Intermediate Operations (Transformations)

### `map()` – Transform elements
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println); // Output: ALICE, BOB, CHARLIE
```

### `filter()` – Select elements based on condition
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println); // Output: 2, 4
```

### `sorted()` – Sort elements
```java
List<String> names = List.of("Charlie", "Alice", "Bob");
names.stream()
     .sorted()
     .forEach(System.out::println); // Output: Alice, Bob, Charlie
```

### `distinct()` – Remove duplicates
```java
List<Integer> numbers = List.of(1, 2, 2, 3, 4, 4, 5);
numbers.stream()
       .distinct()
       .forEach(System.out::println); // Output: 1, 2, 3, 4, 5
```

### `limit()` – Limit the number of elements
```java
Stream<Integer> infiniteStream = Stream.iterate(1, n -> n + 1);
infiniteStream.limit(5).forEach(System.out::println); // Output: 1, 2, 3, 4, 5
```

### `skip()` – Skip first N elements
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
numbers.stream()
       .skip(2)
       .forEach(System.out::println); // Output: 3, 4, 5
```

## 3. Terminal Operations (Produce Results)

### `forEach()` – Iterate through elements
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
names.stream().forEach(System.out::println);
```

### `count()` – Count elements
```java
long count = Stream.of(1, 2, 3, 4, 5).count();
System.out.println(count); // Output: 5
```

### `collect()` – Convert stream to a collection
```java
import java.util.List;
import java.util.stream.Collectors;

List<String> names = List.of("Alice", "Bob", "Charlie");
List<String> upperCaseNames = names.stream()
                                   .map(String::toUpperCase)
                                   .collect(Collectors.toList());
System.out.println(upperCaseNames); // Output: [ALICE, BOB, CHARLIE]
```

### `reduce()` – Aggregate elements
```java
int sum = Stream.of(1, 2, 3, 4, 5)
                .reduce(0, Integer::sum);
System.out.println(sum); // Output: 15
```

### `min()` and `max()` – Get min/max element
```java
int min = Stream.of(5, 3, 8, 1, 2).min(Integer::compareTo).get();
int max = Stream.of(5, 3, 8, 1, 2).max(Integer::compareTo).get();
System.out.println(min); // Output: 1
System.out.println(max); // Output: 8
```

### `anyMatch()`, `allMatch()`, `noneMatch()` – Match elements with conditions
```java
boolean anyEven = Stream.of(1, 2, 3, 4).anyMatch(n -> n % 2 == 0); // true
boolean allEven = Stream.of(2, 4, 6).allMatch(n -> n % 2 == 0); // true
boolean noneNegative = Stream.of(1, 2, 3).noneMatch(n -> n < 0); // true
```

## 4. Parallel Streams

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
numbers.parallelStream()
       .forEach(System.out::println); // Execution order may vary
```

## 5. Grouping and Partitioning

### Grouping Elements
```java
import java.util.Map;
import java.util.List;
import java.util.stream.Collectors;

Map<Integer, List<String>> grouped = List.of("apple", "banana", "cherry", "date")
        .stream()
        .collect(Collectors.groupingBy(String::length));
System.out.println(grouped); // Output: {5=[apple], 6=[banana, cherry], 4=[date]}
```

### Partitioning Elements
```java
Map<Boolean, List<Integer>> partitioned = List.of(1, 2, 3, 4, 5, 6)
        .stream()
        .collect(Collectors.partitioningBy(n -> n % 2 == 0));
System.out.println(partitioned); // Output: {false=[1, 3, 5], true=[2, 4, 6]}
```

## Summary

| Operation      | Description |
|---------------|-------------|
| `stream()`    | Create a stream from a collection |
| `map()`       | Transform each element |
| `filter()`    | Filter elements based on a condition |
| `sorted()`    | Sort elements |
| `distinct()`  | Remove duplicates |
| `limit()`     | Limit number of elements |
| `skip()`      | Skip first N elements |
| `forEach()`   | Process each element |
| `count()`     | Count elements |
| `collect()`   | Convert stream to collection |
| `reduce()`    | Aggregate elements |
| `min()/max()` | Find min/max element |
| `anyMatch()`  | Check if any element matches condition |
| `allMatch()`  | Check if all elements match condition |
| `noneMatch()` | Check if no elements match condition |
| `parallelStream()` | Process elements in parallel |

**Note:** Streams are **lazy**, meaning operations are executed only when a terminal operation is called.

---
