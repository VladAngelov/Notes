# Alfresco Community Edition


## Introduction

> Alfresco Community Edition is an open source Enterprise Content Management (ECM) system that manages all the content within an enterprise and provides the services and controls that manage this content.

> *Alfresco Community Edition е система за управление на съдържание (данни) с отворен код и осигурява сървиси и контроли, с които да управлява съдържанието.*

> At the core of the Community Edition system is a repository supported by a server that persists content, metadata, associations, and full text indexes.

> *В основата на системата Alfresco Community Edition е хранилище (repository), поддържано (подпомагано) от сървър, който съхранвяа съдържанието, мета данните, асоциации и индексите.*

> Programming interfaces support multiple languages and protocols upon which developers can create custom applications and solutions.

> *Програминя интерфейс поддържа множество езици и протоколи, чрез които разработчиците, могат да създадат свои проложения и решеняи.*

> Out-of-the-box applications provide standard solutions such as document management, and web content management.

> *Готовите приложения осигуряват стандартни решения като управление на документи и управление на уеб съдържание.*

> As an entirely Java application, the Community Edition system runs on virtually any system that can run Java Enterprise Edition.

> *Това е JAVA приложение, което моде да върви на всяка система, която може да стартира JEE.*

> At the core is the Spring platform, providing the ability to modularize functionality, such as versioning, security, and rules. 

> *Изградено е на Spring платформа, предоставяща възможността за модулиране на функционалността като версия, сигурност и правила.*

> Alfresco uses scripting to simplify adding new functionality and developing new programming interfaces. 

> *Alfresco използва скриптове, за да опрости добавянето на нова функционалност и разработването на нови програмни интерфейси.*

> *This portion of the architecture is known as web scripts and can be used for both data and presentation services.*

> *Тази част от архитектурата е известна като уеб скриптове и може да се използва както за данни, така и за презентационни услуги.*


## Software Architecture

> At its core is a repository that provides a store for content, and a wide range of services that can be used by content applications to manipulate the content.
> *В основата му е хранилище (repo), което осигурява склад (store) за съдържание и широк спектър от услуги (services), които могат да се използват от приложенията (за съдържание) за манипулирането му.*

> * The three main componets that the Content Services consist of:
>   * The Platform
>   * The User Interface (UI)
>   * The Search engine
> *These components are implemented as separate web applications.*

> * *Трите главни компонента, които Content Services се състои:*
>   * *Платформата*
>   * *Потребителския интерфейс*
>   * *Търсачката*
> *Тези компоненти са имплементирани като отделни уеб приложения.*

> * The Platform
>   * The main component and is implemented in the *alfresco.war* web application. 
>   * It provides the repository where content is stored plus all the associated content services. 

> * *Платформата*
>   * *Главният компонент и е имплементиран в alfresco.war уеб приожението.*
>   * *Осигурява мястото, където съдържанието е съхранявано и всички свързани услуги(сървиси).*

> * User Interface (UI)
>   * Alfresco Share provides a web client interface for the repository and is implemented as the *share.war* web application.
>   * Share makes it easy for users to manage their sites, documents, users and so on. 
  
> * *Потребителски интерфейс*
>   * *Alfresco Share предоставя уеб клиентски интерфейс за хранилището и се изпълнява като уеб приложение* share.war.
>   * *Споделянето улеснява потребителите да управляват своите сайтове, документи, потребители и т.н.*

> * Share
>   * The search functionality is implemented on top of Apache Solr 6 and provides the indexing of all content, which enables powerful search functionality.
>   * There are also mobile clients that will access the content via ReST APIs provided by the platform.

> * *Търсене*
>   * *Функционалността за търсене е имплементирана в Apache Solr 6 и осигурява индексацията на цялото съдържание, което позволява мощна функционалност за търсене.*
>   * *Има мобилни клиенти, които достъпват съдържанието чрез ReST APIs, осигурено от "Платформата".*

> The Platform and UI components run in the same Apache Tomcat web application server. 
> The Search component runs in its own Jetty web application server. 

> *"Платформата" и потребителския интерфейс вървят на един и същи Tomcat уеб базиран сървър.*
> *Компонентът на търсачката върви на собствен Jetty уеб базиран сървър.*

> The Platform is usually also integrated with a Directory Server (LDAP) to be able to sync users and groups with Content Services. 
> And most installations also integrates with an SMTP server so the Platform can send emails, such as site invitations.

> *"Платформата" обикновено е интегрирана с LDAP, за да бъде възможно синхронизирането на потребителите и групите със сървисите.*
> *Повечето инсталации също интегрират SMTP сървър, така че "Платформата" да може да изпраща мейли и покани за сайтове (вътрешна структура).*


### Platform architecture

> The platform architecture consists of the repository and related services. 

> *Архитектурата на платформата се състои от хранилището и свързаните с него услуги.* 

> The platform contains the key extension points for building your own extensions.

