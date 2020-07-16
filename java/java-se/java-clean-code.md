# Java Clean Code

<h3 id="content">Content</h3>

- Variable
- Container
- IO  Stream



### Variable

- isSuccess => success.

### Container

- Initializing Array

```java
        private static final List<String> myList = new ArrayList<>(Arrays.asList("a","b"));
```

- Initializing Map

```java
        private Map<String,String> myMap = new HashMap<String,String>()
        {
            private static final long serialVersionUID = 1L;
            {
                put("1","a");
                put("2","b");
            }
        };
```

- Arrays.asList(...) 可以用来初始化数组常量。但是要支持数组长度可变的话，需要再包装一次，如 new ArrayList<>(Arrays.asList(...))。不包装直接往list中add元素的话会报java.lang.UnsupportedOperationException错误，因为Arrays中的数组是final修饰的。
- Traverse Map can't add or remove elements. It will throw Exception "java.util.ConcurrentModificationException". For example. You can traverse map save you need remove key of key-value, then remove them by key.

```java
        for (String s : myMap.keySet())
        {
            if (myMap.get("2") != null)
            {
                myMap.remove("2");
            }
            if (myMap.get("3") == null)
                myMap.put("3","c");
        }
```



### IO Stream

- Auto close IO stream.

```java
        try
        (
            BufferedWriter writer = new BufferedWriter(new FileWriter(file));
        )
        {
            for (String line : text)
            {
                writer.write(line + "\r\n");
            }
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
```

