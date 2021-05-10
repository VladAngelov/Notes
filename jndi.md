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


### Directory and LDAP Packages

#### Directory Package
The *javax.naming.directory* package extends the *javax.naming* package to provide functionality for accessing directory services in addition to naming services.
This package allows applications to retrieve associated with objects stored in the directory and to search for objects using specified attributes.

#### The Directory Context
The *DirContext* interface represents a directory context.
*DirContext* also behaves as a naming context by extending the Context interface - any directory object can also provide a naming context.
It defines methods for examining and updating attributes associated with a directory entry.

* Attributes
    * You use getAttributes() method to retrieve the attributes associated with a directory entry (for which you supply the name). Attributes are modified using modifyAttributes() method.
    * You can add, replace, or remove attributes and/or attribute values using this operation.

* Searches
    * *DirContext* contains methods for performing content based searching of the directory.
    * In the simplest and most common form of usage, the application specifies a set of attributes possibly with specific values to match and submits this attribute set to the search() method.
    * Other overloaded forms of search() support more sophisticated search filters.

#### LDAP Package

The *javax.naming.ldap* package contains classes and interfaces for using features that are specific to the LDAP v3 that are not already covered by the more generic *javax.naming.directory* package.
Most JNDI applications that use the LDAP will find the *javax.naming.directory* package sufficient and will not need to use the *javax.naming.ldap* package at all.
This package is primarily for those applications that need to use "extended" operations, controls, or unsolicited notifications.

* "Extended" Operation
    * In addition to specifying well defined operations such as search and modify, the LDAP v3 (RFC 2251) specifies a way to transmit yet-to-be defined operations between the LDAP client and the server. These operations are called "extended" operations.
    * An "extended" operation may be defined by a standards organization such as the Internet Engineering Task Force (IETF) or by a vendor.

* Controls
    * The LDAP v3 allows any request or response to be augmented by yet-to-be defined modifiers, called controls . A control sent with a request is a request control and a control sent with a response is a response control.
    * A control may be defined by a standards organization such as the IETF or by a vendor.
    * Request controls and response controls are not necessarily paired, that is, there need not be a response control for each request control sent, and vice versa.

* Unsolicited Notifications
    * In addition to the normal request/response style of interaction between the client and server, the LDAP v3 also specifies unsolicited notifications--messages that are sent from the server to the client asynchronously and not in response to any client request.

#### The LDAP Context
The *LdapContext* interface represents a context for performing "extended" operations, sending request controls, and receiving response controls.
Examples of how to use these features are described in the JNDI Tutorial's Controls and Extensions lesson. *https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html*

### Controls and Extensions
*The javax.naming.ldap Package*
The LDAP v3 was designed with extensibility in mind. 
It is extensible in two ways: by using *controls* and by using *extensions*. 

* Controls
    * The LDAP v3 allows the behavior of any operation to be modified through the use of controls.
    * Any number of controls may be sent along with an LDAP request, and any number of controls may be returned with its results.
    * Controls can be standard or proprietary.
    * *For example, you can send a Sort control along with a "search" operation that tells the server to sort the results of the search according to the "name" attribute.*

* Extensions
    * In addition to the repertoire of predefined operations, such as "search" and "modify," the LDAP v3 defines an "extended" operation.
    * The "extended" operation takes a request as the argument and returns a response.
    * The request contains:
        * An identifier that identifies the request and the arguments of the request;
        * And the response contains the results of performing the request.
    * The pair of extended operation request/response is called an extension.
    * *For example, an extension is possible for Start TLS, which is a request for the client to the server to activate the TLS protocol. These extensions can be standard (defined by the LDAP community) or proprietary (defined by a particular directory vendor).*

#### Controls
*A request control is sent from the client to the server along with an LDAP operation. A response control is sent from the server to the client along with an LDAP response.*

* A control can be either:
    * a request control - а control that accompanies an LDAP v3 request sent by the client. The JNDI has two types of request controls: 
        * those that are associated with connection establishment, called connection request controls
        * and those that are associated with a context, called context request controls
        
    * a response control - а control that accompanies an LDAP v3 response sent by the server. 

