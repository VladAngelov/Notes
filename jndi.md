# JNDI

> *The Java Naming and Directory Interface (JNDI) is an application programming interface (API) that provides naming and directory functionality to applications written using the Java programming language.* 


## Architecture

The JNDI architecture consists of an API and a service provider interface (SPI). 
Java applications use the JNDI API to access a variety of naming and directory services. 
**The SPI** enables a variety of naming and directory services to be plugged in transparently, thereby allowing the Java application using the JNDI API to access their services.
*(https://docs.oracle.com/javase/jndi/tutorial/figures/jndi/getStarted/jndiarch.jpg)*

### The SPI
*Service Provider Package*

The *javax.naming.spi* package provides the means by which developers of different naming/directory service providers can develop and hook up their implementations so that the corresponding services are accessible from applications that use the JNDI.

#### Plug-In Architecture
The javax.naming.spi package allows different implementations to be plugged in dynamically.

#### Java Object Support
The *javax.naming.spi* package supports implementors of *Context.lookup()* and related methods to return Java object. 
And with the implementors of *Context.bind()* and related methods can accept Java objects and store the objects in a format acceptable to the underlying naming/directory service. 

#### Multiple Naming Systems (Federation)
JNDI operations allow applications to supply names that span multiple naming systems.
In the process of completing an operation, one service provider might need to interact with another service provider.

*Naming example (https://docs.oracle.com/javase/jndi/tutorial/getStarted/examples/naming.html)*

*read an attribute from a directory service, in this case an LDAP directory (https://docs.oracle.com/javase/jndi/tutorial/getStarted/examples/directory.html)*


## Packaging
The JNDI is included in the Java 2 SDK, v1.3 and later releases.
It is also available as a Java Standard Extension for use with the JDK 1.1 and the Java 2 SDK, v1.2.
It extends the v1.1 and v1.2 platforms to provide naming and directory functionality. 

*To use the JNDI, you must have the JNDI classes and one or more service providers.*
* The Java 2 SDK, v1.3 includes three service providers for the following naming/directory services: 
    * Lightweight Directory Access Protocol (LDAP)
    * Common Object Request Broker Architecture (CORBA) Common Object Services (COS) name service
    * Java Remote Method Invocation (RMI) Registry 

* The JNDI is divided into five packages:
    * javax.naming
    * javax.naming.directory
    * javax.naming.event
    * javax.naming.ldap
    * javax.naming.spi
