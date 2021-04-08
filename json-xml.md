# JSON-XML


## JSON

*What is JSON?*
> JSON is an abbreviation for Javascript Object Notation.
> Most commonly used for client-server communication.
> We can easy to save it to a file or record in the database. 
> Easy to read/write and language-independent.
> JSON format uses key-value pairs to describe data.
> A JSON value can be another JSON object, array, number, string, boolean (true/false) or null.

*   The JSON string is enclosed by curly braces {}.
*   The keys and values of JSON must be enclosed in quotation marks {"}.
*   If there is more data (more key => value pairs), we use commas (,) to separate.
*   JSON keys should be unsigned letters or numbers, _, and no spaces, the first character should not be set to numbers.


## JSON Schema
*What is JSON Schema?*
> A declarative language for validating the format and structure of a JSON Object.
> It allows us to specify the number of special primitives to describe exactly what a valid JSON Object will look like.
* The JSON Schema specification is divided into three parts:
    * JSON Schema Core: The JSON Schema Core specification is where the terminology for a schema is defined;
    * JSON Schema Validation: The JSON Schema Validation specification is the document that defines the valid ways to define validation constraints. This document also defines a set of keywords that can be used to specify validations for a JSON API;
    * JSON Hyper-Schema: This is another extension of the JSON Schema spec, wherein, the hyperlink and hypermedia-related keywords are defined;


## XML

*What is XML?*
> XML stands for eXtensible Markup Language also called the extensible markup language proposed by the World Wide Web Consortium (http://www.w3.org/) to create other markup languages.
> This is a simple subset that can describe many different types of data, so it is very useful in sharing data between systems.
> Tags in XML are often not predefined, but they are created according to user conventions.

*   XML is extensible: 
    * XML allows you to create your own custom tags to suit your application.
*   XML carries data, not displaying it:
    * XML allows you to store data regardless of how it will be displayed.
*   XML is a common standard: 
    * XML was developed by the World Wide Web Consortium (W3C) and is available as an open standard.

> XML is built on a nested node structure.    


## Serialization

> In general, we can understand serialization as a process of transforming data into bits.
> Serialized data can be transferred through a network or stored, and recreated when needed. 
*Serialization takes an in-memory data structure and converts it into a series of bytes that can be stored and transferred.*

## Deserialization

> Deserialization takes a series of bytes and converts it to an in-memory data structure that can be consumed programmatically.

*Marshal and Unmarshal are synonymous with Serialize and Deserialize.*

* The thing to understand is how those stream of bytes are interpreted or manipulated so that we get the exact same Object/ same state. There are various ways to achieve that. Some of them are:
    * XML: Convert Object to XML, transfer it over a network or store it in a file/db. Retrieve it and convert it back to the object with same state. In Java we use JAXB(Java architecture for XML binding) library.(From java 6 it comes bundled with JDK).
    * JSON: Same can be done by converting the Object to JSON (JavaScript Object notation). Again there is GSON library that can be used for this.
    * Or we can use the Serialization that is provided by the OOP language itself. For example, in Java you can serialize an Object my making it implement Serializable interface and writing to Object Stream.


### JSON-Java library
> org.json (https://search.maven.org/classic/#search%7Cgav%7C1%7Cg%3A%22org.json%22%20AND%20a%3A%22json%22)
> Provides us with classes that are used to parse and manipulate JSON in Java.
> convert between JSON, XML, HTTP Headers, Cookies, Comma-Delimited List or Text, etc.

```java
    <dependency>
        <groupId>org.json</groupId>
        <artifactId>json</artifactId>
        <version>20180130</version>
    </dependency>
```
*version for the example*
*simple example:*

```java
    import org.json.JSONObject;
    import org.json.XML;

    JSONObject json = new JSONObject(jsonObj.toString());
    String xml = XML.toString(json);
    // xml => one row XML
```
