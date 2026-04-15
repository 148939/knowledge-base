# Java 并发与线程

## 1. 核心概述（官方定义）

Java 并发编程是 Java 平台提供的用于处理多线程和并发操作的 API。Java 从诞生之初就内置了对多线程的支持，提供了丰富的并发工具类和框架。

### 核心概念
- **线程（Thread）**：程序执行的最小单元
- **进程（Process）**：程序运行的实例
- **并行（Parallelism）**：同时执行多个任务
- **并发（Concurrency）**：交替执行多个任务
- **线程安全**：多线程环境下正确执行
- **同步**：协调多个线程的执行顺序

---

## 2. 核心知识点（结构化层级）

### 2.1 线程基础

#### Thread 类
- 创建线程
- 启动线程
- 线程状态
- 线程优先级
- 线程命名

#### Runnable 接口
- 函数式接口
- 与 Thread 配合使用
- lambda 表达式

#### Callable 接口
- 有返回值
- 可抛出异常
- 与 Future 配合使用

### 2.2 线程生命周期

#### 线程状态
- NEW：新建
- RUNNABLE：可运行
- BLOCKED：阻塞
- WAITING：等待
- TIMED_WAITING：定时等待
- TERMINATED：终止

#### 状态转换
- start()：NEW → RUNNABLE
- wait()：RUNNABLE → WAITING
- sleep()：RUNNABLE → TIMED_WAITING
- join()：等待其他线程
- interrupt()：中断线程

### 2.3 线程同步

#### synchronized 关键字
- 同步方法
- 同步代码块
- 静态同步
- 锁的对象

#### volatile 关键字
- 可见性
- 禁止指令重排序
- 不保证原子性

#### 锁对象
- Object 的 wait()、notify()、notifyAll()
- 等待/通知机制
- 生产者/消费者模式

### 2.4 并发工具类

#### java.util.concurrent 包
- Executor 框架
- 线程池
- Future 和 FutureTask
- CompletableFuture

#### 原子类
- AtomicInteger
- AtomicLong
- AtomicBoolean
- AtomicReference
- CAS（Compare And Swap）

#### 并发集合
- ConcurrentHashMap
- CopyOnWriteArrayList
- CopyOnWriteArraySet
- ConcurrentLinkedQueue
- BlockingQueue

#### 同步工具
- CountDownLatch
- CyclicBarrier
- Semaphore
- Exchanger
- Phaser

### 2.5 锁框架

#### Lock 接口
- ReentrantLock
- 公平锁与非公平锁
- tryLock()
- lockInterruptibly()
- Condition

#### ReadWriteLock
- ReentrantReadWriteLock
- 读锁共享
- 写锁排他
- 适用场景

#### StampedLock
- 乐观读
- 悲观读/写
- 性能优化

### 2.6 线程池

#### ThreadPoolExecutor
- 核心参数
- 工作队列
- 拒绝策略
- 线程工厂

#### 预定义线程池
- Executors.newFixedThreadPool()
- Executors.newCachedThreadPool()
- Executors.newSingleThreadExecutor()
- Executors.newScheduledThreadPool()

#### 线程池监控
- getActiveCount()
- getPoolSize()
- getCompletedTaskCount()
- 关闭线程池

### 2.7 并发设计模式

#### 不变模式
- 不可变对象
- final 关键字
- 线程安全

#### 生产者/消费者
- wait/notify
- BlockingQueue
- 解耦生产和消费

#### Future 模式
- 异步执行
- 结果获取
- CompletableFuture

#### 读写锁模式
- 读多写少
- 性能优化
- ReentrantReadWriteLock

### 2.8 并发问题

#### 竞态条件
- 原子性
- i++ 问题
- 解决方案

#### 死锁
- 产生条件
- 避免死锁
- 检测死锁

#### 活锁
- 概念
- 与死锁的区别
- 解决方案

#### 饥饿
- 线程饥饿
- 公平锁
- 优先级问题

---

## 3. 官方标准语法 / API

### 3.1 创建线程

