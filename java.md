# JAVA

## The PostConstruct 
The PostConstruct annotation is used on a method that needs to be executed after dependency injection is done to perform any initialization.
This method must be invoked before the class is put into service.
This annotation must be supported on all classes that support dependency injection.
The method annotated with PostConstruct must be invoked even if the class does not request any resources to be injected.
Only one method in a given class can be annotated with this annotation. 
The method on which the PostConstruct annotation is applied must fulfill all of the following criteria:
* The method must not have any parameters except in the case of interceptors in which case it takes an InvocationContext object as defined by the Interceptors specification.
* The method defined on an interceptor class or superclass of an interceptor class must have one of the following signatures:
   * void <METHOD>(InvocationContext)
   * Object <METHOD>(InvocationContext) throws Exception

*Note: A PostConstruct interceptor method must not throw application exceptions, but it may be declared to throw checked exceptions including the java.lang.Exception if the same interceptor method interposes on business or timeout methods in addition to lifecycle events. If a PostConstruct interceptor method returns a value, it is ignored by the container.*

* The method defined on a non-interceptor class must have the following signature:
   * void <METHOD>()
* The method on which the PostConstruct annotation is applied may be public, protected, package private or private.
* The method must not be static except for the application client.
* The method should not be final.
* If the method throws an unchecked exception the class must not be put into service except in the case where the exception is handled by an interceptor.
