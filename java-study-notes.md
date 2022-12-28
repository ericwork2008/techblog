---
description: Study Notes for Java
---

# Java Study Notes

## 参考资料 <a href="#can-kao-zi-liao" id="can-kao-zi-liao"></a>

* [https://gitbook.cn/gitchat/column/5d493b4dcb702a087ef935d9](https://gitbook.cn/gitchat/column/5d493b4dcb702a087ef935d9)
* [https://www.programcreek.com/simple-java/](https://www.programcreek.com/simple-java/)
* [https://www.youtube.com/c/DefogTech](https://www.youtube.com/c/DefogTech) Java Thread
* [https://www.educative.io/courses/java-multithreading-for-senior-engineering-interviews/](https://www.educative.io/courses/java-multithreading-for-senior-engineering-interviews/)
* [https://www.coursera.org/learn/java-chengxu-sheji/lecture](https://www.coursera.org/learn/java-chengxu-sheji/lecture)
* 《深入理解java虚拟机》
* [https://github.com/mengdd/Effective-Java-Reading-Notes](https://github.com/mengdd/Effective-Java-Reading-Notes)

## Integer Cache <a href="#integer-cache" id="integer-cache"></a>

需要注意，如果值超过了【-128， 127】不能比较Integer a == Integer b

## String intern <a href="#string-intern" id="string-intern"></a>

String常量的intern化。

## Reference <a href="#reference" id="reference"></a>

非常好的一篇文章： [https://zhuanlan.zhihu.com/p/59601457](https://zhuanlan.zhihu.com/p/59601457)

* **Strong Reference**, it is strongly reachable through a chain of strong references
* **Soft Reference**, objects with SoftReference are only collected when the JVM absolutely needs memory.
* **Weak Reference**, The object can be reached only by traversing a weak reference, Garbage collector can collect an object if only weak references are pointing to it.
*   **Phantom Reference**, phantom references are not automatically cleared by the garbage collector as they are enqueued. An object that is reachable via phantom references will remain so until all such references are cleared or themselves become unreachable. Before Java 9, Whilst Weak and Soft references are put in the queue after the object is finalized, Phantom references are put in the queue before the object is finalized. Phantom references are automatically cleared (set to null) in **Java 9 and after**. Another **difference** between Phantom references and other references is that the `get()` method of a phantom reference always returns null even before a GC has occurred. **We can't get a referent of a phantom reference.** The referent is never accessible directly through the API and this is why we need a reference queue to work with this type of references.

    The Garbage Collector adds a phantom reference to a reference queue **after the finalize method of its referent is executed**. It implies that the instance is still in the memory.

Reference queues are designed for making us aware of actions performed by the Garbage Collector. When GC run, it will have items in the queue. Non-strong references is that you never know when they will start returning null because they can be garbage collected anytime by the GC. `ReferenceQueue` is a provision by Java to help us know when the weak reference is eligible for GC. [https://www.baeldung.com/java-phantom-reference](https://www.baeldung.com/java-phantom-reference)

```java
  Integer i = new Integer(420);
  WeakReference<Integer> myRec = new WeakReference<>(i, q); //the second parameter is the Reference queue
```

**引用队列**：如果软引用和弱引用被GC回收，JVM就会把这个引用加到引用队列里，如果是虚引用，在回收前就会被加到引用队列里。

## 内部类 <a href="#nei-bu-lei" id="nei-bu-lei"></a>

[https://zhuanlan.zhihu.com/p/97034966](https://zhuanlan.zhihu.com/p/97034966)

能使用嵌套类就使用嵌套类，如果内部类需要和外部类联系，才使用内部类。最后**不涉及到类成员方法和成员变量的方法，最好定义为static**可以防止内部类内存泄露

如同非静态方法不能访问静态成员一样，非静态内部类也被设计为不能拥有静态变量，因此内部类不能定义static对象和方法. new内部类时，用'outObj.new InClass()'

### 局部类 <a href="#ju-bu-lei" id="ju-bu-lei"></a>

方法中定义的类，与局部变量一样不能用public/private/protected/static修饰。但是可以用final/abstract修饰。**不能访问同一个函数里的局部变量，除非该局部变量是final的**

### Boxing/Unboxing/Cache <a href="#boxingunboxingcache" id="boxingunboxingcache"></a>

### 反射使用总结 <a href="#fan-she-shi-yong-zong-jie" id="fan-she-shi-yong-zong-jie"></a>

反射获取调用类可以通过 Class.forName()，反射获取类实例要通过 newInstance()，相当于 new 一个新对象，反射获取方法要通过 getMethod()，获取到类方法之后使用 invoke() 对类方法进行调用。如果是类方法为私有方法的话，getDeclaredMethod() 获取该方法，然后需要通过 setAccessible(true) 来修改方法的访问限制，以上的这些操作就是反射的基本使用。

### Generic arrays are illegal in Java <a href="#generic-arrays-are-illegal-in-java" id="generic-arrays-are-illegal-in-java"></a>

[https://www.ibm.com/developerworks/java/library/j-jtp01255/index.html](https://www.ibm.com/developerworks/java/library/j-jtp01255/index.html)

`List<?>` 可以容纳任意类型，只不过 `List<?>` 被赋值之后，就不允许添加和修改操作了

### Stream API <a href="#stream-api" id="stream-api"></a>

将集合中的常见操作抽取出来。provides utilities "to support functional-style operations on streams of values". Lambda实现了函数式编程，与流结合构成一种全新的思考问题的方式。

Steps

```
1. Obtain a Stream  (
    Collection.stream()； Arrays.stream(arr)
    Map没有stream但是提供了类似的方法map.putIfAbsent(), map.computeIfAbsent()
    map.merge())
2. call zero or more non-terminal operation
3. call terminal operation
```

```
Stream.of(list1, list2).forEach(output::addAll); //将2个list合并为一个
```

.stream()换成.parallelStream()可以成为可以支持并行计算的流。

### Serialization <a href="#serialization" id="serialization"></a>

```
java.io.Serializable
java.io.Externalizable
ObjectOutput
ObjectInput
ObjectOutputStream
ObjectInputStream
```

![](https://www.codejava.net/images/articles/javase/fileio/objectoutputstream-objectinputstream/ObjectStreamsAPI.png)

![](https://img-blog.csdn.net/20170420102507340?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZnV6aG9uZ21pbjA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) ![](https://img-blog.csdn.net/20170420102600622?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZnV6aG9uZ21pbjA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Note that only objects of classes that implement the java.io.**Serializable** interface can be written to and read from an output/input stream. Serializable is a marker interface, which doesn’t define any methods. Only objects that are marked ‘serializable’ can be used with ObjectOutputStream and ObjectInputStream.

```java
//Creates an ObjectInputStream that reads from the specified InputStream.ObjectInputStream(InputStream in);
```

A subclass can't be serialized if its non-serializable super class doesn't provide an accessible parameterless constructor. 如果要序列化的类有父类，要想将在父类中定义过的变量序列化下来，那么父类也应该实现java.io.Serialization接口。

`Externalizable` interface provides a way to implement a custom serialization mechanism, A class that implements the `Externalizable` interface is responsible to save and restore the contents of its own instances.

```
import java.io.*;

class Demonstration {
    public static void main( String args[] ) throws Exception {
        EducativeCourse course = new EducativeCourse("C. H. Afzal");

        // Serialization code
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        try (ObjectOutput out = new ObjectOutputStream(bos)) {
            out.writeObject(course);
            out.flush();
        } catch (Exception e) {
            throw e;
        }
        byte[] courseInBytes = bos.toByteArray();

        // Deserialization code
        ByteArrayInputStream bis = new ByteArrayInputStream(courseInBytes);
        try (ObjectInput in = new ObjectInputStream(bis)) {
            EducativeCourse deserializeCourse = (EducativeCourse) in.readObject();
            System.out.println(deserializeCourse.company);

        } catch (Exception e) {
            throw e;
        }

    }
}

class Course {
    String company;
    public Course() {
    }
    Course(String company) {
        this.company = company;
    }
}

class EducativeCourse extends Course implements Serializable {
    public EducativeCourse(String authorName) {
        super("Educative");
        this.authorName = authorName;
    }
    private String authorName;
}
```

### Java Error <a href="#java-error" id="java-error"></a>

The general convention is that errors are reserved for use by the JVM to indicate resource deficiencies, invariant failures, or other conditions that make it impossible to continue execution.

### Java数组的初始化方式 <a href="#java-shu-zu-de-chu-shi-hua-fang-shi" id="java-shu-zu-de-chu-shi-hua-fang-shi"></a>

```java
//使用 new 指定数组大小后进行初始化
int[] number = new int[2];
number[0] = 1;
number[1] = 2;

//使用 new 指定数组元素的值
int[] number = new int[]{1, 2, 3, 5, 8};

//直接指定数组元素的值
int[] number = {1,2,3,5,8};
```

### Enum <a href="#enum" id="enum"></a>

Java中Enum是个特殊类。

## ## == 与 equals的区别 <a href="#yu-equals-de-qu-bie" id="yu-equals-de-qu-bie"></a>

\==引用相等，equals内容相等

## Class <a href="#class" id="class"></a>

**top-level class in Java be declared static?** NO

a local class can only access local variables that are declared final.

### Java Access <a href="#java-access" id="java-access"></a>

top level & member level

top level:

* A public class is accessible across different packages.
* A package private class is only visible to other classes within the same package.

这个图是member level

![](broken-reference)

C++ 有 friend, Java 有package default.

### Abstract <a href="#abstract" id="abstract"></a>

```java
abstract void printFullName();
```

```
void printFullName()=0;
```

### Modifiers <a href="#modifiers" id="modifiers"></a>

The access specifier for an overriding method can **allow more, but not less**, access than the overridden method. For example, a protected instance method in the superclass can be made public, but not private, in the subclass.

You will get a compile-time error if you attempt to change an instance method in the superclass to a static method in the subclass, and vice versa.

### Immutable <a href="#immutable" id="immutable"></a>

[https://www.geeksforgeeks.org/create-immutable-class-java/](https://www.geeksforgeeks.org/create-immutable-class-java/)

In Java, all the [wrapper classes](https://www.geeksforgeeks.org/wrapper-classes-java/) (like Integer, Boolean, Byte, Short) and String class is immutable.

### 注意 <a href="#zhu-yi" id="zhu-yi"></a>

extends used to implent abstract or sub class

implements used to implement an interface

## 异常 <a href="#yi-chang" id="yi-chang"></a>

1. Checked异常必须被显式地捕获或者传递，如Basic try-catch-finally Exception Handling一文中所说。而unchecked异常则可以不必捕获或抛出。
2. Checked异常继承java.lang.Exception类。Unchecked异常继承自java.lang.RuntimeException类。

* In case of `try-catch-finally` block, the exception from the `try` block is suppressed and the exception from the `finally` block is propagated.
* In case of `try-with-resources` the exception from the `try` block is thrown and the one from the `finally` block is added as a suppressed exception to the exception object from the `try` block. Note that there's no explicit finally block since it is implicit in the `try-with-resources` statement.

## 泛型 <a href="#fan-xing" id="fan-xing"></a>

[https://wiki.jikexueyuan.com/project/java-collection/](https://wiki.jikexueyuan.com/project/java-collection/)

[https://stackoverflow.com/questions/24796273/incompatible-types-list-of-list-and-arraylist-of-arraylist](https://stackoverflow.com/questions/24796273/incompatible-types-list-of-list-and-arraylist-of-arraylist)

```java
Incompatible Types.

List<List<Integer>> output = new ArrayList<ArrayList<Integer>>();
```

Box 不是Box 类型. In general, if a type `A` extends type `B` then `List<A>` is not a subtype of `List<B>`. The common parent of `List<A>` and `List<B>` is `List<?>`

<img src="https://i.stack.imgur.com/IW0vU.gif" alt="" data-size="original">

`?`, called the wildcard, represents an unknown type, Semantically, Java generics are defined by **erasure**, whereas C++ templates are defined by **expansion**.

* 它通过确保类型必须是T的子类来设定类型的上界
* 它通过确保类型必须是T的父类来设定类型的下界

**wildcard can only be used in defining arguments to a method in a method signature. It can't appear in the return type expression. Lowerbounds (`<T super Tiger>`) can't be used as return types. They can only be specified as method arguments. Cab Upperbounds be used as the return types(???).**

Usually, list with unbounded wildcard or an extends wildcard bound are used to declare the parameter types in the method signature and not as variables inside a method.

```java
class NumberCollectionBounded<T extends Number>{};

//bounded by at most one class and as many interfaces as desired. 
class NumberCollectionBounded<T extends Number & Comparable & Serializable> {
     // ... class body
}

void someMethod(List<? extends Number> num) {
    // ... method body
}
```

### Arrays vs. Collections <a href="#arrays-vs-collections" id="arrays-vs-collections"></a>

* Arrays are covariant, that is **an array of type person is a subtype of an array of objects**. However, collections are invariant that is **a list of type person is not a subtype of list of objects.**

```java
//Collection can use wildcard to achive the purpose. The following can pass all collections of Animal subtype
static void printAnimal(Collection<? extends Animal> animals) {};
Collection<? extends Object> myColl = new ArrayList<Object>();

but The only element that you can ever insert into a List<?> is null. The wildcard type prevents us from extracting elements from the List<?> as any type other than Object.
```

* Arrays are reified, that is they know the type of their components at runtime and enforce it. One can't store a `String` type in an array of `Double`. On the other hand, Collections enforce their type constraints only at compile time and erase element type information at runtime.

### Bridge Methods <a href="#bridge-methods" id="bridge-methods"></a>

**Bridge methods are synthetic methods created by the compiler as part of the erasure process when compiling a class or interface that extends a parameterized class or implements a parameterized interface.**

### **PECS**原则 <a href="#pecs-yuan-ze" id="pecs-yuan-ze"></a>

仅从某个结构中获取值时使用 extends 通配符；仅将值放入某个结构时使用 super 通配符；同时执行以上两种操作时不要使用通配符。extends限定了通配符类型的**上界**，所以我们可以安全地从其中读取；而super限定了通配符类型的**下界**，所以我们可以安全地向其中写入。 我们把那些只能从中读取的对象称为**生产者**（Producer），我们可以从生产者中安全地读取；只能写入的对象称为**消费者**（Consumer）。 因此这里就是著名的**PECS**原则：Producer-Extends, Consumer-Super。

## Lambda表达式 <a href="#lambda-biao-da-shi" id="lambda-biao-da-shi"></a>

Java中其实它是匿名类的一个实例。函数式接口可以用Lambda表达式表示。实现了将代码当成数据来处理。

## ClassLoader <a href="#classloader" id="classloader"></a>

Ensuring proper class loading process is the responsibility of a ClassLoader. ClassLoaders are instances of `java.lang.ClassLoader` class and each and every class in a Java program has to be loaded by some ClassLoader.

* **Bootstrap Class Loader:** The bootstrap class loader loads the core Java libraries located in the /jre/lib directory. This class loader, which is part of the core JVM, is written in native code.
* **Extensions Class Loader:** The extensions class loader loads the code in the extensions directories (/jre/lib/ext, or any other directory specified by the _**java.ext.dirs\\**_ system property.
* **System Class Loader:** The system class loader loads code found on _**java.class.path\\**_, which maps to the **CLASSPATH** environment variable. The default value of the classpath is **"."** the current directory
* **Static Loading:** classes are statically loaded via the `new` operator
* **Dynamic Loading:** classes are programmatically loaded by using the `Class.forName()` or the `loadClass()` method. The difference between the two is that the former one initializes the object after loading it while the latter one only loads the class but doesn’t initialize the object.

## Java difference vs C++ <a href="#java-difference-vs-c" id="java-difference-vs-c"></a>

### Initialize variables <a href="#initialize-variables" id="initialize-variables"></a>

all Java class variable is reference, need equal to a obj newed by new().

[https://stackoverflow.com/questions/1560685/why-must-local-variables-including-primitives-always-be-initialized-in-java](https://stackoverflow.com/questions/1560685/why-must-local-variables-including-primitives-always-be-initialized-in-java)

In Java, **class and instance variables** assume a default value (null, 0, false) if they are not initialized manually. However, **local** variables don't have a default value.

Unless a local variable has been assigned a value, the compiler will refuse to compile the code that reads it. IMHO, this leads to the conclusion, that initializing a local variable with some default value (like null, which might lead to a NullPointerException later) when it is declared is actually a bad thing. Consider the following example:

### Array <a href="#array" id="array"></a>

```
C++ 可以声明和初始化
int test_score[5]{100,95,99,87,88};
int test_score[]{100,95,99,87,88};
不像java, int[]会在c++中报错。

但是java如下初始会报错，不能有size 5, 如果用数组值来初始化. 所有的java 变量除了primitive都是reference.初始化都需要赋值。
int[] test_score=new int[5]{100,95,99,87,88}; 
```

C++中的Array名字代表了第一个元素的location, index代表offset。没有bounds check..

| C++                                                                  | Java |
| -------------------------------------------------------------------- | ---- |
| Element\_Type array\_name\[constant number of elements]{init\_list}; |      |
| char vowels\[]{'a','e','i','o','u'};                                 |      |
|                                                                      |      |

### Why java doesn't have initializer list <a href="#why-java-doesnand39t-have-initializer-list" id="why-java-doesnand39t-have-initializer-list"></a>

* [https://stackoverflow.com/questions/7154654/why-doesnt-java-have-initializer-lists-like-in-c](https://stackoverflow.com/questions/7154654/why-doesnt-java-have-initializer-lists-like-in-c)

In C++, initializer lists are necessary because of a few language features that are either not present in Java or work differently in Java:

1. **`const`**: In C++, you can define a fields that are marked `const` that cannot be assigned to and must be initialized in the initializer list. Java does have `final` fields, but you can assign to `final` fields in the body of a constructor. In C++, assigning to a `const` field in the constructor is illegal.
2. **References**: In C++, references (as opposed to pointers) must be initialized to bind to some object. It is illegal to create a reference without an initializer. In C++, the way that you specify this is with the initializer list, since if you were to refer to the reference in the body of the constructor without first initializing it you would be using an uninitialized reference. In Java, object references behave like C++ pointers and can be assigned to after created. They just default to `null` otherwise.
3. **Direct subobjects**. In C++, an object can contain object directly as fields, whereas in Java objects can only hold _references_ to those objects. That is, in C++, if you declare an object that has a `string` as a member, the storage space for that string is built directly into the space for the object itself, while in Java you just get space for a reference to some other `String` object stored elsewhere. Consequently, C++ needs to provide a way for you to give those subobjects initial values, since otherwise they'd just stay uninitialized. By default it uses the default constructor for those types, but if you want to use a different constructor or no default constructor is available the initializer list gives you a way to bypass this. In Java, you don't need to worry about this because the references will default to `null`, and you can then assign them to refer to the objects you actually want them to refer to. If you want to use a non-default constructor, then you don't need any special syntax for it; just set the reference to a new object initialized via the appropriate constructor.

In the few cases where Java might want initializer lists (for example, to call superclass constructors or give default values to its fields), this is handled through two other language features: the `super` keyword to invoke superclass constructors, and the fact that Java objects can give their fields default values at the point at which they're declared. Since C++ has multiple inheritance, just having a single `super` keyword wouldn't unambiguously refer to a single base class, and prior to C++11 C++ didn't support default initializers in a class and had to rely on initializer lists.

### virtual function table <a href="#virtual-function-table" id="virtual-function-table"></a>

In C++, virtual function need declare explicitly

Java, function is virtual by default.

按照下面的说法，java的虚表是在构造函数调用之前就被设定了。不像C++，**vtable** 是在构造函数中才初始化的啊，而不是在其之前。因此构造函数不能为虚函数

[https://stackoverflow.com/questions/9554379/virtual-tables-and-abstract-in-java/9554482](https://stackoverflow.com/questions/9554379/virtual-tables-and-abstract-in-java/9554482)

In Java, invocations of instance methods normally go through virtual method tables. (There are exceptions to this. Constructors, private methods, final methods, and methods of final classes cannot be overridden, so these methods can be invoked without going through a vtable. And `super` calls do not go through vtables, since they are inherently not polymorphic.)

**Every object holds a pointer to a class handle, which contains a vtable. This pointer is set as soon as the object is allocated (with `NEW`) and before any constructors are called.** So in Java, it is safe for constructors to make virtual method calls, and they will be properly directed to the target's implementation of the virtual method.

So when `Base`'s constructor calls `foo()`, it invokes `Derived.foo`, which prints `Derived.x`. But `Derived.x` hasn't been assigned yet, so the default value of `0` is read and printed.

### 类继承 <a href="#lei-ji-cheng" id="lei-ji-cheng"></a>

C++ 的继承 有public/private/protected继承，而java没有

## Java API design <a href="#java-api-design" id="java-api-design"></a>

* Client shouldn't change object returned. So we need have a copy of mutable object

```java
 Collections.unmodifiableList() //
```

* All public API, need verify the passed-in parameter, for we don't know what kind client will used the API
* Avoid defining methods with more than four parameters.
* Use the Interface class as parameter
* **The overloaded method to be invoked is decided at compile time and not runtime.**
* **In case of an overridden method, the correct version to invoke is decided at runtime contrary to overloaded methods**
* **Return empty arrays or Collections but not `null` from methods**

## Java Resource Manager <a href="#java-resource-manager" id="java-resource-manager"></a>

* Don't use finalize (Java invokes the finalizer on an object at most one time)
* **try-with-resources**. **Objects that implement the interface** `java.lang.AutoCloseable` **can be used with the** `try-with-resources` **statement**
* Using a `PhantomReference` to perform cleanup when object is garbage collected

## JCF <a href="#jcf" id="jcf"></a>

synchronizedList() must in synchronized block, but`CopyOnWriteArrayList` which is slower for writes but doesn't have this issue

```java
//Collections.synchronizedMap() used for hashmap
List list = Collections.synchronizedList(new ArrayList());
    ...
synchronized (list) {
    Iterator i = list.iterator(); // Must be in synchronized block
    while (i.hasNext())
        foo(i.next());
}
```

To support concurrent threads, JCF use following methods

* Copy on write. An immutable copy is created of the backing collection and whenever a **write** operation is attempted, the copy is discarded and a new copy with the change is created. no special action for **read**.
* Compare And Swap(CAS). it makes a local copy of the variable and performs the calculation without getting exclusive access. Only when it is ready to update the variable does it call CAS, which in one atomic operation compares the variable’s value with its value at the start and, if they are the same, updates it with the new value. If they are not the same, the variable must have been modified by another thread;
* Lock

## Lambda <a href="#lambda" id="lambda"></a>

Anonymous classes can maintain state as they can have instance or static variables whereas lambda expressions are just method implementations.

```java
parameter -> expression                        //one parameter
(parameter1, parameter2) -> expression         //two parameters
(parameter1, parameter2) -> { code block }    //code block
```

## Java Thread <a href="#java-thread" id="java-thread"></a>

参考 《Java Concurrency in practice》

### 线程 <a href="#xian-cheng" id="xian-cheng"></a>

线程的创建，分为以下三种方式：

* 继承 Thread 类，重写 run 方法
* 实现 Runnable 接口，实现 run 方法
* 实现 Callable 接口，实现 call 方法。 Callable 的调用是可以有返回值的，它弥补了之前调用线程没有返回值的情况、
* JDK8 之后用Lambda创建线程： **new** Thread(() -> System.out.println("Lambda Of Thread.")).start();

All stateless objects and their corresponding classes are thread-safe. All method local primitive variable always is thread confined. Usually, static variables are declared threadlocal to keep separate values on a per thread basis.

```
ThreadLocal<Integer> counter = ThreadLocal.withInitial(() -> 0);
```

ThreadLocal 的主要方法是 set(T) 和 get()，用于多线程间的数据隔离，ThreadLocal 也提供了 InheritableThreadLocal 子类，用于实现多线程间的数据共享。但使用 ThreadLocal 一定要注意用完之后使用 remove() 清空 ThreadLocal，不然会操作内存溢出的问题。

### Best Practices <a href="#best-practices" id="best-practices"></a>

* Don't creating the thread object in the constructor
* private fields of the of the class also become accessible to the instance of the anonymous inner class that we pass in to the `Thread` class's constructor.

```java
public class BadClassDesign {
    private File file;
    public BadClassDesign() throws InterruptedException {
        Thread t = new Thread(() -> {
            System.out.println(this.file);
        });
        t.start();
        t.join();
    }
}
```

### Daemon/User Thread <a href="#daemonuser-thread" id="daemonuser-thread"></a>

User threads are high-priority threads. **The JVM will wait for any user thread to complete its task before terminating it.** even referred by local variable.

On the other hand, **daemon threads are low-priority threads whose only role is to provide services to user threads.** When User threads exit, daemon thread will be terminated too. `setDaemon(true);`

### Java thread states <a href="#java-thread-states" id="java-thread-states"></a>

![](http://web.science.mq.edu.au/\~mattr/courses/object\_oriented\_development\_practices/9/lifecycle.png)

[https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/](https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/)

All these thread concurrency functions can cause Java thread switch b/w different states.

### 线程的执行 <a href="#xian-cheng-de-zhi-hang" id="xian-cheng-de-zhi-hang"></a>

```
Thread joinThread = new Thread(() -> {
    try {
        System.out.println("执行前");
        Thread.sleep(1000);
        this.yield();//交出执行权
        System.out.println("执行后");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
joinThread.start();
joinThread.join();//这里等待线程结束，然后再继续执行当前线程
System.out.println("主程序");
```

`System.exit(0)` 可以让整个程序退出；interrupt()可以中断线程

```
Thread interruptThread = new Thread() {
    @Override
    public void run() {
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            System.out.println("i：" + i);
            if (this.isInterrupted()) {
                break;
            }
        }
    }
};
interruptThread.start();
Thread.sleep(10);
interruptThread.interrupt();
```

java.util.concurrent包提供了线程并行所用到的一些方法

#### Callable & Runnable & Future <a href="#callable-andamp-runnable-andamp-future" id="callable-andamp-runnable-andamp-future"></a>

**CompletableFuture**

```
CompletableFuture.supplyAsync(...)
    .thenApply(...)
    .thenApply(...);
```

#### Executor Framework <a href="#executor-framework" id="executor-framework"></a>

It provides a standard means of decoupling task submission from task execution, describing tasks with Runnable. task execution framework, which simplifies management of task and thread lifecycles and provides a simple and flexible means for decoupling task submission from execution policy

**ThreadPool**

A thread pool is tightly bound to a work queue holding tasks waiting to be executed. Worker threads have a simple life: request the next task from the work queue, execute it, and go back to waiting for another task.

创建线程池有两种方式：ThreadPoolExecutor 和 Executors

**ThreadPoolExecutor**

ThreadPoolExecutor 是创建线程池最传统和最推荐使用的方式，创建时要设置线程池的核心线程数和最大线程数还有任务队列集合，如果任务量大于队列的最大长度，线程池会先判断当前线程数量是否已经到达最大线程数，如果没有达到最大线程数就新建线程来执行任务，如果已经达到最大线程数，就会执行拒绝策略（拒绝策略可自行定义）。线程池可通过 submit() 来调用执行，从而获得线程执行的结果，也可以通过 shutdown() 来终止线程池。

```
submit()/execute()：执行线程池
shutdown()/shutdownNow()：终止线程池
isShutdown()：判断线程是否终止
getActiveCount()：正在运行的线程数
getCorePoolSize()：获取核心线程数
getMaximumPoolSize()：获取最大线程数
getQueue()：获取线程池中的任务队列
allowCoreThreadTimeOut(boolean)：设置空闲时是否回收核心线程
```

**Executors**

Executors 可以创建 6 种不同类型的线程池，其中 newFixedThreadPool() 适合执行单位时间内固定的任务数，newCachedThreadPool() 适合短时间内处理大量任务，newSingleThreadExecutor() 和 newSingleThreadScheduledExecutor() 为单线程线程池，而 newSingleThreadScheduledExecutor() 可以执行周期性的任务，是 newScheduledThreadPool(n) 的单线程版本，而 newWorkStealingPool() 为 JDK 8 新增的并发线程池，可以根据当前电脑的 CPU 处理数量生成对比数量的线程池，但它的执行为并发执行不能保证任务的执行顺序。

![](broken-reference)

* CachedThreadPool, if all thread are busy create new, if thread is idle for 60s, kill it.
* FixedThreadPool
* ScheduledPool
* SingleThreadedExecutor

```java
ExecutorService es = Executors.newFixedThreadPool(1);
```

ExecutorService threadpool and localthread variables. ThreadLocal object is associated to a thread and not a task. Hence ThreadLocal should be used carefully with Thread Pool.

must shutdown the excutor when exiting the main thread。

**ThreadPoolExecutor VS Executors**

ThreadPoolExecutor 和 Executors 都是用来创建线程池的，其中 ThreadPoolExecutor 创建线程池的方式相对传统，而 Executors 提供了更多的线程池类型（6 种），但很不幸的消息是在实际开发中并不推荐使用 Executors 的方式来创建线程池。

### Thread cancel & Interrupts <a href="#thread-cancel-andamp-interrupts" id="thread-cancel-andamp-interrupts"></a>

[https://www.youtube.com/watch?v=-7ZB-jpaPPo](https://www.youtube.com/watch?v=-7ZB-jpaPPo)

* interrupted() can clear the interrupt flat.
* polling the interrupted status by isInterrupted()
*   To stop/cancel a task, can request to set a cancel flag, the thread loop check the flag then to exit

    ```java
    //all will interrupt all thread in the pool
    future.cancel(true);
    executor.shutDown();
    executor.shutDownNow(); 

    //Stop a task in timeout
    //scheduler can stop the task in 10 minutes
    scheduler.schedue(()->{task.stop();}, 10, TimeUnit.MINUTES);

    //future also can cancel a task in timeout
    try{
        future.get(10,TimeUnit.MINUTES);
    }catch(InterruptException || ExcutionException e){

    }catch ( TimeoutException te){
        future.cancel(true); //use interrupt
        task.stop(); //use violotile
    }
    ```

### Timer <a href="#timer" id="timer"></a>

```java
Timer timer= new Timer("Display");
TimerTask task=new MyTask();
timer.schedule(task,1000,1000);
```

### Java fibers <a href="#java-fibers" id="java-fibers"></a>

## 线程安全 <a href="#xian-cheng-an-quan" id="xian-cheng-an-quan"></a>

[**Java Multithreading, Concurrency, and Performance Optimization**](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80\&mid=39197\&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fjava-multithreading-concurrency-performance-optimization%2F) course by Michael Pogrebinsy on Udemy

线程安全的解决方案有以下几个维度：

* 数据不共享，单线程可见，比如 ThreadLocal 就是单线程可见的；
* 使用线程安全类，比如 StringBuffer 和 JUC（java.util.concurrent）下的安全类；
* 使用同步代码或者锁。

#### Memory Model <a href="#memory-model" id="memory-model"></a>

The JMM defines a partial ordering2 called happens-before on all actions within the program.

A correctly synchronized program is one with no data races; correctly synchronized programs exhibit sequential consistency, meaning that all actions within the program appear to happen in a fixed, global order

#### Memory Visibility <a href="#memory-visibility" id="memory-visibility"></a>

[http://tutorials.jenkov.com/java-concurrency/java-memory-model.html](http://tutorials.jenkov.com/java-concurrency/java-memory-model.html)

> According to the java memory model "changes to fields made by one thread are guaranteed to be visible to other threads only under some conditions.

This is true if the writes of one thread happens before a synchronization action.

Synchronized blocks also guarantee that all variables accessed inside the synchronized block will be read in from main memory, and when the thread exits the synchronized block, all updated variables will be flushed back to main memory again, regardless of whether the variable is declared volatile or not.

If a variable is declared `volatile` then whenever a thread writes or reads to the `volatile` variable, the read and write always happen in the main memory

synchronized 既能保证可见性，又能保证原子性，而 volatile 只能保证可见性，无法保证原子性。对于多个线程写某个变量的情况，需要用synchronized. 单个线程写，单个线程读可以用volatile. Race Condition 常见Pattern

*   check-then-act. Check a condition, but the condition may impacted by other thread

    * busy-wait， poll in a loop for a predicate/condition to become true before making progress

    ```java
    synchronized (signal) {
        while (!condition) { //condition 没有ready继续等待
            signal.wait();
        }
        //Action
    }
    ```
*   read-modify-write.

    [https://en.wikipedia.org/wiki/Read%E2%80%93modify%E2%80%93write](https://en.wikipedia.org/wiki/Read%E2%80%93modify%E2%80%93write)

#### synchronized/visibility/atomic/volatile <a href="#synchronizedvisibilityatomicvolatile" id="synchronizedvisibilityatomicvolatile"></a>

Provide an monitor in java. synchronize on this, synchronize on an object. synchronized是Java语言中一个重量级的操作,会涉及到用户态和内核态的切换。当JDK 6中加入了大量针对synchronized锁的优化措施（下一节我们就会讲解这些优化措施）之后，相同的测试中就发现synchronized与ReentrantLock的性能基本上能够持平。

后面会讲到Lock，java.util.concurrent.locks.Lock接口便成了Java的另一种全新的互斥同步手段。基于Lock接口，用户能够以非块结构（Non-Block Structured）来实现互斥同步，从而摆脱了语言特性的束缚，改为在类库层面去实现同步，这也为日后扩展出不同调度算法、不同特征、不同性能、不同语义的各种锁提供了广阔的空间。

提供了一个将组合action bind为atomic操作，同时也提供了visibility

AtomicInteger

_`volatile`_ to ensure threads reading it see the most recent value. Volatile variables aren't cached in registers or caches where they are hidden from other processors. Volatile variables only guarantee memory visibility but not atomicity.

### Synchronized/notify/notifyAll/wait <a href="#synchronizednotifynotifyallwait" id="synchronizednotifynotifyallwait"></a>

`wait()` should always be used with a while loop that checks for a condition and if found false should make the thread wait again. **the thread must hold the monitor of the object on which it'll call** `wait()`, Like the wait method, `notify()` can only be called by the thread which owns the monitor for the object

we can synchronize multiple times on the same object. A thread that owns a monitor can **reenter** the same monitor if it so desires.

```
Object lock = new Object();
// The standard idiom for using the wait() method
synchronized (obj) {
   while (<condition does not hold>)
       obj.wait(); // Lock released & reacquired on wakeup
       // ...         Do Processing
}

//常见API
lock.wait();
lock.notify();
lock.notifyAll();
thread.wait() 和 thread.wait(0) 是相同的
A thread waiting on a monitor can be interrupted by other threads via the interrupt() method.
```

Once the interrupted exception is thrown, the interrupt status/flag is cleared.

Static`Thread.interrupted()` and the other is `Thread.currentThread().isInterrupted()`. The important difference between the two is that the static method would return the interrupt status and also clear it at the same time.

### Difference b/w notifyAll() & notify() <a href="#difference-bw-notifyall-andamp-notify" id="difference-bw-notifyall-andamp-notify"></a>

![](https://www.programcreek.com/wp-content/uploads/2011/12/java-monitor-associate-with-object.jpg?ezimgfmt=ng:webp/ngcb12)

**Lock pool**: Suppose that thread A already has a lock on an object (note: not a class), while other threads want to call a synchronized method (or synchronized block) of the object, because these threads are entering the synchronized method of the object. Before you can get the ownership of the object's lock, but the object's lock is currently owned by thread A, so these threads enter the object's lock pool.

**Waiting pool**: Suppose a thread A calls an object's wait() method, and thread A releases the object's lock (because the wait() method must appear in synchronized, so naturally before the wait() method is executed, thread A It already has the lock of the object, and thread A enters the **waiting pool of the object**. If another thread calls the notifyAll() method of the same object, then the **threads** in the waiting pool of the object will all enter the lock pool of the object, ready to compete for the ownership of the lock. If another thread calls the notify() method of the same object, then **only one thread** (random) in the waiting pool of the object will enter the object's lock pool.

[http://www.zzcblogs.top/2018/03/15/java-notify%E5%92%8CnotifyAll%E5%8C%BA%E5%88%AB/#:\~:text=%E4%BB%8E%E5%AD%97%E9%9D%A2%E4%B8%8A%E6%84%8F%E6%80%9D%E6%9D%A5,%E8%AF%A5%E5%AF%B9%E8%B1%A1wait%E7%9A%84%E7%BA%BF%E7%A8%8B%E3%80%82\&text=%E4%BC%98%E5%85%88%E7%BA%A7%E9%AB%98%E7%9A%84%E7%BA%BF%E7%A8%8B,%E5%9B%9E%E5%88%B0%E7%AD%89%E5%BE%85%E6%B1%A0%E4%B8%AD%E3%80%82](http://www.zzcblogs.top/2018/03/15/java-notify%E5%92%8CnotifyAll%E5%8C%BA%E5%88%AB/)

### 线程安全的类 <a href="#xian-cheng-an-quan-de-lei" id="xian-cheng-an-quan-de-lei"></a>

ArrayList\&HashMap不是线程安全的， Vector\&HashTable是线程安全的。Collections.synchronizedArrayList(list)可以生成一个线程安全的对象

![](broken-reference)

### synchronizer <a href="#synchronizer" id="synchronizer"></a>

A synchronizer is any object that coordinates the control flow of threads based on its state. Blocking queues can act as synchronizers; other types of synchronizers include semaphores, barriers, and latches.

#### Semaphore <a href="#semaphore" id="semaphore"></a>

[https://www.geeksforgeeks.org/semaphore-in-java/](https://www.geeksforgeeks.org/semaphore-in-java/)

![](https://media.geeksforgeeks.org/wp-content/uploads/d2.jpeg)

Semaphore（信号量）用于管理多线程中控制资源的访问与使用. As having a limited number of permits to give out. If a semaphore has given out all the permits it has, then any new thread that comes along requesting for a permit will be blocked, till an earlier thread with a permit returns it to the semaphore. A semaphore with a single permit is called a binary semaphore.

Semaphore used to resolve the **missing signal**. The producer can call `acquire()` on a binary semaphore (one with max permits = 1) and the consumer can call `release()` before attempting to consume. This way the “signal” is in a sense stored for the consumer whenever the producer produces something.

可以用wait/notify来模拟实现Semaphore.

```
Semaphore semZero = new Semaphore(0);
Semaphore semOne = new Semaphore(1);

For semZero all acquire() calls will block and tryAcquire() calls will return false, until you do a release(). 为零，是为了等待资源ready

For semOne the first acquire() calls will succeed and the rest will block until the first one releases.
```

**CountingSemaphore**

A counting semaphore. Conceptually, a semaphore maintains a set of permits. Each [`acquire()`](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Semaphore.html#acquire\(\)) blocks if necessary until a permit is available, and then takes it. Each [`release()`](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Semaphore.html#release\(\)) adds a permit, potentially releasing a blocking acquirer. However, no actual permit objects are used; the `Semaphore` just keeps a count of the number available and acts accordingly.

#### CountDownLatch <a href="#countdownlatch" id="countdownlatch"></a>

A latch is a synchronizer that can delay the progress of threads until it reaches its terminal state.

CountDownLatch 有两个重要的方法：

* countDown()：使计数器减 1；
* await()：当计数器不为 0 时，则调用该方法的线程阻塞，当计数器为 0 时，可以唤醒等待的一个或者全部线程。

#### FutureTask <a href="#futuretask" id="futuretask"></a>

also work as a latch. can be in one of three states: waiting to run, running, or completed.

#### Barriers <a href="#barriers" id="barriers"></a>

Barriers are similar to latches in that they block a group of threads until some event has occurred. The key difference is that with a barrier, all the threads must come together at a barrier point at the same time in order to proceed.

### Lock <a href="#lock" id="lock"></a>

spinlock

* keep trying to acquire the lock without going to wait state
* Assumption: most lock are used only for short period of time

Lock 必须为同一个线程拥有，[`ReentrantLock#unlock()`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/ReentrantLock.html#unlock--) states

> If the current thread is not the holder of this lock then `IllegalMonitorStateException` is thrown.

[https://www.baeldung.com/java-concurrent-locks](https://www.baeldung.com/java-concurrent-locks)

[https://xpadro.com/2015/02/java-concurrency-tutorial-locking-explicit-locks.html](https://xpadro.com/2015/02/java-concurrency-tutorial-locking-explicit-locks.html)

```java
//常用的模板是
public void run() {
       lock.lock();
       try {
            ......
       } finally {
           lock.unlock();
       }
}
```

![](https://xpadro.com/wp-content/uploads/2015/02/explicitLocking.png)

#### Lock Condition <a href="#lock-condition" id="lock-condition"></a>

no direct equivalent of a theoretical mutex in Java. **Lock** can be used, a blocked thread will constantly poll in a loop for a predicate/condition to become true before making progress.

```
        lock.lock();
        while (condition) {
            // Release the mutex to give other threads
            lock.unlock();
            // Reacquire the mutex before checking the condition
            lock.lock();
        }
        lock.unlock();
```

Condition对象由Lock对象的newCondition()方法生成，从而允许一个锁产生多个条件变量，提供了比单纯lock更精细的控制。可以根据实际情况来等待不同条件,用lock和await-and-signal来代替synchronized和wait-and-notify.

```
ReentrantLock lock = new ReentrantLock();
Condition newCallbackArrived = lock.newCondition();
//wait in thread A
newCallbackArrived.await();

//Signal in thread B
newCallbackArrived.signal();
```

#### ReentrantLock <a href="#reentrantlock" id="reentrantlock"></a>

![](broken-reference)

顾名思义，ReentrantLock锁在同一个时间点只能被一个线程锁持有；而可重入的意思是，ReentrantLock锁，可以被单个线程多次获取。

[https://www.cnblogs.com/xiaoxi/p/7651360.html#:\~:text=ReentrantLock%E6%98%AF%E4%B8%80%E4%B8%AA%E5%8F%AF%E9%87%8D,%E4%B8%8B%E6%9B%B4%E4%BD%B3%E7%9A%84%E6%80%A7%E8%83%BD%E3%80%82](https://www.cnblogs.com/xiaoxi/p/7651360.html)

reentrant lock only lock/unlock in same thread. ReentrantLock 常见方法如下：

* lock()：用于获取锁
* unlock()：用于释放锁
* tryLock()：尝试获取锁
* getHoldCount()：查询当前线程执行 lock() 方法的次数
* getQueueLength()：返回正在排队等待获取此锁的线程数
* isFair()：该锁是否为公平锁

#### 各种锁类型 <a href="#ge-zhong-suo-lei-xing" id="ge-zhong-suo-lei-xing"></a>

乐观锁和悲观锁

**独占锁和共享锁**

**可重入锁**

**自旋锁**

### 其它线程同步类 <a href="#qi-ta-xian-cheng-tong-bu-lei" id="qi-ta-xian-cheng-tong-bu-lei"></a>

本文我们介绍了四种比 synchronized 更高级的线程同步类，其中 CountDownLatch、CyclicBarrier、Phaser 功能比较类似都是实现线程间的等待，只是它们的侧重点有所不同，其中 CountDownLatch 一般用于等待一个或多个线程执行完，才执行当前线程，并且 CountDownLatch 不能重复使用；CyclicBarrier 用于等待一组线程资源都进入屏障点再共同执行；Phaser 是 JDK 7 提供的功能更加强大和更加灵活的线程辅助工具，等待所有线程达到之后，继续或开始新的一组任务，Phaser 提供了动态增加和消除线程同步个数功能。而 Semaphore 提供的功能更像锁，用于控制一组资源的访问权限。

#### Mutex <a href="#mutex" id="mutex"></a>

A mutex allows only a single thread to access a resource. A mutex, on the other hand, is strictly limited to serializing access to shared state among competing threads. Same thread must call acquire & release on the mutex. But binary Semaphore, different thread can call acquire & release. **A mutex is owned by the thread acquiring it, till the point, it releases it, whereas for a semaphore there's no notion of ownership.**

**Java doesn't have Mutex, but monitor can be think as mutex.**

## 线程同步的经典问题 <a href="#xian-cheng-tong-bu-de-jing-dian-wen-ti" id="xian-cheng-tong-bu-de-jing-dian-wen-ti"></a>

### DeadLock & LiveLock <a href="#deadlock-andamp-livelock" id="deadlock-andamp-livelock"></a>

* Lock-ordering deadlocks
* Deadlocks between cooperating objects. (Calling a method with no locks held is called an open call)
* Resource deadlocks

[https://www.baeldung.com/java-deadlock-livelock](https://www.baeldung.com/java-deadlock-livelock)

### Consumer/Producer <a href="#consumerproducer" id="consumerproducer"></a>

生产者/消费者的场景是，生产者buffer 满等待，到buffer 有空。消费者buffer空等待到buffer非空。

![](broken-reference)

```java
private ReentrantLock lock new ReentrantLock(true);
private Condition notEmpty = lock.newCondition();
private Condition notFull = lock.newCondition();

public void put(E e){
    lock.lock();
    try{
        while(queue.size()==max){
            notFull.await();  //<== wait notFull
        }
        queue.add(e);
        notEmpty.signalAll();
    }finally{
        lock.unlock();
    }
}

public E take(){
    lock.lock();
    try{
        while(queue.size()==0){
            notEmpty.await(); //<== wait notEmpty
        }
        E item = queue.remove();
        notFull.signalAll();
        return item;
    }finally{
        lock.unlock();
    }
}
```

### Multiple reader, one writer <a href="#multiple-reader-one-writer" id="multiple-reader-one-writer"></a>

* reader to enter the critical section, we need to make sure that there's no **writer** in progress. It is ok to have other readers in the critical section since they aren't making any modifications
* allow a writer to enter the critical section, we need to make sure that there's **no reader or writer** in the critical section

![](broken-reference)

用syncronized/wait/notify实现ReadWriteLock。

```java
class ReadWriteLock {
    //synchronized 用来保护下面这两个变量
    boolean isWriteLocked = false;  //<== writing
    int readers = 0;                //<== have reader reading

    public synchronized void acquireReadLock() throws InterruptedException {
        while (isWriteLocked) {
            wait();
        }
        readers++;
    }

    public synchronized void releaseReadLock() {
        readers--;
        notify();
    }

    public synchronized void acquireWriteLock() throws InterruptedException {
        while (isWriteLocked || readers != 0) {
            wait();
        }
        isWriteLocked = true;
    }

    public synchronized void releaseWriteLock() {
        isWriteLocked = false;
        notify();
    }
}
```

### Ordered Printing <a href="#ordered-printing" id="ordered-printing"></a>

任务之间有依赖关系。

1. 可以用一个flag变量指示该执行那个任务了。如果不是这个变量就等待

```
synchronized(this) { 
    while (flag == 0) {
        try {
            this.wait();
        }catch (Exception e) { }
    }
    System.out.println("Bar");
    flag = 0;
    this.notifyAll();
}
```

1. CountDownLatch

```
如下使得task2依赖于task1完成
CountDownLatch latch1 = new CountDownLatch(1);
//task1 
latch1.countDown();
//task2 
latch1.await(); //count down >0, block
```

1. Use Condition

### 哲学家就餐问题 <a href="#zhe-xue-jia-jiu-can-wen-ti" id="zhe-xue-jia-jiu-can-wen-ti"></a>

[https://leetcode.com/problems/the-dining-philosophers/](https://leetcode.com/problems/the-dining-philosophers/)

Deadlock cases

* if all the philosophers simultaneously grab their left fork, none would be able to eat.

Solution 1) Five forks and four philosophers deadlock is impossible. 但是我在leetcode上run, 有问题

Solution 2) Make any one of the philosophers pick-up the left fork first instead of the right one

### Singleton <a href="#singleton" id="singleton"></a>

```java
//SOlution 1, volatile with double check
private voilate Resource rs=null;
public static Resource getInstance(){
    if(rs==null){
        synchronized(this){
            if(rs==null){
                rs=new Resource;
            }
        }
    }
    return rs;
}

//Solution 2, 
public class Resource{
    private class Holder {
        static final Resource INSTANCE = new Resource;
    }
    public static Resource getInstande(){
        return Holder.INSTANCE;
    }
}

//Solution 3 ENUM
```

## Java 虚拟机 <a href="#java-xu-ni-ji" id="java-xu-ni-ji"></a>

![](file:///C:/Users/ericl/AppData/Roaming/marktext/images/2022-08-15-10-15-32-image.png?msec=1670388363121)

程序计数器、虚拟机栈、本地方法栈3个区域随线程而生，随线程而灭，栈中的栈帧随着方法的进入和退出而有条不紊地执行着出栈和入栈操作。而Java堆和方法区这两个区域则有着很显著的不确定性：一个接口的多个实现类需要的内存可能会不一样，一个方法所执行的不同条件分支所需要的内存也可能不一样，只有处于运行期间，我们才能知道程序究竟会创建哪些对象，创建多少个对象，这部分内存的分配和回收是动态的。

## GC work <a href="#gc-work" id="gc-work"></a>

[https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/](https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/)