```java
// 继承 Thread 类
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running: " + getName());
    }
}

// 使用
Thread thread1 = new MyThread();
thread1.start();

// 实现 Runnable 接口
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable running");
    }
}

// 使用
Thread thread2 = new Thread(new MyRunnable());
thread2.start();

// 使用 lambda 表达式
Thread thread3 = new Thread(() -> {
    System.out.println("Lambda thread running");
});
thread3.start();

// 使用 Callable 和 Future
Callable<String> callable = () -> {
    Thread.sleep(1000);
    return "Callable result";
};

FutureTask<String> futureTask = new FutureTask<>(callable);
Thread thread4 = new Thread(futureTask);
thread4.start();

// 获取结果
String result = futureTask.get();
System.out.println(result);
```

### 3.2 线程基本操作

```java
// 线程信息
Thread currentThread = Thread.currentThread();
System.out.println("Thread name: " + currentThread.getName());
System.out.println("Thread ID: " + currentThread.getId());
System.out.println("Thread priority: " + currentThread.getPriority());
System.out.println("Thread state: " + currentThread.getState());

// 线程休眠
Thread.sleep(1000); // 休眠 1 秒

// 线程让步
Thread.yield(); // 让出 CPU 时间片

// 线程加入
Thread otherThread = new Thread(() -> {
    try {
        Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
otherThread.start();
otherThread.join(); // 等待 otherThread 执行完成

// 线程中断
Thread thread = new Thread(() -> {
    while (!Thread.interrupted()) {
        // 执行任务
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt(); // 重新设置中断状态
            break;
        }
    }
});
thread.start();
Thread.sleep(1000);
thread.interrupt(); // 中断线程

// 守护线程
Thread daemonThread = new Thread(() -> {
    while (true) {
        // 后台任务
    }
});
daemonThread.setDaemon(true); // 设置为守护线程
daemonThread.start();
```

### 3.3 synchronized 关键字

```java
// 同步实例方法
class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// 同步静态方法
class StaticCounter {
    private static int count = 0;
    
    public static synchronized void increment() {
        count++;
    }
}

// 同步代码块
class BankAccount {
    private int balance;
    private Object lock = new Object();
    
    public void deposit(int amount) {
        synchronized (lock) {
            balance += amount;
        }
    }
    
    public void withdraw(int amount) {
        synchronized (this) {
            balance -= amount;
        }
    }
}

// 等待/通知机制
class ProducerConsumer {
    private List<Integer> buffer = new ArrayList<>();
    private int capacity = 10;
    
    public synchronized void produce(int item) throws InterruptedException {
        while (buffer.size() == capacity) {
            wait(); // 等待消费者消费
        }
        buffer.add(item);
        notifyAll(); // 通知消费者
    }
    
    public synchronized int consume() throws InterruptedException {
        while (buffer.isEmpty()) {
            wait(); // 等待生产者生产
        }
        int item = buffer.remove(0);
        notifyAll(); // 通知生产者
        return item;
    }
}
```

### 3.4 volatile 关键字

```java
// 可见性示例
class VisibilityExample {
    private volatile boolean flag = false;
    
    public void writer() {
        flag = true; // 写操作
    }
    
    public void reader() {
        while (!flag) {
            // 等待 flag 变为 true
        }
        System.out.println("Flag is true now");
    }
}

// 双重检查锁定单例模式
class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) { // 第一次检查
            synchronized (Singleton.class) {
                if (instance == null) { // 第二次检查
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// 注意：volatile 不能保证原子性
class AtomicProblem {
    private volatile int count = 0;
    
    public void increment() {
        count++; // 这不是原子操作！
    }
}
```

### 3.5 原子类

```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;
import java.util.concurrent.atomic.AtomicReference;

// AtomicInteger
class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // 原子递增
    }
    
    public int getCount() {
        return count.get();
    }
}

// AtomicInteger 的常用方法
AtomicInteger atomicInt = new AtomicInteger(0);
atomicInt.incrementAndGet(); // ++i
atomicInt.getAndIncrement(); // i++
atomicInt.decrementAndGet(); // --i
atomicInt.getAndDecrement(); // i--
atomicInt.addAndGet(5); // i += 5
atomicInt.getAndAdd(5); // temp = i; i +=5; return temp
atomicInt.compareAndSet(0, 10); // CAS 操作

// AtomicReference
class User {
    private String name;
    private int age;
    
    // constructor, getters, setters
}

AtomicReference<User> atomicUser = new AtomicReference<>();
User newUser = new User("Alice", 30);
atomicUser.set(newUser);
User currentUser = atomicUser.get();
```

