# Java Top Questions You Must Know

<h3 id="content">Content</h3>

- I. Basics
  - [Differences between JDK, JRE and JVM?](#dbj)
  - [ ] Java every version features?
  - [ ] why java is platform independent?
  - [int vs Integer?](#ivi)
  - [equals vs == in String](#ev=)
  - [Function value passing or reference passing](#fvp)
  - [String, StringBuider, StringBuffer](#sss)
- II. Classes

  - [Object-oriented Features](#oof)
  - [Package access](#pa)
  - Polymorphism
    - [instanceof in Polymorphism](#iip)
    - [Function Calling in Polymorphism](#pfc)
    - [Object Instantiating in Polymorphism](#poo)
- III. Library & Advanced
  - Exception
    - [Common Exceptions](#ces)
    - [Exception return statement](#ers)
    - [Nested Exception Run Sequence](#ner)
  - IO
    - [Common IO Stream](#cis)
    - [IO Common Operations](#ico)
  - Container
    - [Common Container Class](#ccc)
    - [How to avoid ConcurrentModificationException](#hta)
    - [Thread Safe Container  and Thread-Safe](#tsc)
    - [What Is difference between different implementation Class of Container and How to choose](#wdc)
    - [How HashMap Works In Java](#hhw)
    - [Container Uitility Class](#cuc)
- IV. JVM
  - [How java works?](#hjw)
  - [Class Loading Process](#clp)
  - [Java Object Creation Process](#joc)
  - [Java Run-time memory](#jrm)
  - [JVM Configurations](#jcs)
  - [ ] **JVM Optimizations**
- V. Concurrency
  - [synchronized](#sync)
  - [Thread Pool](#tp)
  - [ ] **Concurrency API and keyword**
  - [ ] **Concurrency  Code Questions**
  - [Thread Deadlock](#tdk)
- VI. Network Communication
  - [I/O Models](#iom)
  - Socket Implementation
- VII. Design Pattern

  - [ ] [Common Design Patterns](#cdp)

### Main



### I. Basic



<h3 id="dbj"># Differences between JDK, JRE and JVM?</h3>

JVM(Java Virtual Machine)

- Specification, implementation, and instance.

- JVM Tasks: Loads code(class file), Verifies code, Executes code, Provides runtime environment.

JRE(Java Runtime Environment)

- It is a set of software tools.
- It provides the runtime environment.
- It is the implementation of JVM. It physically exists.
- It contains a set of libraries + other files that JVM users at runtime.

JDK(Java Development Kit)

- It contains JRE + development tools(javac, java, jar, javadoc etc)

Reference:

[Difference between JDK, JRE, and JVM](https://www.javatpoint.com/difference-between-jdk-jre-and-jvm)

[`back to content`](#content)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="ivi"># int vs Integer</h3>

Java supprots Auto boxing and Auto unboxing.

```java

Integer i = 1; // actual is Integer i = Integer.valueOf(1);
int j = i; // actual is int j = i.intValue();
```
Sources View

```java
Integer.class
public final class Integer extends Number implements Comparable<Integer> {
    private final int value;
    public Integer(int value) {
        this.value = value;
    }
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
    public int intValue() {
        return value;
    }
}

Integer$IntegerCache.class

private static class IntegerCache {
    static final int low = -128;
    static final int high; // high value may be configured by property, default 127.
    static final Integer cache[];
    static{
        ...
        int h = 127;
    	String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);
        assert IntegerCache.high >= 127;
    }
}

```

> cache of Byte, Short, Integer, Long all is between -128~127. Only Integer cache max value can set over 127.


__Interger == Integer  Example__

```java

Integer a1 = 1;
Integer a2 = 1;
Integer b1 = 128;
Integer b2 = 128;

a1 == a2 // true
b1 == b2 // false

```

>  key point : Integer == Integer contrast is reference address. IntegerCache affected the result.



__Integer == int or int == Integer Example__

```java
Integer a1 = 1;
Integer a2 = 128;
int b1 = 1;
int b2 = 128;

a1 == b1 // true
a2 == b2 // true
```

>  key point : int == Integer contrast is numeric value. Integer will auto unbox. It always == is true.

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="ev="># equals vs == in String</h3>



```java
String a1 = "hello";
String a2 = "he" + "llo";
String b1 = new String("hello");
String b2 = "he" + new String("llo");
String b3 = new String("he") + "llo";
String b4 = new String("hello");
System.out.println(a1 == a2); // true
System.out.println(a1 == b*); // false
System.out.println(b* == b*); // false
System.out.println(b*.equals(*)); // true
/*
	new String("xxx")    // string store in heap.
	"xxx"                // store in String static pool, no repeat string in there.
	new String("xxx") + "xxx"  // 
*/
```

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="fvp"># Function value passing or reference passing</h3>

Data Type consist of primitive data type and reference type.

Primitive Types: byte, short, int, long, float, double, boolean, char.

Reference Types: array, Object, String. WrapperClass Object

__Value passing: all of primitive types, String, all of wrapperClass Object__

__Reference passing: array, Object (Exclude String, WrapperClass)__

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="sss"># String, StringBuider, StringBuffer</h3>


immutable

- Their accomplishment all are  by char array.
- String is immutable whereas StringBuffer and StringBuilder are mutable classes. 
- String concat operation uses StringBuilder or StringBuffer better than String.

Thread Safety

- StringBuffer is thread safe and synchronized whereas StringBuilder is not. StringBuilder is more faster than StringBuffer.
- non-multi threaded environment uses StringBuilder better than StringBuffer.

References:

<https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder>



[`back to content`](#content)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---



### II. Classes

<h3 id="oof"> # Object-oriented Features</h3>

__Main Features of OOP__

- Abstract
- Encapsulation
- Inheritance
- Polymorphism

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="pa"> # Package Access</h3>

Key Wrods | Class | Pacakage | Not Package but Sub | Not Package not sub 
--- | --- | --- | --- | ---
private        | √ | × | × | × | 
default        | √ | √ | × | × | 
protected    | √ | √ | √ | × | 
public         | √ | √ | √ | √ | 

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------




<h3 id="iip">#  instanceof in Polymorphism</h3>



```java
class A{}
class B extends A{}
class C extends A{}
class D extends B{}

A obj = new D();
System.out.println(obj instanceof B); 
System.out.println(obj instanceof C); 
System.out.println(obj instanceof D); 
System.out.println(obj instanceof A); 
B  b = new B();
System.out.println(b instanceof B);
System.out.println(b instanceof A);
/* 
result:
    true
    false
    true
    true
    true
    true
*/
```

[`back to content`](#content)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="pfc"># Function Calling in Polymorphism </h3>

```java
public class A {
    public String show(D obj) {
        return ("A and D");
    }
    public String show(A obj) {
        return ("A and A");
    } 
}

public class B extends A{
    public String show(B obj){
        return ("B and B");
    }
    public String show(A obj){
        return ("B and A");
    } 
}

public class C extends B{
}

public class D extends B{
}

public class Test {
    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new B();
        B b = new B();
        C c = new C();
        D d = new D();
        
        System.out.println("1--" + a1.show(b));
        System.out.println("2--" + a1.show(c));
        System.out.println("3--" + a1.show(d));
        System.out.println("4--" + a2.show(b));
        System.out.println("5--" + a2.show(c));
        System.out.println("6--" + a2.show(d));
        System.out.println("7--" + b.show(b));
        System.out.println("8--" + b.show(c));
        System.out.println("9--" + b.show(d));    
    }
}
运行结果为：
1--A and A
2--A and A
3--A and D
4--B and A
5--B and A
6--A and D
7--B and B
8--B and B
9--A and D
```

__Consequences__

```java
A a1 = new A();
A a2 = new B();
B b = new B();
```

Super type and super Object - a1：(1)Only find in super. (2)call in super

Super type and son Object - a2: (1)only find in super. (2)First call in son.if not exist in son, then call super.

Son type and son object - b: (1)First find in son, then find in super. (2)Find just call.

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="poo"># Object Instantiating in Polymorphism</h3>



```java
public class Parent {
	private String name = "parent";
	public Parent()
	{
		System.out.println("Parent constructor...");
		print();
	}
	
	public void print()
	{
		System.out.println("Parent print...");
		System.out.println("name=" + name);
	}
}

public class Son extends Parent{
	private String name = "son";
	
	public Son()
	{
		System.out.println("Son constructor..");
	}
	
	public void print()
	{
		System.out.println("Son print..");
		System.out.println("name=" + this.name);
	}
}

public static void main(String[] args)
{
    Parent parent = new Son();
}
```

What is the main function print result?

```java
Parent Constructor...

Son print...

name=null   // When son print() execute by Polymorphism, but instance variales are not initialize and the Son contructor not execute. 

Son Constructor...
```



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---



### III. Library & Advanced




<h3 id="ces"># Common Exceptions</h3>

**java.lang:**

1. ArithmeticException
2. ArrayIndexOutOfBoundsException
3. ClassCastException
4. ClassNotFoundException
5. CloneNotSupportedException
6. IllegalArgumentExcepion
7. IllegalMonitorStateException
8. IllegalThreadStateException
9. IndexOutOfBoundsException
10. InterruptedException
11. NullPointerException
12. NumberFormatedException

**java.util:**

1. ConcurrentModificationException

**java.io :**

1. EOFException
2. FileNotFoundException
3. IOException
4. NotSerializableException

__Other:__

1. SQLExcepion

Classify by packages

- **Casting**: ClassCastException
- **Arrays**: ArrayIndexOutOfBoundsException, NullPointerException
- **Collections**: NullPointerException, ClassCastException (if you're not using autoboxing and you screw it up)
- **IO**: java.io.IOException, java.io.FileNotFoundException, java.io.EOFException
- **Serialization**: java.io.ObjectStreamException (AND ITS SUBCLASSES, which I'm too lazy to enumerate)
- **Threads**: InterruptedException, SecurityException, IllegalThreadStateException
- **Potentially common to all situations**: NullPointerException, IllegalArgumentException

Classify by checked

**Unchecked Exception List**
ArrayIndexOutOfBoundsException
ClassCastException
IllegalArgumentException
IllegalStateException
NullPointerException
NumberFormatException
AssertionError
ExceptionInInitializerError
StackOverflowError
NoClassDefFoundError

**Checked Exception List**
Exception
IOException
FileNotFoundException
ParseException
ClassNotFoundException
CloneNotSupportedException
InstantiationException
InterruptedException
NoSuchMethodException
NoSuchFieldException

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="ers"># Exception return statement</h3>

```java
try
{
	System.out.println("try...");
	int i = 1/0;
	return 1;
}
catch(Exception e)
{
	System.out.println("catch...");
	return 2;
}
finally
{
	System.out.println("finally...");
	return 3;
}
// result return 3;
```

- try block return statement will not be executed if it after occur exception statement.

- if catch have return statement will be executed, if finally have return statement will be executed and cover catch block return statement.

[`back to content`](#content)

---


<h3 id="ner"># Nested Exception Run Sequence</h3>

The finally not must run. using System.exit(0) in try or catch block.

```java
 try
 {
     System.out.println("try...");
     System.exit(0);
 }
 finally
 {
     System.out.println("finally...")
 }
```

Nested exception run example

```java
try
{
	try
	{
		System.out.println("try...");
		int i = 1/0;
	}
	catch (NullPointerException e )
	{
		System.out.println("catch...");
	}
	finally
	{
		System.out.println("finally...");
	}
}
catch(Exception e) 
{
	System.out.println("base catch...");
}
finally
{
	System.out.println("base finally...");
}
/* 
	try...
  	finally...
   	base catch..
   	base finally..
 */
```

__Consequences__

`try` - `try` execute until the last executable line.

`catch` - It can execute outside `catch` block if inside `catch` can't catch the exception.

`finally` - all `finally` block must execute from inside to outside.

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------




<h3 id="cis"># Common IO Stream</h3>

| classify | function       | byte input           | byte output          | char reader         | char writer         |
| -------- | -------------- | -------------------- | -------------------- | ------------------- | ------------------- |
|          | 抽象基类       | InputStream          | OutputStream         | Reader              | Writer              |
| 节点流   | 访问文件       | FileInputSream       | FileOutputStream     | FileReader          | FileWriter          |
|          | 访问数值       | ByteArrayInputStream | ByteArrayOutStream   | **CharArrayReader** | **CharArrayWriter** |
|          | 访问管道       | PipedInputStream     | **PipedOutStream**   | **PipedReader**     | **PipedWriter**     |
|          | 访问字符串     |                      |                      | **StringReader**    | **StringWriter**    |
| 处理流   | 缓冲流         | BufferedInputStream  | BufferedOutputStream | BufferedReader      | BufferedWriter      |
|          | 转换流         |                      |                      | InputStreamReader   | OutputStreamWriter  |
|          | 对象流         | ObjectInputStream    | ObjectOutputStream   |                     |                     |
|          | (过滤)抽象基类 | FilterInputStream    | *FilterOutputStream* | *FilterReader*      | *FilterWriter*      |
|          | 打印流         |                      | PrintStream          |                     | PrintWriter         |
|          | 退回输入流     | PushBackInputStream  |                      | PushbackReader      |                     |
|          | 特殊流         | DataInputStream      | DataOutputStream     |                     |                     |

__Consequences__

1. Java IO是采用的是装饰模式，即采用**处理流**来包装**节点流**的方式，来达到代码通用性。

2. 处理流和节点流的区分方法，**节点流**在新建时需要一个数据源（文件、网络）作为参数，而**处理流**需要一个节点流作为参数。

3. **处理流**的作用就是提高代码通用性，编写代码的便捷性，提高性能。

4. **节点流**都是对应抽象基类的实现类，它们都实现了抽象基类的基础读写方法。其中read（）方法如果返回-1，代表已经读到数据源末尾。

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="ico"># IO Common Operations</h3>

read byte/char from stream

write byte[]/char to stream

```java
try (
	BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:/test.txt"));
	BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("D:/test2.txt"));
)
{
	StringBuilder sb = new StringBuilder();
	
	// byte[] IO
	byte[] buf = new byte[20];
	int len = -1;
	while ((len = bis.read(buf)) != -1)
	{
		bos.write(buf, 0, len);
		bos.flush();
		sb.append(new String(buf, 0, len, StandardCharsets.UTF_8));
	}
	
	
	// char IO
	/*int ch = -1;
	char[] c = new char[1];
	while ((ch = bis.read()) != -1)
	{
		bos.write(ch);
		bos.flush();
		c[0] = (char)ch;
		sb.append(c);
	}*/
	
	System.out.println(sb.toString());
} catch (FileNotFoundException e) {
	e.printStackTrace();
} catch (IOException e) {
	e.printStackTrace();
}
```

read char from stream 

write char to stream

```java
try (
	BufferedReader br = new BufferedReader(new FileReader("D:/test.txt"));
	BufferedWriter bw = new BufferedWriter(new FileWriter("D:/test2.txt"));
)
{
	StringBuilder sb = new StringBuilder();
	
	// string io
	/*String buf = null;
	while ((buf = br.readLine()) != null)
	{
		buf += "\r\n";
		bw.write(buf);
		sb.append(buf);
	}*/
	
	// char[] IO
	char[] buf = new char[1024];
	int len = 0;
	while ((len = br.read(buf)) != -1)
	{
		bw.write(buf, 0, len);
		sb.append(buf, 0, len);
	}
	
	// char IO
	/*int c = -1;
	char[] str = new char[1];
	while ((c = br.read()) != -1)
	{
		bw.write(c);
		str[0] = (char) c;
		sb.append(str);
	}*/
	
	System.out.println(sb.toString());
} catch (FileNotFoundException e) {
	e.printStackTrace();
} catch (IOException e) {
	e.printStackTrace();
}
```

read object from stream

write object to stream

```java
try (
	ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:/object.txt"));
	ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:/object.txt"));
)
{
	oos.writeObject(new Draft());
	Draft obj = (Draft) ois.readObject();
} catch (ClassNotFoundException e) {
	e.printStackTrace();
}
```



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="ccc"># Common Container Class</h3>

```java
java.util
java.util.concurrent

I-Collection
	I-List
		*C-AbstractList
            C-LinkedList
            C-ArrayList
            ?C-Vector
                ?C-Stack
	I-Set
		*C-AbstractSet
            C-HashSet
                C-LinkedHashSet
            C-TreeSet
        I-SortedSet
        	I-NavigableSet
            	C-TreeSet
	I-Queue
		*C-AbstractQueue
			C-PriorityQueue
			C-LinkedBlockingQueue
		I-BlockingQueue
		I-Deque
	
I-Map
	*C-AbstractMap
        C-HashMap
            C-LinkedHashMap
        C-TreeMap
	I-SortedMap
		I-NavigableMap
			C-TreeMap

I-Iterator
	I-ListIterator
I-Comparable
C-Collections
C-Arrays

```


[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="hta"># How to avoid ConcurrentModificationException</h3>

**When does ConcurrentModificationException occur**

List use iterator() to remove/add element can occur ConcurrentModificationException.

When Set traverse, remove/add element occur ConcurrentModificationException.

When Map traverse, remove/add element occur ConcurrentModificationException.

**Occur java.util.ConcurrentModificationException Example**

```java
// list
Iterator<String> iterator = myList.iterator();
while (iterator.hasNext())
{
	if (iterator.next().equals("a"))
	{
		myList.remove("a"); //myList.add("new");
	}
}

// set
for (String s : mySet)
{
	if (s.equals("a"))
	{
		mySet.remove(s); //mySet.add("new");
	}
}
// set2
Iterator<String> iterator = mySet.iterator();
while (iterator.hasNext())
{
	if (iterator.next().equals("a"))
	{
		mySet.remove("a"); //mySet.add("new");
	}
}

// map
for (Map.Entry<String, String> entry : myMap.entrySet())
{
	if (entry.getKey().equals("a"))
	{
		myMap.remove("a"); //myMap.put("new", "new_value");
	}
}
// map2
for (String key : myMap.keySet())
{
	if (key.equals("a"))
	{
		myMap.remove("a"); //myMap.put("new", "new_value");
	}
}
```

__Why does ConcurrentModificationException occur__ 

Iterator fail-fast property checks for any modification in the structure of the underlying collection everytime we try to get the next element. If there are any modifications found, it throws `ConcurrentModificationException`. All the implementations of Iterator in Collection classes are fail-fast by design except the concurrent collection classes like ConcurrentHashMap and CopyOnWriteArrayList.

```java
// HashTable.java

transient int modCount;

final Node<K,V> nextNode() {
    Node<K,V>[] t;
    Node<K,V> e = next;
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
    if (e == null)
        throw new NoSuchElementException();
    if ((next = (current = e).next) == null && (t = table) != null) {
        do {} while (index < t.length && (next = t[index++]) == null);
    }
    return e;
}
public final void remove() {
    Node<K,V> p = current;
    if (p == null)
        throw new IllegalStateException();
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
    current = null;
    K key = p.key;
    removeNode(hash(key), key, null, false, false);
    expectedModCount = modCount;
}
```



__How to avoid ConcurrentModificationException occur__

CopyOnWriteArrayList instead of ArrayList

ConcurrentHashMap instead of HashMap 

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="tsc"># Thread Safe Container  and Thread-Safe</h3>

Vector, Hashtable, Properties and Stack are synchronized classes, so they are thread-safe and can be used in multi-threaded environment. Java 1.5 Concurrent API included some collection classes that allows modification of collection while iteration because they work on the clone of the collection, so they are safe to use in multi-threaded environment.



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="#wdc"># What Is difference between different implementation Class of Container, and How to choose</h3>



__What is difference between HashMap and Hashtable?__

   HashMap and Hashtable both implements Map interface and looks similar, however there are following difference between HashMap and Hashtable.

   1. HashMap allows null key and values whereas Hashtable doesn’t allow null key and values.
   2. Hashtable is synchronized but HashMap is not synchronized. So HashMap is better for single threaded environment, Hashtable is suitable for multi-threaded environment.
   3. `LinkedHashMap` was introduced in Java 1.4 as a subclass of HashMap, so incase you want iteration order, you can easily switch from HashMap to LinkedHashMap but that is not the case with Hashtable whose iteration order is unpredictable.
   4. HashMap provides Set of keys to iterate and hence it’s fail-fast but Hashtable provides Enumeration of keys that doesn’t support this feature.
   5. Hashtable is considered to be legacy class and if you are looking for modifications of Map while iterating, you should use ConcurrentHashMap.



__How to decide between HashMap and TreeMap?__

   For inserting, deleting, and locating elements in a Map, the HashMap offers the best alternative. If, however, you need to traverse the keys in a sorted order, then TreeMap is your better alternative. Depending upon the size of your collection, it may be faster to add elements to a HashMap, then convert the map to a TreeMap for sorted key traversal.



__What are similarities and difference between ArrayList and Vector?__

   ArrayList and Vector are similar classes in many ways.

      1. Both are index based and backed up by an array internally.
      2. Both maintains the order of insertion and we can get the elements in the order of insertion.
      3. The iterator implementations of ArrayList and Vector both are fail-fast by design.
      4. ArrayList and Vector both allows null values and random access to element using index number. These are the differences between ArrayList and Vector.
      5. Vector is synchronized whereas ArrayList is not synchronized. However if you are looking for modification of list while iterating, you should use CopyOnWriteArrayList.
      6. ArrayList is faster than Vector because it doesn’t have any overhead because of synchronization.
      7. ArrayList is more versatile because we can get synchronized list or read-only list from it easily using Collections utility class.

__What is difference between Array and ArrayList? When will you use Array over ArrayList?__

   Arrays can contain primitive or Objects whereas ArrayList can contain only Objects.
   Arrays are fixed size whereas ArrayList size is dynamic.
   Arrays doesn’t provide a lot of features like ArrayList, such as addAll, removeAll, iterator etc.

   Although ArrayList is the obvious choice when we work on list, there are few times when array are good to use.

   - If the size of list is fixed and mostly used to store and traverse them.
   - For list of primitive data types, although Collections use autoboxing to reduce the coding effort but still it makes them slow when working on fixed size primitive data types.
   - If you are working on fixed multi-dimensional situation, using [][] is far more easier than List<List<>>

__What is difference between ArrayList and LinkedList?__

   ArrayList and LinkedList both implement List interface but there are some differences between them.

   1. ArrayList is an index based data structure backed by Array, so it provides random access to it’s elements with performance as O(1) but LinkedList stores data as list of nodes where every node is linked to it’s previous and next node. So even though there is a method to get the element using index, internally it traverse from start to reach at the index node and then return the element, so performance is O(n) that is slower than ArrayList.
   2. Insertion, addition or removal of an element is faster in LinkedList compared to ArrayList because there is no concept of resizing array or updating index when element is added in middle.
   3. LinkedList consumes more memory than ArrayList because every node in LinkedList stores reference of previous and next elements.

   

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="hhw"># How HashMap Works In Java</h3>



HashMap stores key-value pair in Node<K,V> implements Map.Entry. HashMap works on hasing algorithm and uses hashCode() and equals() method in `put` and `get` methods. 

HashMap consist of the Array of LinkedList. 

When we call `put` method by passing key-value pair, HashMap uses key hashCode() with hasing find out the index of array to strore the key-value pair. The Entry is stored in the LinkedList. It uses equals() method to check if the passed key already exists, if yes it overwirtes the value else it creates a new entry and store this key-value entry.

When we call `get` method by passing key, again it uses the hashCode() to find the index in the array and then use equals() method  to find the correct Entry and return it's value.

HashMap's capacity, load factor, threshold resizing. HashMap initial default capacity is 16 and load factor is 0.75. Threshold is capacity multiplied by load factor and whenever we try to add an entry, if map size is greater than threshold, HashMap rehashes the contents of map into a new array with a larger capacity. The capacity is always power of 2, so if you know that you need to store a large number of key-value pairs, for example in caching data from database, it’s good idea to initialize the HashMap with correct capacity and load factor.

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="cuc">Container Utility Class</h3>

__How can we sort a list of Objects?__
If we need to sort an array of Objects, we can use Arrays.sort(). If we need to sort a list of objects, we can use Collections.sort(). Both these classes have overloaded sort() methods for natural sorting (using Comparable) or sorting based on criteria (using Comparator).
Collections internally uses Arrays sorting method, so both of them have same performance except that Collections take sometime to convert list to array.

__How can we create a synchronized collection from given collection?__

We can use `Collections.synchronizedCollection(Collection c)` to get a synchronized (thread-safe) collection backed by the specified collection.

__While passing a Collection as argument to a function, how can we make sure the function will not be able to modify it?__

We can create a read-only collection using Collections.unmodifiableCollection(Collection c) method before passing it as argument, this will make sure that any operation to change the collection will throw UnsupportedOperationException.

__What are best practices related to Java Collections Framework?__

- Chosing the right type of collection based on the need, for example if size is fixed, we might want to use Array over ArrayList. If we have to iterate over the Map in order of insertion, we need to use TreeMap. If we don’t want duplicates, we should use Set.
- Some collection classes allows to specify the initial capacity, so if we have an estimate of number of elements we will store, we can use it to avoid rehashing or resizing.
- Write program in terms of interfaces not implementations, it allows us to change the implementation easily at later point of time.
- Always use Generics for type-safety and avoid ClassCastException at runtime.
- Use immutable classes provided by JDK as key in Map to avoid implementation of hashCode() and equals() for our custom class.
- Use Collections utility class as much as possible for algorithms or to get read-only, synchronized or empty collections rather than writing own implementation. It will enhance code-reuse with greater stability and low maintainability.


[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


References:

<https://www.journaldev.com/1330/java-collections-interview-questions-and-answers#java8-collections>

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---


### IV. JVM



<h3 id="hjw"># How Java Works</h3>

Javac convert source code => bytecode file

JVM Class Loader loading bytecode to memory, `Byte Code Verifier` verifying bytecode, `Execution Engine` convert bytecode file => machine code

Most Computer languages use the "compile-link-execute" format.

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="clg"># Class Loading Process</h3>

Process of Class Loading

- JVM Loading

- JVM Linking

- JVM Initialization

__类加载过程__

①加载阶段。是指把类的.class文件中的二进制数据读入内存中，把它存放在运行时数据区的方法区内，然后在堆区创建一个java.lang.Class对象，用来封装类在方法区内的数据结构。

②连接阶段。包括验证、准备和解析类的二进制

③初始化阶段。给类的静态变量赋予正确的初始值，若有显示赋值，则把值赋给变量，若没有，则取默认值，然后，执行静态代码块。

__类加载的时机__

java虚拟机只有在程序首次主动使用一个类和接口时才会初始化它。

下例6种程序对类或接口的主动使用

①创建类的实例：用new语句创建实例，或者通过反射、克隆及反序列化手段来创建实例。

②调用类的静态方法。

③访问类和接口的静态变量或者为该静态变量赋值。

④调用java API中某些反射方法，Class.forName("Worker");

⑤初始化一个类的子类。

⑥java虚拟机启动时被标明为启动类的类。

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3 id="joc"># Java Object Creation Process in run-time memory</h3>

__对象创建过程__

①为对象在堆中分配内存空间

②将对象的实例变量自动初始化为默认值。若实例变量在声明时被显式的初始化，则把相应的值赋给变量。然后，运行构造代码块

③调用构造方法

④返回对象的引用（Sample s=new Sample（）表示将对象的引用赋给s引用变量）

（注：若该类有父类，则先初始化父类的实例变量、执行父类构造代码块，调用父类的构造方法。再初始化该类的实例变量，执行构造代码块，调用构造方法）



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="jrm">Java Run-time memory</h3>

java运行时内存分配

①程序计数器

程序计数器是一块较小的区域，它的作用可以看做是当前线程所执行的字节码的行号指示器。在虚拟机的模型里，字节码指示器就是通过改变程序计数器的值来指定下一条需要执行的指令。分支，循环等基础功能就是依赖程序计数器来完成的。

②堆

堆是Java虚拟机所管理的内存中最大的一块。堆是所有线程共享的一块区域，在虚拟机启动时创建。堆的唯一目的是存放对象实例，几乎所有的对象实例都在这里分配

③方法区

法区也是线程共享的区域，用于存储已经被虚拟机加载的类信息，常量，静态变量和即时编译器（JIT）编译后的代码等数据。

④运行时常量池

运行时常量池是方法区的一部分，Class文件中除了有类的版本，字段，方法，接口等信息以外，还有一项信息是常量池用于存储编译器生成的各种字面量和符号引用，这部分信息将在类加载后存放到方法区的运行时常量池中。

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="jcs">JVM Configurations</h3>

- Heap Memory
- Garbage Collection (choosing of right Garbage Collection algorithm is critical)
- GC Logging  (Checking  the JVM’s *Garbage Collection* performance by logging the *GC* activity in human readable format)
- Handling Out of Memory (dump heap memory into a physical file)
- 32/64 bit
- Misc

__Heap__

`-Xms<heap size>[unit]`

`-Xmx<heap size>[unit]`

unit g for GB, m for MB and k for KB.

For Example, assign minimum 2 GB and maximum 5 GB to JVM.

`Xms2GB -Xmx5G`

__Garbage Collection__

```java
-XX:+UseSerialGC
-XX:+UseParallelGC
-XX:+USeParNewGC
-XX:+UseG1GC
```

__GC Logging__

```
-XX:+UseGCLogFileRotation  
-XX:NumberOfGCLogFiles=10
-XX:GCLogFileSize=50M 
-Xloggc:/home/user/log/gc.log
```

__Handling Out of Memory__

```java
-XX:+HeapDumpOnOutOfMemoryError 
-XX:HeapDumpPath=./java_pid<pid>.hprof
-XX:OnOutOfMemoryError="< cmd args >;< cmd args >" 
-XX:+UseGCOverheadLimit
```

__32/64 bit__
In the OS environment where both 32 and 64-bit packages are installed, the JVM automatically chooses 32-bit environmental packages.
If we want to set the environment to 64 bit manually, we can do so using below parameter:

`-d<OS bit>`





[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h3 id="jos">JVM Optimizations</h3>



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------








### V. Concurrency






<h3 id="sync"># synchronized</h3>

synchronized:

- 被synchronized修饰的代码，多个线程使用相同的synchronized锁，只能同时被一个线程执行。
- 同步锁，可以是类级别的（调用一个类的静态方法互斥），可以是对象级别的（调用同一个对象的方法互斥）。

```java
public class Demo{
    // this static method can be executed by one thread execute at a time. The lock is Demo.class.
    public synchronized static void foo(){
        
    }
}

public class Demo {
    // this instatnce method can be executed by one thread execute at a time when multiple thread using same object. The lock is this.
    public synchronized void foo(){
        
    }
}

public class Test implements Runnable{
    @Override
    public void run(){
        synchronized (Demo.class){
            Demo.foo(); // when one thread execute this line, get the lock, Other thread can't enter synchronized block.
        }
    }
} 

public class Test implements Runnable{
    private Demo demo;
    
    public Test(Demo demo)
    {
        this.demo = demo;
    }
    
    @Override
    public void run()
    {
        synchronized (demo){
            demo.foo(); // when one thread execute this line, get the lock, Other thread uses same demo object, can't enter synchronized block.
        }
    }
}

```

[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="tp"># Thread Pool</h3>

What is Thread Pool

- It manages the pool of worker threads.
- It contains a queue that keeps tasks waiting to get executed.

Why use thread pool

- Focus on the tasks. you want the thread to perform instead of thread mechanics.
- Easy to manage tasks to perform.
- Solution for the problem of thread cycle overhead. (Repeating create and destroy thread)
- Solution for the problem of resource thrashing(if every request create a new thread, too many threads cause system to run out of memory).

How to use thread pool

- Java provides the Executor framework which is centered around the Executor interface.

```java
java.lang
		class-Thread
		interface-Runnable
java.util.concurrent
        interface-Executor
            interface-ExecutorService
                *class-AbstractExecutorService
                    class-ThreadPoolExecutor
                        class-ScheduleThreadPoolExecutor
                    class-ForkJoinPool
                interface-ScheduleExecutorService
                        class-ScheduleThreadPoolExecutor
        
        class-Executors
		interface-Future
			*class-ForkJoinTask
				*class-RecursiveTask
		interface-Callable
```

- Executor 

its sub-interface-ExecutorService, class-ThreadPoolExecutor, class-ScheduledThreadPoolExecutor.

- Executors 

It is a utility class that provices useful methods to work with ExecutorService, ScheduleExecutorService, ThreadFactory, and Callable classes through various factory methods.

```java
class-Executors methods
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }

```

- ThreadPoolExecutor

```java
class-ThreadPoolExecutor
	public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, 
    	long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue)
	{
    	this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
	}
	public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }
```

- Callable

It java 5 introduced java.util.concurrenct.Callable interface. It is similar to Runnable interface, but it can return any Object and able to throw Exception.

- Future

Java Callable tasks return java.utl.concurrent.Future object. It provide get() method get the Callable task return Object.

Using Example

Single Thread Pool by Executor Example

```java
    Executor executor = Executors.newSingleThreadExecutor();
    executor.execute(() -> System.out.println("Hello World")); // Runnable
	executor.shutdown();
```



Fixed Thread Pool by Executors Example

```java
    ExecutorService executor = Executors.newFixedThreadPool(5);
    for (int i=0; i < 10; i++)
    {
        executor.execute(new Runnable()
        {
            @Override
            public void run(){
                System.out.println(Thread.currentThread().getName() + " start...");
                Thread.sleep(3000);
                System.out.println(Thread.currentThread().getName() + " end");
            }
        });
    }
    executor.shutdown();
    while (! executor.isTerminated()) {
    }
    System.out.println("Finished all threads");
```

Fixed Thread Pool by ThreadPoolExecutor Example

```java
    ThreadPoolExecutor executor = new ThreadPoolExecutor(10, 10,
        0L, TimeUnit.MILLISECONDS,
        new LinkedBlockingQueue<Runnable>());
    for (int i=0; i < 10; i++)
    {
        executor.execute(new Runnable() {
            @Override
            public void run() {
                System.out.println("hh");
            }
        });
    }
    executor.shutdown();
    while (! executor.isTerminated()) {
    }
    System.out.println("Finished all threads");
```

Callable & Future Example

```java
    ExecutorService executor = Executors.newFixedThreadPool(10);
    Callable<String> callable = new Callable<String>(){
        @Override
        public String call() throws Exception {
            Thread.sleep(1000);
            return Thread.currentThread().getName();
        }
    };
    for (int i=0; i < 100; i++){
        Future<String> future = executor.submit(callable);
        System.out.println(future.get());
    }
    executor.shutdown();
    while (! executor.isTerminated()){
    }
    System.out.println("Finished all threads");

 	ExecutorService executorService = Executors.newFixedThreadPool(10);
    Future<String> future = executorService.submit(() -> "Hello World"); //Callable
    String result = future.get();
	executor.shutdown();
```



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



<h3 id="tdk">Thread Deadlock</h3>

What is deadlock

A situation where two or more threads are blocked forever, waiting for each other.

__How to avoid deadlock__

- don't use multiple threads
- don't hold several locks at once. If you do, always acquire the locks in the same order.
- don't execute foreign code while holding a lock.
- use interruptible locks.

Deadlock Example

```java
	static Object lock1 = new Object();
	static Object lock2 = new Object();
	public static void main(String[] args) throws InterruptedException, ExecutionException
	{
		new Thread(new Runnable() {
			@Override
			public void run()
			{
				synchronized(lock1)
				{
					System.out.println(Thread.currentThread().getName() + "hold lock1...");
					sleep();
					synchronized(lock2)
					{
						System.out.println(Thread.currentThread().getName() + "hold lock2...");
						sleep();
					}
				}
			}
		}).start();
		new Thread(() -> {
			synchronized(lock2)
			{
				System.out.println(Thread.currentThread().getName() + "hold lock2...");
				sleep();
				synchronized(lock1)
				{
					System.out.println(Thread.currentThread().getName() + "hold lock1...");
				}
			}
		}).start();
	}
```



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



### Network Communication

<h3 id="iom">I/O Models</h3>

> 《UNIX网络编程》说得很清楚，5种IO模型分别是阻塞IO模型、非阻塞IO模型、IO复用模型、信号驱动的IO模型、异步IO模型；前4种为同步IO操作，只有异步IO模型是异步IO操作。

- IO (BIO, blocking IO)
- NIO (Non-blocking IO)
- IO multiplexing
- Signal Driven IO
- Asynchronous IO

**IO**

IO (Input/Output，输入/输出)即数据的读取（接收）或写入（发送）操作，通常用户进程中的一个完整IO分为两阶段：用户进程空间<-->内核空间、内核空间<-->设备空间（磁盘、网络等）。

LINUX中进程无法直接操作I/O设备，其必须通过系统调用请求kernel来协助完成I/O动作；内核会为每个I/O设备维护一个缓冲区。

对于一个输入操作来说，进程IO系统调用后，内核会先看缓冲区中有没有相应的缓存数据，没有的话再到设备中读取，因为设备IO一般速度较慢，需要等待；内核缓冲区有数据则直接复制到进程空间。

> 所以，对于一个网络输入操作通常包括两个不同阶段：

> > （1）等待网络数据到达网卡→读取到内核缓冲区，数据准备好；
> >
> > （2）从内核缓冲区复制数据到进程空间。

**阻塞IO模型**

进程发起IO系统调用后，进程被阻塞，转到内核空间处理，整个IO处理完毕后返回进程。操作成功则进程获取到数据。

典型应用：阻塞socket、Java BIO

特点：

进程阻塞挂起不消耗CPU资源，及时响应每个操作**；实现难度低、开发应用较容易；适用并发量小的网络应用开发；

不适用并发量大的应用**：因为一个请求IO会阻塞进程，所以，得为每请求分配一个处理进程（线程）以及时响应，系统开销大。

**非阻塞IO模型**

进程发起IO系统调用后，如果内核缓冲区没有数据，需要到IO设备中读取，进程返回一个错误而不会被阻塞；进程发起IO系统调用后，如果内核缓冲区有数据，内核就会把数据返回进程。

典型应用：socket是非阻塞的方式（设置为NONBLOCK）

特点：

进程轮询（重复）调用，消耗CPU的资源**；

实现难度低、开发应用相对阻塞IO模式较难；

适用并发量较小、且不需要及时响应的网络应用开发；

**IO复用模型**

多个的进程的IO可以注册到一个复用器（select）上，然后用一个进程调用该select， select会监听所有注册进来的IO；如果select没有监听的IO在内核缓冲区都没有可读数据，select调用进程会被阻塞；而当任一IO在内核缓冲区中有可数据时，select调用就会返回；

典型应用：select、poll、epoll三种方案，nginx都可以选择使用这三个方案;Java NIO;

特点：

专一进程解决多个进程IO的阻塞问题，性能好；Reactor模式;

实现、开发应用难度较大；

适用高并发服务应用开发：一个进程（线程）响应多个请求；

**信号驱动IO模型**

当进程发起一个IO操作，会向内核注册一个信号处理函数，然后进程返回不阻塞；当内核数据就绪时会发送一个信号给进程，进程便在信号处理函数中调用IO读取数据。

特点：回调机制，实现、开发应用难度大；

**异步IO模型**

当进程发起一个IO操作，进程返回（不阻塞），但也不能返回果结；内核把整个IO处理完后，会通知进程结果。如果IO操作成功则进程直接获取到数据。

典型应用：JAVA7 AIO、高性能服务器应用

特点：

不阻塞，数据一步到位；Proactor模式；

需要操作系统的底层支持，LINUX 2.5 版本内核首现，2.6 版本产品的内核标准特性；

实现、开发应用难度大；

非常适合高性能高并发应用；



**阻塞IO和非阻塞IO调用和模型**

BIO： 在用户进程（线程）中调用执行的时候，进程会等待该IO操作，而使得其他操作无法执行。

NIO：在用户进程中调用执行的时候，无论成功与否，该IO操作会立即返回，之后进程可以进行其他操作（当然如果是读取到数据，一般就接着进行数据处理）。

**同步IO和异步IO**

同步IO：导致请求进程阻塞，直到I/O操作完成。
异步IO：不导致请求进程阻塞。

References: 
[5种IO模型、阻塞IO和非阻塞IO、同步IO和异步IO](https://blog.csdn.net/tjiyu/article/details/52959418) 

### VII. Design Patterns



<h3 id="cdp"># Common Design Patterns</h3>

(Composite, Strategy, Decorator, Abstract Factory, Bridge, )

- Creational Patterns
  - Abstract Factory
  - Singleton
- Structural Patterns
  - Proxy
- Behavioral Patterns
  - Strategy

__Abstract Factory__

**1.What is it**

- Abstract Factory patterns work around a super-factory which creates other factories. This factory is also called as factory of factories. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

- Provide an interface for creating families of related or dependent object without specify their concrete classes.

**2.  Why use it**

- 抽象工厂方法是针对与一个产品族，使得易于交换产品系列。如精装版书籍转换为普通装书籍。

__3.Implementation__

**Basic Structure**

- Concrete factories implements abstract factory
- Concrete factory product one abstract product
- Concrete products implements abstract product.
- FactoryProvider provide a concrete factory.

```java

interface AbstractFactory                                                                                 
	abstract T create(String type);

class ConcreteFactoryA implements AbstractFactory
	public ProductA create(String type){}

class ConocreteFactoryB implements AbstractFactory
	public ProductB create(String type){}
    
interface AbstractProductA
class ProductA1 implements AbstractProductA
class ProductA2 implements AbstractProductA
	
interface AbstractProductB
class ProductB1 implements AbstractProductB
class ProductB2 implements AbstractProductB

public class FactoryProvider
    pubilc static AbstractFactory getFactory(String choice){}
public class Client
```



**Detail Example**

```java
public interface AbstractFactory<T> {
	T create(String type);
}
public class AnimalFactory implements AbstractFactory<Animal>{
	@Override
	public Animal create(String type) {
		if ("dog".equals(type))
		{
			return new Dog();
		}
		else if ("cat".equals(type))
		{
			return new Cat();
		}
		return null;
	}
}
public class PlantFactory implements AbstractFactory<Plant>{
	@Override
	public Plant create(String type) {
		if ("tree".equals(type))
		{
			return new Tree();
		}
		else if ("grass".equals(type))
		{
			return new Grass();
		}
		return null;
	}
}

public interface Animal {
	String getName();
}
public class Cat implements Animal{
	private String name = "cat";
	@Override
	public String getName() {
		return name;
	}
}
public class Dog implements Animal{
	private String name = "dog";
	@Override
	public String getName() {
		return name;
	}
}

public interface Plant {
	String getName();
}
public class Grass implements Plant{
	private String name = "grass";
	@Override
	public String getName() {
		return name;
	}
}
public class Tree implements Plant{
	private String name = "tree";
	@Override
	public String getName() {
		return name;
	}
}

public class FactoryProvider {
	public static AbstractFactory getFactory(String type)
	{
		if ("animal".equals(type))
		{
			return new AnimalFactory();
		}
		else if ("plant".equals(type))
		{
			return new PlantFactory();
		}
		return null;
	}
}

public class Client {
	public static void main(String[] args)
	{
		AbstractFactory<?> factory = FactoryProvider.getFactory("animal");
		Animal animal = (Animal) factory.create("dog");
		System.out.println(animal.getName());
		
		AbstractFactory<?> factory2 = FactoryProvider.getFactory("plant");
		Plant plant = (Plant) factory2.create("tree");
		System.out.println(plant.getName());
	}
}
```



__Singleton__

**1. What is it**

Ensure a class only has one instance, and provide a global point of access to it.

**2. Why use it**

**3. Implementation**

Requirement

- private constructor
- static get instance method

```java
​```java
// hungry mode
public class SingleObject_hungry 
{
	private final static SingleObject_hungry instance = new SingleObject_hungry();
	private SingleObject_hungry() {}
	public static SingleObject_hungry getInstance() 
	{
		return instance;
	}
}
// lazy
public class SingleObject_lazy 
{
	private static SingleObject_lazy instance;
	private SingleObject_lazy() {}
	public synchronized static SingleObject_lazy getInstance() 
	{
		if (instance == null)
		{
			synchronized (SingleObject_lazy.class)
			{
				if (instance == null)
				{
					instance = new SingleObject_lazy();
				}
			}
		}
		return instance;
	}
}
// lazy2
public class SingleObject_lazy2 
{
	private SingleObject_lazy2() {}
	
	private static class SingletonInstance
	{
		private static final SingleObject_lazy2 INSTANCE = new SingleObject_lazy2();
	}
	
	public static SingleObject_lazy2 getInstance() 
	{
		return SingletonInstance.INSTANCE;
	}
}
```

__Proxy__

**1. What is it**

- a (proxy) class represents functionality of another class..

**2..why use it**

- Hide actual Object detail. Wrap it actual Object method.
- Controls and manage access to the actual Object.

**3. Implementation**

- proxyObject and realObject all implements abstractObject.
- proxyObject create realObject.
- proxyObject wrap methods of realObject.

Basic structure

```java
interface AstractObject
class ObjectA implements AbstractObject
class ProxyObjectA implements AbstractObject
class Client
    AbsractObject obj = new ProxyObjectA();
```

Detail Implementation

```java
public interface Image 
{
	void display();
}
public class RealImage implements Image
{
	private String fileName;
	public RealImage() {}
	public RealImage(String fileName)
	{
		this.fileName = fileName;
	}
	@Override
	public void display() 
	{
		System.out.println("display image...");
	}
}
public class ProxyImage implements Image
{
	private String fileName;
	private RealImage realImage;
	public ProxyImage() {}
	public ProxyImage(String fileName)
	{
		this.fileName = fileName;
		realImage = new RealImage(this.fileName);
	}
	
	@Override
	public void display()
	{
		realImage.display();
		System.out.println("proxy...");
	}
}
public class Client 
{
	public static void main(String[] args) 
	{
		Image image = new ProxyImage("hello.txt");
		image.display();
	}
}
```



References:

<https://www.baeldung.com/java-abstract-factory-pattern>

<https://www.tutorialspoint.com/design_pattern/abstract_factory_pattern.htm>



[`back to content`](#content)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

--END--