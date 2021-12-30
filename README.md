# Oracle Weblogic desde Cero 
### by Apasoft Training, Udemy

This course prepares for exam am 1Z0-133, **Oracle WebLogic Server 12c: Administration I**

## Introduction 
Weblogic is an application server for enterprise Java applications. It is a Java EE compliant server (a standard). Java EE include specifications such as JDBC, JMS, JMX etc. Other application servers in the same category are IBM Webshpere, Jboss etc. 

Java EE servers _run_ over _one_ JVM (only one ?), which will include, other than the standard JVM libraries, all the libraries in the Java EE specification (some version). Java EE servers will _implement_ those specifications, but may add  some vendor-specific libraries as well. See the Java EE 8 specs below. Weblogic 12c is compatible with  Java EE 7. 

Weblogic supports clustering for high availability, fault tolerance, and scalability. 

Weblogic is the engine supporting all, or most, the _Fusion Middleware_ products of Oracle:
- Oracle SOA
- Oracle Business Bus
- Oracle Business Intelligence (BI)
- Oracle BPM
- Oracle Identity 

etc.

Java EE servers receive request from Java or the web, for example. Any Java EE server will have two big layers. One is the _web layer_ (presentation layer) which will include HTML, CSS, JSP, Servlets etc. This layer runs the web part of the applications. The second layer is the _business layer_, which includes  EJB etc. The objects in the business layer contain the business logic and are invoked by the presentation layer. **Tomcat** is not a Java EE compliant server, as it can natively run only a web layer.

Enterprise application need to access external resources such as databases, web services, Crms, Erps etc.

## Java EE 8
#### https://www.oracle.com/it/java/technologies/java-ee-glance.html
Java EE 8 continues to improve API and programming models needed for today's applications and adds features requested by our world-wide community. This release modernizes support for many industry standards and continues simplification of enterprise ready APIs. Enhancements include:

- Java Servlet 4.0 API with HTTP/2 support
- Enhanced JSON support including a new JSON binding API
- A new REST Reactive Client API
- Asynchronous CDI Events
- A new portable Security API
- Server-Sent Events support (Client & Server-side)
- Support for Java SE 8 new capabilities (e.g. Date & Time API, Streams API, annotations enhancements)

Java EE 8 builds on Java EE 7. The following JSRs are new or updated in Java EE 8:
- JSR 366 – Java EE 8 Platform
- JSR 365 – Contexts and Dependency Injection (CDI) 2.0
- JSR 367 – The Java API for JSON Binding (JSON-B) 1.0
- JSR 369 – Java Servlet 4.0
- JSR 370 – Java API for RESTful Web Services (JAX-RS) 2.1
- JSR 372 – JavaServer Faces (JSF) 2.3
- JSR 374 – Java API for JSON Processing (JSON-P)1.1
- JSR 375 – Java EE Security API 1.0
- JSR 380 – Bean Validation 2.0
- JSR 250 – Common Annotations 1.3
- JSR 338 – Java Persistence 2.2
- JSR 356 – Java API for WebSocket 1.1
- JSR 919 – JavaMail 1.6

## Weblogic architecture
wl has wl Domains, which is a logical name. A wl **domain** is a set of wl servers. Each wl server will run in an independent JVM. wl servers in a same domain will have common access a set of resources, and Java applications. They are servers that collaborate, and are administrated together. They may run Java applications.

Each wl domain will have one, and only one, **Admin Server**. In production environment, its sole purpose should be running Weblogic Administration Console (web console?). There is also a cli administration console, WLST. In development environment however, it is common to run applications in the Admin Server as well.

The Admin Server manages **managed servers** in its domain. The managed servers are the servers running the applications. Managed servers can be grouped in **clusters**. Servers in a cluster will contain the same applications, will have access to the same resources, same behaviour etc (high availability cluster).

Each wl domain will also have **machines**. A machine is also logical concept, but they do are _associated_ with physical, or virtual, machines. All wl servers in a domain, including the Admin Server, will be running "inside" a given machine. A cluster of managed servers may include servers running in different machines.

We take real or virtual machines, and assign to them, first, machines, and then, wl servers.

Inside a wl domain we can create a **cluster**

![image info](./pictures/domain.png)

## Installation of Oracle Fusion Middleware
In order to install Oracle Fusion Middleware you have to donwload the installer first from your Oracle account. I downloaded `fmw_12.2.1.4.0_infrastructure.jar`. It is then installed with `java -jar fmw_12.2.1.4.0_infrastructure.jar`. The active JDK most be one from Oracle. The installation directory I chose is (_Oracle home_):
```text
~/Oracle/Middleware/Oracle_Home$ ll
total 40
drwxr-x---  9 camilo camilo 4096 dic 30 13:32 ./
drwxr-x---  3 camilo camilo 4096 dic 30 13:30 ../
drwxr-x---  6 camilo camilo 4096 dic 30 13:30 coherence/
drwxr-x---  8 camilo camilo 4096 dic 30 13:31 em/
drwxr-x--- 16 camilo camilo 4096 dic 30 13:32 inventory/
drwxr-x--- 11 camilo camilo 4096 dic 30 13:31 OPatch/
drwxr-x--- 18 camilo camilo 4096 dic 30 13:31 oracle_common/
-rw-r-----  1 camilo camilo  129 dic 30 13:32 oraInst.loc
drwxr-x---  8 camilo camilo 4096 dic 30 13:32 oui/
drwxr-x---  8 camilo camilo 4096 dic 30 13:31 wlserver/
```
Inside this directory we might later install other products from the Oracle Middleware suite. Directories here are:
- coherence/: files of this product from Oracle. Coherence is a grid memory (cluster) used by Oracle for session data storage etc.
- em/: Console of the enterprise manager, including the Enterprise Manager Fusion Control ? From here we can administer all the middleware applications in Oracle Fusion Middleware, eg. Oracle Business Intelligence, Oracle Identity, Oracle Portal etc. The Weblogic server, though, has its own console.
- inventory/: ever present. Contains components and patches installed in the Oracle home.
- OPatch/: ever present. Contains binaries and libraries for patches. 
- oracle_common/: programs and libraries common to wl server. Important for configurations.
- oui/: files for Oracle Universal (Un)Installer.
- wlserver/: This is the _Weblogic home_. Files and everything of Weblogic.Important.

