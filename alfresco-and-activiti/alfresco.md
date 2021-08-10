# Alfresco Community Edition

---

## Introduction

Alfresco Community Edition is an open source Enterprise Content Management (ECM) system that manages all the content within an enterprise and provides the services and controls that manage this content.
> *Alfresco Community Edition е система за управление на съдържание (данни) с отворен код и осигурява сървиси и контроли, с които да управлява съдържанието.*


At the core of the Community Edition system is a repository supported by a server that persists content, metadata, associations, and full text indexes.
> *В основата на системата Alfresco Community Edition е хранилище (repository), поддържано (подпомагано) от сървър, който съхранвяа съдържанието, мета данните, асоциации и индексите.*


Programming interfaces support multiple languages and protocols upon which developers can create custom applications and solutions.
> *Програминя интерфейс поддържа множество езици и протоколи, чрез които разработчиците, могат да създадат свои проложения и решеняи.*


Out-of-the-box applications provide standard solutions such as document management, and web content management.
> *Готовите приложения осигуряват стандартни решения като управление на документи и управление на уеб съдържание.*


As an entirely Java application, the Community Edition system runs on virtually any system that can run Java Enterprise Edition.
> *Това е JAVA приложение, което моде да върви на всяка система, която може да стартира JEE.*


At the core is the Spring platform, providing the ability to modularize functionality, such as versioning, security, and rules. 
> *Изградено е на Spring платформа, предоставяща възможността за модулиране на функционалността като версия, сигурност и правила.*


Alfresco uses scripting to simplify adding new functionality and developing new programming interfaces. 
> *Alfresco използва скриптове, за да опрости добавянето на нова функционалност и разработването на нови програмни интерфейси.*


*This portion of the architecture is known as web scripts and can be used for both data and presentation services.*
> *Тази част от архитектурата е известна като уеб скриптове и може да се използва както за данни, така и за презентационни услуги.*


## Software Architecture

At its core is a repository that provides a store for content, and a wide range of services that can be used by content applications to manipulate the content.
> *В основата му е хранилище (repo), което осигурява склад (store) за съдържание и широк спектър от услуги (services), които могат да се използват от приложенията (за съдържание) за манипулирането му.*


* The three main componets that the Content Services consist of:
    * The Platform
    * The User Interface (UI)
    * The Search engine
*These components are implemented as separate web applications.*

> * *Трите главни компонента, които Content Services се състои:*
>   * *Платформата*
>   * *Потребителския интерфейс*
>   * *Търсачката*
> *Тези компоненти са имплементирани като отделни уеб приложения.*


* The Platform
    * The main component and is implemented in the *alfresco.war* web application. 
    * It provides the repository where content is stored plus all the associated content services. 

> * *Платформата*
>   * *Главният компонент и е имплементиран в alfresco.war уеб приожението.*
>   * *Осигурява мястото, където съдържанието е съхранявано и всички свързани услуги(сървиси).*


* User Interface (UI)
    * Alfresco Share provides a web client interface for the repository and is implemented as the *share.war* web application.
    * Share makes it easy for users to manage their sites, documents, users and so on. 
  
> * *Потребителски интерфейс*
>   * *Alfresco Share предоставя уеб клиентски интерфейс за хранилището и се изпълнява като уеб приложение* share.war.
>   * *Споделянето улеснява потребителите да управляват своите сайтове, документи, потребители и т.н.*


* Share
    * The search functionality is implemented on top of Apache Solr 6 and provides the indexing of all content, which enables powerful search functionality.
    * There are also mobile clients that will access the content via ReST APIs provided by the platform.

> * *Търсене*
>   * *Функционалността за търсене е имплементирана в Apache Solr 6 и осигурява индексацията на цялото съдържание, което позволява мощна функционалност за търсене.*
>   * *Има мобилни клиенти, които достъпват съдържанието чрез ReST APIs, осигурено от "Платформата".*


The Platform and UI components run in the same Apache Tomcat web application server. 
The Search component runs in its own Jetty web application server. 
> *"Платформата" и потребителския интерфейс вървят на един и същи Tomcat уеб базиран сървър.*
> *Компонентът на търсачката върви на собствен Jetty уеб базиран сървър.*


