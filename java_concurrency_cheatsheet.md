# Java Concurrency in Practice Cheat Sheet

## 1. Thread Safety & Synchronization

### **synchronized Keyword (JDK 1.0)**
```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

### **volatile Keyword (JDK 1.2)**
```java
class VolatileExample {
    private volatile boolean running = true;

    void stop() {
        running = false;
    }

    void run() {
        while (running) {
            // Do some work
        }
    }
}
```

### **wait(), notify(), notifyAll() (JDK 1.0)**
```java
class SharedResource {
    private boolean available = false;

    public synchronized void produce() throws InterruptedException {
        while (available) {
            wait();
        }
        available = true;
        notifyAll();
    }

    public synchronized void consume() throws InterruptedException {
        while (!available) {
            wait();
        }
        available = false;
        notifyAll();
    }
}
```

## 2. Executors & Thread Pools (JDK 1.5)

### **ThreadPoolExecutor**
```java
import java.util.concurrent.*;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        executor.submit(() -> System.out.println("Task executed by " + Thread.currentThread().getName()));
        executor.shutdown();
    }
}
```

## 3. Concurrent Collections (JDK 1.5)

### **ConcurrentHashMap**
```java
import java.util.concurrent.ConcurrentHashMap;

public class ConcurrentMapExample {
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("A", 1);
        System.out.println(map.get("A"));
    }
}
```

### **CopyOnWriteArrayList**
```java
import java.util.concurrent.CopyOnWriteArrayList;

public class CopyOnWriteListExample {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("Hello");
        System.out.println(list.get(0));
    }
}
```

## 4. Locks & Synchronization Mechanisms (JDK 1.5, 1.8)

### **ReentrantLock**
```java
import java.util.concurrent.locks.ReentrantLock;

class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void accessResource() {
        lock.lock();
        try {
            System.out.println("Resource accessed by " + Thread.currentThread().getName());
        } finally {
            lock.unlock();
        }
    }
}
```

### **StampedLock (JDK 1.8)**
```java
import java.util.concurrent.locks.StampedLock;

class StampedLockExample {
    private final StampedLock lock = new StampedLock();
    private int data = 0;

    public void write(int value) {
        long stamp = lock.writeLock();
        try {
            data = value;
        } finally {
            lock.unlockWrite(stamp);
        }
    }

    public int read() {
        long stamp = lock.readLock();
        try {
            return data;
        } finally {
            lock.unlockRead(stamp);
        }
    }
}
```

## 5. Atomic Variables (JDK 1.5, 1.8)

### **AtomicInteger**
```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicExample {
    private static final AtomicInteger counter = new AtomicInteger(0);

    public static void main(String[] args) {
        counter.incrementAndGet();
        System.out.println(counter.get());
    }
}
```

### **LongAdder (JDK 1.8)**
```java
import java.util.concurrent.atomic.LongAdder;

public class LongAdderExample {
    public static void main(String[] args) {
        LongAdder adder = new LongAdder();
        adder.increment();
        System.out.println(adder.sum());
    }
}
```

## 6. Future & Async Programming (JDK 1.5, 1.8)

### **Future with Callable**
```java
import java.util.concurrent.*;

public class FutureExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<Integer> future = executor.submit(() -> 42);
        System.out.println("Result: " + future.get());
        executor.shutdown();
    }
}
```

### **CompletableFuture (JDK 1.8)**
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture.supplyAsync(() -> "Hello").thenAccept(System.out::println);
    }
}
```

## 7. Fork/Join Framework (JDK 1.7, 1.8)

### **ForkJoinPool**
```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

class SumTask extends RecursiveTask<Integer> {
    private final int[] arr;
    private final int start, end;

    public SumTask(int[] arr, int start, int end) {
        this.arr = arr;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= 10) {
            int sum = 0;
            for (int i = start; i < end; i++) sum += arr[i];
            return sum;
        }
        int mid = (start + end) / 2;
        SumTask left = new SumTask(arr, start, mid);
        SumTask right = new SumTask(arr, mid, end);
        left.fork();
        return right.compute() + left.join();
    }
}

public class ForkJoinExample {
    public static void main(String[] args) {
        int[] arr = new int[100];
        ForkJoinPool pool = new ForkJoinPool();
        int sum = pool.invoke(new SumTask(arr, 0, arr.length));
        System.out.println("Sum: " + sum);
    }
}