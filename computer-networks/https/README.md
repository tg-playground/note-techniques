# HTTPS 



### What is HTTPS

**Hypertext Transfer Protocol Secure** (**HTTPS**) is an extension of the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) (HTTP). It is used for [secure communication](https://en.wikipedia.org/wiki/Secure_communications) over a [computer network](https://en.wikipedia.org/wiki/Network_operating_system), and is widely used on the Internet.[[1\]](https://en.wikipedia.org/wiki/HTTPS#cite_note-1)[[2\]](https://en.wikipedia.org/wiki/HTTPS#cite_note-2) In HTTPS, the [communication protocol](https://en.wikipedia.org/wiki/Communication_protocol) is encrypted using [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS), or, formerly, its predecessor, Secure Sockets Layer (SSL). The protocol is therefore also often referred to as **HTTP over TLS**,[[3\]](https://en.wikipedia.org/wiki/HTTPS#cite_note-3) or **HTTP over SSL**.

HTTPS should not be confused with the little-used [Secure HTTP](https://en.wikipedia.org/wiki/Secure_Hypertext_Transfer_Protocol) (S-HTTP) specified in [RFC 2660](https://tools.ietf.org/html/rfc2660)

### History

In 1994, [Netscape Communications](https://en.wikipedia.org/wiki/Netscape_Communications) created HTTPS for its [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape_Navigator) web browser.

In May 2000. Originally, HTTPS was used with the [SSL](https://en.wikipedia.org/wiki/Secure_Sockets_Layer) protocol. As SSL evolved into [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS), HTTPS was formally specified by [RFC 2818](https://tools.ietf.org/html/rfc2818).

- [RFC 2818](https://tools.ietf.org/html/rfc2818), HTTP Over TLS



### Using Certbot to get a Free HTTPS Certificate

You can use Certbot to easily obtain and configure a free certificate from Let’s Encrypt, a joint project of EFF, Mozilla, and many other sponsors.

 installing and running Certbot on a server.

we kindly suggest that you use the Certbot packages provided by your package manager (see [certbot.eff.org](https://certbot.eff.org/)). If such packages are not available, we recommend using `certbot-auto`, which automates the process of installing Certbot on your system.

### System Requirements

requires Python 2.7 or 3.4+ running on a UNIX-like operating system.

### Install Process



### Cert location

```
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
```



