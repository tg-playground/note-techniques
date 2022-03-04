# log4j2.xml Configurations

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Properties>
        <Property name="my_pattern">%style{%d}{red} %style{[%t]}{Green} %highlight{%-5level}{STYLE=Logback} %style{%c{1.}.%M:%L}{cyan}\n - %msg%n</Property>
        <Property name="filename">app.log</Property>
    </Properties>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="${my_pattern}" disableAnsi="false"/>
        </Console>
        <File name="File" fileName="${filename}">
            <PatternLayout pattern="${my_pattern}" disableAnsi="false"/>
        </File>
    </Appenders>
    <Loggers>
        <Logger name="com.taogen.example" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>
```

## Properties

```xml
<Properties>
    <Property name="my_pattern">%style{%d}{red} %style{[%t]}{Green} %highlight{%-5level}{STYLE=Logback} %style{%c{1.}.%M:%L}{cyan}\n - %msg%n</Property>
    <Property name="filename">app.log</Property>
</Properties>
```

## Appenders

Console

```xml
<Appenders>
    <Console name="Console" target="SYSTEM_OUT">
        <PatternLayout pattern="${my_pattern}"/>
    </Console>
</Appenders>
```

File

```xml
<Appenders>
    <File name="File" fileName="${filename}">
        <PatternLayout pattern="${my_pattern}" disableAnsi="false"/>
    </File>
</Appenders>
```

RollingFile

```xml

```



## Loggers

```xml
<Loggers>
    <Logger name="com.taogen.example" level="debug" additivity="false">
        <AppenderRef ref="Console"/>
        <AppenderRef ref="File"/>
    </Logger>
    <Root level="error">
        <AppenderRef ref="Console"/>
    </Root>
</Loggers>
```

