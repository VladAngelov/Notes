# Active Directory
*Active Directory is a directory service that stores information about objects on a network and makes this information available to users and network administrators.*

A directory is a hierarchical structure that stores information about objects on the network and makes this information easy for administrators and users to find and use. 
Active Directory uses a structured data store as the basis for a logical, hierarchical organization of directory information.
A directory service, such as Active Directory Domain Services (AD DS), provides the methods for storing directory data and making this data available to network users and administrators.
*For example:*
* AD DS stores information about user accounts:
    * names,
    * passwords,
    * phone numbers,
    * and so on,
    * enables other authorized users on the same network to access this information.

## Directory data store
The Active Directory directory service uses a data store for all directory information. This data store is often referred to as the directory.
* The directory contains information about objects such as:
    * users,
    * groups,
    * computers,
    * domains,
    * organizational units,
    * security policies.
*This information can be published for use by users and administrators.*

The directory is stored on domain controllers and can be accessed by network applications or services and a domain can have one or more domain controllers.
Changes made to the directory on one domain controller are replicated to other domain controllers in the domain, domain tree, or forest.

Active Directory uses four distinct directory partition types to store and copy different types of data.
Directory partitions contain domain, configuration, schema, and application data.
*Directory data is stored in the Ntds.dit file on the domain controller. ( https://docs.microsoft.com/en-us/previous-versions/orphan-topics/ws.10/cc755915(v=ws.10)?redirectedfrom=MSDN )*

* Directory data replicated between domain controllers includes the following:
    * Domain data - holds information about objects within a domain *(e-mail contacts, user and computer account attributes, and published resources that are of interest to administrators and users)*.
    * Connfiguration data - describes the topology of the directory and includes a list of all domains, trees, and forests and the locations of the domain controllers and global catalogs.
    * Schema data - is the formal definition of all object and attribute data that can be stored in the directory. (*https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc756876(v=ws.10)*)
    * Application data - Data stored in the application directory partition is intended to satisfy cases where information needs to be replicated but not necessarily on a global scale. *Application directory partitions are not part of the directory data store by default; they must be created, configured, and managed by the administrator.*

    *If a domain controller is also a global catalog, it stores a subset of the directory data for all other domains in the forest. ( https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759623(v=ws.10) )*


## Quotas and directory partitions
Quotas can help prevent the denial of service that can occur if a security principal accidentally, or intentionally, creates objects until the affected domain controller runs out of storage space.
Quotas are specified and administered for each directory partition separately, the schema partition has no quotas.

 On a given directory partition, you can assign quotas for any security principal, including users, inetOrgPersons, computers, and groups.
 Members of the Domain Admins and Enterprise Admins groups are exempt from quotas. In some cases, a security principal might be covered by multiple quotas. 
 > *For example:*
 > A user might be assigned an individual quota, and also belong to one or more security groups that also have quotas assigned to them. In such cases, the effective quota is the maximum of the quotas assigned to the security principal.

 *If a security principal is not assigned a quota either directly or through a group membership, a default quota on the partition governs the security principal.*
 *If you do not explicitly set the default quota on a given partition, the default quota of that partition is unlimited.*

## Best practices
*https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc778219(v=ws.10)*

* As a security best practice, it is recommended that you do not log on to your computer with administrative credentials.
    * When you are logged on to your computer without administrative credentials, you can use Run as to accomplish administrative tasks.

* To further secure Active Directory, it is recommended that you implement the following security guidelines:
    * Rename or disable the Administrator account (and guest account) in each domain to prevent attacks on your domains.
    * Physically secure all domain controllers in a locked room.
    * Manage the security relationship between two forests and simplify security administration and authentication across forests.
    * To provide additional protection for the Active Directory schema, remove all users from the Schema Admins group, and add a user to the group only when schema changes need to be made. Once the change has been made remove the user from the group.
    * Restrict user, group, and computer access to shared resources and to filter Group Policy settings.
    * Avoid disabling the use of signed or encrypted LDAP traffic for Active Directory administrative tools.
    * Some default user rights assigned to specific default groups may allow members of those groups to gain additional rights in the domain, including administrative rights.
    * Use global groups or universal groups instead of domain local groups when specifying permissions on domain directory objects replicated to the global catalog.

* Establish as a site every geographic area that requires fast access to the latest directory information.
    * Establishing areas that require immediate access to up-to-date Active Directory information as separate sites will provide the resources required to meet your needs.
    
* Place at least one domain controller in every site, and make at least one domain controller in each site a global catalog.
    * Sites that do not have their own domain controllers and at least one global catalog are dependent on other sites for directory information and are less efficient.

* Perform regular backups of domain controllers in order to preserve all trust relationships within that domain.