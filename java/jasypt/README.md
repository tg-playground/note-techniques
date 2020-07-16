# Jasypt



## Build Jasypt with Spring Boot

1. Build Maven project

2. Add dependencies in `pom.xml`

   ```
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.1.6.RELEASE</version>
   </parent>
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>com.github.ulisesbocchio</groupId>
           <artifactId>jasypt-spring-boot-starter</artifactId>
           <version>LATEST</version>
       </dependency>
   </dependencies>
   <build>
       <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
           <plugins>
               <!-- Package as an executable jar/war. $ mvn package spring-boot:repackage -->
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
                   <executions>
                       <execution>
                           <goals>
                               <goal>repackage</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
           </plugins>
       </pluginManagement>
   </build>
   ```

3. Generating password

   ```shell
   $ java -cp jasypt-1.9.2.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="Jack" password="supersecretz" algorithm=PBEWITHMD5ANDDES
   $ java -cp jasypt-1.9.2.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="Beijing" password="supersecretz" algorithm=PBEWITHMD5ANDDES
   ```

4. Writing configuration file `application.yml`

   ```
   #Jack
   userdemo.name: ENC(RRvBBWqju65d4AV9pI4Qiw==) 
   # Beijing
   usercity.name: ENC(auyX4dYw1pmmrmWAYPGXng==)
   ```

5. Write Application `App.java`

   ```
   package com.taogen;
   
   import com.ulisesbocchio.jasyptspringboot.annotation.EnableEncryptableProperties;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.stereotype.Component;
   import org.springframework.web.bind.annotation.RestController;
   
   @SpringBootApplication
   @EnableEncryptableProperties
   public class App
   {
   
       public static void main(String[] args)
       {
           SpringApplication.run(App.class, args);
       }
   
       @Component
       public class MyRunner implements CommandLineRunner
       {
   
           @Value("${userdemo.name}")
           private String username;
   
           @Value("${usercity.name}")
           private String cityname;
           @Override
           public void run(String... args) throws Exception {
               System.out.println("username is = " + username);
               System.out.println("cityname is = " + cityname);
           }
       }
   }
   ```

6. Package to Spring Boot runnable jar

   ```shell
   $ mvn -Djasypt.encryptor.password=<your_password> package spring-boot:repackage
   # eg: my password is supersecretz
   $ mvn -Djasypt.encryptor.password=supersecretz package spring-boot:repackage
   ```

7. Running jar

   ```shell
   $ java -jar target/<project_name.jar> --jasypt.encryptor.password=<your_password>
   # eg: my project name is jasypt, my password is supersecretz
   $ java -jar target/jasypt-1.0-SNAPSHOT.jar --jasypt.encryptor.password=supersecretz
   ```

## Command Lines



```
# Generate password
java -cp jasypt-1.9.2.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="<your_text>" password="<your_password>" algorithm=PBEWITHMD5ANDDES

# Maven test/pakcage/run
mvn -Djasypt.encryptor.password=<your_password> test
mvn -Djasypt.encryptor.password=<your_password> package spring-boot:repackage
mvn -Djasypt.encryptor.password=<your_password> spring-boot:run

# Running with spring-boot jar
java -jar target/<project_name.jar> --jasypt.encryptor.password=<your_password>
```
