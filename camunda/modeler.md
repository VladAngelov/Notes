# Camunda Modeler

## Assignments and Identity Service
*https://docs.camunda.org/manual/7.15/reference/bpmn20/tasks/user-task/*

Although the Camunda engine provides an identity management component, which is exposed through the IdentityService, it does not check whether a provided user is known by the identity component. This allows integration of the engine with existing identity management solutions when it is embedded into an application.

However, note that you can use the identity service in a service / bean or listener to query your user repository if this is useful to you.


## Authorization Service

> *Camunda allows users to authorize access to the data it manages. This makes it possible to configure which user can access which process instances, tasks etc…*
> *Authorization has a performance cost and introduces some complexity. It should only be used if it is required.*

### When is Authorization required?

> *Not every Camunda setup needs to enable authorization. In many scenarios, Camunda is embedded into an application and the application itself ensures that users can only access data they are authorized to access. Generally speaking, authorization is only required if untrusted parties interact with the process engine api directly. If you embed the process engine into a Java application, usually, you do not need to enable authorization. The application can control how the api is accessed.*

* Situations in which authorization is required:
    * Camunda Rest API is made accessible to users who should not have full access even after authentication.
    * Camunda Webapplication is made accessible to users who should not have full access even after authentication.
    * Other situations in which an untrusted user can directly construct the queries and commands executed on the process engine.
* Situations in which authorization is **not** required:
    * An application completely controls the Api methods invoked on the process engine.
    * Camunda Webapplication is made accessible to users who can have full access after authentication.


### Basic Principles

#### Authorizations
> An Authorization assigns a set of Permissions to an identity to interact with a given Resource.

#### Identities
> * Camunda BPM distinguished two types of identities: 
>    * users
>    * groups
> *Authorizations can either range over all users (userId = ANY), an individual User or a Group of users.*

#### Permissions
*A Permission defines the way an identity is allowed to interact with a certain resource.*

* The following permissions are available:
    * None - stands for “no action”
    * All - permission will vanish from a user if a single permission is revoked
    * Read
    * Update
    * Create
    * Delete
    * Access
    * Read Task
    * Update Task
    * Task Work
    * Task Assign
    * Create Instance
    * Read Instance
    * Update Instance
    * Migrate Instance
    * Delete Instance
    * Read History
    * Delete History

#### Authorization Type

* There are three types of authorizations:
    * Global Authorizations - (AUTH_TYPE_GLOBAL)
        * Range over all users and groups (userId = ANY) and are usually used for fixing the “base” permission for a resource.
    * Grant Authorizations - (AUTH_TYPE_GRANT)
        * Range over users and groups and grant a set of permissions.
        * Grant authorizations are commonly used for adding permissions to a user or group that the global authorization revokes.
    * Revoke Authorizations - (AUTH_TYPE_REVOKE)
        * Range over users and groups and revoke a set of permissions.
        * Revoke authorizations are commonly used for revoking permissions to a user or group the the global authorization grants.


#### Authorization Precedence

Authorizations may range over all users, an individual user or a group of users or they may apply to an individual resource instance or all instances of the same type (resourceId = ANY). 

* The precedence is as follows:
    * An authorization applying to an individual resource instance precedes over an authorization applying to all instances of the same resource type.
    * An authorization for an individual user precedes over an authorization for a group.
    * A Group authorization precedes over a GLOBAL authorization.
    * A Group GRANT authorization precedes over a Group REVOKE authorization.
    * A User GRANT authorization precedes over a User REVOKE authorization.


#### When are Authorizations checked?

* Authorizations are checked if:
    * the configuration option authorizationEnabled is set to true (default value is false).
    * there is a currently authenticated user.

The last item means that even if authorization is enabled, authorization checks are only performed if a user is currently authenticated. If no user is authenticated, then the engine does not perform any checks.

When using the Camunda Webapps, it is always ensured that a user is authenticated before the user can access any restricted resources. When embedding the process engine into a custom application, the application needs to take care of authentication if it needs authorization checks to be performed.


#### Permissions by Resource

This section explains which permissions are available on which resources.

#### Additional Task Permissions

A user can perform different actions on a task, like assigning the task, claiming the task or completing the task. If a user has “Update” permission on a task (or “Update Task” permission on the corresponding process definition) then the user is authorized to perform all these task actions. If finer grained authorizations are required, the permissions “Task Work” and “Task Assign” can be used. The intuition behind “Task Work” is that it only authorizes the user to work on a task (i.e., claim and complete it) but not assign it to another user or in another way “distribute work” to colleagues.

##### Default task permissions

*When a user is related to a task by being an assignee or a candidate user or a part of a candidate group or an owner, then these users get the default permission as either “Task Work” or “Update” based on the configuration setting “defaultUserPermissionNameForTask”.*

> If the “defaultUserPermissionNameForTask” is not set, then by default UPDATE permission is granted.


#### Additional Process Definition Permissions

* In Addition to Update, Read and Delete, the following permissions are available on the Process Definition Resource:
    * Read Task
    * Update Task
    * Task Work
    * Task Assign
    * Create Instance
    * Read Instance
    * Update Instance
    * Migrate Instance
    * Delete Instance
    * Read History
    * Delete History

> *The “Create Instance” permission is required for starting new process instances.*

#### Application Permissions

The resource “Application” uniquely supports the “Access” permission. The Access permission controls whether a user has access to a Camunda webapplication. 
* Out of the box, it can be granted for the following applications (resource ids):
    * admin
    * cockpit
    * tasklist
    * *(Any / All)



Example:

Department_boss_1 requires following permissions

    ‘Create_instance’ and ‘read’ permission on process definition (department 1)
    ‘Create’ permission on process instance (department 1)

You can create a group for department_1 members ‘members_department_1’

members_department_1 should have the following permission

    ‘Read’ permission on process definition (department) - If required.

when Department_1_boss assigns the task to the group ‘members_department_1’, then by default, members of department_1 can read and perform actions on those tasks.

Note: members of department_2 cannot see these tasks and processes as they dont have permission on department_1 processes or tasks