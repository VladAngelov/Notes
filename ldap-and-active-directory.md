# LDAP and Active Directory

## LDAP
> *Linux*

The Lightweight Directory Access Protocol is an open, vendor-neutral, industry standard application protocol for accessing and maintaining distributed directory information services over an Internet Protocol (IP) network. **Directory services** play an important role in developing intranet and Internet applications by allowing the sharing of information about users, systems, networks, services, and applications throughout the network.

LDAP is specified in a series of **Internet Engineering Task Force** (IETF) Standard Track publications called Request for Comments (RFCs), using the description language **ASN.1**.

A common use of LDAP is to provide a central place to store usernames and passwords. This allows many different applications and services to connect to the LDAP server to validate users.

LDAP is based on a simpler subset of the standards contained within the **X.500** standard. Because of this relationship, LDAP is sometimes called X.500-lite.

* LDAP Syntax
    * CN=Bob,OU=Users,DC=Youtube,DC=Com
    * CN = Canonical Name (object or name)
    * OU = Organizational Unit (Folder in Active directory)
    * DC = Domain Controller (Where it is)

### Directory Service
In computing, a directory service or name service maps the names of network resources to their respective network addresses. It is a shared information infrastructure for locating, managing, administering and organizing everyday items and network resources, which can include volumes, folders, files, printers, users, groups, devices, telephone numbers and other objects. A directory service is a critical component of a network operating system. A directory server or name server is a server which provides such a service. Each resource on the network is considered an object by the directory server. Information about a particular resource is stored as a collection of attributes associated with that resource or object.

A directory service defines a namespace for the network. The namespace is used to assign a name (unique identifier) to each of the objects. Directories typically have a set of rules determining how network resources are named and identified, which usually includes a requirement that the identifiers be unique and unambiguous. When using a directory service, a user does not have to remember the physical address of a network resource; providing a name locates the resource. Some directory services include access control provisions, limiting the availability of directory information to authorized users. 

### The Internet Engineering Task Force 
IETF is an open standards organization, which develops and promotes voluntary Internet standards, in particular the standards that comprise the Internet protocol suite (TCP/IP).

### ASN.1
Abstract Syntax Notation One (ASN.1) is a standard interface description language for defining data structures that can be serialized and deserialized in a cross-platform way. It is broadly used in telecommunications and computer networking, and especially in cryptography.

Protocol developers define data structures in ASN.1 modules, which are generally a section of a broader standards document written in the ASN.1 language. The advantage is that the ASN.1 description of the data encoding is independent of a particular computer or programming language. 

### X.500
X.500 is a series of computer networking standards covering electronic directory services.


## Active Directory
> *Windows*

Active Directory (AD) is Microsoft's proprietary directory service. It runs on Windows Server and allows administrators to manage permissions and access to network resources.

Active Directory stores data as objects. An object is a single element, such as a user, group, application or device, e.g., a printer. Objects are normally defined as either resources, such as printers or computers, or security principals, such as users or groups.

Active Directory categorizes directory objects by name and attributes.

Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks. It is included in most Windows Server operating systems as a set of processes and services.
Initially, Active Directory was only in charge of centralized domain management. 
However, Active Directory became an umbrella title for a broad range of directory-based identity-related services.

A server running the Active Directory Domain Service (AD DS) role is called a domain controller. It authenticates and authorizes all users and computers in a Windows domain type network. Assigning and enforcing security policies for all computers and installing or updating software.

Active Directory uses Lightweight Directory Access Protocol (LDAP) versions 2 and 3, Microsoft's version of Kerberos and DNS.
