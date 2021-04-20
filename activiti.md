# Activiti

* *Sources:*
    * *GitHub: https://github.com/Activiti/Activiti*
        
        
* Required software:
    * JDK 7+        
    * Activiti development can be done with the IDE of your choice (Activiti Designer for Eclipse Kepler or Luna only).
         
* Experimental features:
    * Sections marked with [EXPERIMENTAL] should not be considered stable (documentation?).         
    * Classes as configuration values are supported and can be considered stable.
         
## Activiti database setup
    
> To run the Activiti UI app with a standalone H2 or another database the activiti-app.properties in the WEB-INF/classes/META-INF/activiti-app of the Activiti UI web application.

## Configuration
> The Activiti process engine is configured through an XML file called activiti.cfg.xml

> * The easiest way to obtain a ProcessEngine, is to use the org.activiti.engine.ProcessEngines class:
>   * ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
>   * This will look for an activiti.cfg.xml file on the classpath and construct an engine based on the configuration in that file;
>   * That configuration XML is a Spring configuration;
            
> The ProcessEngineConfiguration object can also be created programmatically using the configuration file. 
        
> * It is also possible to use a different bean id:
>    * ProcessEngineConfiguration.createProcessEngineConfigurationFromResource(String resource, String beanName);
            
> It is also possible not to use a configuration file, and create a configuration based on defaults.
        
> All ProcessEngineConfiguration.createXXX() methods return a ProcessEngineConfiguration that can further be tweaked if needed. 
        
> After calling the buildProcessEngine() operation, a ProcessEngine is created:
```java
 ProcessEngine processEngine = ProcessEngineConfiguration
                  .createStandaloneInMemProcessEngineConfiguration()
                  .setDatabaseSchemaUpdate(ProcessEngineConfiguration.DB_SCHEMA_UPDATE_FALSE)
                  .setJdbcUrl("jdbc:h2:mem:my-own-db;DB_CLOSE_DELAY=1000")
                  .setAsyncExecutorActivate(false)
                  .buildProcessEngine();
```                    
> * ProcessEngineConfiguration bean:
>   * This bean is then used to construct the ProcessEngine;
>   * There are multiple classes available that can be used to define the processEngineConfiguration;
>   * These classes represent different environments, and set defaults accordingly;
>   * org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration: the process engine is used in a standalone way;
>   * By default, the database will only be checked when the engine boots;
>   * org.activiti.engine.impl.cfg.StandaloneInMemProcessEngineConfiguration: this is a convenience class for unit testing purposes;
>   * An H2 in-memory database is used by default;
>   * The database will be created and dropped when the engine boots and shuts down;
>   * org.activiti.spring.SpringProcessEngineConfiguration: To be used when the process engine is used in a Spring environment;
>   * org.activiti.engine.impl.cfg.JtaProcessEngineConfiguration: To be used when the engine runs in standalone mode, with JTA transactions;

> * Database configuration:
>   * There are two ways to configure the database that the Activiti engine will use:
>       * The first option is to define the JDBC properties of the database:
>           * jdbcUrl: JDBC URL of the database
>           * jdbcDriver: implementation of the driver for the specific database type
>           * jdbcUsername: username to connect to the database
>           * jdbcPassword: password to connect to the database
>       * The data source that is constructed based on the provided JDBC properties will have the default MyBatis connection pool settings:
>            * jdbcMaxActiveConnections: 
>                * number of active connections maximum (default) is 10
>                * jdbcMaxCheckoutTime: 
>                    * the amount of time a connection can be checked out from the connection pool before it is forcefully returned (default) is 20000 milliseconds  (20 seconds)
>                * jdbcMaxWaitTime: 
>                    * this is a low level setting that gives the pool a chance to print a log status and re-attempt the acquisition of a connection in the case that it is taking unusually long (default) is 20000 (20 seconds)                   
>            *  javax.sql.DataSource implementation and inject it into the process engine configuration (DBCP, C3P0, Hikari, Tomcat Connection Pool, etc.);
>            * databaseType - automatically analyzed from the database connection metadata (h2, mysql, oracle, postgres, mssql, db2);
>            * databaseSchemaUpdate: allows to set the strategy to handle the database schema on process engine boot and shutdown:
>                * false (default): Checks the version of the DB schema against the library when the process engine is being created and throws an exception if the versions don’t match
>                * true: Upon building the process engine, a check is performed and an update of the schema is performed if it is necessary. If the schema doesn’t exist, it is created
>                * create-drop: Creates the schema when the process engine is being created and drops the schema when the process engine is being closed
               
                
                
