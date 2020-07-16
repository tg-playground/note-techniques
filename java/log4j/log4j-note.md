# Log4j 2 Note

Content

- Architecture
- Log4j 2 API
  - Overview
    - Hello World!
    - Substituting Parameters
    - Formatting Parameters
    - Mixing Loggers with Formatter Loggers
    - Java 8 lambda support for lazy logging
    - Logger Names
  - Log Builder
  - Flow Tracing
  - Markers
  - Event Logging
  - [ ] Messages
  - [ ] Thread Context
- Configuration
  - xxx
- Usage
  - Static vs non-Static
  - Loggers
  - Logger Name vs Class Name
  - Logging in the Cloud
- xxx
- Log4j Configuration File Structure

## Architecture

### Main Components

- LoggerContext
- Configuration
- Filter
- Logger
- LoggerConfig
- Appender
- Layout

LogManager -> LoggerContext

LoggerContext ->Logger

Logger -> LoggerConfig

LoggerConfig->Appender

#### Logger Hierarchy

In Log4j 1.x the Logger Hierarchy was maintained through a relationship between Loggers. In Log4j 2 this relationship no longer exists. Instead, the hierarchy is maintained in the relationship between LoggerConfig objects.

Named Hierarchy. A LoggerConfig is said to be an *ancestor* of another LoggerConfig if its name followed by a dot is a prefix of the *descendant* logger name. A LoggerConfig is said to be a *parent* of a *child* LoggerConfig if there are no ancestors between itself and the descendant LoggerConfig. For example, the LoggerConfig named "com.foo" is a parent of the LoggerConfig named"com.foo.Bar". 

The root LoggerConfig resides at the top of the LoggerConfig hierarchy. It is exceptional in that it always exists and it is part of every hierarchy. A Logger that is directly linked to the root LoggerConfig can be obtained as follows:

```
Logger logger = LogManager.getLogger(LogManager.ROOT_LOGGER_NAME);
```

Alternatively, and more simply:

```
Logger logger = LogManager.getRootLogger();
```

