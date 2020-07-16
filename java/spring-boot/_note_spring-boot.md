# Spring Boot Note

### Configuration Annotations

`@SpringBootApplication`

One of the things the `@SpringBootApplication` does is a component scan. But, it only scans on **sub-packages**. i.e. if you put that class in *com.mypackage*, then it will scan for all classes in sub-packages i.e. com.mypackage.*.

setting up a `Spring boot` project, have your Application class (the one that contains the `@SpringBootApplication` annotation in the base package.

`@ComponentScan`

you can also add a `@ComponentScan` to a class specifying the package you want scan. i.e `@ComponentScan("com.mypackage")`

### Difference between @SpringBootApplication vs @EnableAutoConfiguration annotation in Spring Boot

Both **@SpringBootApplication** and **@EnableAutoConfiguration** can be used to enable the **auto-configuration feature of Spring Boot** there is a subtle difference between them.

@SpringBootApplication is actually a combination of three annotations: 

- **@Configuration** which is used in Java-based configuration on Spring framework.
- **@ComponentScan** to enable component scanning of components.
- **@EnableAutoConfgiuration** to enable auto-configuration.

Spring Boot designers realize that these three annotations are frequently used together so they bundled them into @SpringBootApplicaiton. Now, instead of three annotations you just need to specify one annotation on your Main class.

### References

[Configuration using annotation @SpringBootApplication - stackoverflow](https://stackoverflow.com/questions/33619532/configuration-using-annotation-springbootapplication)

[Difference between @SpringBootApplication vs @EnableAutoConfiguration annotations in Spring Boot](https://www.java67.com/2018/05/difference-between-springbootapplication-vs-EnableAutoConfiguration-annotations-Spring-Boot.html)