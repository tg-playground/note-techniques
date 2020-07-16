# Java Version-wise Features History

Reference: <https://howtodoinjava.com/java-version-wise-features-history/>

What are **new java features in java** version 7 or 8? These are pretty much frequently asked questions in java interviews. In this page, I listing down all **JDK changes from JDK 1.x to Java SE 8**, sequentially. Though I have tried to cover as much as information I can gather, though if you know something which I missed below, please let me know and I will add that information.

## Java SE 9 Features

**Possible Release Date** : `September 21, 2017`. Please see the updated release info [here](http://openjdk.java.net/projects/jdk9/).

Proposed features are:

- Support for multi-gigabyte heaps
- Better native code integration
- Self-tuning JVM
- Java Module System
- Money and Currency API
- jshell: The Java Shell
- Automatic parallelization

## Java SE 8 Features

**Release Date** : `March 18, 2014`

Code name culture dropped. Included features were:

- [Lambda expression](https://howtodoinjava.com/java8/complete-lambda-expressions-tutorial-in-java/) support in APIs
- [Functional interface](https://howtodoinjava.com/java8/functional-interface-tutorial/) and [default methods](https://howtodoinjava.com/java8/default-methods-in-java-8/)
- [Optionals](https://howtodoinjava.com/java8/java-8-optionals-complete-reference/)
- Nashorn – JavaScript runtime which allows developers to embed JavaScript code within applications
- Annotation on Java Types
- [Unsigned Integer Arithmetic](https://howtodoinjava.com/java8/java-8-exact-airthmetic-operations-supported-in-math-class/)
- Repeating annotations
- [New Date and Time API](https://howtodoinjava.com/java8/date-and-time-api-changes-in-java-8-lambda/)
- Statically-linked JNI libraries
- Launch JavaFX applications from jar files
- Remove the permanent generation from GC

## Java SE 7 Features

**Release Date** : `July 28, 2011`

This release was called “Dolphin”. Included features were:

- JVM support for dynamic languages
- Compressed 64-bit pointers
- [Strings in switch](https://howtodoinjava.com/java-7/string-class-is-supported-in-switch-statement-in-java-7/)
- [Automatic resource management in try-statement](https://howtodoinjava.com/java-7/automatic-resource-management-with-try-with-resources-in-java-7/)
- [The diamond operator](https://howtodoinjava.com/java-7/improved-type-inference-in-java-7/)
- Simplified varargs method declaration
- Binary integer literals
- [Underscores in numeric literals](https://howtodoinjava.com/java-7/improved-formatted-numbers-in-java-7/)
- [Improved exception handling](https://howtodoinjava.com/java-7/improved-exception-handling-in-java-7/)
- [ForkJoin Framework](https://howtodoinjava.com/java-7/forkjoin-framework-tutorial-forkjoinpool-example/)
- [NIO 2.0](https://howtodoinjava.com/category/java-7-features/nio/) having support for multiple file systems, file metadata and symbolic links
- [WatchService](https://howtodoinjava.com/java-7/auto-reload-of-configuration-when-any-change-happen/)
- Timsort is used to sort collections and arrays of objects instead of merge sort
- APIs for the graphics features
- Support for new network protocols, including SCTP and Sockets Direct Protocol

## Java SE 6 Features

**Release Date** : `December 11, 2006`

This release was called “Mustang”. Sun dropped the “.0” from the version number and version became Java SE 6. Included features were:

- Scripting Language Support
- Performance improvements
- JAX-WS
- JDBC 4.0
- Java Compiler API
- JAXB 2.0 and StAX parser
- Pluggable annotations
- New GC algorithms

## J2SE 5.0 Features

**Release Date** : `September 30, 2004`

This release was called “Tiger”. Most of the features, which are asked in java interviews, were added in this release.

Version was also called 5.0 rather than 1.5. Included features are listed down below:

- [Generics](https://howtodoinjava.com/java/generics/complete-java-generics-tutorial/)
- [Annotations](https://howtodoinjava.com/2014/06/09/complete-java-annotations-tutorial/)
- Autoboxing/unboxing
- [Enumerations](https://howtodoinjava.com/java-5/guide-for-understanding-enum-in-java/)
- Varargs
- [Enhanced `for each` loop](https://howtodoinjava.com/java/basics/enhanced-for-each-loop-in-java/)
- [Static imports](https://howtodoinjava.com/java/basics/static-import-declarations-in-java/)
- New [concurrency utilities](https://howtodoinjava.com/java-5/java-executor-framework-tutorial-and-best-practices/) in `java.util.concurrent`
- `Scanner` class for parsing data from various input streams and buffers.

## J2SE 1.4 Features

**Release Date** : `February 6, 2002`

This release was called “Merlin”. Included features were:

- `assert` keyword
- [Regular expressions](https://howtodoinjava.com/java-regular-expression-tutorials/)
- Exception chaining
- Internet Protocol version 6 (IPv6) support
- [New I/O; NIO](https://howtodoinjava.com/java-nio-tutorials/)
- Logging API
- Image I/O API
- Integrated XML parser and XSLT processor (JAXP)
- Integrated security and cryptography extensions (JCE, JSSE, JAAS)
- Java Web Start
- Preferences API (java.util.prefs)

## J2SE 1.3 Features

**Release Date** : `May 8, 2000`

This release was called “Kestrel”. Included features were:

- HotSpot JVM
- Java Naming and Directory Interface (JNDI)
- Java Platform Debugger Architecture (JPDA)
- JavaSound
- Synthetic proxy classes

## J2SE 1.2 Features

**Release Date** : `December 8, 1998`

This release was called “Playground”. This was a major release in terms of number of classes added (almost trippled the size). “J2SE” term was introduced to distinguish the code platform from J2EE and J2ME. Included features were:

- `strictfp` keyword
- Swing graphical API
- Sun’s JVM was equipped with a JIT compiler for the first time
- Java plug-in
- [Collections framework](https://howtodoinjava.com/java/collections/useful-java-collection-interview-questions/)

## JDK 1 Features

**Release Date** : `January 23, 1996`

This was the [initial release](https://web.archive.org/web/20080205101616/http://www.sun.com/smi/Press/sunflash/1996-01/sunflash.960123.10561.xml) and was originally called **Oak**. This had very unstable APIs and one java web browser named **WebRunner**.

The first stable version, JDK 1.0.2, was called Java 1.

On February 19, 1997, JDK 1.1 was released havind a list of big features such as:

- AWT event model
- Inner classes
- JavaBeans
- JDBC
- RMI
- [Reflection](https://howtodoinjava.com/java/related-concepts/real-usage-examples-of-reflection-in-java/) which supported Introspection only, no modification at runtime was possible.
- JIT (Just In Time) compiler for Windows