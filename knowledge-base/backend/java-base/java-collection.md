# Java 集合框架

## 1. 核心概述（官方定义）

Java 集合框架（Java Collections Framework）是 Java 平台提供的一套用于存储和操作对象组的接口和类。它提供了高性能、高质量的数据结构实现，以及算法支持。集合框架位于 `java.util` 包中。

### 核心接口
- **Collection**：集合层次结构的根接口
- **List**：有序集合，允许重复元素
- **Set**：不允许重复元素的集合
- **Map**：键值对映射
- **Queue**：队列，FIFO 数据结构
- **Deque**：双端队列

---

## 2. 核心知识点（结构化层级）

### 2.1 Collection 接口

#### 基本方法
- add(E e)：添加元素
- remove(Object o)：删除元素
- contains(Object o)：检查是否包含
- size()：获取大小
- isEmpty()：检查是否为空
- clear()：清空集合
- iterator()：获取迭代器

#### 批量操作
- addAll(Collection<? extends E> c)
- removeAll(Collection<?> c)
- retainAll(Collection<?> c)
- containsAll(Collection<?> c)

### 2.2 List 接口

#### 特性
- 有序集合
- 允许重复元素
- 可以通过索引访问
- 可以插入和删除元素

#### 实现类
- **ArrayList**：动态数组，随机访问快
- **LinkedList**：双向链表，插入删除快
- **Vector**：同步的动态数组（已过时）
- **Stack**：栈，LIFO（已过时）

#### 特有方法
- get(int index)：获取指定位置元素
- set(int index, E element)：替换元素
- add(int index, E element)：插入元素
- remove(int index)：删除指定位置元素
- indexOf(Object o)：查找元素索引
- lastIndexOf(Object o)：查找最后出现的索引
- subList(int from, int to)：获取子列表
- listIterator()：列表迭代器

### 2.3 Set 接口

#### 特性
- 不允许重复元素
- 最多包含一个 null
- 不保证顺序

#### 实现类
- **HashSet**：基于哈希表，无序
- **LinkedHashSet**：哈希表 + 链表，保持插入顺序
- **TreeSet**：基于红黑树，有序

### 2.4 SortedSet 接口

#### 特性
- 有序的 Set
- 元素按自然顺序或 Comparator 排序

#### 实现类
- **TreeSet**

#### 特有方法
- first()：第一个元素
- last()：最后一个元素
- headSet(E to)：小于指定元素的子集
- tailSet(E from)：大于等于指定元素的子集
- subSet(E from, E to)：区间子集

### 2.5 Map 接口

#### 特性
- 键值对映射
- 键不能重复
- 每个键最多映射一个值

#### 实现类
- **HashMap**：基于哈希表，无序
- **LinkedHashMap**：哈希表 + 链表，保持插入顺序
- **TreeMap**：基于红黑树，有序
- **Hashtable**：同步的哈希表（已过时）

#### 基本方法
- put(K key, V value)：添加键值对
- get(Object key)：获取值
- remove(Object key)：删除键值对
- containsKey(Object key)：检查键是否存在
- containsValue(Object value)：检查值是否存在
- size()：获取大小
- isEmpty()：检查是否为空
- clear()：清空
- keySet()：获取键集合
- values()：获取值集合
- entrySet()：获取键值对集合

### 2.6 SortedMap 接口

#### 特性
- 有序的 Map
- 按键排序

#### 实现类
- **TreeMap**

#### 特有方法
- firstKey()：第一个键
- lastKey()：最后一个键
- headMap(K toKey)：小于指定键的子映射
- tailMap(K fromKey)：大于等于指定键的子映射
- subMap(K from, K to)：区间子映射

### 2.7 Queue 接口

#### 特性
- FIFO（先进先出）数据结构
- 在尾部添加，在头部移除

#### 实现类
- **LinkedList**：实现了 Deque
- **PriorityQueue**：优先级队列
- **ArrayDeque**：双端队列

#### 基本方法
- add(E e)：添加元素（失败抛异常）
- offer(E e)：添加元素（失败返回 false）
- remove()：移除元素（失败抛异常）
- poll()：移除元素（失败返回 null）
- element()：获取头部（失败抛异常）
- peek()：获取头部（失败返回 null）

### 2.8 Deque 接口

#### 特性
- 双端队列
- 可以在两端添加和移除元素
- 可以作为栈使用

#### 实现类
- **LinkedList**
- **ArrayDeque**

