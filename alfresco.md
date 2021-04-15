# Alfresco Community Edition


## Introduction

> Alfresco Community Edition is an open source Enterprise Content Management (ECM) system that manages all the content within an enterprise and provides the services and controls that manage this content.
*Alfresco Community Edition е система за управление на съдържание (данни) с отворен код и осигурява сървиси и контроли, с които да управлява съдържанието.*

> At the core of the Community Edition system is a repository supported by a server that persists content, metadata, associations, and full text indexes.
*В основата на системата Alfresco Community Edition е хранилище (repository), поддържано (подпомагано) от сървър, който съхранвяа съдържанието, мета данните, асоциации и индексите.*

> Programming interfaces support multiple languages and protocols upon which developers can create custom applications and solutions.
*Програминя интерфейс поддържа множество езици и протоколи, чрез които разработчиците, могат да създадат свои проложения и решеняи.*

> Out-of-the-box applications provide standard solutions such as document management, and web content management.
*Готовите приложения осигуряват стандартни решения като управление на документи и управление на уеб съдържание.*

> As an entirely Java application, the Community Edition system runs on virtually any system that can run Java Enterprise Edition.
*Това е JAVA приложение, което моде да върви на всяка система, която може да стартира JEE.*

> At the core is the Spring platform, providing the ability to modularize functionality, such as versioning, security, and rules. 
*Изградено е на Spring платформа, предоставяща възможността за модулиране на функционалността като версия, сигурност и правила.*

> Alfresco uses scripting to simplify adding new functionality and developing new programming interfaces. 
*Alfresco използва скриптове, за да опрости добавянето на нова функционалност и разработването на нови програмни интерфейси.*

> *This portion of the architecture is known as web scripts and can be used for both data and presentation services.*
*Тази част от архитектурата е известна като уеб скриптове и може да се използва както за данни, така и за презентационни услуги.*


## Software Architecture

> At its core is a repository that provides a store for content, and a wide range of services that can be used by content applications to manipulate the content.
*В основата му е хранилище (repo), което осигурява склад (store) за съдържание и широк спектър от услуги (services), които могат да се използват от приложенията (за съдържание) за манипулирането му.*

> * The three main componets that the Content Services consist of:
>   * The Platform
>   * The User Interface (UI)
>   * The Search engine
*These components are implemented as separate web applications.*

> * *Трите главни компонента, които Content Services се състои:*
>   * *Платформата*
>   * *Потребителския интерфейс*
>   * *Търсачката*
*Тези компоненти са имплементирани като отделни уеб приложения.*

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