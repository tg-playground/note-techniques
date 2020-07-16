# Servlet Note

Content

- The Servlet
- The Request
- Servlet Context
- The Response
- Filters
- Sessions
- Dispatching Requests
- Web Applications
- Application Lifecycle Events
- Mapping Requests to Servlets
- Security
- Deployment Descriptor

## The Request

### Request URI

`Request URI = Context Path + Servlet Path + Path Info + Query String`, eg:

```
<url-pattern>/user/*</url-pattern>
/myProject/user/get?name=Tom

ContextPath: /myProject
ServletPath: /user
PathInfo: /get
QueryString: ?name=Tom
```



```java
request.getRequestDispatcher("requestDisPatcherUri") // request dispatcher uri is a path relative to the context root. the uri just servlet path + page info.

response.sendRedirect("redirectUri") // redirect uri need complete request uri.
```

## Servlet Context

```java
// get web resources, resource is a absolute path start with project context. 
InputStream inputStream = getServletContext().getResourceAsStream("/file");
// get class resources, resource is a relative path relative to WEB-INF/classes directory.
InputStream inputStream = ClassLoader.getSystemClassLoader().getResourceAsStream("file");
```



## The Response



## Filters



## Sessions

### session management by url rewriting

1. Session Filter

```java
// SessionFilter: redirect new url contains jsessionid, when cookie is banned. Don't redirect contains jsessionid when cookie is enable.
```

2. in servlet. 

```java
resonse.sendRedirect(response.encodeRedirectURL("{your_uri}")); // it will add jsessionid to redirect uri if not cookies.
request.getRequestDispatcher("/{your_uri}").forward(request, response); // forward not need update url, not jsessionid url will filter by SessionFilter
```

3. in JSP

```html
<c:url value="{your_uri}" /> <!-- it will auto add jsessionid to uri -->
```

4. Logout remove create new session

```java
HttpSession session = req.getSession(false);
if (session != null){
    session.invalidate();
    logger.debug("Old sessionId is {}", session.getId());
}
session = req.getSession(true);
logger.debug("New sessionId is {}", session.getId());
String redirectUri = resp.encodeRedirectURL(req.getContextPath() + "/login.jsp");
resp.sendRedirect(redirectUri);
```

5. Authentication Filter

```java
HttpSession session = request.getSession();
if (session.isNew() && ! request.getRequestURI().contains("/login.jsp")) {
    logger.debug("Unauthorized access request");
    response.sendRedirect(servletContext.getContextPath() + "/login.jsp");
} else {
    filterChain.doFilter(servletRequest, servletResponse);
}
```

## Dispatching Requests



## Web Applications

### Web Application Deployment

When a web application is deployed into a container, the following steps must be
performed, in this order, before the web application begins processing client
requests.

- Instantiate an instance of each event listener identified by a <listener> ele-
  ment in the deployment descriptor.
- For instantiated listener instances that implement ServletContextListener ,
  call the contextInitialized() method.
- Instantiate an instance of each filter identified by a <filter> element in the de-
  ployment descriptor and call each filter instance’s init() method.
- Instantiate an instance of each servlet identified by a <servlet> element that
  includes a <load-on-startup> element in the order defined by the load-on-
  startup element values, and call each servlet instance’s init() method.

### Inclusion of a web.xml Deployment Descriptor

A web application is NOT required to contain a web.xml if it does NOT contain any
Servlet, Filter, or Listener components. In other words an application containing
only static files or JSP pages does not require a web.xml to be present.

## Application Lifecycle Events



## Mapping Requests to Servlets



## Security



## Deployment Descriptor



--END--

