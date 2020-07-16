# Java EE Questions

<h3 id="content">Content</h3>

- Java EE
  - [What is Java EE](#wije)
  - [Are EJB/Spring different implementations of Java EE?](#eas)
- Servlet/JSP
  - Basic
    - [What is Servlet](#wis)
    - [Lifecycle of Servlet](#los)
    - [Flow of Request in Servlet](#for)
    - [Servlet Interfaces And Implementation Class](#sic)
  - Core
    - [What are Servlet Core Concept](#scc)
    - [Servlet Collaboration](#scn)
    - [Attribute in Servlet](#ais)
    - [Session Tracking](#stg)
    - [How to use session when client ban cookie](#htus)
    - [ ] Uploading Files
    - [ ] Asynchronous Processing and Nonblocking I/O
  - JSP
    - JSP Scriptlet, Declarations, Expression
    - JSP Directives
    - JSP Actions
    - [JSP Implicit Object](#jio)
    - 
- JDBC
  - [Transaction Isolation Levels](#til)

- Web Server
    - Apache Tomcat
      - [What is Apache Tomcat](#wit)
      - [Apache HTTP server and Apache Tomcat](#aat)
      - [Web Servers and Servlet Containers](#was)
- HTTP
  - HTTP request
  - HTTP Common request method
  - get vs post
  - Content Type
  - Status Code
- Spring Framework
  - [...What is Spring](#wis)
  - [...What are Configuration files in Spring](#wacf)
  - [...IOC Implementation Principle](#iip)
  - [...AOP Implementation Principle](#aip)
  - [...How @transactional work in spring](#htw)
- Spring Boot
  - [...What is SpringBoot]()
  - [...Spring Boot Features]()
  - [...Spring Boot Implementation Principle.]()
  - [...Spring Boot Configuration]()
- ORM Framework
  - Hibernate
    - [...What is Hibernate]()
    - [...What are Configuration files in Hibernate]()
  - MyBatis
    - [...What is MyBatis]()
    - [...What are Configuration files in MyBatis]()
    - [...What is differences between MyBatis and Hibernate]()
    - [...Why the Dao Interface can mapper its mapper.xml]()
  - JPA
- Restful
- Web Service
- Cache
- Web Security
  - OAuth
  - Shiro
  - Spring Security

---



### Main

### Java EE

<h3 id="wije"># What is Java EE</h3>

What is Java EE

- It translates to Java Enterprise Edition.

- **Java EE is a collection of specifications for developing and deploying enterprise applications.** (In general, enterprise applications refer to software hosted on servers that provide the applications that support the enterprise. The specifications (defined by Sun) describe services, application programming interfaces (APIs), and protocols. The Java EE product provider is typically an application-server, web-server, or database-system vendor who provides classes that implement the interfaces defined in the specifications. These vendors compete on implementations of the Java EE specifications.)

- **Java EE is actually a collection of technologies and APIs** for the Java platform designed to support "Enterprise" Applications which can generally be classed as large-scale, distributed, transactional and highly-available applications designed to support mission-critical business requirements. Transactions and distribution are key.

- Java EE is indeed an **abstract** specification. Anybody is open to develop and provide a working implementation of the specification. The *concrete* implementations are the so-called application servers, like [WildFly](http://wildfly.org/), [TomEE](http://tomee.apache.org/), [GlassFish](http://glassfish.org/), [Liberty](http://www-03.ibm.com/software/products/en/appserv-was-liberty-core), [WebLogic](http://www.oracle.com/technetwork/middleware/weblogic/overview/index.html), etc. There are also servlet containers which implement only the JSP/Servlet part of the huge Java EE API, such as [Tomcat](http://tomcat.apache.org/), [Jetty](http://www.eclipse.org/jetty/), etc.

  We, Java EE developers, should write code utilizing the specification (i.e. import *only* `javax.*`classes in our code instead of implementation specific classes such as `org.jboss.wildfly.*`, `com.sun.glassfish.*`, etc) and then we'll be able to run our code on any implementation (thus, on any application server). If you're familiar with JDBC, it's basically the same concept as how JDBC drivers work. See also a.o. [In simplest terms, what is a factory?](https://stackoverflow.com/questions/7550612/in-simplest-terms-what-is-a-factory)

The 13 core technologies that make up Java EE are:

1. JDBC
2. JNDI
3. EJBs
4. RMI
5. JSP
6. Java Servlets
7. XML
8. JMS
9. Java IDL
10. JTS
11. JTA
12. JavaMail
13. JAF

References

- [What is Java EE?](https://stackoverflow.com/questions/106820/what-is-java-ee)

- [What exactly is Java EE?](https://stackoverflow.com/questions/7295096/what-exactly-is-java-ee)

[back to  content](#content)

---



<h3 id="eas"># Are EJB/Spring different implementations of Java EE?</h3>

Are EJB/Spring different implementations of Java EE?

- No. EJB is part of Java EE. Spring is a standalone framework which substitutes and improves many parts of Java EE. Spring doesn't necessarily require Java EE to run. A barebones servletcontainer like Tomcat is already sufficient. Simply put, Spring is a competitor of Java EE. E.g. "Spring" (standalone) competes EJB/JTA, Spring MVC competes JSF/JAX-RS, Spring DI/IoC/AOP competes CDI, Spring Security competes JAAS/JASPIC, etc.
- Back during the old J2EE/EJB2 times, the EJB2 API was terrible to implement and maintain. Spring was then a much better alternative to EJB2. But since EJB3 (Java EE 5), the EJB API was much improved based on lessons learnt from Spring. Since CDI (Java EE 6), there's not really a reason to look at again *another* framework like Spring to make the developers more easy as to developing among others the service layer.
- Only when you're using a barebones servletcontainer such as Tomcat and can't move on to a Java EE server, then Spring is more attractive as it's easier to install Spring on Tomcat. It isn't possible to install e.g. an EJB container om Tomcat without modifying the server itself, you would basically be reinventing TomEE.

References

- [What exactly is Java EE?](https://stackoverflow.com/questions/7295096/what-exactly-is-java-ee)

[back to content](#content)

---



### Servlet/JSP

### Basic

<h3 id="wis"># What is Servlet</h3>

What is Servlet

- Servlet is a technology i.e. used to create web application.
- Servlet is a web component that is deployed on the server to create dynamic web page. 
- Servlet is an API that provides many interfaces and classes including documentations.
- Servlet is an interface that must be implemented for creating any servlet.
- Servlet is a class that extend the capabilities of the servers and respond to the incoming request. It can respond to any type of requests.
- A servlet is simply a class which responds to a particular type of network request - most commonly an HTTP request. Basically servlets are usually used to implement web applications - but there are also various frameworks which operate on top of servlets (e.g. Struts) to give a higher-level abstraction than the "here's an HTTP request, write to this HTTP response" level which servlets provide.

References

- [What is Java Servlet?](https://stackoverflow.com/questions/7213541/what-is-java-servlet)

[back to content](#content)

---



<h3 id="los"># Lifecycle of Servlet</h3>

**Lifecycle of Servlet**

- Servlet class loaded.
- Servlet instance created.
- init() method is invoked. Initializes the servlet.
- service() method invoked.
- destroy() method invoked.

**Details of Lifecycle of Servlet**

Servlet class loaded

- Servlet class loaded is on application startup or at the first time when the servlet is invoked.
- The classloader is responsible to load the servlet class. The servlet class is loaded when the first request for the servlet is received by the web container.
- using <load-on-startup>, you can be sure that your servlets will be loaded when the app is starting up and be available.

Servlet instance created

- The web container creates the instance of a servlet after loading the servlet class. The servlet instance is created only once in the servlet life cycle.

init method is invoked

- The web container calls the init method only once after creating the servlet instance. The init method is used to initialize the servlet. 

service method is invoked

- The web container calls the service method each time when request for the servlet is received. If servlet is not initialized, it follows the first three steps as described above then calls the service method. If servlet is initialized, it calls the service method. Notice that servlet is initialized only once.

destory method is invoked

- All the Servlet instances are destroyed when server is shutting down or on application disposal call it destroy() method.
- The web container calls the destroy method before removing the servlet instance from the service. It gives the servlet an opportunity to clean up any resource for example memory, thread etc.
- You can't destroy the Servlet manually and Servlet is just like worker not for data container. 

**Advantage of load-on-startup element**

- As you know well, servlet is loaded at first request. That means it consumes more time at first request. If you specify the load-on-startup in web.xml, servlet will be loaded at project deployment time or server start. So, it will take less time for responding to first request.
- If you pass the positive value, the lower integer value servlet will be loaded before the higher integer value servlet. In other words, container loads the servlets in ascending integer value. The 0 value will be loaded first then 1, 2, 3 and so on.

```java
  <servlet>  
     <servlet-name>servlet1</servlet-name>  
     <servlet-class>com.javatpoint.FirstServlet</servlet-class>  
     <load-on-startup>0</load-on-startup>  
  </servlet> 
```



References

- [Life Cycle of a Servlet](https://www.javatpoint.com/life-cycle-of-a-servlet)

- [When exactly a servlet is destroyed?](https://stackoverflow.com/questions/24832470/when-exactly-a-servlet-is-destroyed)

[back to content](#content)

---



<h3 id="for"># Flow of Request in Servlet</h3>

**Flow of request**

- Client sends HTTP request to Web server
- Web server forwards that HTTP request to web container.
- Since Servlet can not understand HTTP, its a Java program, it only understands objects, so web container converts that request into valid request object
- Web container spins a thread for each request
- All the business logic goes inside doGet() or doPost() callback methods inside the servlets
- Servlet builds a Java response object and sends it to the container. It converts that to HTTP response again to send it to the client

**How does the Container know which Servlet client has requested for?**

- There’s a file called web.xml
- This is the master file for a web container
- You have information about servlet in this file-
  - servlets
    - Servlet-name
    - Servlet-class
  - servlet-mappings \- the path like /Login or /Notifications is mapped here in
    - Servlet-name
    - url-pattern
  - and so on
- Every servlet in the web app should have an entry into this file
- So this lookup happens like- url-pattern -> servlet-name -> servlet-class

**Server-side Java stack**

- Hardware
  - Firmware
    - Operating System
      - Java Virtual Machine
        - Java Server
          - Your Java Application
          - Servlet API

References

- [What is a Java servlet? Request handling for Java web applications - JavaWorld](https://www.javaworld.com/article/3313114/what-is-a-java-servlet-request-handling-for-java-web-applications.html)



[back to content](#content)

---



<h3 id="sic"""># Servlet Interfaces And Abstract Classes</h3>

Servlet Interfaces

```java
I-Servlet
	*-GenericSevlet implements Servlet, ServletConfig
		*-HttpServlet extends HttpServlet
I-ServletRequest
	I-HttpServletRequest extends ServletRequest
		HttpServletRequestWrapper implements HttpServletRequest
I-ServletResponse
	I-HttpServletResponse 
I-RequestDispatcher
I-ServletConfig
I-HttpSession
I-ServletContext
I-java.util.EventListener
	I-javax.servlet.ServletContextListener
	I-HttpSessionListener
	I-ServletRequestListener
I-Filter
I-FilterChain
I-FilterConfig
```

**Servlet Interface**

- Servlet interface needs to be implemented for creating any servlet (either directly or indirectly). It provides 3 life cycle methods that are used to initialize the servlet, to service the requests, and to destroy the servlet and 2 non-life cycle methods.

- ```java
  void init(ServletConfig config);
  void service(ServletRequest request, ServletResponse response);
  void destory();
  ServletConfig getServletConfig();
  String getServletInfo();
  ```

**GenericServlet Abstract class**

- GenericServlet class implements Servlet, ServletConfig and Serializable interfaces. It provides the implementation of all the methods of these interfaces except the service method. GenericServlet provides simple versions of the life-cycle methods init and destroy and of the methods in the ServletConfig interface.
- GenericServlet class can handle any type of request so it is protocol-independent.
- You may create a generic servlet by inheriting the GenericServlet class and providing the implementation of the service method. To write a generic servlet, it is sufficient to override the abstract service() method.

**HttpServlet Abstract class**

- The HttpServlet class extends the GenericServlet class and implements Serializable interface. It provides http specific methods such as doGet, doPost, doHead, doTrace etc.\

**GenericServet VS HttpServlet**

- the Servlet class doesn't know about any protocols. It is the HttpServlet that understands the HTTP protocol. A SMTPServlet would override the service() method of Servlet to handle, for example, the MAIL, RCPT, and DATA SMTP "verbs" - maybe with a doMail(), doRecipient(), and doData(). There would likely be other methods to handle the protocol. But the interaction would be protocol specific - thus the generic base class and protocol specific child class.

**ServletRequest Interface**

- An object of ServletRequest is used to provide the client request information to a servlet such as content type, content length, parameter names and values, header informations, attributes etc.

**ServletResponse Interface**

**ServletConfig Interface**

- An object of ServletConfig is created by the web container for each servlet. This object can be used to get configuration information from web.xml file.

- If the configuration information is modified from the web.xml file, we don't need to change the servlet. So it is easier to manage the web application if any specific content is modified from time to time.

- The core advantage of ServletConfig is that you don't need to edit the servlet file if information is modified from the web.xml file.

- ```
    <servlet> 
    	......  
      <init-param>  
        <param-name>parametername</param-name>  
        <param-value>parametervalue</param-value>  
      </init-param>  
      ......  
    </servlet>  
  ```

**HttpSession Interface**

- Session simply means a particular interval of time.
- Session Tracking is a way to maintain state (data) of an user. It is also known as session management in servlet.
- Http protocol is a stateless so we need to maintain state using session tracking techniques. Each time user requests to the server, server treats the request as the new request. So we need to maintain the state of an user to recognize to particular user.

**ServletContext Interface**

- An object of ServletContext is created by the web container at time of deploying the project. This object can be used to get configuration information from web.xml file. There is only one ServletContext object per web application.

- If any information is shared to many servlet, it is better to provide it from the web.xml file using the **<context-param>** element.

- Easy to maintain if any information is shared to all the servlet, it is better to make it available for all the servlet. We provide this information from the web.xml file, so if the information is changed, we don't need to modify the servlet. Thus it removes maintenance problem.

- ```java
  <web-app>  
   ......  
    <context-param>  
      <param-name>parametername</param-name>  
      <param-value>parametervalue</param-value>  
    </context-param>  
   ......  
  </web-app>  
  ```

- ```java
  ServletContext application=getServletConfig().getServletContext();  
  ```

**ServletContextListener Interface**

- Events are basically occurrence of something. Changing the state of an object is known as an event.

- We can perform some important tasks at the occurrence of these exceptions, such as counting total and current logged-in users, creating tables of the database at time of deploying the project, creating database connection object etc.

- There are many Event classes and Listener interfaces in the javax.servlet and javax.servlet.http packages.

- ```java
  <web-app>
  	<listener>
  		<listener-class>com.wiiun.petkit.web.listener.MyServletContextListener</listener-class>
  	</listener>
  </web-app>  
  ```

**Filter Interface**

- A filter is an object that is invoked at the preprocessing and postprocessing of a request.

- It is mainly used to perform filtering tasks such as conversion, logging, compression, encryption and decryption, input validation etc.

- The servlet filter is pluggable, i.e. its entry is defined in the web.xml file, if we remove the entry of filter from the web.xml file, filter will be removed automatically and we don't need to change the servlet.

- ```java
  <web-app>  
    
  <filter>  
  <filter-name>...</filter-name>  
  <filter-class>...</filter-class>  
  </filter>  
     
  <filter-mapping>  
  <filter-name>...</filter-name>  
  <url-pattern>...</url-pattern>  
  </filter-mapping>  
    
  </web-app>  
  ```

****

References

- [Servlet Interface - javaTpoint](https://www.javatpoint.com/Servlet-interface)

[back to content](#content)

---

### Servlet Core

<h3 id="scc"># What are Servlet Core Concept</h3>

- Attribute in Servlet
- Session Tracking
- Event and Listener
- Servlet Filter

[back to content](#content)

---



<h3 id="scn"># Servlet Collaboration</h3>

**RequestDispatcher Interface**

- `void forward(ServletRequest request,ServletResponse response)` : Forwards a request from a servlet to another resource (servlet, JSP file, or HTML file) on the server.
-  `void include(ServletRequest request,ServletResponse response)`: Includes the content of a resource (servlet, JSP page, or HTML file) in the response.

**How to get the object of RequestDispatcher**

```java
RequestDispatcher getRequestDispatcher(String resource);  
```
Example code: 

```java
// forward()
RequestDispatcher rd=request.getRequestDispatcher("servlet2");
rd.forward(request, response);

// include()
RequestDispatcher rd=request.getRequestDispatcher("/index.html");  
rd.include(request, response);  
```

Example of real

```java
login.html -> loginServlet --Y--> WelcomeServlet -> home.html
                           --N-->include login.html
```

**HttServletResponse Interface**

- `void sendRedirect(String location)`: to redirect response to another resource, it may be servlet, jsp or html file.

Example code:

```java
response.sendRedirect("http://www.google.com");
```

**Difference between forward() and sendRedirect() method**

- The forward() method works at server side. The sendRedirect() method works at client side.
- The forward() sends the same request and response objects to another servlet. The sendRedirect() always sends a new request.
- The forward() can work within the server only. The sendRedirect()can be used within and outside the server.

References

- [RequestDispatcher in Servlet - javaTpoint](https://www.javatpoint.com/requestdispatcher-in-servlet)

[back to content](#content)

---

<h3 id="ais"># Attribute in Servlet</h3>

Scope Objects

- An **attribute in servlet** is an object that can be set, get or removed from one of the following scopes: request scope, session scope, application scope.

- The servlet programmer can pass informations from one servlet to another using attributes. It is just like passing object from one class to another so that we can reuse the same object again and again.

- There are following 4 attribute specific methods

- ```java
  void setAttribute(String name,Object object);
  Object getAttribute(String name);
  Enumeration getInitParameterNames();
  void removeAttribute(String name);
  ```

Socpe                  |  Class
---                         | -------
Application scope      | javax.servlet.ServletContext
Session scope               | javax.servlet.http.HttpSession
Request scope              | subtype of javax.servlet.servletRequest
Page scope                    | javax.servlet.jsp.JspContext

[back to content](#content)

---



<h3 id="stg"># Session Tracking</h3>

**Session Tracking**

- **Session Tracking** is a way to maintain state (data) of an user. It is also known as **session management**in servlet.
- Http protocol is a stateless so we need to maintain state using session tracking techniques. Each time user requests to the server, server treats the request as the new request. So we need to maintain the state of an user to recognize to particular user.

**Session tracking Techniques**

- Cookies
- Hidden Form field
- URL rewriting
- HttpSession

**What is Cookie**

- A **cookie** is a small piece of information that is persisted between the multiple client requests.
- A cookie has a name, a single value, and optional attributes such as a comment, path and domain qualifiers, a maximum age, and a version number.

How Cookie Work

- By default, each request is considered as a new request. In cookies technique, we add cookie with response from the servlet. So cookie is stored in the cache of the browser. After that if request is sent by the user, cookie is added with request by default. Thus, we recognize the user as the old user.

Cookie Advantage

- Simplest technique of maintaining the state.
- Cookies are maintained at client side.

Cookie Disadvantage

- It will not work if cookie is disabled from the browser.

- Only textual information can be set in Cookie object.

```java
Cookie
    Cookie(String name, String value);
    void setMaxAge(int expiry);
    String getName();
    String getValue();
    void setName(String name);
    void setValue(String value);
HttpServletRequest
    Cookie[] getCookies();
HttpServletResponse
    void addCookie(Cookie ck);
```

Code example

```java
// set cookie
Cookie ck = new Cookie("user","sonoo jaiswal");//creating cookie object  
response.addCookie(ck);//adding cookie in the response  

// get cookie
Cookie ck[] = request.getCookies();  
for(int i=0;i<ck.length;i++){  
 out.print("<br>"+ck[i].getName()+" "+ck[i].getValue()); 
}  
```

**URL Rewriting**

- In URL rewriting, we append a token or identifier to the URL of the next Servlet or the next resource. We can send parameter name/value pairs using the following format

Advantage 

- It will always work whether cookie is disabled or not (browser independent).
- Extra form submission is not required on each pages.

Disadvantage

- It will work only with links.
- It can send Only textual information.

**HttpSession Interface**

```java
HttpSession session = request.getSession();
session.setAttribute("attributeKey", "Sample Value");
```



[back to content](#content)

---



<h3 id="htus"># How to use session when client disable cookie</h3>

~~get session by id~~

```java
//Session session = request.getSession().getSessionConext().getSession(sessionid);
```

Right implementation

```java
MySessionLinsterner implements HttpSessionListener
{
    HashMap sessionContainer = new HashMap<>();
    sessionContainer.put(sessionid, session);
}
```

**URL Rewriting**

When cookies are enabled, the session is stored in a cookie under the name `JSESSIONID`.

If cookies are disabled, the container should rewrite the session id as a GET parameter (i.e. `&JSESSIONID=1223456fds` at the end of all URLs).

If the URL rewriting isn't on by default, see your container's documentation on how to enable it.

You might want to consider modern frameworks (for example Spring MVC with Thymeleaf) which will automate this for you. Otherwise you need to make sure you're rewriting URLs with `response.encodeURL()` as Ouney directs in his answer.



[back to content](#content)

---

### JSP

<h3 id="jio"># JSP Implicit Object</h3>

**What is JSP Implicit**

- These Objects are the Java objects that the JSP Container makes available to the developers in each page and the developer can call them directly without being explicitly declared.
- JSP Implicit Objects are also called **pre-defined variables**.

**JSP Implicit**

- **request**, This is the **HttpServletRequest** object associated with the request.
- **response**, This is the **HttpServletResponse** object associated with the response to the client.
- **out**, This is the **PrintWriter** object used to send output to the client.
- **session**, This is the **HttpSession** object associated with the request.
- **application**, This is the **ServletContext** object associated with the application context.
- **config**, This is the **ServletConfig** object associated with the page.
- **pageContext**, This encapsulates use of server-specific features like higher performance **JspWriters**.
- **page**, This is simply a synonym for **this**, and is used to call the methods defined by the translated servlet class.
- **Exception**, The **Exception** object allows the exception data to be accessed by designated JSP.



[back to content](#content)

---




### Tomcat

<h3 id="wit"># What is Apache Tomcat</h3>



[`back to content`](#content)

---

<h3 id="aat"># Apache HTTP Server and Apache Tomcat</h3>

Difference between the Apache HTTP Server and Apache Tomcat

[back to content](#content)

---

<h3 id="was"># Web Servers and Servlet Containers</h3>

What are differences between web servers and servlet containers

- Web Server(HTTP Server): A server which is capable of handling HTTP requests, sent by a client and respond back with a HTTP response.
- Web Container or Servlet Container or Servlet Engine : is used to manage the components like Servlets, JSP. It is a part of the web server.

[back to content](#content)

---

### JDBC

<h3 id="til"># Transaction Isolation Levels</h3>



| Level                | **dirty reads** | **non-repeatable reads** | **phantom reads** |
| -------------------- | --------------- | ------------------------ | ----------------- |
| **READ_UNCOMMITTED** | yes             | yes                      | yes               |
| **READ_COMMITTED**   | no              | yes                      | yes               |
| **REPEATABLE_READ**  | no              | no                       | yes               |
| **SERIALIZABLE**     | no              | no                       | no                |

READ_UNCOMMITTED: 读未提交，即能够读取到没有被提交的数据.
READ_COMMITTED: 读已提交，即能够读到那些已经提交的数据.
REPEATABLE_READ: 数据读出来之后加锁,防止别人修改它。
SERLALIZABLE: 串行化，最高的事务隔离级别，不管多少事务，挨个运行完一个事务的所有子事务之后才可以执行另外一个事务里面的所有子事务.

[`back to content`](#content)

---

### HTTP



### Spring Framework

TODO

- [Spring interview questions and answers](https://howtodoinjava.com/interview-questions/top-spring-interview-questions-with-answers/#ioc_in_spring)

<h3 id=""> #What is Spring</h3>



[`back to content`](#content)

---

<h3 id=""> #What are Configuration files in Spring</h3>

[`back to content`](#content)

---

<h3 id=""> #IOC Implementation Principle</h3>

**Spring IoC container**

- It is a process whereby objects define their dependencies, that is, the other objects they work with, only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method.
- The container then *injects* those dependencies when it creates the bean.
- The `org.springframework.beans` and `org.springframework.context` packages are the basis for Spring Framework's IoC container. 
- The `BeanFactory` interface provides an advanced configuration mechanism capable of managing any type of object. `ApplicationContext` is a sub-interface of `BeanFactory.` It adds easier integration with Spring's AOP features; message resource handling (for use in internationalization), event publication; and application-layer specific contexts such as the `WebApplicationContext` for use in web applications.
- In short, the `BeanFactory` provides the configuration framework and basic functionality, and the `ApplicationContext` adds more enterprise-specific functionality. The`ApplicationContext` is a complete superset of the `BeanFactory`



In short, "The Container" is a Spring instance in charge of managing the lifecycle of your beans.

To create one, basically, you should do something like

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("/application-context.xml");
```

**What is IOC**

- Spring helps in the creation of loosely coupled applications because of **Dependency Injection**.
- In Spring, objects define their associations (dependencies) and do not worry about how they will get those **dependencies**. It is the responsibility of Spring to provide the required dependencies for creating objects.

For example: Suppose we have an object `Employee` and it has a dependency on object `Address`. We would define a bean corresponding to `Employee` that will define its dependency on object `Address`.

When Spring tries to create an `Employee` object, it will see that `Employee` has a dependency on `Address`, so it will first create the `Address` object (dependent object) and then inject it into the `Employee` object.

- Inversion of Control (**IOC**) and Dependency Injection (**DI**) are used interchangeably. IOC is achieved through DI. DI is the process of providing the dependencies and IOC is the end result of DI. (**Note:** DI is not the only way to achieve IOC. There are [other ways](https://en.wikipedia.org/wiki/Inversion_of_control#Implementation_techniques) as well.)
- By DI, the responsibility of creating objects is shifted from our application code to the Spring container; this phenomenon is called IOC.
- Dependency Injection can be done by setter injection or constructor injection.

**Create IOC Container**

```java
ApplicationContext context = new FileSystemXmlApplicationContext("beans.xml");
HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
```



**Spring Bean Life Cycle**



**BeanFactory Interface**

```java
I-BeanFactory
    Object getBean(String name);
    boolean containsBean(String name);
    boolean isSingleton(String name);
    boolean isPrototype(String name);
    Class<?> getType(String name); // the type of the bean
    String[] getAliases(String name); //All of those aliases point to the same bean
I-ListableBeanFactory extends BeanFactory
	boolean containsBeanDefinition(String beanName);
	String[] getBeanNamesForType(ResolvableType type);
	String[] getBeanNamesForAnnotation(Class<? extends Annotation> annotationType);
I-AutowireCapableBeanFactory extends BeanFactory
	// Typical methods for creating and populating external bean instances
	<T> T createBean(Class<T> beanClass); //Fully create a new bean instance of the given class.
	void autowireBean(Object existingBean);
	Object Object configureBean(Object existingBean, String beanName);

	// Specialized methods for fine-grained control over the bean lifecycle
    Object createBean(Class<?> beanClass, int autowireMode, boolean dependencyCheck);
    Object autowire(Class<?> beanClass, int autowireMode, boolean dependencyCheck);
    void autowireBeanProperties(Object existingBean, int autowireMode, boolean dependencyCheck);
    void applyBeanPropertyValues(Object existingBean, String beanName);
    Object initializeBean(Object existingBean, String beanName);
    Object applyBeanPostProcessorsBeforeInitialization(Object existingBean, String beanName);
    void destroyBean(Object existingBean);

    // Delegate methods for resolving injection points
    Object resolveDependency(DependencyDescriptor descriptor, String requestingBeanName);
I-ApplicationContext
	String getId(); // Return the unique id of this application context.
	String getApplicationName();
	long getStartupDate();
	ApplicationContext getParent();
	AutowireCapableBeanFactory getAutowireCapableBeanFactory();
```

**Spring IOC Start Analysis**

```java
class ClassPathXmlApplicationContext 
	public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, ApplicationContext parent){
 
        super(parent);
            // 解析配置文件列表，放置到上面说的那个 configResources 数组中
        setConfigLocations(configLocations);
        if (refresh) {
          refresh(); // 核心方法
        }
      }
}
class AbstractApplicationContext{
    public void refresh() throws BeansException, IllegalStateException {
    
    
    }
    
}

```

- **DefaultListableBeanFactory** : 我们可以看到 ConfigurableListableBeanFactory 只有一个实现类 DefaultListableBeanFactory，而且实现类 DefaultListableBeanFactory 还通过实现右边的 AbstractAutowireCapableBeanFactory 通吃了右路。所以结论就是，最底下这个家伙 DefaultListableBeanFactory 基本上是最牛的 BeanFactory 了，这也是为什么这边会使用这个类来实例化的原因。
- **BeanDefinition** interface:  A BeanDefinition describes a bean instance, which has property values, constructor argument values, and further information supplied by concrete implementations.

过程

- 创建 Bean 容器前的准备工作
- 创建 Bean 容器（BeanFactory）实例，加载并注册 Bean
    - 加载 Bean
      -  loadBeanDefinitions(beanFactory) 这个方法将根据配置，加载各个 Bean，然后放到 BeanFactory 中。
      - 读取配置的操作在 XmlBeanDefinitionReader 中，其负责加载配置、解析。
      - 经过漫长的链路，一个配置文件终于转换为一颗 DOM 树了。
      - 由Document得到BeanDefinition
    - 注册 Bean
    	- 注册了各个 BeanDefinition 到注册中心，并且发送了注册事件。
- Bean 容器实例化完成后
- 准备 Bean 容器: prepareBeanFactory(factory)
- 初始化所有的 singleton beans
- 创建 Bean
- 创建 Bean 实例
- bean 属性注入

**Spring Default Scope**

- Spring's default scope *is* singleton. It's just that your idea of what it means to be a singleton doesn't match how Spring defines singletons.

- If you tell Spring to make two separate beans with different ids and the same class, then you get two separate beans, each with singleton scope. All singleton scope means is that when you reference something with the same id, you get the same bean instance back.

References 

- [The IoC container - spring.io](https://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/beans.html)
- [What is Dependency Injection and Inversion of Control in Spring Framework? - Stack Overflow](https://stackoverflow.com/questions/9403155/what-is-dependency-injection-and-inversion-of-control-in-spring-framework)
- [Spring IOC 容器源码分析 - ImportNew](http://www.importnew.com/27469.html)

[`back to content`](#content)

---

<h3 id=""> #AOP Implementation Principle</h3>

```java
    <aop:config proxy-target-class="true">
        <aop:aspect id="time" ref="timeHandler">
            <aop:pointcut id="addAllMethod" expression="execution(* org.xrq.action.aop.Dao.*(..))" />
            <aop:before method="printTime" pointcut-ref="addAllMethod" />
            <aop:after method="printTime" pointcut-ref="addAllMethod" />
        </aop:aspect>
    </aop:config>
```

- AOP 实现源头，加载Bean定义的时候应该有过特殊的处理。
- AOP Bean 加载
    - AOP Bean定义加载——根据织入方式将<aop:before>、<aop:after>转换成名为adviceDef的RootBeanDefinition
    - AOP Bean定义加载——将名为adviceDef的RootBeanDefinition转换成名为advisorDefinition的RootBeanDefinition
    - AOP Bean定义加载——AopNamespaceHandler处理<aop:pointcut>流程
- 在每个Bean初始化之后，如果需要，调用AspectJAwareAdvisorAutoProxyCreator中的postProcessBeforeInitialization 为 Bean生成代理。
- 创建代理对象实例
    - 代理对象实例化—-判断是否为<bean>生成代理
    - 代理对象实例化—-为<bean>生成代理代码上下文梳理
    - 代理对象实例化—-创建AopProxy接口实现类
    - 代理对象实例化—-通过getProxy方法获取<bean>对应的代理
- 调用代理方法原理

References


- [Spring源码分析：AOP源码解析 - importNew](http://www.importnew.com/24430.html)
- [java之架构基础-动态代理&cglib - importNew](http://www.importnew.com/19749.html)

[`back to content`](#content)

---

<h3 id=""> #How @transactional work in spring</h3>

[`back to content`](#content)

---






### Spring Boot

### # What is Spring Boot

简化基于spring项目的配置。

### # Spring Boot features or goals

Advantages

- 简化配置，快速开始开发。没有繁琐的配置，专注于业务开发。
- 内置tomcat和jetty容器，可以直接启动。
- 没有代码生成和xml配置。
- Simplified & version conflict free dependency management through the starter POMs.
- We can quickly setup and run standalone, web applications and micro services at very less time.
- You can just assemble the jar artifact which comes with an embedded Tomact, Jetty or Undertow application server and you are ready to go.
- Spring Boot provides HTTP endpoints to access application internals like detailed metrics, application inner working, health status, etc.
- No XML based configurations at all. Very much simplified properties. The beans are initialized, configured and wired automatically.
- The Spring Initializer provides a project generator to make you productive with the certain technology stack from the beginning. You can create a skeleton project with web, data access (relational and NoSQL datastores), cloud, or messaging support.

Disadvantage

- Spring boot may unnecessarily increase the deployment binary size with unused dependencies.

- If you are a control freak, I doubt Spring Boot would fit your needs.

- Spring Boot sticks good with micro services. The Spring Boot artifacts can be deployed directly into Docker containers. In a large and monolithic based applications, I would not encourage you to use Spring Boot.

Shortcomings

### # Spring Boot 实现原理

在pom.xml文件中把spring boot 作为parent父项目。

```java
	<!-- Inherit defaults from Spring Boot -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath/>
	</parent>
	
	<!-- Add typical dependencies for a web application -->
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>
	
    <!-- Package as an executable jar -->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

### # Spring Boot Configuration 

.properties or .yml

1).properties

```
app.user.name = javastack
```

2).yml

```
app:
    user:
        name: javastack
```

主要配置

application.yml:

```java
server:
  session-timeout: 1800
  tomcat:
      uri-encoding: UTF-8

security:
  basic:
    enabled: false
sms:
  isOpen: false
spring:
  thymeleaf:
    mode: LEGACYHTML5
    cache: false
    encoding: UTF-8
    content-type: text/html; charset=utf-8
  jackson:
    time-zone: GMT+8
    date-format: yyyy-MM-dd HH:mm:ss
  profiles: 
    active: dev
  http:
    multipart:
      max-file-size: 30Mb
      max-request-size: 30Mb
    encoding:
      force: true
      charset: utf-8
      enabled: true  
mybatis: 
  configuration:
    map-underscore-to-camel-case: true
  mapper-locations: mybatis/**/*Mapper.xml
  typeAliasesPackage: com.wiiun.training.**.domain


```

application-pro.yml

```java
training:
  uploadPath : C:/Tools/apache-tomcat-8.0.23/files/admin/upload/
  isForTest: true
  prePicture: /files/picture/
  host: http://www.ruixinyingyu.com/admin
  apiHost: http://www.ruixinyingyu.com/api
  sms:
    uri: http://login.qianjingtong.cn/msg/HttpBatchSendSM
logging:
  level:
    com.wiiun.training: debug
spring:
  http:
    multipart:
      max-file-size: 100Mb
      max-request-size: 100Mb
  thymeleaf:
    cache: false
    encoding: UTF-8
    content-type: text/html; charset=utf-8
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/training?allowMultiQueries=true&useUnicode=true&characterEncoding=utf8
    username: training
    password: 2AvDywXxeyTB8Zab
    initialSize: 1
server:
  port: 80

    # 合并多个DruidDataSource的监控数据
    #useGlobalDataSourceStat: true

```



### Hibernate



### MyBatis

<h3 id=""># mybatis mapper interface principle</h3>
mybatis mapper interface principle

赋值给 mapper 接口的实例是引用的对象是一个代理对象，这个代理对象是由 JDK 动态代理创建的。

在解析 mybatis-config.xml 配置文件的时候：

```java
<mappers>
    <mapper class="shfq.annotation.AddressMapper"/>
</mappers>
```

会循环处理 < mappers/> 内的所有< mapper/>，在上面的例子中只有一个 mapper 并且是个接口，在解析 mapper 的时候，mybatis 会通过 Java 反射，获取到接口的所有方法。然后循环处理每一个方法。接口中的方法包含的信息主要有：参数、返回类型、方法注解、方法名称。

- 返回类型的作用主要是根据返回类型创建对象，然后把结果集中的数据通过调用 Java 反射把相关的数据设置到对象中。
- 参数的主要作用是为 sql 语句传递参数。
- 方法注解里包含了 sql 语句信息，方法注解里还可以包含很多其他的信息。
- 方法名称的作用主要是为了标志唯一性，接口名称和方法名称拼接在一起就可以唯一地区分一个方法（Java 里允许方法重载，但 mybatis 不支持方法名称相同但参数不同的情况）


References

- [mybatis mapper 接口原理 - CSDN](https://blog.csdn.net/shfqbluestone/article/details/52917270)