The Platform is usually also integrated with a Directory Server (LDAP) to be able to sync users and groups with Content Services. 
And most installations also integrates with an SMTP server so the Platform can send emails, such as site invitations.
> *"Платформата" обикновено е интегрирана с LDAP, за да бъде възможно синхронизирането на потребителите и групите със сървисите.*
> *Повечето инсталации също интегрират SMTP сървър, така че "Платформата" да може да изпраща мейли и покани за сайтове (вътрешна структура).*


### Platform architecture

The platform architecture consists of the repository and related services. 
> *Архитектурата на платформата се състои от хранилището и свързаните с него услуги.* 


The platform contains the key extension points for building your own extensions.
> *Платформата съдържа ключовите точки за разширение за изграждане на собствени разширения.*


* The platform consists of:
    * All services
    * Developer extension points
    * APIs 
    * ReST API

> * *Платформата се състои от:*
>   * *Всички сървиси*
>   * *Точки за разширение от програмистите*
>   * *APIs*
>   * *ReST API*


* Install and deploy methods
    * Non-containerized deployment
        * Manual installation - This is also referred to as the zip distribution deployment, and is the most flexible way of manually deploying all the required components. This method is recommended for customers and partners who require complete control over the installation process. (https://docs.alfresco.com/content-services/community/install/zip/)
        * Ansible playbooks - Ansible playbooks provide an automated way of deploying a complete Community Edition system on “bare metal” servers, Virtual Machines, or Virtual Private Cloud instances, without the use of containers. The single playbook provided can be configured to deploy single or multi-machine instances, with a number of supported configuration options. (https://docs.alfresco.com/content-services/community/install/ansible/)
    * Containerized deployment
        * Docker Compose - Alfresco provides Docker Compose files, which are best suited for the quick deployment of development and test instances of Content Services using Docker. Customers who would like to deploy their production environments using *docker-compose* files, can extend and adapt the provided script as necessary.
        * Helm charts - Helm charts are the most common way of deploying Content Services in orchestrated Kubernetes environments. (https://docs.alfresco.com/content-services/community/install/containers/helm/)

> * *Инсталация*
>   * *Без контейнери*
>       * *Ръчно инсталиране - това се нарича още чрез zip и е най-гъвкавия начин за ръчно добавяне и развитие на задължителните компоненти. Този метод се препоръчва за потребители и партньори, които искат пълен контрол на инсталационния процес.* (https://docs.alfresco.com/content-services/community/install/zip/)
>       * *Ansible playbooks - Ansible playbooks осигурява автоматизиран начин за пълно инсталиране на Community Edition системата на “чисто метални” сървари (празни?), Виртуални машини или инстанции на виртуални частно облаци, без употребата на контейнери. Може да бъде настроено дали да настройва една или много машинни инстанции с броя на поддържаните опции.* (https://docs.alfresco.com/content-services/community/install/ansible/)
>   * *Чрез контейнери*
>       * *Docker Compose - Alfresco осигурява Docker Compose файлове, които са най-бързия начин за deployment на разработката и тестване. Потребителите, които искат да си използват техни разработки използвайки docker-compose файлове, могат да разширят скриптовете ако е нужно.*
>       * *Helm charts - Helm charts са най-често използвани, когато се оркестрира чрез Kubernetes.* (https://docs.alfresco.com/content-services/community/install/containers/helm/)



### UI architecture

The Web UI architecture consists of a number of web clients and the Application Development Framework (ADF) *ADF doesn't work because a lot of things are deprecated*.
> *Архитектурата на потрбителския уеб интерфейс се състои от редица уеб клиенти и средата за разработка на приложения на Алфреско (ADF), но не работи, защото е много остаряло и е deprecated.*

* Our Web UI architecture consists of:
    * Presentation - View and Component
    * Services - Logic
    * API
    * DB

> * *Нашата UI архитектура се състои от:*
>   * *Презентационен слой - Изгледи и компоненти*
>   * *Сървиси - Логика*
>   * *API*
>   * *DB*


From |  | To
---- | --- | --
Presentation | --> | Alfresco APIs
Presentation | --> | Services
Services | --> | Alfresco APIs
Services | --> | DB



### Integration

* Access protocols:
    * HTTP - The main protocol used to access the repository via for example the ReST APIs.
    * WebDAV - Web-based Distributed Authoring and Versioning is a set of HTTP extensions that lets you manage files collaboratively on web servers.
    * FTP - File Transfer Protocol - standard network protocol for file upload, download and manipulation. Useful for bulk uploads and downloads.
    * Alfresco Office Service - (AOS) allow you to access Content Services directly from all your Microsoft Office applications.
    * CMIS - 1.0 and 1.1 standards to allow your application to manage content and metadata in an on-premise repository.
    * IMAP - Internet Message Access Protocol - allows access to email on a remote server.
    * SMTP - It is possible to email content into the repository (InboundSMTP), a folder can be dedicated as an email target.

> * *Протоколи за достъп:*
>   * *HTTP - Главният протокол, който се използва за достъп на хранилището.*
>   * *WebDAV - Web-based Distributed Authoring and Versioning е съвкупност от HTTP разширения, които позволяват управлението на файловете на уеб сървър.*
>   * *FTP - File Transfer Protocol - стандартен мрожови протокол за качване, сваляне и манипулиране на файлове.*
>   * *Alfresco Office Service - (AOS) позволява достъп до сървисите доректно от всички Microsoft Office приложения.*
>   * *CMIS - Напълно имплементирани 1.0 и 1.1 стандарти позволяват на приложенито да управялява съдържанието, метаданните в локалното хранилище.*
>   * *IMAP - Internet Message Access Protocol - позволява достъп до мейл от външен сървър.*
>   * *SMTP - Възможно е да се изпраща съдържание по мейл към хранилището към папка, която може да бъде мишена на мейла (email target).*



## Repository

* The Alfresco Repository actually provides three APIs.
    * Repository Foundation Services - a set of local Java Interfaces covering all capabilities which are ideal for clients who wish to embed the Repository. 
    *The other two APIs are built on top of the Foundation Services, and thus take full advantage of the encapsulated content model logic and rules.*
    * Java Content Repository (JCR) API - a standard Java API for accessing Content Repositories (standardised read and write access).
    * Web Services - supports remote access and bindings to any client environment, not just Java.

> * *The Alfresco Repository осигурява 3 API-та.*
>   * *Repository Foundation Services - набор от локални Java интерфейси, обхващащи всички възможности, което е идеално за клиенти, които желаят да го маниполират локално.*
> *Другите две API-та са вдигнати върху Foundation Services(те) и това ни позволява да се използва напълно от логиката на модела на съдържанието и правилата.* 
>   * *Java Content Repository (JCR) API - е стандартно Java API достъпващо хранилищата на съдържанието (стандартен достъп за четене и писане).*
>   * *Web Services - поддържа външния достъп и  supports remote access and bindings to any client environment, not just Java.*



### Repository Deployment Options
*Alfresco Repository is designed to be deployed in a variety of environments and deployment topologies. Much of support for this comes from the Spring Framework.*
> *Хранилището е проектирано да може да се използва от много различни среди. Голяма част на подръжката идва от Spring Framework.*


#### Embedded Repository
An Embedded Repository is contained directly within a host where the host communicates with the Repository in the same process via the Repository Foundation Services.
> *Вградено хранилище се съдържа директно в хост, където хостът комуникира с хранилището в същия процес чрез Repository Foundation Services.*


Typical hosts include content-rich Client applications that require content-oriented storage, retrieval and services.
> *Типичните хостове включват клиентски приложения с много съдържание, които изискват хранилище (място) за съхранение на съдържание, извличане и услуги, ориентирани към съдържанието.*


Other hosts include test harnesses and Repository client samples.
> *Други хостове включват тестове и примери.*


These usually initialise a Repository on start-up and then close the Repository on shut-down.
> *Обикновено се инициализира хранолището при старт и се затваря при изключване.*



#### Repository Server
A Repository Server is a stand-alone server capable of servicing requests over remote protocols and providing appropriate responses.
> *Repository Server е самостоятелен сървър, способен да обслужва заявки през отдалечени протоколи и да предоставя подходящи отговори.*


A single server can support any number of different applications and clients.  New applications may be added arbitrarily.
> *Един сървър може да поддържа произволен брой различни приложения и клиенти. Нови приложения могат да се добавят произволно.*



#### Repository Application Server

The Repository is still deployed within a Web Server, but instead of exposing a set of remote protocols for client communication, a Web Application is bundled instead.
> *Хранилището все още е разположено в рамките на уеб сървър, но вместо да излага набор от отдалечени протоколи за комуникация с клиенти, вместо това се включва в уеб приложението.*


The Web Application becomes the host for an embedded Repository and remote access is via the Application i.e. HTTP.
> *Уеб приложението става хост за вградено хранилище и отдалеченият достъп е чрез приложението, т.е. HTTP.*



#### Clustered Repository Server

A Clustered Repository Server supports large numbers of requests by employing multiple processes against a single Repository store.
> *Clustered Repository Server поддържа голям брой заявки от много процеси към едно хранилище.*


Each Embedded Repository is hosted in its own Web Server and the collection as a whole (i.e. the Cluster) acts as a single Repository.
> *Всяко Embedded Repository е качено (хостнато) на собствен уеб сървър и всички заедно като Кластър, действат като едно хранилище.*



#### Hot Backup

One Repository Server is designated the Master and another completely separate Repository Server is designated the Slave.
> *Един Repository Server е Master, а друг напълно отделен е Slave.*


The live application is hosted on the Master and as it used, synchronous and asynchronous replication updates are made to the Slave i.e. the backup (read-only mode).
> *Приложението се хоства на Master-а и синхронно или асинхронно ъпдейтва Slave-а, който е само в режим на четене.*



## Apache Tika

*This is used for both metadata extraction, and content transformation.*
> *Използва се за извличане на метаданни и трансформация на съдържанието.*

* For metadata extraction:
    * it allows easy extraction of the metadata of documents and their translation into your content model.

> * *За извличане на метаданни:*
>   * *позволява лесно извличане на метаданни от документи и тяхната трянсформация в модела на съдържанието.*


* For content transformation:
    * it allows the production of plain text, HTML and XML (XHTML) versions of content.

> * *За трансформация на съдържание:*
>   * *позволява продукцията на текст - HTML и XML (XHTML) версии на съдържанието.*



### Tika and Metadata Extraction
*Many of the existing extractors in Alfresco have been converted to use Tika.*
> *Много от съществуващите extractor-и в Alfresco използват Tika.*


#### Auto Detect

The Auto-Detect parser allows the extraction of metadata from any files which are supported by Tika, but where no dedicated metadata extractor exists.
It provides a common set of mappings from Tika metadata to the Alfresco content model, which will be used across all files that are handled by the auto-detect parser fall-back.

> *Auto-Detect parser позволява извличането на метаданните от всякакви файлове, които са поддържани от Tika, където не съществува специален екстрактор на метаданни.*
> *То предоставя общ набор от съпоставяния от метаданните на Tika към Alfresco модел на съдържанието, който ще се използва във всички файлове, които се обработват от автоматичното откриване отстъпник на парсер.*


#### For full control

* Firstly:
    * You should create a new Metadata Extractor class that extends from org.alfresco.repo.content.metadata.TikaPoweredMetadataExtracter
    * Your class should register the mimetypes it handles via the contructor of the superclass, and override the getParser method to return the appropriate Tika Parser object for your file type
    * If needed, you can also override the extractSpecific method to control the mapping

* Once you have written your extractor class:
    * You need to register it with the repository
    * Configure the mappings


## Version and History of a file

Uploading a new version of a file means replacing the content and creating a new entry in the version history.
> *Качването на нова версия на файл означава подмяна на съдържанието и създаване на нов запис в историята на версиите.*


## Get file version history 
*https://docs.alfresco.com/content-services/6.0/develop/rest-api-guide/folders-files/*

When a file has versioning turned on you can get its version history.
> *Когато даден файл има включено управление на версии, можете да получите историята на версиите му.*

When we have a file with versioning turned on (i.e. the cm:versionable aspect applied) we can retrieve its version history. This will give us information about all the previous versions of the file that are available for download.
> *Когато имаме файл с включено управление на версиите, можем да извлечем неговата история на версиите. Това ще ни даде информация за всички предишни версии на файла, които са достъпни за изтегляне.*

