# Java Note

### Content

<div id="content"></div>
- Introduction to Java
- II. Libraries
- I. Basics
  - [Variable and Basic Types](#vabt)
  - Strings , Vectors, and Arrays
  - Expressions
  - Statements
  - Functions
  - Objects and Classes
  - Inheritance, Polymorphism
  - [Interface, Lambda and Inner classes](#ilai)
  - Exceptions, Assertions, and Logging
  - Object the Superclass
  - Wrapper Classes
  - Enum Types
  - Generic Programming
- II. Libraries
  - IO library
  - Container (Collections framework)
  - [Date Time Library](#dtli)
  - Regular expressions
- III. Advanced Topics
  - Reflect and proxies
  - The event listener model
  - Annotations
  - Concurrency
  - The Stream API
  - File processing
  - Databases Programming
  - XML processing
  - Internationalization
  - Network Programming
  - GUI
  - Swing
  - JavaFX
  - Native methods
  - JVM
- IV. Others
  - Java 8 Features
  - Java 9 Features

### Main

### Variable and Basic Types

<div id="vabt"></div>
#### Big Number



### Interface, Lambda and Inner classes

<div id="ilai"></div>
#### Lambda

##### What is Lambda

> lambda expressions as a way of supporting functional programming in Java. Functional programming is a paradigm that allows programming using expressions i.e. declaring functions, passing functions as arguments and using functions as statements (rightly called expressions in Java8).

Lambda Expression

- Lambda Expression in Place of Anonymous Class.

- Using Consumer interface implements functional programming.

Why Lambdas

Advantages and disadvantages



##### The Syntax of Lambda Expression



##### Functional Interfaces

You can supply a lambda expression whenever an object of an interface with a **single** abstract method is expected. Such  an interface is called `functional interface`. For example, the interface Runnable only has void run(), the Comparator interface only has int compare(T o1, T o2).

```java
// Comparator funcational interface
String[] planets = new String[] {"Mercury", "Earth"};
Arrays.sort(planets, (first, second) -> {return first.length() - second.length();});
```

Notice: If an interface declares an abstract method overriding one of the public methods of java.lang.Object, that also does not count toward the interface's abstract method count since any implementation of the interface will have an implementation from java.lang.Object or elsewhere. Refer to [javadoc of  FunctionalInterface](https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html). 

Although the Comparator interface has two methods that are `int compare(T o1, T o2)` and `boolean equals(Object obj)`. But the `equals()` method is overriding `java.lang.Object`, It does not count toward the interface's abstract method count. 

The `@FunctionalInterface` is for to check "Has the interface only a single abstract method?".

##### Method References

##### Constructor References

##### Variable Scope

##### Processing Lambda Expressions

```java
public interface Consumer<T> {}
public interface Iterable<T> {
    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
}
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
list.forEach(i -> System.out.println(i));
```

References

[Functional Programming in Java8 Using Lambda Expressions](http://www.tothenew.com/blog/functional-programming-in-java8-using-lambda-expressions/)

[`back to content`](#content)



### Data Time Library

<div id="dtli"></div>
- java.util 
  - Date
  - Calendar
  - TimeZone
  - SimpleDateFormat
  - TimeUnit
- java.time // Since Java 8
  - LocalDate
  - Period
  - LocalDateTime
  - Duration
  - ChronoUnit
  - ZonedDateTime
  - Temporal
- JodaTime
  - LocalDate
  - Period
- Date4J
  - DateTime

**Java 8 Date Time API**

- based on the popular Java library called JodaTime.

- simplified date and time processing and fixed many shortcomings of the old date library.

- **API Clarity**. New API is clear, concise and easy to understand. It does not have a lot of inconsistencies found in the old library such as the field numbering (in Calendar months are zero-based, but days of week are one-based).

- **API Flexibility**. working with multiple representations of time.

  ```
  Instant – represents a point in time (timestamp)
  LocalDate – represents a date (year, month, day)
  LocalDateTime – same as LocalDate, but includes time with nanosecond precision
  OffsetDateTime – same as LocalDateTime, but with time zone offset
  LocalTime – time with nanosecond precision and without date information
  ZonedDateTime – same as OffsetDateTime, but includes a time zone ID
  OffsetLocalTime – same as LocalTime, but with time zone offset
  MonthDay – month and day, without year or time
  YearMonth – month and year, without day or time
  Duration – amount of time represented in seconds, minutes and hours. Has nanosecond precision
  Period – amount of time represented in days, months and years
  ```

- **Immutability and Thread-Safety**.

- **Method Chaining**. Allowing to implement complex transformations in a single line of code.

References
[Migrating to the New Java 8 Date Time API](https://www.baeldung.com/migrating-to-java-8-date-time-api)

[`back to content`](#content)