#### 特有方法
- addFirst(E e)：在头部添加
- addLast(E e)：在尾部添加
- offerFirst(E e)：在头部添加（返回 boolean）
- offerLast(E e)：在尾部添加（返回 boolean）
- removeFirst()：移除头部（抛异常）
- removeLast()：移除尾部（抛异常）
- pollFirst()：移除头部（返回 null）
- pollLast()：移除尾部（返回 null）
- getFirst()：获取头部（抛异常）
- getLast()：获取尾部（抛异常）
- peekFirst()：获取头部（返回 null）
- peekLast()：获取尾部（返回 null）
- push(E e)：压入栈（头部）
- pop()：弹出栈（头部）

### 2.9 迭代器

#### Iterator
- hasNext()：检查是否有下一个元素
- next()：获取下一个元素
- remove()：移除当前元素

#### ListIterator
- 继承 Iterator
- 可以向前遍历
- 可以添加、替换元素
- previous()：获取前一个元素
- hasPrevious()：检查是否有前一个元素
- add(E e)：添加元素
- set(E e)：替换元素
- nextIndex()：下一个索引
- previousIndex()：前一个索引

### 2.10 工具类

#### Collections
- 提供静态工具方法
- sort(List)：排序
- shuffle(List)：打乱
- reverse(List)：反转
- swap(List, int, int)：交换
- fill(List, Object)：填充
- copy(List, List)：复制
- min(Collection)：最小值
- max(Collection)：最大值
- frequency(Collection, Object)：频率
- disjoint(Collection, Collection)：不相交检查
- addAll(Collection, T...)：添加多个元素
- unmodifiableXxx()：不可修改视图
- synchronizedXxx()：同步视图
- checkedXxx()：类型安全视图
- emptyXxx()：空集合
- singletonXxx()：单元素集合
- nCopies(int, T)：n 个副本

#### Arrays
- 提供数组工具方法
- asList(T...)：数组转 List
- sort()：排序
- binarySearch()：二分查找
- equals()：比较数组
- fill()：填充
- copyOf()：复制
- copyOfRange()：范围复制
- toString()：数组转字符串
- deepToString()：多维数组转字符串
- hashCode()：数组哈希码
- deepHashCode()：多维数组哈希码

---

## 3. 官方标准语法 / API

### 3.1 List 使用

```java
import java.util.ArrayList;
import java.util.List;
import java.util.LinkedList;

// ArrayList
List<String> arrayList = new ArrayList<>();
arrayList.add("Apple");
arrayList.add("Banana");
arrayList.add("Orange");

String first = arrayList.get(0);
arrayList.set(1, "Blueberry");
arrayList.add(1, "Mango");
arrayList.remove(2);
boolean contains = arrayList.contains("Apple");
int size = arrayList.size();

// 遍历
for (String fruit : arrayList) {
    System.out.println(fruit);
}

// 使用迭代器
Iterator<String> iterator = arrayList.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}

// LinkedList
List<String> linkedList = new LinkedList<>();
linkedList.add("A");
linkedList.add("B");
linkedList.add("C");
```

### 3.2 Set 使用

```java
import java.util.Set;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.TreeSet;

// HashSet
Set<String> hashSet = new HashSet<>();
hashSet.add("Apple");
hashSet.add("Banana");
hashSet.add("Apple"); // 重复元素，会被忽略
hashSet.add("Orange");

boolean hasApple = hashSet.contains("Apple");
hashSet.remove("Banana");

// 遍历
for (String fruit : hashSet) {
    System.out.println(fruit);
}

// LinkedHashSet（保持插入顺序）
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("First");
linkedHashSet.add("Second");
linkedHashSet.add("Third");

// TreeSet（自然排序）
Set<Integer> treeSet = new TreeSet<>();
treeSet.add(5);
treeSet.add(2);
treeSet.add(8);
treeSet.add(1);
// 输出：1, 2, 5, 8
```

### 3.3 Map 使用

```java
import java.util.Map;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.TreeMap;

// HashMap
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Apple", 10);
hashMap.put("Banana", 20);
hashMap.put("Orange", 15);

int appleCount = hashMap.get("Apple");
hashMap.put("Apple", 12); // 更新值
hashMap.remove("Banana");
boolean hasOrange = hashMap.containsKey("Orange");
boolean has10 = hashMap.containsValue(10);

// 遍历键
for (String key : hashMap.keySet()) {
    System.out.println(key + ": " + hashMap.get(key));
}

// 遍历值
for (Integer value : hashMap.values()) {
    System.out.println(value);
}

// 遍历键值对
for (Map.Entry<String, Integer> entry : hashMap.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// 使用 forEach
hashMap.forEach((key, value) -> 
    System.out.println(key + ": " + value)
);

// LinkedHashMap（保持插入顺序）
Map<String, String> linkedHashMap = new LinkedHashMap<>();
linkedHashMap.put("a", "First");
linkedHashMap.put("b", "Second");

// TreeMap（按键排序）
Map<Integer, String> treeMap = new TreeMap<>();
treeMap.put(3, "Three");
treeMap.put(1, "One");
treeMap.put(2, "Two");
// 按 1, 2, 3 顺序
```