### 3.6 并发集合

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

// ConcurrentHashMap
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("key1", 1);
map.put("key2", 2);
int value = map.get("key1");
map.putIfAbsent("key3", 3); // 原子操作

// CopyOnWriteArrayList
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
list.add("A");
list.add("B");
list.remove("A");

// BlockingQueue - 生产者/消费者
BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(10);

// 生产者
Thread producer = new Thread(() -> {
    try {
        for (int i = 0; i < 100; i++) {
            queue.put(i); // 队列满时阻塞
            System.out.println("Produced: " + i);
            Thread.sleep(100);
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});

// 消费者
Thread consumer = new Thread(() -> {
    try {
        for (int i = 0; i < 100; i++) {
            int item = queue.take(); // 队列空时阻塞
            System.out.println("Consumed: " + item);
            Thread.sleep(150);
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});

producer.start();
consumer.start();
```

### 3.7 线程池

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

// 固定大小线程池
ExecutorService fixedPool = Executors.newFixedThreadPool(5);

// 缓存线程池
ExecutorService cachedPool = Executors.newCachedThreadPool();

// 单线程池
ExecutorService singlePool = Executors.newSingleThreadExecutor();

// 提交任务
for (int i = 0; i < 10; i++) {
    final int taskId = i;
    fixedPool.submit(() -> {
        System.out.println("Task " + taskId + " running");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
}

// 关闭线程池
fixedPool.shutdown();
try {
    if (!fixedPool.awaitTermination(60, TimeUnit.SECONDS)) {
        fixedPool.shutdownNow();
    }
} catch (InterruptedException e) {
    fixedPool.shutdownNow();
}

// 自定义 ThreadPoolExecutor
ThreadPoolExecutor customPool = new ThreadPoolExecutor(
    2, // 核心线程数
    5, // 最大线程数
    60L, TimeUnit.SECONDS, // 空闲线程存活时间
    new LinkedBlockingQueue<>(100), // 工作队列
    new ThreadPoolExecutor.CallerRunsPolicy() // 拒绝策略
);
```

### 3.8 Lock 接口

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

// ReentrantLock
class LockCounter {
    private int count = 0;
    private Lock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock(); // 获取锁
        try {
            count++;
        } finally {
            lock.unlock(); // 释放锁
        }
    }
    
    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}

// 使用 Condition
class BoundedBuffer {
    private final Lock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();
    
    private final Object[] items = new Object[10];
    private int putptr, takeptr, count;
    
    public void put(Object x) throws InterruptedException {
        lock.lock();
        try {
            while (count == items.length) {
                notFull.await(); // 等待缓冲区不满
            }
            items[putptr] = x;
            if (++putptr == items.length) putptr = 0;
            ++count;
            notEmpty.signal(); // 通知缓冲区非空
        } finally {
            lock.unlock();
        }
    }
    
    public Object take() throws InterruptedException {
        lock.lock();
        try {
            while (count == 0) {
                notEmpty.await(); // 等待缓冲区非空
            }
            Object x = items[takeptr];
            if (++takeptr == items.length) takeptr = 0;
            --count;
            notFull.signal(); // 通知缓冲区不满
            return x;
        } finally {
            lock.unlock();
        }
    }
}
```

### 3.9 同步工具类

```java
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.Semaphore;

// CountDownLatch - 等待多个线程完成
CountDownLatch latch = new CountDownLatch(3);

for (int i = 0; i < 3; i++) {
    final int threadId = i;
    new Thread(() -> {
        try {
            System.out.println("Thread " + threadId + " working");
            Thread.sleep(1000);
            latch.countDown(); // 计数器减 1
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }).start();
}

latch.await(); // 等待计数器变为 0
System.out.println("All threads completed");

// CyclicBarrier - 等待多个线程到达屏障
CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All threads reached the barrier");
});

for (int i = 0; i < 3; i++) {
    final int threadId = i;
    new Thread(() -> {
        try {
            System.out.println("Thread " + threadId + " waiting at barrier");
            barrier.await(); // 等待其他线程
            System.out.println("Thread " + threadId + " passed barrier");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }).start();
}

// Semaphore - 控制并发访问数
Semaphore semaphore = new Semaphore(2); // 最多 2 个线程同时访问

for (int i = 0; i < 5; i++) {
    final int threadId = i;
    new Thread(() -> {
        try {
            semaphore.acquire(); // 获取许可
            System.out.println("Thread " + threadId + " acquired permit");
            Thread.sleep(1000);
            semaphore.release(); // 释放许可
            System.out.println("Thread " + threadId + " released permit");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }).start();
}
```

---

## 4. 官方示例代码（可运行）

### 4.1 线程安全的计数器

```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

// 使用 synchronized
class SynchronizedCounter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// 使用 ReentrantLock
class LockCounter {
    private int count = 0;
    private Lock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
    
    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}

// 使用 AtomicInteger
class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet();
    }
    
    public int getCount() {
        return count.get();
    }
}

// 测试
public class CounterTest {
    public static void main(String[] args) throws InterruptedException {
        final int THREAD_COUNT = 10;
        final int INCREMENT_PER_THREAD = 1000;
        
        // 测试 SynchronizedCounter
        SynchronizedCounter syncCounter = new SynchronizedCounter();
        Thread[] threads = new Thread[THREAD_COUNT];
        
        for (int i = 0; i < THREAD_COUNT; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < INCREMENT_PER_THREAD; j++) {
                    syncCounter.increment();
                }
            });
        }
        
        for (Thread thread : threads) {
            thread.start();
        }
        
        for (Thread thread : threads) {
            thread.join();
        }
        
        System.out.println("SynchronizedCounter: " + syncCounter.getCount());
        
        // 测试 AtomicCounter
        AtomicCounter atomicCounter = new AtomicCounter();
        threads = new Thread[THREAD_COUNT];
        
        for (int i = 0; i < THREAD_COUNT; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < INCREMENT_PER_THREAD; j++) {
                    atomicCounter.increment();
                }
            });
        }
        
        for (Thread thread : threads) {
            thread.start();
        }
        
        for (Thread thread : threads) {
            thread.join();
        }
        
        System.out.println("AtomicCounter: " + atomicCounter.getCount());
    }
}
```

### 4.2 生产者消费者模式

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

// 使用 BlockingQueue
class Producer implements Runnable {
    private BlockingQueue<Integer> queue;
    private int maxItems;
    
    public Producer(BlockingQueue<Integer> queue, int maxItems) {
        this.queue = queue;
        this.maxItems = maxItems;
    }
    
    @Override
    public void run() {
        try {
            for (int i = 0; i < maxItems; i++) {
                queue.put(i);
                System.out.println("Produced: " + i);
                Thread.sleep(100);
            }
            queue.put(-1); // 结束标记
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

class Consumer implements Runnable {
    private BlockingQueue<Integer> queue;
    
    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {
        try {
            while (true) {
                int item = queue.take();
                if (item == -1) {
                    break; // 结束
                }
                System.out.println("Consumed: " + item);
                Thread.sleep(150);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class ProducerConsumerTest {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
        int maxItems = 20;
        
        Thread producerThread = new Thread(new Producer(queue, maxItems));
        Thread consumerThread = new Thread(new Consumer(queue));
        
        producerThread.start();
        consumerThread.start();
    }
}
```