### JNDI (Java Naming and Directory Interface) Datasource Configuration
> By default, the database configuration for Activiti is contained within the db.properties files in the WEB-INF/classes of each web application.
            
> By using JNDI to obtain the database connection, the connection is fully managed by the Servlet Container and the configuration can be managed outside the war deployment.
        
> * Configuration
>   * Configuration of the JNDI datasource will differ depending on what servlet container application you are using.       
>   * JNDI properties:
>       * datasource.jndi.name: the JNDI name of the Datasource;
>       * datasource.jndi.resourceRef: Set whether the lookup occurs in a J2EE container (if the prefix "java:comp/env/" needs to be added if the JNDI name doesn’t already contain it). Default is "true".

 
### Creating the database tables

> MySQL version lower than 5.6.4 has no support for timestamps or dates with millisecond precision.    
        
> * The easiest way:
>   * Add the activiti-engine jars to your classpath;
>   * Add a suitable database driver;
>   * Add an Activiti configuration file (activiti.cfg.xml) to your classpath, pointing to your database;
>   * Execute the main method of the DbSchemaCreate class;
         
### Database table names explained
    
> * The database names of Activiti all start with ACT_  
>   * ACT_RE_*: RE stands for repository - static information such as process definitions and process resources (images, rules, etc.);    
>   * ACT_RU_*: RU stands for runtime - contain the runtime data of process instances, user tasks, variables, jobs, etc. Activiti only stores the runtime data during process instance execution, and removes the records when a process instance ends;
>   * ACT_ID_*: ID stands for identity - contain identity information, such as users, groups, etc.;
>   * ACT_HI_*: HI stands for history - contain historic data, such as past process instances, variables, tasks, etc.;
>   * ACT_GE_*: general data - which is used in various use cases;
    
        
 ### Database upgrade
> Make sure you make a backup of your database (using your database backup capabilities) before you run an upgrade.
        
> To upgrade, you have to start with putting the following configuration property in your activiti.cfg.xml configuration file:
```xml
    <beans>
        <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
        <!-- ... -->
        <property name="databaseSchemaUpdate" value="true" />
        <!-- ... -->
        </bean>
    </beans>
```                 

## Job executor activation ---
> By default, the AsyncExecutor is not activated and not started. 
        
> With the following configuration the async executor can be started together with the Activiti Engine:
```xml           
    <property name="asyncExecutorActivate" value="true" />
```           
> * Mail server configuration.
>   * Configuring a mail server is optional;
        
> * History configuration.
>   * Customizing the configuration of history storage is optional;
            
> * Exposing configuration beans in expressions and scripts.
>   * By default, all beans that you specify in the activiti.cfg.xml configuration or in your own Spring configuration file are available to expressions and in the scripts;
            
> * Deployment cache configuration:
>   * All process definition are cached to avoid hitting the database every time a process definition is needed and because process definition data doesn’t change; 
>   * By default, there is no limit on this cache;
>   * To limit the process definition cache, add following property:
        ```xml
            <property name="processDefinitionCacheLimit" value="10" />
        ```
>   * For own cache implementation this must be a bean that implements the org.activiti.engine.impl.persistence.deploy.DeploymentCache interface:
        ```xml
            <property name="processDefinitionCache">
                <bean class="org.activiti.MyCache" />
            </property>
        ```
>   * There is a similar property called "knowledgeBaseCacheLimit" and "knowledgeBaseCache" for configuring the rules cache (this is only needed when you use the rules task in your processes); 
            

## Logging
    
> All logging (activiti, spring, mybatis, …​) is routed through SLF4J and allows for selecting the logging-implementation of your choice.
        
> SLF4J (Simple Logging Facade for Java) - serves as a simple facade or abstraction for various logging frameworks (e.g. java.util.logging, logback, log4j) allowing the end user to plug in the desired logging framework at deployment time.
        
> By default no SFL4J-binding jar is present in the activiti-engine dependencies, this should be added in your project in order to use the logging framework of your choice.
               
## Mapped Diagnostic Contexts
    
> Activiti supports Mapped Diagnostic Contexts feature of SLF4j.
        
> * Basic information (none of these information are logged by default):
>   * processDefinition Id as mdcProcessDefinitionID;
>   * processInstance Id as mdcProcessInstanceID;
>   * execution Id as mdcExecutionId;
    

## Event handlers
    
> *The event mechanism in the Activiti engine allows you to get notified when various events occur within the engine.*

> *It’s possible to register a listener for certain types of events as opposed to getting notified when any type of event is dispatched.*
        