### 3.4 Queue 和 Deque 使用

```java
import java.util.Queue;
import java.util.LinkedList;
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.PriorityQueue;

// Queue（FIFO）
Queue<String> queue = new LinkedList<>();
queue.offer("First");
queue.offer("Second");
queue.offer("Third");

String first = queue.peek(); // 获取但不移除
String removed = queue.poll(); // 获取并移除

// PriorityQueue（优先级队列）
Queue<Integer> priorityQueue = new PriorityQueue<>();
priorityQueue.offer(5);
priorityQueue.offer(2);
priorityQueue.offer(8);
priorityQueue.offer(1);
// 按 1, 2, 5, 8 顺序出队

// Deque（双端队列）
Deque<String> deque = new ArrayDeque<>();
deque.addFirst("First");
deque.addLast("Last");
deque.offerFirst("New First");
deque.offerLast("New Last");

String head = deque.peekFirst();
String tail = deque.peekLast();
String removedFirst = deque.pollFirst();
String removedLast = deque.pollLast();

// 作为栈使用
Deque<String> stack = new ArrayDeque<>();
stack.push("A");
stack.push("B");
stack.push("C");
String top = stack.pop(); // "C"
```

### 3.5 Collections 工具类

```java
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;
import java.util.Set;
import java.util.HashSet;

List<Integer> list = new ArrayList<>();
list.add(5);
list.add(2);
list.add(8);
list.add(1);

// 排序
Collections.sort(list); // [1, 2, 5, 8]

// 打乱
Collections.shuffle(list);

// 反转
Collections.reverse(list);

// 填充
Collections.fill(list, 0); // [0, 0, 0, 0]

// 最大值、最小值
Integer max = Collections.max(list);
Integer min = Collections.min(list);

// 频率
int freq = Collections.frequency(list, 5);

// 添加多个元素
Collections.addAll(list, 1, 2, 3, 4, 5);

// 不可修改的集合
List<Integer> unmodifiableList = Collections.unmodifiableList(list);
Set<Integer> unmodifiableSet = Collections.unmodifiableSet(new HashSet<>());

// 同步集合
List<String> synchronizedList = Collections.synchronizedList(new ArrayList<>());

// 空集合
List<String> emptyList = Collections.emptyList();
Set<String> emptySet = Collections.emptySet();
Map<String, String> emptyMap = Collections.emptyMap();

// 单元素集合
List<String> singletonList = Collections.singletonList("Only One");
Set<String> singletonSet = Collections.singleton("Only One");
```

### 3.6 Arrays 工具类

```java
import java.util.Arrays;
import java.util.List;

// 数组排序
int[] numbers = {5, 2, 8, 1, 9};
Arrays.sort(numbers); // [1, 2, 5, 8, 9]

// 二分查找
int index = Arrays.binarySearch(numbers, 5); // 2

// 比较数组
int[] array1 = {1, 2, 3};
int[] array2 = {1, 2, 3};
boolean equal = Arrays.equals(array1, array2); // true

// 填充数组
int[] filled = new int[5];
Arrays.fill(filled, 42); // [42, 42, 42, 42, 42]

// 复制数组
int[] original = {1, 2, 3, 4, 5};
int[] copy = Arrays.copyOf(original, 3); // [1, 2, 3]
int[] rangeCopy = Arrays.copyOfRange(original, 1, 4); // [2, 3, 4]

// 数组转字符串
System.out.println(Arrays.toString(original)); // [1, 2, 3, 4, 5]

// 多维数组转字符串
int[][] matrix = {{1, 2}, {3, 4}};
System.out.println(Arrays.deepToString(matrix)); // [[1, 2], [3, 4]]

// 数组转 List
String[] stringArray = {"A", "B", "C"};
List<String> list = Arrays.asList(stringArray);
// 注意：返回的 List 是固定大小的，不能添加或删除元素

// 创建可变的 List
List<String> mutableList = new ArrayList<>(Arrays.asList(stringArray));
```

---

## 4. 官方示例代码（可运行）

### 4.1 学生管理系统