### 4.3 线程池使用示例

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.Callable;
import java.util.concurrent.TimeUnit;
import java.util.List;
import java.util.ArrayList;

// 任务类
class CalculationTask implements Callable<Long> {
    private int number;
    
    public CalculationTask(int number) {
        this.number = number;
    }
    
    @Override
    public Long call() throws Exception {
        System.out.println("Calculating for " + number + " in thread " + 
            Thread.currentThread().getName());
        Thread.sleep(1000);
        return (long) number * number;
    }
}

public class ThreadPoolExample {
    public static void main(String[] args) throws Exception {
        // 创建线程池
        ExecutorService executor = Executors.newFixedThreadPool(3);
        List<Future<Long>> futures = new ArrayList<>();
        
        // 提交任务
        for (int i = 1; i <= 10; i++) {
            Future<Long> future = executor.submit(new CalculationTask(i));
            futures.add(future);
        }
        
        // 获取结果
        long total = 0;
        for (Future<Long> future : futures) {
            total += future.get();
        }
        
        System.out.println("Total sum: " + total);
        
        // 关闭线程池
        executor.shutdown();
        if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
            executor.shutdownNow();
        }
    }
}
```

### 4.4 死锁示例与避免

```java
// 死锁示例
class DeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();
    
    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock 1");
                
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                
                System.out.println("Thread 1: Waiting for lock 2");
                synchronized (lock2) {
                    System.out.println("Thread 1: Holding lock 1 and 2");
                }
            }
        });
        
        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock 2");
                
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                
                System.out.println("Thread 2: Waiting for lock 1");
                synchronized (lock1) {
                    System.out.println("Thread 2: Holding lock 2 and 1");
                }
            }
        });
        
        thread1.start();
        thread2.start();
    }
}

