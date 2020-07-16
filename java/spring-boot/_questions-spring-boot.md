# Questions of Spring Boot

### Content

- What is spring boot? Why need to use spring boot? How to use? Its pros and cons?
- What does spring-boot-starter-parent exactly do in pom file
- What is the function of spring-boot-starter-web  dependency
- Spring Boot Work Principles

### Good Questions Post Need Arrange

https://www.java67.com/2018/05/difference-between-springbootapplication-vs-EnableAutoConfiguration-annotations-Spring-Boot.html

### Main

### What is spring boot?  

**What is Spring Boot**

- It is a tool. It providing configurations in pom.xml (such as properties, dependencies, and plugins), some annotation, and some code library. It makes you less concerned about configuration and dependencies. Fastly developing Spring-based application.

**Why need to use it**

**How to use**

**Its pros and cons**

Advantages

Disadvantages

### What does spring-boot-starter-parent exactly do in pom file

Such as

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.1.6.RELEASE</version>
</parent>
```

or

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>2.1.6.RELEASE</version>
	<type>pom</type>
	<scope>import</scope>
</dependency>
```

- It provides place for common configuration for `CHILD POM`s.
- The `spring-boot-starter-parent` as parent or dependency is the parent POM providing dependency and plugin management for Spring Boot-based applications. It contains the default versions of Java to use, the default versions of dependencies that Spring Boot uses, and the default configuration of the Maven plugins. 
- complete configuration of spring-boot-starter-parent  refer to [spring-boot-starter-parent/pom.xml](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-starters/spring-boot-starter-parent/pom.xml)

References

[What does spring-boot-starter-parent exactly do in pom file? - stack overflow](https://stackoverflow.com/questions/43305016/what-does-spring-boot-starter-parent-exactly-do-in-pom-file)

[Spring-boot-starter-parent Example](https://howtodoinjava.com/spring-boot2/spring-boot-starter-parent-dependency/)

Spring boot reference documentation

---

### What is the function of spring-boot-starter-web  dependency

It provides common configuration and dependencies for current project.

complete configuration of spring-boot-starter-web  refer to [spring-boot-starter-web/pom.xml](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-starters/spring-boot-starter-web/pom.xml)





### References

