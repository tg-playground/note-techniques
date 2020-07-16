# Web Crawler by Java

A typical crawler process is a loop consisting of fetching, parsing, link extraction, and processing of the output (storing, indexing). Though the devil is in the details, i.e. how to be "polite" and respect `robots.txt`, meta tags, redirects, rate limits, URL canonicalization, infinite depth, retries, revisits, etc.

### HTTP Client

- [Apache HttpClient](http://hc.apache.org/httpcomponents-client-ga/)

### Java-based HTML Parser

- [Jsoup](http://jsoup.org/)
  - Jsoup-html标签解析
  - JsonPath-json数据解析
- [Jaunt API](http://jaunt-api.com/)
- [HtmlCleaner](http://htmlcleaner.sourceforge.net/)
- [JTidy](http://jtidy.sourceforge.net/)
- [NekoHTML](http://nekohtml.sourceforge.net/)
- [TagSoup](http://ccil.org/~cowan/XML/tagsoup/)

### web crawler Framework

- Large Weight
  - [Apache Nutch](https://wiki.apache.org/nutch/FrontPage#What_is_Apache_Nutch.3F). 适合做搜索引擎，分布式爬虫是其中一个功能。
  - Heritrix. 比较成熟的爬虫。
- Light Weight
  - [crawler4j](https://github.com/yasserg/crawler4j). 简单的轻量级网络爬虫。
  - [webmagic](https://github.com/code4craft/webmagic). 国人作品，推荐
- [gecco](https://github.com/xtuhcy/gecco)
- [Norconex HTTP Collector](https://www.norconex.com/collectors/collector-http/flow)
- [vidageek crawler](https://github.com/vidageek/crawler)
- [Webmuncher](https://github.com/dadepo/Webmuncher)

### Jsoup VS Crawler4J

`jsoup` is a Java library for working with real-world HTML. It provides a very convenient API for extracting and manipulating data, using the best of DOM, CSS, and jquery-like methods.

`crawler4j` is an open source web crawler for Java which provides a simple interface for crawling the Web. Using it, you can setup a multi-threaded web crawler in few minutes.

The difference, `Crawler4J` is a crawler with some simple operations for parsing (you could extract the images in one line), but there is no implementation for complex `CSS` queries. `Jsoup` is a parser that gives you a simple API for `HTTP` requests. For anything more complex there is no implementation.

### Why Need Using Crawler Framework

There are a lot of complex function when you want to crawler a website.

1. Give base URI (home page)
2. Take all the URIs from each page and retrieve the contents of those too.
3. Move recursively for every URI you retrieve.
4. Retrieve the contents only of URIs that are inside this website (there could be external URIs referencing another website, we don't need those).
5. Avoid circular crawling. Page A has URI for page B (of the same site). Page B has URI for page A, but we already retrieved the content of page A (the `About` page has a link for the `Home` page, but we already got the contents of `Home` page so don't visit it again).
6. The crawling operation must be multithreaded
7. The website is vast. It contains a lot of pages. We only want to retrieve 50 URIs beginning from `Home` page.

Some Parse Function

1. Get all paragraphs of a page
2. Get all images
3. Remove invalid tags (tags that do not comply to the `HTML` specs)
4. Remove script tags

Using crawler framework, you don't to implements a lot of function by yourself. You can just call the API support by frameworks.



### References

[Java Web Crawler Libraries](https://stackoverflow.com/questions/11282503/java-web-crawler-libraries)

[Crawler4j vs. Jsoup for the pages crawling and parsing in Java](https://stackoverflow.com/questions/34888510/crawler4j-vs-jsoup-for-the-pages-crawling-and-parsing-in-java)

[开发网络爬虫应该选择 Nutch、Crawler4j、WebMagic、scrapy、WebCollector 还是其他的？这里按照我的经验随便扯淡一下：上面说的爬虫，基本可以分 3 类](https://hacpai.com/article/1496842430974)