# SSL

<h3 id="content">Content</h3>

- Basic Concepts
- Creating the keystore file
- Creating the Certificate Signing Request (CSR)
- Installing an SSL Certificate on Keystore 
- Configuration Tomcat Using HTTPS
- Limiting Servlet Project SSL Usage
- Others

### Main

### Basic Concepts

**Certificate** 

It contains public key and private key. CA using public key encrypt. Web Server using private decrypt.

format

- PKCS#7 (`.p7b`)  it already includes the necessary intermediate and root certificates. 
- PEM (`.crt`) encoded file contains a private key or a certificate.
- DER
- PKCS#12 (`.pfx`) a keystore format used by some application. It can contain private keys or public keys.

**Keystore**

It is a file for to store certificates. The file suffix is `.jks` generally. It can have no file suffix.

**Keytool**

Keytool is the Java tool to manage keystores and certificates. 

Keytool and IKeyMan only recognize PKCS 12 keystores, so there is a need to transform the PFX/PEM files into PKCS12 files.

### Steps to install SSL certificate on Tomcat web server

- ##### **Initial Checklist**

  - Buy/renew SSL Certificate
  - Generate CSR with SHA-2 algorithm
  - Save the CSR & Private key file on your server
  - Apply for SSL Certificate Issuance
  - Submit SSL Certificate issuance documents as per CA’s requirement (Only for Extended & Organization Validation)
  
- Download SSL Certificate Files

- Install SSL certificate on Keystore

- Configuration of SSL Connector on Tomcat server.xml



### Creating the Keystore file

Java environment using keytool command.

```shell
// generating keystore by keytool command
$ keytool -genkey -alias [youralias] -keyalg RSA -keystore [/preferred/keystore/path]
Enter keystore password:
Re-enter new password:
What is your first and last name?
What is the name of your organizational unit?

// check the details of the certificate
$ keytool -list -keystore example.jks –v
```



### Creating the Certificate Signing Request (CSR)

It will be used by the Certificate Authority of your choice to generate the Certificate SSL will present to other parties during the handshake.

```shell
$ keytool -certreq -keyalg RSA -alias [youralias] -file [yourcertificatname].csr -keystore [path/to/your/keystore]
```



### Installing an SSL Certificate on Keystore 

Java environment using keytool command

```shell
// PKS#7 (.p7b)
$ keytool -import -trustcacerts -alias tomcat -keystore example.jks -file example.p7b
keytool -import -trustcacerts -alias server -file website-name.p7b -keystore website-name.jks

// PEM (.crt)
// To import a root certificate
$ keytool -import -alias root -keystore example.jks -trustcacerts -file root.crt
// To import an intermediate certificate
$ keytool -import -alias intermediate -keystore example.jks -trustcacerts -file intermediate.crt
// To import the certificate issued for the domain name
$ keytool -import -alias tomcat -keystore example.jks -file example.crt

// PKCS#12 (.pfx)
// PEM TO PKCS#12
$ openssl pkcs12 -export -out your_pfx_certificate.pfx -inkey your_private.key -in your_pem_certificate.crt -certfile CA-bundle.crt
```

check the details of the certificate

```shell
// check the details of the certificate
$ keytool -list -keystore example.jks –v
```



### Configuration Tomcat Using HTTPS

Home_Directory/conf/server.xml

```xml
<Connector port="443" protocol="HTTP/1.1"
SSLEnabled="true"
scheme="https" secure="true" clientAuth="false"
sslProtocol="TLS" keystoreFile="/your_path/yourkeystore.jks"
keystorePass="password_for_your_key_store" />
```

If your keystore contains more than one private key alias, you need to add ‘*keyAlias*’ directive with the reference to a needed alias.

`keyAlias="tomcat"`

Save the changes and restart Tomcat web service.

JSSE

```xml
<Connector port="8443" maxThreads="150" scheme="https" secure="true" SSLEnabled="true" keystoreFile="path/to/your/keystore" keystorePass="YourKeystorePassword" clientAuth="false" keyAlias="yourAlias" sslProtocol="TLS"/>
```

Apache Portable Runtime (APR)

```xml
<Connector port="8443" scheme="https" secure="true" SSLEnabled="true" SSLCertificateFile="/path/to/your/certificate.crt" SSLCertificateKeyFile="/path/to/your/keyfile" SSLPassword="YourKeystorePassword" SSLCertificateChainFile="path/to/your/root/certificate" keyAlias="yourAlias" SSLProtocol="TLSv1"/>
```





### Limiting Servlet Project SSL Usage

Enabling SSL in Tomcat's server.xml file causes all files to be run both as secure and insecure pages, which can cause unnecessary server load. You can choose which applications offer SSL connections on a per-application basis by adding the following <security-constraint> element to the application's WEB-INF/web.xml file

```xml
<security-constraint>
	<web-resource-collection>
		<web-resource-name>YourAppsName</web-resource-name>
		<url-pattern>/*</url-pattern>
	</web-resource-collection>
	<user-data-constraint>
        <!-- "CONFIDENTIAL" or "NONE" -->
		<transport-guarantee>CONFIDENTIAL</transport-guarantee>
	</user-data-constraint>
</security-constraint>
```



### Others

**Common Errors Caused By Aliases and Passwords**

If you encounter any errors with your SSL configuration, make sure that you have correctly entered settings such as keystore passwords and aliases. These values are case sensitive for some of the supported keystore formats.



### References

1 [How to install an SSL certificate on a Tomcat server](https://helpdesk.ssls.com/hc/en-us/articles/203505171-How-to-install-an-SSL-certificate-on-a-Tomcat-server)
2 [A Simple Step-By-Step Guide To Apache Tomcat SSL Configuration](https://www.mulesoft.com/tcat/tomcat-ssl)
3 [How to configure Tomcat to support SSL or https](https://www.mkyong.com/tomcat/how-to-configure-tomcat-to-support-ssl-or-https/)
4 [How to Install SSL Certificate on Tomcat Web Server](https://aboutssl.org/how-to-install-ssl-certificate-on-tomcat-web-server/)