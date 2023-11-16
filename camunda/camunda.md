# Camunda

## Process Engine & Infrastructure

### Process Engine

* The process engine is a Java library responsible for executing BPMN 2.0 processes, CMMN 1.1 cases and DMN 1.3 decisions. 		
* It has a lightweight POJO core and uses a relational database for persistence.
* ORM mapping is provided by the MyBatis mapping framework.


### Spring Framework Integration
	   
* The camunda-engine Spring framework integration is located inside the camunda-engine-spring maven module and can be added to apache maven-based projects.
		
* Process Engine Configuration
	* You can use a Spring application context XML file to bootstrap the process engine. 
	* *#It* is possible to bootstrap both application-managed and container-managed process engines through Spring.			
*  Configure an Application-Managed Process Engine
	* The ProcessEngine can be configured as a regular Spring bean;
		* CDI and Java EE Integration	
*  The camunda-engine-cdi module provides programming model integration with CDI (Context and Dependency Injection).	   
* CDI is the Java EE 6 standard for Dependency Injection. The camunda-engine-cdi integration leverages both the configuration of the Camunda engine and the extensibility of CDI.
	* A custom El-Resolver for resolving CDI beans (including EJBs) from the process;
	* Support for @BusinessProcessScoped beans (CDI beans, the lifecycle of which are bound to a process instance);
	* Declarative control over a process instance using annotations;
	* The Process Engine is hooked-up to the CDI event bus;
	* Works with both Java EE and Java SE;
	* Support for unit testing;
	* Runtime Container Integration	
* BPM Platform Services

*To inspect the current state of configured process engines and deployed process applications, the class org.camunda.bpm.BpmPlatform offers access to the ProcessEngineService and the ProcessApplicationService.*

* ProcessEngineService  
	* It can be accessed by calling BpmPlatform.getProcessEngineService(). 
	* It offers access to the default process engine, as well as any process engine by its name as specified in the process engine configuration. It returns ProcessEngine objects from which any services for a specific engine can be accessed;                                	 
* ProcessApplicationService
	* The ProcessApplicationService is accessible via BpmPlatform.getProcessApplicationService();
	* It provides details on the process application deployments made on the application server it is running on. That means that it does not provide a global view across all nodes in a cluster;
	* Given a process application name, a ProcessApplicationInfo object can be retrieved that contains details on the deployments made by this process application. These correspond to the process archives declared in processes.xml;
	* Application-specific properties can be retrieved such as the servlet context path in case of a servlet process application;
* JNDI Bindings for BPM Platform Services
	* The BPM Platform Services (Process Engine Service and Process Application Service) are provided via JNDI Bindings with the following JNDI names:
		* Process Engine Service: java:global/camunda-bpm-platform/process-engine/ProcessEngineService!org.camunda.bpm.ProcessEngineService
		* Process Application Service: java:global/camunda-bpm-platform/process-engine/ProcessApplicationService!org.camunda.bpm.ProcessApplicationService
	* On JBoss AS 7 and Wildfly, you are able get any of these BPM Platform Services through a JNDI lookup;
	* On Apache Tomcat 8 you have to do quite a bit more to be able to do a lookup to get one of these BPM Platform Services;


###  Apache Tomcat 7 Integration

* JNDI Bindings
	* To use the JNDI Bindings for BPM Platform Services on Apache Tomcat 7 you have to add the file META-INF/context.xml to your process application and add the resource links;
	* Furthermore, declare the dependency on the JNDI binding inside the WEB-INF/web.xml deployment descriptor.			
* Job Executor Configuration    
* Tomcat Default Job Executor
	* The BPM platform on Apache Tomcat 7.x uses the default job executor;
	* The default job executor uses a ThreadPoolExecutor which manages a thread pool and a job queue;
	* The core pool size, queue size, maximum pool size and keep-alive-time can be configured in the bpm-platform.xml;
	* After configuring job acquisition, it is possible to set the values with the help of a *< properties>* tag;
* Core Pool Size
	* The ThreadPoolExecutor automatically adjusts the size of the thread pool;
	* The default core pool size is 3;
*  Queue Size
	* The ThreadPoolExecutor includes a job queue for buffering jobs;
	* The default maximum length of the job queue is 3;
*  Maximum Pool Size
	* The default maximum pool size is 10;				
*  KeepAlive
	* The default keepalive time is 0;					
* Clustered Deployment
	* In a clustered deployment, multiple job executors will work with each other;
	* Each job executor allocates a UUID which is used for identifying locked job ownership in the job table;    
* The Camunda JBoss/WildFly Subsystem              
	* Deploy the process engine as shared JBoss module;
	* Configure the process engine in standalone.xml / domain.xml and administer it though the JBoss Management System;
	* Process Engines are native JBoss Services with service lifecycles and dependencies;
	* Automatic deployment of BPMN 2.0 processes (through the Process Application API);
	* (JBoss AS 7 only) - Use a managed Thread Pool provided by JBoss Threads in combination with the Job Executor;
	* (Wildfly only) - Use a managed Thread Pool for the Job Executor configured through the Camunda BPM Subsystem;