> *Criticality*
When a client sends a request control to a server, it specifies the control's criticality.
The criticality determines whether the server can ignore the request control. 
When it receives a critical request control, it must either process the request with the control or reject the entire request.
When a server receives a noncritical request control, it must process the request either with the control or by ignoring the control.
It can't reject the request simply because it does not support the control (supposed to send back an "unsupported critical extension" error,).
*You use Control.isCritical() to determine whether a control is critical.*

> *Identification*
A designer defining a control must assign it a unique object identifier (OID).

*You use Control.getID() to get a control's identifier.* 

> *Encoding*
A control's definition specifies how it is to be encoded and transmitted between client and server.
This encoding is done by using ASN.1 BER.
Typically, an application does not need to deal with a control's encoding.
This is because the implementation classes for a control deal with any encoding/decoding, as well as provide the application with type-friendly interfaces for constructing and accessing a control's fields.
Service providers use *Control.getEncodedValue()* to retrieve a control's encoded value for transmission to the server.

#### "Extended" Operations
*The LDAP v3 defines the "extended" operation, which takes a request as the argument and returns a response. The request contains an identifier that identifies the request and the arguments of the request. The response contains the results of performing the request. The pair of "extended" operation request/response is called an extension.*

The *javax.naming.ldap* package defines the interface *ExtendedRequest* to represent the argument to an "extended" operation, and the interface *ExtendedResponse* to represent the result of the operation. 
An extended response is usually paired with an extended request but not necessarily vice versa. 
That is, you can have an extended request that has no corresponding extended response.

> *Extensions Supported by LDAP Servers*
Support for specific extensions is LDAP server-dependent. Eventually, when extensions are standardized, a set of popular extensions supported by most LDAP servers might be available. 
However, proprietary and vendor-specific extensions might still be around. 
*Here is a simple program for finding out the list of extensions that an LDAP server supports:*
```java
import javax.naming.*;
import javax.naming.directory.*;
import java.util.Hashtable;
/**
 * Demonstrates how to discover a server's supported extensions.
 *
 * usage: java ServerExts
 */
class ServerExts {
    public static void main(String[] args) {
        try {
            // Create initial context
            DirContext ctx = new InitialDirContext();

            // Read supportedextension from root DSE
            Attributes attrs = ctx.getAttributes(
            "ldap://localhost:389", new String[]{"supportedextension"});

            System.out.println(attrs);

            // Close the context when we're done
            ctx.close();
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

> *Implementations*

You typically will deal with implementation classes that implement *ExtendedRequest* and *ExtendedResponse* rather than directly use their methods. 
Such implementation classes typically have type-friendly constructors and accessor methods. 

*For example, suppose that an LDAP server supports a Get Time "extended" operation. It would supply classes such as GetTimeRequest and GetTimeResponse, so that applications can use this feature. An application would use these classes as follows.*
```java
// Invoke the "extended" operation
GetTimeResponse resp =
    (GetTimeResponse) lctx.extendedOperation(new GetTimeRequest());

// Get the "extended" operation's (decoded) response
long time = resp.getTime();
```

*The GetTimeRequest and GetTimeResponse classes might be defined as follows.*
```java
public class GetTimeRequest implements ExtendedRequest {
    // User-friendly constructor 
    public GetTimeRequest() {
    };

    // Methods used by service providers
    public String getID() {
        return GETTIME_REQ_OID;
    }
    public byte[] getEncodedValue() {
        return null;  // No value is needed for the Get Time request
    }
    public ExtendedResponse createExtendedResponse(
        String id, byte[] berValue, int offset, int length) throws NamingException {
        return new GetTimeResponse(id, berValue, offset, length);
    }
}

public class GetTimeResponse implements ExtendedResponse {
    long time;
    // Called by GetTimeRequest.createExtendedResponse()
    GetTimeResponse(String id, byte[] berValue, int offset, int length) 
        throws NamingException {
        // Check the validity of the id
        long time =  ... // Decode berValue to get the time
    }

    // These are type-safe and user-friendly methods
    public java.util.Date getDate() { return new java.util.Date(time); }
    public long getTime() { return time; }

    // These are low-level methods
    public byte[] getEncodedValue() {
        return // berValue saved;
    }
    public String getID() {
        return GETTIME_RESP_OID;
    }
}
```