> *Платформата съдържа ключовите точки за разширение за изграждане на собствени разширения.*

> * The platform consists of:
>   * All services
>   * Developer extension points
>   * APIs 
>   * ReST API

> * *Платформата се състои от:*
>   * *Всички сървиси*
>   * *Точки за разширение от програмистите*
>   * *APIs*
>   * *ReST API*

> * Install and deploy methods
>   * Non-containerized deployment
>       * Manual installation - This is also referred to as the zip distribution deployment, and is the most flexible way of manually deploying all the required components. This method is recommended for customers and partners who require complete control over the installation process. (https://docs.alfresco.com/content-services/community/install/zip/)
>       * Ansible playbooks - Ansible playbooks provide an automated way of deploying a complete Community Edition system on “bare metal” servers, Virtual Machines, or Virtual Private Cloud instances, without the use of containers. The single playbook provided can be configured to deploy single or multi-machine instances, with a number of supported configuration options. (https://docs.alfresco.com/content-services/community/install/ansible/)
>   * Containerized deployment
>       * Docker Compose - Alfresco provides Docker Compose files, which are best suited for the quick deployment of development and test instances of Content Services using Docker. Customers who would like to deploy their production environments using *docker-compose* files, can extend and adapt the provided script as necessary.
>       * Helm charts - Helm charts are the most common way of deploying Content Services in orchestrated Kubernetes environments. (https://docs.alfresco.com/content-services/community/install/containers/helm/)

> * *Инсталация*
>   * *Без контейнери*
>       * *Ръчно инсталиране - това се нарича още чрез zip и е най-гъвкавия начин за ръчно добавяне и развитие на задължителните компоненти. Този метод се препоръчва за потребители и партньори, които искат пълен контрол на инсталационния процес.* (https://docs.alfresco.com/content-services/community/install/zip/)
>       * *Ansible playbooks - Ansible playbooks осигурява автоматизиран начин за пълно инсталиране на Community Edition системата на “чисто метални” сървари (празни?), Виртуални машини или инстанции на виртуални частно облаци, без употребата на контейнери. Може да бъде настроено дали да настройва една или много машинни инстанции с броя на поддържаните опции.* (https://docs.alfresco.com/content-services/community/install/ansible/)
>   * *Чрез контейнери*
>       * *Docker Compose - Alfresco осигурява Docker Compose файлове, които са най-бързия начин за deployment на разработката и тестване. Потребителите, които искат да си използват техни разработки използвайки docker-compose файлове, могат да разширят скриптовете ако е нужно.*
>       * *Helm charts - Helm charts са най-често използвани, когато се оркестрира чрез Kubernetes.* (https://docs.alfresco.com/content-services/community/install/containers/helm/)


### UI architecture

> The Web UI architecture consists of a number of web clients and the Application Development Framework (ADF) *ADF doesn't work because a lot of things are deprecated*.

> *Архитектурата на потрбителския уеб интерфейс се състои от редица уеб клиенти и средата за разработка на приложения на Алфреско (ADF), но не работи, защото е много остаряло и е deprecated.*

> * Our Web UI architecture consists of:
>   * Presentation - View and Component
>   * Services - Logic
>   * API
>   * DB

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

> * Access protocols:
>   * HTTP - The main protocol used to access the repository via for example the ReST APIs.
>   * WebDAV - Web-based Distributed Authoring and Versioning is a set of HTTP extensions that lets you manage files collaboratively on web servers.
>   * FTP - File Transfer Protocol - standard network protocol for file upload, download and manipulation. Useful for bulk uploads and downloads.
>   * Alfresco Office Service - (AOS) allow you to access Content Services directly from all your Microsoft Office applications.
>   * CMIS - 1.0 and 1.1 standards to allow your application to manage content and metadata in an on-premise repository.
>   * IMAP - Internet Message Access Protocol - allows access to email on a remote server.
>   * SMTP - It is possible to email content into the repository (InboundSMTP), a folder can be dedicated as an email target.

> * *Протоколи за достъп:*
>   * *HTTP - Главният протокол, който се използва за достъп на хранилището.*
>   * *WebDAV - Web-based Distributed Authoring and Versioning е съвкупност от HTTP разширения, които позволяват управлението на файловете на уеб сървър.*
>   * *FTP - File Transfer Protocol - стандартен мрожови протокол за качване, сваляне и манипулиране на файлове.*
>   * *Alfresco Office Service - (AOS) позволява достъп до сървисите доректно от всички Microsoft Office приложения.*
>   * *CMIS - Напълно имплементирани 1.0 и 1.1 стандарти позволяват на приложенито да управялява съдържанието, метаданните в локалното хранилище.*
>   * *IMAP - Internet Message Access Protocol - позволява достъп до мейл от външен сървър.*
>   * *SMTP - Възможно е да се изпраща съдържание по мейл към хранилището към папка, която може да бъде мишена на мейла (email target).*
