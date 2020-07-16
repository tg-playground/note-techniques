# Java SSL Note

Content

- SSL Ecosystem
- Generate Keystore by keytool of JDK
- Configuring Tomcat for Enable HTTPS or HTTP/2
- HTTPS working on your local development environment
- References

## Main

## SSL Ecosystem



## Generate Keystore by keytool of JDK

To create a new `JKS` keystore from scratch, containing a single self-signed Certificate

```shell
$ cd {JAVA_HOME}\bin
$ keytool -genkey -alias {tomcat} -keyalg RSA
Enter keystore password: # input your keystore password
Re-enter new password: 
What is your first and last name? # Enter for set Unknown
What is the name of your organizational unit? # Enter for set Unknown
What is the name of your organization? # Enter for set Unknown
What is the name of your City of Locality? # Enter for set Unknown
What is the name of your State or Province? # Enter for set Unknown
What is the two-letter country code for this unit? # Enter for set Unknown
is CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown correct?
[no]: # Input yes or no
Enter key password for <tomcat>
     (RETURN if same as keystore password): # Enter for same as keystore password
Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore C:\Users\Taogen\.keystore -destkeystore C:\Users\Taogen\.keystore -deststoretype pkcs12".
```

## Configuring Tomcat for Enable HTTPS or HTTP/2

```xml
	<!-- ******************* my configurations *******************-->
	<!-- Define a SSL Coyote HTTP/1.1 Connector on port 8443
	<Connector
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           port="8443" maxThreads="200"
           scheme="https" secure="true" SSLEnabled="true"
           keystoreFile="${user.home}/.keystore" keystorePass="changeit"
           clientAuth="false" sslProtocol="TLS"/>
	

	<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
		maxThreads="150" scheme="https" secure="true"
		clientAuth="false" sslProtocol="TLS"
		keystoreFile="${user.home}/.keystore"
		keystorePass="changeit">
		<UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol"/>
	</Connector>
	
	<Connector port="8443" protocol="org.apache.coyote.http11.Http11AprProtocol"
               maxThreads="150" SSLEnabled="true" >
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
            <Certificate certificateKeyFile="C:\certificate\localhost.key"
                         certificateFile="C:\certificate\localhost.crt"
                         
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
	-->
	

	<Connector
           protocol="org.apache.coyote.http11.Http11AprProtocol"
           port="8443" maxThreads="200"
           scheme="https" secure="true" SSLEnabled="true"
           SSLCertificateFile="C:\certificate\localhost.crt"
           SSLCertificateKeyFile="C:\certificate\localhost.key"
           SSLVerifyClient="optional" SSLProtocol="TLSv1+TLSv1.1+TLSv1.2">
		<UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
	</Connector>
```



## HTTPS working on your local development environment

Problem: 

- Local certificate getting rejected was that Chrome had deprecated support for commonName matching in certificates.

Solution: 

- using [OpenSSL](https://www.openssl.org/) to generate all of our certificates.

Steps

Step 0: Install OpenSSL 

[OpenSSL](https://www.openssl.org/)

[OpenSSL for Window](https://wiki.openssl.org/index.php/Binaries)

### Step 1: Generate Root SSL certificate

Create private key `rootSSL.key`

```shell
openssl genrsa -des3 -out rootSSL.key 2048
....................................................................................+++++
..............+++++
e is 65537 (0x010001)
Enter pass phrase for rootSSL.key: # Input your private key password, eg: changeit
Verifying - Enter pass phrase for rootSSL.key: # Input again
```

Create root certificate `rootSSl.pem`

```shell
openssl req -x509 -new -nodes -key rootSSL.key -sha256 -days 1024 -out rootSSL.pem
Enter pass phrase for rootSSL.key: # Enter the password for the root SSL key we created in step 1
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:
```

Note: you can choose to create a certificate file that lasts for X number of days. We’re going to choose 1024 days in this example, but you can select any amount – the longer, the better.

You don’t have to put your legit information in here as we’re only running SSL certificates on the local development environment, but I like to do it properly.

Move root private key and root certificate files

```
md c:\certificate
move rootSSL.key C:\certificate
move rootSSL.pem C:\certificate
```





### Step 2: Get Operating System Trust the root SSL certificate

Press the Windows key + R

Type “MMC” and click “OK”

Go to “File > Add/Remove Snap-in”

Click “Certificates” and “Add”

Select “Computer Account” and click “Next”

Select “Local Computer” then click “Finish”

Click “OK” to go back to the MMC window

Double-click “Certificates (local computer)” to expand the view

Select “Trusted Root Certification Authorities”, right-click “Certificates” and select “All Tasks” then “Import”

Click “Next” then Browse and locate the “rootSSL.pem” file we created in step 2

Select “Place all certificates in the following store” and select the “Trusted Root Certification Authorities store”. Click “Next” then click “Finish” to complete the wizard.

Browse the certificates to see yours in the list.

Now you can start issuing SSL certificates for all your local domains.

### Step 3: Create a domain certificate for `localhost` 

create a file called “localhost.key” which contains the private key information for that domain.

```shell
$ openssl req -new -sha256 -nodes -out localhost.csr -newkey rsa:2048 -keyout localhost.key -subj "/C=AU/ST=NSW/L=Sydney/O=Client One/OU=Dev/CN=client-1/emailAddress=hello@client-1.local"
Generating a RSA private key
.................................................................................................................................+++++
..............+++++
writing new private key to 'localhost.key'
-----
```

When you are issuing certificates for your own local domains, replace “client-1.local” with your local server domain name.

You can also change the “-subj” parameter to reflect your country, state, location etc.

Create a domain certificate for `localhost` 

```shell
$ openssl x509 -req -in localhost.csr -CA C:\certificate\rootSSL.pem -CAkey C:\certificate\rootSSL.key -CAcreateserial -out localhost.crt -days 500 -sha256 -extensions "authorityKeyIdentifier=keyid,issuer\n basicConstraints=CA:FALSE\n keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment\n  subjectAltName=DNS:localhost"
Signature ok
subject=C = AU, ST = NSW, L = Sydney, O = Client One, OU = Dev, CN = client-1, emailAddress = hello@client-1.local
Getting CA Private Key
Enter pass phrase for C:\certificate\rootSSL.key: # input your rootSSL key password, eg: changeit
```

When you are issuing certificates for your own local domains, replace “client-1.local” with your local server domain name.

Enter the password for the root SSL certificate when prompted.

You can see all the files we have created; “client-1.local.crt” and “client-1.local.key” are the files you will need to add to your web server configuration for the local development site.

Move localhost.crt and localhost.key

```shell
$ move localhost.crt C:\certificate
$ move localhost.key C:\certificate
```



### Step 4: Using the New Local Domain Certificates in Your Web Server





## References

- SSL
  - [What is a Root SSL Certificate?](https://support.dnsimple.com/articles/what-is-ssl-root-certificate/)

- Configuring Tomcat 
  - [SSL/TLS Configuration HOW-TO - Apache Tomcat](https://tomcat.apache.org/tomcat-8.0-doc/ssl-howto.html)
  - [Configure Tomcat support HTTP/2](https://huongdanjava.com/configure-tomcat-support-http-2.html)

- Enable HTTPS on Localhost 
  - [How to get HTTPS working on your local development environment in 5 minutes - MacOS](https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/)
  - [How to Get HTTPS Working in Windows 10 Localhost Dev Environment](https://zeropointdevelopment.com/how-to-get-https-working-in-windows-10-localhost-dev-environment/)