> * Event listener implementation
>   * The only requirement an event-listener has, is to implement org.activiti.engine.delegate.event.ActivitiEventListener.
            
> * Configuration and setup
>   * If an event-listener is configured in the process engine configuration, it will be active when the process engine starts and will remain active after subsequent reboots of the engine.
>   * The order of dispatching events is determined on the order the listeners were added.
            
> * Adding listeners at runtime.
>   * It’s possible to add and remove additional event-listeners to the engine by using the API (RuntimeService).
>   * The listeners added at runtime are not retained when the engine is rebooted.
            
> * Adding listeners to process definitions.
>   * It’s possible to add listeners to a specific process-definition. 
>   * The listeners will only be called for events related to the process definition and to all events related to process instances that are started with that specific process definition.
>   * For events related to entities, it’s also possible to add listeners to a process-definition that get only notified when entity-events occur for a certain entity type.
>   * Supported values for the entityType are: 
>       * attachment;
>       * comment;
>       * execution;
>       * identity-link;
>       * job;
>       * process-instance; 
>       * process-definition;
>       * task;
                
> * Dispatching events through API.
>   * The event-dispatching mechanism through the API, allow to dispatch custom events to any listeners that are registered in the engine.    
            
> * Supported event types
>   * Each type corresponds to an enum value in the org.activiti.engine.delegate.event.ActivitiEventType.    
            
> * Additional remarks.
>   * Only listeners are notified in the engine the events are dispatched from (only events that originated in the engine the listener is registered for).    
            
                  
## The Activiti API
        
> * The Process Engine API and services.
>   * The engine API is the most common way of interacting with Activiti (starting point is the ProcessEngine).
>       * ProcessEngines.getDefaultProcessEngine(); - will initialize and build a process engine the first time it is called and afterwards always return the same process engine;
>       * Proper creation of all process engines can be done with ProcessEngines.init(); 
>       * Proper closing of all process engines can be done with ProcessEngines.destroy();
*Ex.:*
```java
    ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
    RuntimeService runtimeService = processEngine.getRuntimeService();
    RepositoryService repositoryService = processEngine.getRepositoryService();
    TaskService taskService = processEngine.getTaskService();
    ManagementService managementService = processEngine.getManagementService();
    IdentityService identityService = processEngine.getIdentityService();
    HistoryService historyService = processEngine.getHistoryService();
    FormService formService = processEngine.getFormService();
    DynamicBpmnService dynamicBpmnService = processEngine.getDynamicBpmnService();
```
> * Exception strategy.
>   * The base exception in Activiti is the org.activiti.engine.ActivitiException, an unchecked exception.
            
> * Working with the Activiti services.
>   * The way to interact with the Activiti engine is through the services exposed by an instance of the org.activiti.engine.ProcessEngine class.
>   * To make this process known to the Activiti engine, we must deploy it first.
> *Ex.:*
```java
    ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
    RepositoryService repositoryService = processEngine.getRepositoryService();
    repositoryService.createDeployment()
        .addClasspathResource("org/activiti/test/VacationRequest.bpmn20.xml")
        .deploy();
```     
> * Starting a process instance.
>   * After deploying the process definition to the Activiti engine, we can start new process instances from it.
>   * Everything related to the runtime state of processes can be found in the RuntimeService.
            
> * Completing tasks.
>   * When the process starts, the first step will be a user task. 
>   * To continue the process instance, we need to finish this task ( taskService.complete(task.getId(), taskVariables); ).
*Ex.:*
```java
    // Fetch all tasks for the management group
    TaskService taskService = processEngine.getTaskService();
    List<Task> tasks = taskService.createTaskQuery().taskCandidateGroup("management").list();
    for (Task task : tasks) {
        Log.info("Task available: " + task.getName());
    }
```                 
> * Suspending and activating a process.
>   * It’s possible to suspend a process definition.
>   * Suspending the process definition is done through the RepositoryService.
            
                    
## Query API
> *There are two ways of querying data from the engine: The query API and native queries.*
        
> Query API (all of which are applied together as a logical AND):
*Ex.:*
```java
    List<Task> tasks = taskService.createTaskQuery()
        .taskAssignee("kermit")
        .processVariableValueEquals("orderId", "0815")
        .orderByDueDate().asc()
        .list();            
```     