// 避免死锁 - 固定锁顺序
class DeadlockAvoidance {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();
    
    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock 1");
                
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                
                System.out.println("Thread 1: Waiting for lock 2");
                synchronized (lock2) {
                    System.out.println("Thread 1: Holding lock 1 and 2");
                }
            }
        });
        
        Thread thread2 = new Thread(() -> {
            synchronized (lock1) { // 也先获取 lock1
                System.out.println("Thread 2: Holding lock 1");
                
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                
                System.out.println("Thread 2: Waiting for lock 2");
                synchronized (lock2) {
                    System.out.println("Thread 2: Holding lock 1 and 2");
                }
            }
        });
        
        thread1.start();
        thread2.start();
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 线程安全设计

1. **优先使用不可变对象**
   - 使用 final 关键字
   - 避免共享可变状态
   - 不可变对象天生线程安全

2. **最小化锁的范围**
   - 只锁必要的代码
   - 避免锁整个方法
   - 减少锁的持有时间

3. **使用高级并发工具**
   - 优先使用 java.util.concurrent
   - 避免手写同步代码
   - 使用原子类和并发集合

### 5.2 性能优化

1. **选择合适的锁**
   - 读多写少用 ReadWriteLock
   - 高竞争用 ReentrantLock
   - 乐观读取用 StampedLock

2. **使用线程池**
   - 避免频繁创建销毁线程
   - 合理配置线程池大小
   - 监控线程池状态

3. **减少上下文切换**
   - 合理设置线程数
   - 避免线程饥饿
   - 使用 CAS 代替锁

### 5.3 死锁避免

1. **固定锁顺序**
   - 所有线程按相同顺序获取锁
   - 避免循环等待

2. **使用定时锁**
   - tryLock() 带超时
   - 超时后释放已获取的锁

3. **死锁检测**
   - 使用工具检测死锁
   - ThreadMXBean 检测

### 5.4 线程池最佳实践

1. **合理配置参数**
   - 根据任务类型配置
   - CPU 密集型：N+1
   - I/O 密集型：2*N

2. **选择合适的队列**
   - 有界队列避免 OOM
   - 无界队列需谨慎
   - 优先级队列用于重要任务

3. **正确关闭线程池**
   - 使用 shutdown()
   - 等待终止
   - 必要时 shutdownNow()

### 5.5 并发编程常见陷阱

1. **原子性问题**
   - i++ 不是原子操作
   - 使用原子类
   - 使用锁保证原子性

2. **可见性问题**
   - 使用 volatile
   - 使用 synchronized
   - 使用锁

3. **指令重排序**
   - volatile 禁止重排序
   - final 禁止重排序
   - 锁保证有序性

---

## 6. 官方文档参考链接

- [Java 并发官方文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/package-summary.html)
- [Java 并发教程](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Thread 类文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Thread.html)
- [Executor 框架文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/Executor.html)
- [Lock 接口文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/locks/Lock.html)
- [原子类文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/atomic/package-summary.html)
- [《Java 并发编程实战》](https://jcip.net/)