```java
import java.util.*;

class Student implements Comparable<Student> {
    private int id;
    private String name;
    private double score;

    public Student(int id, String name, double score) {
        this.id = id;
        this.name = name;
        this.score = score;
    }

    public int getId() { return id; }
    public String getName() { return name; }
    public double getScore() { return score; }

    @Override
    public int compareTo(Student other) {
        return Double.compare(this.score, other.score);
    }

    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "', score=" + score + "}";
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return id == student.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}

public class StudentManager {
    
    private List<Student> studentList = new ArrayList<>();
    private Set<Student> studentSet = new HashSet<>();
    private Map<Integer, Student> studentMap = new HashMap<>();

    public void addStudent(Student student) {
        studentList.add(student);
        studentSet.add(student);
        studentMap.put(student.getId(), student);
    }

    public Student findStudentById(int id) {
        return studentMap.get(id);
    }

    public List<Student> sortStudentsByScore() {
        List<Student> sortedList = new ArrayList<>(studentList);
        Collections.sort(sortedList);
        return sortedList;
    }

    public List<Student> sortStudentsByName() {
        List<Student> sortedList = new ArrayList<>(studentList);
        Collections.sort(sortedList, (s1, s2) -> 
            s1.getName().compareTo(s2.getName())
        );
        return sortedList;
    }

    public double getAverageScore() {
        if (studentList.isEmpty()) return 0.0;
        double sum = 0.0;
        for (Student s : studentList) {
            sum += s.getScore();
        }
        return sum / studentList.size();
    }

    public Student getTopStudent() {
        if (studentList.isEmpty()) return null;
        return Collections.max(studentList);
    }

    public static void main(String[] args) {
        StudentManager manager = new StudentManager();
        
        manager.addStudent(new Student(1, "Alice", 85.5));
        manager.addStudent(new Student(2, "Bob", 92.0));
        manager.addStudent(new Student(3, "Charlie", 78.5));
        manager.addStudent(new Student(4, "Diana", 95.0));

        System.out.println("按分数排序:");
        for (Student s : manager.sortStudentsByScore()) {
            System.out.println(s);
        }

        System.out.println("\n按姓名排序:");
        for (Student s : manager.sortStudentsByName()) {
            System.out.println(s);
        }

        System.out.println("\n平均分: " + manager.getAverageScore());
        System.out.println("最高分学生: " + manager.getTopStudent());
        System.out.println("查找 ID=2 的学生: " + manager.findStudentById(2));
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 选择合适的集合

1. **List**
   - 需要随机访问：使用 ArrayList
   - 需要频繁插入删除：使用 LinkedList
   - 需要线程安全：使用 Collections.synchronizedList() 或 CopyOnWriteArrayList

2. **Set**
   - 不需要顺序：使用 HashSet
   - 需要保持插入顺序：使用 LinkedHashSet
   - 需要排序：使用 TreeSet

3. **Map**
   - 不需要顺序：使用 HashMap
   - 需要保持插入顺序：使用 LinkedHashMap
   - 需要排序：使用 TreeMap

4. **Queue/Deque**
   - FIFO：使用 LinkedList 或 ArrayDeque
   - 需要优先级：使用 PriorityQueue
   - 需要栈：使用 ArrayDeque

### 5.2 性能考虑

1. **ArrayList vs LinkedList**
   - ArrayList 随机访问 O(1)，插入删除 O(n)
   - LinkedList 随机访问 O(n)，插入删除 O(1)
   - ArrayList 内存占用小，LinkedList 内存占用大

2. **HashMap vs TreeMap**
   - HashMap 插入、删除、查找 O(1)
   - TreeMap 插入、删除、查找 O(log n)
   - TreeMap 提供有序访问

3. **初始容量**
   - 预估集合大小，设置合适的初始容量
   - 避免频繁扩容
   - ArrayList 默认容量 10，HashMap 默认容量 16

### 5.3 线程安全

1. **Collections.synchronizedXxx()**
   - 返回同步包装器
   - 方法级同步，性能较低
   - 适合低并发场景

2. **java.util.concurrent 包**
   - CopyOnWriteArrayList
   - ConcurrentHashMap
   - ConcurrentSkipListMap
   - ConcurrentSkipListSet
   - 性能更好，适合高并发

### 5.4 最佳实践

1. **使用接口类型**
   - 声明变量使用接口类型
   - 提高代码灵活性
   - List<String> list = new ArrayList<>();

2. **泛型**
   - 使用泛型确保类型安全
   - 避免类型转换错误
   - List<String> 而非 List

3. **不可修改的视图**
   - 使用 Collections.unmodifiableXxx()
   - 防止意外修改
   - 提供只读访问

4. **空集合 vs null**
   - 返回空集合而非 null
   - 避免 NullPointerException
   - 使用 Collections.emptyXxx()

5. **遍历方式**
   - for-each 循环优先
   - 需要删除元素时使用迭代器
   - 避免在遍历时修改集合

---

## 6. 官方文档参考链接

- [Java 集合框架官方文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/package-summary.html)
- [Java 集合教程](https://docs.oracle.com/javase/tutorial/collections/)
- [List 接口文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/List.html)
- [Set 接口文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Set.html)
- [Map 接口文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Map.html)
- [Collections 工具类](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collections.html)
- [Arrays 工具类](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html)