> Native queries:
*Ex.:*
```java
    List<Task> tasks = taskService.createNativeTaskQuery()
        .sql("SELECT count(*) FROM " + managementService.getTableName(Task.class) + " T WHERE T.NAME_ = #{taskName}")
        .parameter("taskName", "gonzoTask")
        .list();

    long count = taskService.createNativeTaskQuery()
        .sql("SELECT count(*) FROM " + managementService.getTableName(Task.class) + " T1, "
        + managementService.getTableName(VariableInstanceEntity.class) + " V1 WHERE V1.TASK_ID_ = T1.ID_")
        .count();
```            
            
### Variables        
> Every process instance needs and uses data (variables in Activiti) to execute the steps it exists of, which are stored in the database.
        
> Variables are often used in Java delegates, expressions, execution- or tasklisteners, scripts, etc.
                      
            
### Transient variables
> Transient variables are variables that behave like regular variables, but are not persisted.
        
> * The following applies for transient variables:
>   * There is no history stored at all for transient variables.
>   * Transient variables are put on the highest parent when set( variable is actually stored on the process instance execution).
>   * A local variant of the method exists if the variable should be set on the specific execution or task.
>   * A transient variable can only be accessed until the next wait state in the process definition.
>   * Transient variables can only be set by the setTransientVariable(name, value).
>   * Transient variables are also returned when calling getVariable(name).
>   * A transient variable shadows a persistent variable with the same name - getVariable("someVariable") is used, the transient variable value will be returned.


### Expressions
        
> *Activiti uses UEL (Unified Expression Language) for expression-resolving.*

### Unit testing    
> *Activiti supports both JUnit versions 3 and 4 styles of unit testing.*
        
### The process engine in a web application 
> *It is possible to create the process engine once when the container boots and shut down the engine when the container goes down.*
 
 
## Spring integration
    
### ProcessEngineFactoryBean
        
> The ProcessEngine can be configured as a regular Spring bean.
        
> That bean takes a process engine configuration and creates the process engine.      
            
> * Expressions
>   * When using the ProcessEngineFactoryBean, by default, all expressions in the BPMN processes will also see all the Spring beans.
                
> * Automatic resource deployment
>   * In the process engine configuration, you can specify a set of resources.
>   * By default, the configuration above will group all of the resources matching the filtering into a single deployment to the Activiti engine. 
>   * There are 3 values that are supported by default for this property:
>       * default: Group all resources into a single deployment and apply duplicate filtering to that deployment. 
>       * single-resource: Create a separate deployment for each individual resource and apply duplicate filtering to that deployment. 
>       * resource-parent-folder: Create a separate deployment for resources that share the same parent folder and apply duplicate filtering to that deployment. 
            
> * JPA with Hibernate 4.2.x
>   * When using Hibernate 4.2.x JPA in service task or listener logic in the Activiti Engine an additional dependency to Spring ORM is needed.
        
 
## Deployment
### Business archives
> To deploy processes, they have to be wrapped in a business archive. 

> A business archive is the unit of deployment to an Activiti Engine.
    
    
## BPMN 2.0
> BPMN stands for Business Process Modeling Notation. 
> It's a specification managed by the Object Management Group (OMG) that defines exactly what the name suggests - a standard syntax for describing business processes.             
> The specification is aimed at both humans (i.e., graphical notation of a business process) and machines (i.e., XML syntax describing the business process).
> * The two main benefits of using a BPMN-compliant workflow engine are:
>   * When business analysts collaborate on business process projects, they can use a common diagram and language to discuss the process, regardless of the tool that is ultimately used to implement it;
>   * Any tool that produces BPMN-compliant XML can be used to define a business process and the resulting XML should theoretically work with any compliant workflow engine.
                
### BPMN 2.0 Constructs
    
> * Events
>   * Events are always visualized as a circle.
>   * Categories: catching or throwing event.
>   * Catching: when process execution arrives in the event, it will wait for a trigger to happen;
>   * Throwing: when process execution arrives in the event, a trigger is fired;
            
> * Timer Event Definitions
>   * Timer events are events which are triggered by defined timer.
>   * They can be used as start event, intermediate event or boundary event. 
>   * The behavior of the time event depends on the business calendar used.
>   * Every timer event has a default business calendar, but the business calendar can also be defined on the timer event definition.
>   * Timer definition must have exactly one element from the following:
*timeDate (ISO 8601 format), ex.:*
```xml
    <timerEventDefinition>
        <timeDate>2011-03-11T12:13:14</timeDate>
    </timerEventDefinition>
```
    
### Error Event Definitions
> A BPMN error is NOT the same as a Java exception.

> BPMN error events are a way of modeling business exceptions.    

### Signal Event Definitions
        
> Signal events are events which reference a named signal. 

> A signal is an event of global scope and is delivered to all active handlers.