* Configure the Job Executor in [standalone](standalone.xml/domain.xml ) *(Wildfly only)*
	* On JBoss AS 7, the thread pool is configured through the [JBoss Threads subsystem](https://docs.camunda.org/manual/7.14/installation/full/jboss/manual/)
	* Since Camunda BPM 7.5, the configuration of the thread pool used by the Job Executor is done in the Camunda subsystem, not in the JBoss Threads subsystem, as it was done before 7.5;
* Configure a Process Engine in [standalone](standalone.xml/domain.xml)
	* Using the Camunda JBoss/Wildfly subsystem, it is possible to configure and manage the process engine through the JBoss Management Model;
* Provide a Custom Process Engine Configuration Class
	* It is possible to provide a custom Process Engine Configuration class on JBoss AS 7 and Wildfly Application Server;
	* The properties map can be used for invoking primitive valued setters (Integer, String, Boolean) that follow the Java Bean conventions;				
* Extend a Process Engine Using Process Engine Plugins
	* It is possible to extend a process engine using the process engine plugins concept;		
*  Using System Properties
	* To externalize environment specific parts of the configuration, it is possible to reference system properties using Ant-style expressions (i.e., ${PROPERTY_KEY});			
* Look Up a Process Engine in JNDI
	* The Camunda JBoss/Wildfly subsystem provides the same JNDI bindings for the ProcessApplicationService and the ProcessEngineService as provided on other containers;
* Manage the Process Engine Through the JBoss Management System
	* To inspect and change the management model, we can use one of the multiple [JBoss Management](https://docs.jboss.org/author/index.html) clients available;            
* Inspect the Configuration
	* It is possible to inspect the configuration using the CLI ([Command Line Interface](jboss-cli.bat/sh));
* Stop the Process Engine Through the JBoss Management System
	* Once the process engine is registered in the JBoss Management Model, it is possible to control it through the management API;        	
* Start the Process Engine Through the JBoss Management System
	* It is also possible to start a new process engine at runtime;
* Use the JBoss JConsole Extensions
	* In some cases, you may find it more convenient to use the JBoss JConsole extension for starting a process engine;
	* The JConsole plugin allows you to inspect the management model graphically and build operations using a wizard;
* Manage Classpath Dependencies
	* When using the Camunda JBoss/Wildfly subsystem, the process engine classes are deployed as JBoss module;
	* By default, the application server will not add this module to the classpath of applications;
* Implicit Module Dependencies with the Process Application API
	* When using the Process Application API, the Camunda JBoss/Wildfly subsystem will detect the @ProcessApplication class in the deployment and automatically add a module dependency between the application and the process engine module.
	* As a result, we don’t have to declare the dependency ourselves.  			 
* Explicit Module Dependencies
	 * If an application does not use the Process Application API but still needs the process engine classes to be added to its classpath, an explicit module dependency is required;
	 * The Camunda JBoss/Wildfly subsystem manages process engines as JBoss Services in the JBoss Module Service Container;
* Implicit Service Dependencies
	* When using the Process Application API, the Camunda JBoss/Wildfly subsystem will detect the @ProcessApplication class in the deployment and automatically add a service dependency between the process application component and the process engine module;
	* This ensures that the process engine is available when the process application is deployed;			
* Explicit Service Dependencies
	* If an application does not use the Process Application API but still needs to interact with a process engine, it is important to declare the dependency on the Process Engine Service explicitly;
 * Job Execution with Managed Resources
* ManagedJobExecutor
	* Integration into application servers without a resource-aware implementation is offered by a specific type of JobExecutor called the ManagedJobExecutor;
	* The purpose of the ManagedJobExecutor is to ensure that job execution within the process engine is correctly controlled by the application server, by using managed resources (primarily: managed threads);
	
*For supported environments, Camunda BPM provides server modules that integrate the Job Execution with the application server’s managed threadpools.*


## Camunda BPM Run
   
*  Camunda BPM Run is a pre-packaged distro of the Camunda BPM platform, including the Camunda webapps (Cockpit, Tasklist, Admin) and the REST API.
* Enterprise


## Service Task
*https://docs.camunda.org/manual/7.14/reference/bpmn20/tasks/service-task/*

*  A Service Task is used to invoke services.
* In Camunda this is done by calling Java code or providing a work item for an external worker to complete asynchronously or invoking a logic which is implemented in form of webservices.
* Calling Java Code
	* Specifying a class that implements a JavaDelegate or ActivityBehavior
		* To specify a class that is called during process execution, the fully qualified classname needs to be provided by the camunda:class attribute:
```xml
	<serviceTask id="javaService" name="My Java Service Task" camunda:class="org.camunda.bpm.MyJavaDelegate"/>
```

* Evaluating an expression that resolves to a [delegation object](https://docs.camunda.org/manual/7.14/user-guide/process-engine/delegation-code/#java-delegate)
* Invoking a method [expression](https://docs.camunda.org/manual/7.14/user-guide/process-engine/expression-language/#use-expression-language-as-delegation-code)
* Evaluating a value expression;                    
	* It is possible to invoke logic which is implemented in form of webservices. camunda:connector is an extension that allows calling REST/SOAP APIs directly from the workflow;
*  Service Task Results
	* The return value of a service execution (for a Service Task exclusively using expressions) can be assigned to an already existing or to a new process variable by specifying the process variable name as a literal value for the camunda:resultVariable attribute of a Service Task definition;
*  External Tasks
	* In contrast to calling Java code, where the process engine synchronously invokes Java logic, it is possible to implement a Service Task outside of the process engine’s boundaries in the form of an external task;    
