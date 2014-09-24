[![Build Status](https://travis-ci.org/Zenedith/swagger-jersey2-gradle-demo-app.png)](https://travis-ci.org/Zenedith/swagger-jersey2-gradle-demo-app)
[![Dependency Status](http://mohi.io/github/Zenedith/swagger-jersey2-gradle-demo-app.svg)](http://mohi.io/github/Zenedith/swagger-jersey2-gradle-demo-app)
# swagger-jersey2-gradle-demo-app
## Created swagger jersey2 gradle demo app

### swagger + jersey
https://github.com/wordnik/swagger-core/tree/master/samples/java-jersey2

```
git clone git@github.com:wordnik/swagger-core.git
```

```
$ cd swagger-core/samples/java-jersey2
```

### gradle (wrapper)

```
git clone https://github.com/Zenedith/gradlew
```

### convert swagger project to gradle

```
$ ./gradlew init
```
#### edit dependencies
**dependencies**: update dependencies versions (versions.gradle file)

```
//spring boot
  compile group: 'org.springframework.boot', name: 'spring-boot-starter', version: sprintBootVersion
  compile group: 'org.springframework.boot', name: 'spring-boot-starter-jetty', version: sprintBootVersion
  compile group: 'org.springframework.boot', name: 'spring-boot-starter-actuator', version: sprintBootVersion

  //needed for MainApplicationInitializer class init
  compile group: 'org.springframework', name: 'spring-web', version: springVersion

  compile group: 'org.hibernate', name: 'hibernate-validator', version: hibernateValidatorVersion
  compile group: 'javax.servlet', name: 'javax.servlet-api', version: javaxServletApiVersion
```

**settings.gradle**:  rootProject.name = 'swagger-jersey2-gradle-demo-app'

### test

```
$ ./gradlew test
```

### war
**build.gradle**: apply plugin: 'java' -> apply plugin: 'war'

```
$ ./gradlew war
```

```
$ ls build/libs/
```

### Running on localhost:8080

There are many ways to run this project as a service:

#### 1) standalone jetty runner
http://search.maven.org : search "jetty-runner"

```
$ wget http://search.maven.org/remotecontent?filepath=org/eclipse/jetty/jetty-runner/9.2.3.v20140905/jetty-runner-9.2.3.v20140905.jar
```

```
$ ./gradlew war
$ java -jar jetty-runner-9.2.3.v20140905.jar build/libs/*.war
```

#### 2) using spring-boot (and embedded jetty)

Using ./gradlew run

```
$ ./gradlew run
```

Running as a jar file from gradle

```
$ ./gradlew jar bootRepackage
$ java -jar build/libs/swagger-jersey2-gradle-demo-app-*.jar
```

#### Checkout localhost:8080

* http://localhost:8080/api-docs
* http://localhost:8080/api-docs/store
* http://localhost:8080/api-docs/user

#### web.xml

* not needed for spring boot
* needed for jetty-runner

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

  <servlet>
    <servlet-name>jersey</servlet-name>
    <servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>
    <init-param>
      <param-name>javax.ws.rs.Application</param-name>
      <param-value>com.wordnik.swagger.sample.Application</param-value>
    </init-param>
    <init-param>
      <param-name>jersey.config.server.wadl.disableWadl</param-name>
      <param-value>true</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>jersey</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>

  <servlet>
    <servlet-name>DefaultJaxrsConfig</servlet-name>
    <servlet-class>com.wordnik.swagger.jaxrs.config.DefaultJaxrsConfig</servlet-class>
    <init-param>
      <param-name>api.version</param-name>
      <param-value>1.0.0</param-value>
    </init-param>
    <init-param>
      <param-name>swagger.api.basepath</param-name>
      <param-value>http://localhost:8080</param-value>
    </init-param>
    <init-param>
      <param-name>swagger.filter</param-name>
      <param-value>com.wordnik.swagger.sample.util.ApiAuthorizationFilterImpl</param-value>
    </init-param>
    <load-on-startup>2</load-on-startup>
  </servlet>
  <filter>
    <filter-name>ApiOriginFilter</filter-name>
    <filter-class>com.wordnik.swagger.sample.util.ApiOriginFilter</filter-class>
  </filter>
  <servlet>
    <servlet-name>Bootstrap</servlet-name>
    <servlet-class>com.wordnik.swagger.sample.Bootstrap</servlet-class>
    <load-on-startup>2</load-on-startup>
  </servlet>
  <filter-mapping>
    <filter-name>ApiOriginFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>

```

#### exmaple of users
**UserData.java**: user1, user2, ..

in browser:

* http://localhost:8080/user/user1 (xml output)
* http://swagger.wordnik.com
* http://swagger.wordnik.com -> http://localhost:8080/api-docs

```
curl http://localhost:8080/user/user1 -i (json output)
```
```
curl http://localhost:8080/user/user1 -i -H "Accept: application/xml” (xml output)
```

### GIT
.gitignore:

```
$ .gitignore:
*.class
.settings
.project
.classpath
precompiled
tmp
eclipse/classes
bin
.DS_Store

# Intellij project files
*.iml
*.ipr
*.iws
*.ids
.idea/
build
out

.gradle
```

Update repo:

```
$ git add .gitignore
$ git add --all
$ git commit -am "initial"
```

# Official Swagger Sample App

## Overview
This is a java project to build a stand-alone server which implements the Swagger spec.  You can find out 
more about both the spec and the framework at http://swagger.wordnik.com.  For more information 
about Wordnik's APIs, please visit http://developer.wordnik.com.  There is an online version of this
server at http://petstore.swagger.wordnik.com/api/api-docs.json

### To build from source
Please follow instructions to build the top-level [swagger-core project](https://github.com/wordnik/swagger-core)

### To run (with Maven)
To run the server, run this task:
<pre>
mvn package -Dlog4j.configuration=file:./conf/log4j.properties jetty:run
</pre>

This will start Jetty embedded on port 8002.

### Testing the server
Once started, you can navigate to http://localhost:8002/api/api-docs.json to view the Swagger Resource Listing.
This tells you that the server is up and ready to demonstrate Swagger.

### Using the UI
There is an HTML5-based API tool available in a separate project.  This lets you inspect the API using an 
intuitive UI.  You can pull this code from here:  https://github.com/wordnik/swagger-ui

You can then open the dist/index.html file in any HTML5-enabled browser.  Upen opening, enter the
URL of your server in the top-centered input box (default is http://localhost:8002/api/api-docs).  Click the "Explore" 
button and you should see the resources available on the server.

### Applying an API key
The sample app has an implementation of the Swagger ApiAuthorizationFilter.  This restricts access to resources
based on api-key.  There are two keys defined in the sample app:

<li>- default-key</li>

<li>- special-key</li>

When no key is applied, the "default-key" is applied to all operations.  If the "special-key" is entered, a
number of other resources are shown in the UI, including sample CRUD operations.  Note this behavior is similar
to that on http://developer.wordnik.com/docs but the behavior is entirely up to the implementor.