> A signal event definition is declared using the signalEventDefinition element.

> The signalEventDefinitions reference the same signal element.
        
> Throwing a Signal Event.
> A signal can either be thrown by a process instance using a BPMN construct or programmatically using java API.
> org.activiti.engine.RuntimeService can be used to throw a signal programmatically:
*Ex.:*
```java
    RuntimeService.signalEventReceived(String signalName); // throws the signal globally to all subscribed handlers
    RuntimeService.signalEventReceived(String signalName, String executionId); //  delivers the signal to a specific execution only
```             
> * Catching a Signal Event.
>   * A signal event can be caught by an intermediate catch signal event or a signal boundary event.
            
> * Querying for Signal Event subscriptions.
>   * It is possible to query for all executions which have subscribed to a specific signal event:
*Ex.:*
```java      
    List<Execution> executions = runtimeService.createExecutionQuery()
        .signalEventSubscriptionName("alert")
        .list();
```
> We could then use the signalEventReceived(String signalName, String executionId) method to deliver the signal to these executions.
        
> * Signal event scope.
>   * The default value for this is attribute is "global".
>   * A signal event is broadcast to all active handlers - all instances of the process catching the signal would receive the event.

> * Message Event Definitions.
>   * Message events are events which reference a named message.
>   * A message event definition is declared using the messageEventDefinition element.
            
            
            
## Options for implementing workflow in your apps   
> Roll your own: option is really not recommended for any but the most basic workflow requirements.
        
> * Standalone engines: 
>   * BPM (Business Process Management) - open source and proprietary;
>   * Standalone engines are most appropriate for extremely high volume or exceedingly complex solutions involving multiple systems. 
>   * Good use for standalone engines is when you are developing a custom application that has workflow requirements. 
>   * Standalone engines can usually talk to any database or content management repository you might have implemented, but they won't be as tightly integrated into the content management system's user interface as the workflow engine built-in to the CMS. 
>   * For content-centric solutions that operate mostly within the scope of the CMS, it is usually less complicated (and less costly) to use the workflow engine embedded within the CMS, provided it has enough functionality to meet the business' workflow requirements.
            
> * Embedded workflow engines: 
>   * The major benefit of leveraging an embedded workflow engine is the tight level of integration for users as well as developers. 
>   * Users can initiate and interact with workflows without leaving the CMS client.
>   * Developers customizing or extending the CMS can work with workflows using the core CMS API.
           
            
## Out-of-the-box workflow options in Alfresco
> *For very simplistic workflows: non-technical end-users can leverage Alfresco's Basic Workflow functionality.*

> *For more complex: Alfresco has Advanced Workflow functionality.*

> * Basic workflows:
>   * Basic workflows are a nice end-user tool.
>   * Workflow use folders and a "forward step/backward step" model.
>   * Basic workflows are limited with regard to the complexity of the business processes they can handle.
    
> * Advanced workflows
>    * Advanced workflows are useful when you need much more control over the business process. 
>    * Typical requirements that require the use of the advanced workflow engine are things like (features of the embedded Activiti workflow engine):
>       * Assignment of tasks to specific users or groups;
>       * Parallel paths through the business process;
>       * Decisions based on content metadata or other criteria;
>       * The ability to handle exceptions;
>       * Timers;

> * Difference between basic and advanced workflows
>   * Alfresco basic workflows:
>       * Are configurable by non-technical end-users via Alfresco Share;
>       * Leverage rules, folders, and actions;
>       * Can only handle processes with single-step, forward and/or backward, serial flows;
>       * Do not support decisions, splits, joins, or parallel flows;
>       * Do not maintain state or metadata about the process itself;
>       * Do not support the concept of task assignment;
>   * Alfresco advanced workflows:
>       * Are defined by business analysts and developers using a graphical tool or by writing XML;
>       * Leverage the power of the embedded Activiti workflow engine;
>       * Can model any business process including decisions, splits, joins, parallel flows, sub-processes, wait states, and timers;
>       * Can include business logic written either in JavaScript or Java, either of which can access the Alfresco API;
>       * Maintain state and process variables (metadata) about the process itself;
>       * Support assigning tasks to individuals, groups, and pools of users;


## Activiti Concepts
       
> * What is Activiti?
>    * Activiti is an open source, standalone workflow engine. 
>    * It can run in a servlet container or it can be embedded in any Java application.
>    * The Activiti engine is responsible for:
>       * managing deployed processes;
>       * instantiating and executing processes;
>       * persisting process state and metadata to a relational database;
>       * tracking task assignment and task lists;