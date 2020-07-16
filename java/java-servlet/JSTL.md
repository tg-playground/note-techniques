# JSTL Note

`<c:url value="/servletUri" />` 

1. `<c:url>`标签会为 url 自动加上 contextPath。
2. 当 jsessionid 存在与浏览器地址栏时，会自动为该页面的所有 `<c:url>` 中的 url 加上 jsessionid。

示例如下：

```html
<c:url value="/login" />
result:
http://localhost/servlet-session/login;jsessionid=98244D588C7B3F0EDB60DD9955207296
```