All other Loggers can be retrieved using the [LogManager.getLogger ](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/LogManager.html#getLoggerjava.lang.String)static method by passing the name of the desired Logger.

#### LoggerContext

The [LoggerContext](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/LoggerContext.html) acts as the anchor point for the Logging system. However, it is possible to have multiple active LoggerContexts in an application depending on the circumstances. 

#### Configuration

Every LoggerContext has an active [Configuration](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/config/Configuration.html). The Configuration contains all the Appenders, context-wide Filters, LoggerConfigs and contains the reference to the StrSubstitutor.

#### Logger

As stated previously, Loggers are created by calling [LogManager.getLogger](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/LogManager.html#getLoggerjava.lang.String). The Logger itself performs no direct actions. It simply has a name and is associated with a LoggerConfig. It extends [AbstractLogger ](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/spi/AbstractLogger.html)and implements the required methods.

#### LoggerConfig

[LoggerConfig](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/config/LoggerConfig.html) objects are created when Loggers are declared in the logging configuration. The LoggerConfig contains a set of Filters that must allow the LogEvent to pass before it will be passed to any Appenders. It contains references to the set of Appenders that should be used to process the event.

#### Main Components

##### Log Levels

LoggerConfigs will be assigned a Log [Level](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/Level.html). The set of built-in levels includes TRACE, DEBUG, INFO, WARN, ERROR, and FATAL. Log4j 2 also supports [custom log levels](https://logging.apache.org/log4j/2.x/manual/customloglevels.html). Another mechanism for getting more granularity is to use [Markers](https://logging.apache.org/log4j/2.x/log4j-api/api.html#Markers) instead.

Each Logger references the appropriate LoggerConfig which in turn can reference its parent, thus achieving the same effect.

#### Filter

Log4j provides Filters that can be applied before control is passed to any LoggerConfig, after control is passed to a LoggerConfig but before calling any Appenders, after control is passed to a LoggerConfig but before calling a specific Appender, and on each Appender. 

#### Appender

Log4j allows logging requests to print to multiple destinations. An output destination is called an [Appender](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/Appender.html). 

**Appender Types**: Currently, appenders exist for the console, files, remote socket servers, Apache Flume, JMS, remote UNIX Syslog daemons, and various database APIs.

More than one Appender can be attached to a Logger. 

**Appender Additivity**: Appenders are inherited additively from the LoggerConfig hierarchy. For example, if a console appender is added to the root logger, then all enabled logging requests will at least print on the console. It is possible to override this default behavior so that Appender accumulation is no longer additive by setting additivity="false" on the Logger declaration in the configuration file.

#### Layout

More often than not, users wish to customize not only the output destination but also the output format. This is accomplished by associating a [Layout](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/Layout.html) with an Appender. The Layout is responsible for formatting the LogEvent according to the user's wishes, whereas an appender takes care of sending the formatted output to its destination.

Log4j comes with many different [Layouts](https://logging.apache.org/log4j/2.x/manual/layouts.html) for various use cases such as JSON, XML, HTML, and Syslog

The [PatternLayout](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/layout/PatternLayout.html), part of the standard log4j distribution, lets the user specify the output format according to conversion patterns similar to the C language printf function. For example, the PatternLayout with the conversion pattern "%r [%t] %-5p %c - %m%n" will output something akin to:

```
176 [main] INFO  org.foo.Bar - Located nearest gas station.
```

#### StrSubstitutor and StrLookup

These provide a mechanism to allow the configuration to reference variables coming from System Properties, the configuration file, the ThreadContext Map, StructuredData in the LogEvent. The variables can either be resolved when the configuration is processed or as each event is processed, if the component is capable of handling it. 

## Log4j 2 API

### Overview

#### Hello World!

```java
public static void main(String[] args) {
    logger.info("Hello, World!");
}
```

#### Substituting Parameters

In Log4j 1.x this could be accomplished by doing:

```java
if (logger.isDebugEnabled()) {    logger.debug("Logging in user " + user.getName() + " with birthday " + user.getBirthdayCalendar());}
```

In Log4j 2

```java
logger.debug("Logging in user {} with birthday {}", user.getName(), user.getBirthdayCalendar());
```

#### Formatting Parameters

Formatter Loggers leave formatting up to you if toString() is not what you want. To facilitate formatting, you can use the same format strings as Java's [Formatter](http://docs.oracle.com/javase/6/docs/api/java/util/Formatter.html#syntax). For example:

```java
public static Logger logger = LogManager.getFormatterLogger("Foo"); 

logger.debug("Logging in user %s with birthday %s", user.getName(), user.getBirthdayCalendar());
logger.debug("Logging in user %1$s with birthday %2$tm %2$te,%2$tY", user.getName(), user.getBirthdayCalendar());
logger.debug("Integer.MAX_VALUE = %,d", Integer.MAX_VALUE);
logger.debug("Long.MAX_VALUE = %,d", Long.MAX_VALUE);
```

To use a formatter Logger, you must call one of the LogManager [getFormatterLogger](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/LogManager.html#getFormatterLoggerjava.lang.Class) methods. The output for this example shows that Calendar toString() is verbose compared to custom formatting:

```
2012-12-12 11:56:19,633 [main] DEBUG: User John Smith with birthday java.util.GregorianCalendar[time=?,areFieldsSet=false,areAllFieldsSet=false,lenient=true,zone=sun.util.calendar.ZoneInfo[id="America/New_York",offset=-18000000,dstSavings=3600000,useDaylight=true,transitions=235,lastRule=java.util.SimpleTimeZone[id=America/New_York,offset=-18000000,dstSavings=3600000,useDaylight=true,startYear=0,startMode=3,startMonth=2,startDay=8,startDayOfWeek=1,startTime=7200000,startTimeMode=0,endMode=3,endMonth=10,endDay=1,endDayOfWeek=1,endTime=7200000,endTimeMode=0]],firstDayOfWeek=1,minimalDaysInFirstWeek=1,ERA=?,YEAR=1995,MONTH=4,WEEK_OF_YEAR=?,WEEK_OF_MONTH=?,DAY_OF_MONTH=23,DAY_OF_YEAR=?,DAY_OF_WEEK=?,DAY_OF_WEEK_IN_MONTH=?,AM_PM=0,HOUR=0,HOUR_OF_DAY=0,MINUTE=0,SECOND=0,MILLISECOND=?,ZONE_OFFSET=?,DST_OFFSET=?]
2012-12-12 11:56:19,643 [main] DEBUG: User John Smith with birthday 05 23, 1995
2012-12-12 11:56:19,643 [main] DEBUG: Integer.MAX_VALUE = 2,147,483,647
2012-12-12 11:56:19,643 [main] DEBUG: Long.MAX_VALUE = 9,223,372,036,854,775,807
```

#### Mixing Loggers with Formatter Loggers

Formatter loggers give fine-grained control over the output format, but have the drawback that the correct type must be specified (for example, passing anything other than a decimal integer for a %d format parameter gives an exception).

If your main usage is to use {}-style parameters, but occasionally you need fine-grained control over the output format, you can use the printf method:

```java
public static Logger logger = LogManager.getLogger("Foo");
 
logger.debug("Opening connection to {}...", someDataSource);
logger.printf(Level.INFO, "Logging in user %1$s with birthday %2$tm %2$te,%2$tY", user.getName(), user.getBirthdayCalendar());
```

#### Java 8 lambda support for lazy logging

In release 2.4, the Logger interface added support for lambda expressions. **This allows client code to lazily log messages without explicitly checking if the requested log level is enabled**. For example, previously you would write:

```java
// pre-Java 8 style optimization: explicitly check the log level
// to make sure the expensiveOperation() method is only called if necessary
if (logger.isTraceEnabled()) {
    logger.trace("Some long-running operation returned {}", expensiveOperation());
}
```

With Java 8 you can achieve the same effect with a lambda expression. You no longer need to explicitly check the log level:

```java
// Java-8 style optimization: no need to explicitly check the log level:
// the lambda expression is not evaluated if the TRACE level is not enabled
logger.trace("Some long-running operation returned {}", () -> expensiveOperation());
```

#### Logger Names

Most logging implementations use a hierarchical scheme for matching logger names with logging configuration. In this scheme, the logger name hierarchy is represented by '.' characters in the logger name, in a fashion very similar to the hierarchy used for Java package names. For example, org.apache.logging.appender and org.apache.logging.filter both have org.apache.logging as their parent. In most cases, applications name their loggers by passing the current class's name to LogManager.getLogger(...). Because this usage is so common, **Log4j 2 provides that as the default when the logger name parameter is either omitted or is null.** For example, in all examples below the Logger will have a name of "org.apache.test.MyTest".

Example 1

```java
package org.apache.test;
 
public class MyTest {
    private static final Logger logger = LogManager.getLogger(MyTest.class);
}
```

Example 2

```java
package org.apache.test;
 
public class MyTest {
    private static final Logger logger = LogManager.getLogger(MyTest.class.getName());
}
```

Example 3 (Recommend)

```java
package org.apache.test;
 
public class MyTest {
    private static final Logger logger = LogManager.getLogger();
}
```

### Log Builder

Log4j has traditionally been used with logging statements like

```java
logger.error("Unable to process request due to {}", code, exception);
```

**This has resulted in some confusion as to whether the exception should be a parameter to the message or if Log4j should handle it as a throwable**. In order to make logging clearer a builder pattern has been added to the API. Using the builder syntax the above would be handled as:

```java
logger.atError().withThrowable(exception).log("Unable to process request due to {}", code);
```

With this syntax it is clear that the exception is to be treated as a Throwable by Log4j.

The Logger class now returns a LogBuilder when any of the atTrace, atDebug, atInfo, atWarn, atError, atFatal, always, or atLevel(Level) methods are called. **The logBuilder then allows a Marker, Throwable, and/or location to be added to the event before it is logged.** A call to the log method always causes the log event to be finalized and sent.

A logging statement with a Marker, Throwable, and location would look like:

```java
logger.atInfo().withMarker(marker).withLocation().withThrowable(exception).log("Login for user {} failed", userId);
```

### Flow Tracing

Functions

- aid in problem diagnosis in development without requiring a debug session
- aid in problem diagnosis in production where no debugging is possible
- help educate new developers in learning the application.

The most used methods are the entry() or traceEntry() and exit() or traceExit() methods. 

The main difference between the entry and traceEntry methods is that the entry method accepts a variable list of objects where presumably each is a method parameter. The traceEntry method accepts a format string followed by a variable list of objects, presumably included in the format String.

```java
public void setMessages(String[] messages) {
    logger.traceEntry(new JsonMessage(messages));
    this.messages = messages;
    logger.traceExit();
}

public String[] getMessages() {
    logger.traceEntry();
    return logger.traceExit(messages, new JsonMessage(messages));
}
```

```xml
<Appenders>
    <File name="log" fileName="target/test.log" append="false">
        <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
    </File>
</Appenders>
```

```
19:08:07.056 TRACE com.test.TestService 19 retrieveMessage -  entry
19:08:07.060 TRACE com.test.TestService 46 getKey -  entry
19:08:07.060 TRACE com.test.TestService 48 getKey -  exit with (0)
19:08:07.060 TRACE com.test.TestService 38 getMessage -  entry parms(0)
19:08:07.060 TRACE com.test.TestService 42 getMessage -  exit with (Hello, World)
19:08:07.060 TRACE com.test.TestService 23 retrieveMessage -  exit with (Hello, World)
19:08:07.061 TRACE com.test.TestService 19 retrieveMessage -  entry
19:08:07.061 TRACE com.test.TestService 46 getKey -  entry
19:08:07.061 TRACE com.test.TestService 48 getKey -  exit with (1)
19:08:07.061 TRACE com.test.TestService 38 getMessage -  entry parms(1)
19:08:07.061 TRACE com.test.TestService 42 getMessage -  exit with (Goodbye Cruel World)
19:08:07.061 TRACE com.test.TestService 23 retrieveMessage -  exit with (Goodbye Cruel World)
```

### Markers

One of the primary purpose of a logging framework is to provide the means to generate debugging and diagnostic information only when it is needed, and **to allow filtering of that information** so that it does not overwhelm the system or the individuals who need to make use of it. 

Log messages can filter by markers. 

Usage:

```java
private static final Marker SQL_MARKER = MarkerManager.getMarker("SQL");
private static final Marker UPDATE_MARKER = MarkerManager.getMarker("SQL_UPDATE").setParents(SQL_MARKER);
private static final Marker QUERY_MARKER = MarkerManager.getMarker("SQL_QUERY").setParents(SQL_MARKER);

logger.debug(QUERY_MARKER, "SELECT * FROM {}", table);
```

Code Example:

```java
private Logger logger = LogManager.getLogger(MyApp.class.getName());
private static final Marker SQL_MARKER = MarkerManager.getMarker("SQL");
private static final Marker UPDATE_MARKER = MarkerManager.getMarker("SQL_UPDATE").setParents(SQL_MARKER);
private static final Marker QUERY_MARKER = MarkerManager.getMarker("SQL_QUERY").setParents(SQL_MARKER);
 
public String doQuery(String table) {
    logger.traceEntry();

    logger.debug(QUERY_MARKER, "SELECT * FROM {}", table);

    String result = ... 

        return logger.traceExit(result);
}
 
public String doUpdate(String table, Map<String, String> params) {
    logger.traceEntry();

    if (logger.isDebugEnabled()) {
        logger.debug(UPDATE_MARKER, "UPDATE {} SET {}", table, formatCols());
    }
    
    String result = ... 
    return logger.traceExit(result);
}
```

Filter by marker on macth

```xml
<Appenders>
    <File name="sql_update_file" fileName="/target/sql_update.log" append="true">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5p %c{1}:%L - %m%n"/>
        <Filters>
            <MarkerFilter marker="SQL_UPDATE" onMatch="ACCEPT" onMismatch="DENY"/>
        </Filters>
    </File>
    <File name="sql_query_file" fileName="/target/sql_query.log" append="true">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5p %c{1}:%L - %m%n"/>
        <Filters>
            <MarkerFilter marker="SQL_QUERY" onMatch="ACCEPT" onMismatch="DENY"/>
        </Filters>
    </File>
</Appenders>
```

### Event Logging

The EventLogger class provides a simple mechanism for logging events that occur in an application. While the EventLogger is useful as a way of initiating events that should be processed by an audit Logging system, by itself it does not implement any of the features an audit logging system would require such as guaranteed delivery.

The EventLogger class uses a Logger named "EventLogger". EventLogger uses a logging level of OFF as the default to indicate that it cannot be filtered. These events can be formatted for printing using the [StructuredDataLayout](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/layout/StructuredDataLayout.html).

Usage:

```java
ThreadContext.put("ipAddress", request.getRemoteAddr());
ThreadContext.clear();
```

Example:

```java
public String doFundsTransfer(Account toAccount, Account fromAccount, long amount) {
    toAccount.deposit(amount);
    fromAccount.withdraw(amount);
    String confirm = UUID.randomUUID().toString();
    StructuredDataMessage msg = new StructuredDataMessage(confirm, null, "transfer");
    msg.put("toAccount", toAccount);
    msg.put("fromAccount", fromAccount);
    msg.put("amount", amount);
    EventLogger.logEvent(msg);
    return confirm;
}
```

### Messages

### Thread Context



## Log4j Configuration File Structure

```xml
<Configuration>
    <!-- custom variable -->
    <Properties>
    	<Property name="{your_name}">{your_value}</Property>
    </Properties>
    
    <!-- Appenders defined: 1. where logging messages are saved. 2.message format(PatternLayout). 3. Rollover Strategy. -->
    <Appenders>
        <RollingFile name="">
            <PatternLayout pattern="" />
        	<Policies>
            	<TimeBasedTriggeringPolicy ... />
                <SizeBasedTriggeringPolicy ... />
            </Policies>
        </RollingFile>
    </Appenders>
    
    <!-- Loggers define loggers -->
    <Loggers>
        <Logger>
            <AppenderRef ref="{your_defined_appender_name}" />
        </Logger>
    </Loggers>
</Configuration>
```

