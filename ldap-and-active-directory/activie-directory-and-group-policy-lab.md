# Active Directory and Group policy Lab
*Udemy Course notes*

## 10. Basic Server Configuration


## 17. Groups and Memberships
* Group scope:
    * Domain local - тази група ще бъде достъпна само от домейна, в който е създадена.
    * Global - може да бъде достъпена от други домейни, ако те са доверени.
    * Universal - също като Global, но могат да бъдат достъпени от други гори (forests) на доверени домейни.

* Group type:
    * Security - оказва правата в АД:
        * достъп до специфични файлове
        * сървъри
        * принтери
        * споделени файлове (споделянето им)
        * и др.
    * Distribution - се изолзва като разпределителен мейл списък:
        * *Пример* - създаване на група с име Suppport и добавяне на служители, при получаване на мейл, той ще достигне до всички.

Чрез "Member Of", може да добавяме дадена група към друга.


## 18. Disabling and Deleting User Accounts with AD
Disabling User Accounts - се осъществява чрез натискане на десен бутон върху дадения потребител и преместването му в друг OU, където ще се намират деактивираните потребители - това може да доведе до проблем с парвата на групите. В АД се променя иконката на потребителя. Активирането става по същия начин.


## 19. Group Policy
Това е инструмент, които се използва от системните администратори за бърза и лесна конфигурация на потребителите и компютрите в АД (при промяна да 1 policy, може да се отразни на много работни станции в домейна).
Чрез Group Policy, може да се имплементират настройки за сигурност за домейна.
* Пример за това:
    * Забрана за достъп до определени сървъри;
    * Забрана за достъп до определени файлове;
    * Инсталиране на софтуер;
    * Фон на десктопаа;

АД, **GPO**, съдържа разделени настройки за компютри и потребители.

> *GPO - Group Policy Object*

* AD Users & Computers -> Tools -> Group Policy Managment
    * Forest:
        * Domains - съдържа всички домейни, които са в forest-a.
        * Sites - съдържа всички сайтове, които може да са настроени чрез "active directory sites and services" (*използва се, когато сървърите физически са разположени на друго място*).
        * Group Policy Modeling
        * Group Policy 
            * Group Policy Modeling и Group Policy се изолзват за отстраняване на неизправности във всяка група.


## 20. Creating and Linking Group Policy Objects (GPOs)

GPOs съдържат настройки и конфигурации, които могат да бъдат приложени на компютри и потребители, съхраняващи се в АД.
Домейнът, може да има няколко GPOs, почти никога не съдържа само 1 GPO.
Едни GPO, може да бъде свързано или приложено многократно едновременно.
* Създаването на GPO е сходно на потребителски акаунт:
    * Group Policy Objects -> New
    * Domain -> Create a GPO in this domain, and Link it here...

При изтриване на GPO, трябва да премахнем линковете от домейните свързани с него (не е автоамтично).
Ако не е linked (свързано) в домейна, няма да има ефект в него. За да се свърже GPO, трябва чрез *Domain -> Link an Existing GPO...*
Може да се дадът права на определ потребител да редактира определено GPO.


## 21. Group Policy Precedence
*Редът или начинът, по който работят, редът или настройките, които да се приложат.*

> *LSDOE*

1. Local GP (least important) - първото, което е приложено на компютъра и е най-маловажно.
2. Site - приложените политики на сайта, презаписвайки всички конфликти, които биха възникнали между локалната и груповата политика.
3. Domain - ще бъдат над настройките на локалния сайт.
4. Organization Unit - това е всяко специфично GPO, приложено върху дадения OU.
    * Sub Organization Unit - ако има;
5. Enforced Group Policy Objects - админа избира кое да бъде Enforced GPO, то има най-голяма тежест.
> *Последно приложенотот GPO побеждава!*

### Computer vs User
Конфигурацията на потребителя побеждава, защото:
1. Първо се прилагат настройките на компютъра
2. След това тези на потребителя

> *Последно приложеното GPO побеждава!*

* Blocked Inheritance
    * OU могат да блокират наследяването;
    * Само GPO, което е в OU, ще бъде приложено;
    * Освен принудителното GPO;


## 22. Edit GPOs
Group Policy Management Editor се използва за редакция на политиките на компютър и потребител. (маркиране на *GPO -> Edit*)


## 23. Troubleshooting Group Policy with MMC
> *RSOP.msc - Resultant Set of Policy*

Чрез търсене на "rsop.msc" в търсачката на компютъра или C:/Windows/System32/rsop.msc


## 24. Troubleshooting Group Policy with Command Prompt (GPResult /r)

CMD команда: | описание
-------------|---------------
gpresoult /r | */r* за репорт
gpupdate | ъпдейт на политиките
gpupdate /force | за да се ативират новите политики

Важен резултат от *gpresoult /r* e **Applied Group Policy Objects**. Чрез този резултат най-лесно се проверява дали е приложена дадена политика към компютъра, на който е стартирана командата.


## 25. Creating Non-Inheriting Organizational Unit for GPO Testing / Troubleshooting
*При промяна на политиките - добавяне или премахване на наследяване се изпълнява команда gpupdate*.
Най-лесния начин да се разбира дали има наследяване на политики, е чрез наименоване с "Non-inheriting".


## 26. Deploying a Desktop Background to your domain with a GPO (Group Policy Object)
*Най-добре да се създават винаги GPO.*
Създаване на Share на папката с картинките, които искаме. Добавяме резрешенията за потребител или група (може и за определен домейн).
В Group Policy Management Editor:
User Configuration --> Policies --> Administrative Templates: Policy --> Desktop --> Desktop --> Desktop Wallpaper(Edit) -> Enabled -> Add path or locatio (*\\SERVER_NAME\Desktop Backgroung\image_name.extension*)
*Може да се изпробва, като се постави пътя в браузъра, трябва да зареди дадената картинка.*


## 27. Setting up an Logo Banner (Interactive Logon)
*Server manager --> Tools --> Group Policy Management*
В domain-а създаване ново GPO, избираме *Edit --> Policies --> Windows Settings --> Security Settings --> Local Policies --> Security Options -> GPO_Name:Message tittle for users...*.
> MCD: gpupdate /force (за ъпдейт на политиката)


## 28. Deploying Software with Group Policy
1. Пресместване/копираме в Share пространството, в избрана папка, добавя се права за определен потребител (група).
2. В домейна се добавя GPO за всяко приложение. Чрез *Edit --> Computer Configuration --> Policies --> Software Settings --> Software installation --> New -->(Open->Network\Share\software_folder_name\software_name.msi)*, след избиране на файла, определяме метода:
    * Published - даваме право на потребителя да инсталира software-a;
    * Assigned - инсталиране без да се правят модификации
    * Advanced - инсталиране с възможност за модификации

> MCD: gpupdate /force (за ъпдейт на политиката и инсталацията на software-a при рестарт)

*Препоръчва се да се добави в Computer Configuration, а не в User Configuration.*


## 29. Configuring Roaming Profiles for User Accounts
Създаване на ново споделено пространство:
> *Server Manager --> File and Storage Services --> Shares -> New Share -> Select type -> Select volume -> Share Name(Name$ прави папкара скрита) -> Допълнителни настройки -> Избиране права за достъп*

Разрешаване достъп на потребител до споделеното пространство (в *Настройване на правата*): 
> *Server Manager --> Tools --> Active Directory Users and Computers --> Добавяне на OU със SG в него, добавяне на потребители в групата -> Настройване на правата*

> Може да се добавя група в OU.

Wildcard | описание
-------- | --------
%username%  (%...%) | в настройките на потребителя за домейна, ще вземе автоматично username-a му


## 30. How to automatically map network share drivres with Group Policy
1. Добавяне на група:
> *Server Manager --> Local Server --> Login to DomainController / Tools -> Active Directory Users and Computers --> Добавяне на група в OU (или където ще я държим в домейна) -> Добавяме поребител към нея (от Domain Users) / група*
2. Добавяне на споделена папка:
> *Server Manager --> Tools --> Active Directory Users and Computers --> Домейн->New->Shared Folder (въвеждане на име и мрежови път - \\име_на_сървър\име_на_група)*
3. *Server Manager --> Tools --> Group Policy Management Editor* добавяме GPO, за всяка от групите (и споделените пространства)
    * User Configuration --> Preferences --> Windows Settings -> Drivre Maps -> New -> Mapped Drive -> избираме от вече създадените споделени пространства (спрямо коя група настройваме)
    * След създаването на Mapped Drive, определяме чрез Security Filtering кои потребители/група ще имат достъп.
    * След добавянето на потребителите/групите с достъп, избираме правата, които имат над споделеното пространство.
4. gpupdate /force


## 31. Configuring Domain Password and Account Lockout Policies with Group Policy
#### Настройките за паролите се правят в Computer Configuration 
*Създаване на правило за по-сложна парола*
В Default Domain Policy, може да видим вече създадените настройки (по подразбиране). Те могат да се редактират чрез:
*Edit --> Computer Configuration --> Windows Settings --> Security Settings --> Account Policies --> Password Policy*
Тук са различни настройки за паролите като:
* Колко време да се помни паролата - колко пъти, трябва да се смени паролата, за да се използва отново първата;
* Колко е минималното и максималното време, през което, трябва да се смени паролата;
* Колко е минималния брой символи;
* Дали да бъде сложна
* Начин на запазване - "reversible encryption" (за по-голяма сигурност "Disabled");

*Edit --> Computer Configuration --> Windows Settings --> Security Settings --> Account Policies --> Account Lockout Policy*
Тук са различни настройки за потребителя като:
* При колко грешно въведени пароли да бъде заключен акаунта;
* За колко време да бъде заключен;
* След колко време да се отключи акаунта;


## 32. Deploying Fine Grained Password Policies (PSOs)
#### Настройки на пароли за потребител или група
*Прави се извън АД и извън политиките на групите.*

1. Създават се чрез *Tools --> ADSI Edit --> Connect to -> (default) -> разгръщане на домейна -> System -> Password Settings Container -> New Object*
2. Въвеждане на следните данни:
    1. (Common-Name) Value:ДобреОписаниИме 
    2. (Password Settings Precedence) Value:1(колкото е по-близо до 1, толкова по-голяма тежест има) 
    3. (Password reverse encryption status for user accounts) Value:FALSE 
    4. (Password History) Value:24(ex) 
    5. (Password complexity status for user accounts) Value:TRUE 
    6. (Minimum Password Length for user accounts) Value:14(ex) 
    7. (Minimum Password Age for user accounts) Value:00:00:00:00(ex) 
    8. (Maximum Password Age for user accounts) Value:07:00:00:00(ex) 
    9. (Lockout threshoul for lockout of user accounts) Value:3(ex) 
    10. (Observation Window for lockout of user accounts) Value:00:00:15:00(ex) 
    11. (Lockout duration for lockout of user accounts) Value:00:00:15:00(ex)
3. След като е готово PSO-то, в *Properties -> msDS-PSOAppliesTo<not set> -> Edit -> Add Windows Account(група)*

При избирането на група, тези политики ще се приложат върху всички потребители, участващи в нея.
В PowerShell се въвежда:
```powershell
import-module ActiveDirectory

Get-ADUser -filter {GivenName -like Paul} –Properties "DisplayName", "msDS-UserPasswordExpiryTimeComputed" | Select-Object -Property "DisplayName",@{Name="ExpiryDate";Expression={[datetime]::FromFileTime($_."msDS-UserPasswordExpiryTimeComputed")}}
```
*GivenName - AD attribute, може да бъде различно, спрямо какво е в нашата АД*


## 33. Configuring Windows Firewall with Group Policy

*Server Manager --> Tools --> Group Policy Management --> Select OU(Right-click) -> Create a GPO in this domain, and Link it here... -> Edit -> Computer Configuration -> Windows Settings -> Security Settings -> Windows Firewall with Advanced Security -> Windows Firewall with Advanced Security -> Inbound Rules / Outband Rules -> New Rule*

Репортите от Firewall:
*rsop.msc --> Computer Configuration --> Administrative Templates*


## 34. Configuring Windows Registry Settings with Group Policy (GPOs)
Computer Configuration (позволява да се отварят всички файлове с Notepad, без значение от формата):

1. *Server Manager --> Tools --> Group Policy Management --> Domain(Right-click) -> Create a GPO in this domain, and Link it here... -> Edit --> Windows Configuration --> Windows Settings --> Registry --> New -> Action: Create -> Hive: HKEY_CLASSES_ROOT -> Key Path: HKEY_CLASSES_ROOT\\\*\\shell\Open With Notepad\command -> Value name: Default -> Value type: REG_SZ -> Value data: notepad.exe %1*
2. CMD: gpupdate /force

User Configuration:
*Server Manager --> Tools --> Group Policy Management --> Domain(Right-click) -> Create a GPO in this domain, and Link it here... -> Edit -> User Configuration -> Windows Settings -> Registry*


## 35. Enabling Script Execution for PowerShell
Среда |  | Начин на работа / прилика 
--- | --- | ---
PowerShell client | ---> | работи като CMD
PowerShell ISE | ---> | прилича на IDE

В PowerShell ISE може да се пишат, запазват и изпълняват скриптове.
Ако има забрана за четене на (някой тип) скриптове от групова политика, никой скрипт няма да тръгне. 
За проверка на грешка при изпълнение и/или разрешаване на *PowerShell ISE* да изпълнява скриптове.
За разрешаване на изпълняване скриптове се отваря:
1. *rsop.msc --> Computer Configuration --> Administrative Templates --> Windows Components --> Windows PowerShell -> Turn on Script Execution*
2. спрямо *GPO Name* се вижда кое GPO го забранява и се променя чрез *Group Policy Management Editor --> Computer Configuration --> Administrative Templates --> Windows Components --> Windows PowerShell -> Turn on Script Execution -> Not Configured*
3. CMD: gpupdate /force   
4. рестарт на PowerShell ISE


## 36. PowerShell Basics

Команда | Описание
------- | --------
Start-Transcript | Запис на командите, които въвеждаме
Get-Command *-AD* | Показване на командите, които са свързани с АД
CLS | почистване на екрана
Get-Alias | Показване на всички алиаси
$name | създаване на променлива
Get-Help command-name | показва информация за командата
Import-module Active Directory | ще ни позволи да изпълняваме команди като (Get-ADUser) 
Get-History | ще покаже командите, които е изплнявал потребителя
Set-ADUser | за попълване на данни

Запис на командите, които въвеждаме:
```powershell
Start-Transcript
```
Показване на командите, които са свързани с АД:
```powershell
Get-Command *-AD*
```
Показване на всички алиаси:
```powershell
Get-Alias
```
Създаване на променлива:
```powershell
$NameOfVariable = ...
```
Ще ни позволи да изпълняваме команди като (Get-ADUser):
```powershell
Import-module Active Directory
```

Примери, които ще върнат базовата информация за даден потребител:
1. С *Get-ADUser -Identity <\string>*:
```powershell
Get-ADUser -Identity 'Administrator'
```
2. С *Get-ADUser -Filter {Name -eq 'string'}*:
```powershell
Get-ADUser -Filter {Name -eq 'Administrator'}

Get-ADUser -Filter {ObjectGUID -eq '275aa8f-...'}
```
> *Get-ADUser -FIlter {SomeOfAttribute -eq 'string'}*

За добавяне на даден атрибут при извикването на *Get-ADUser*, се използва:
```powershell
Get-ADUser -Identity 'Administrator' -Properties Description
```
> *Get-ADUser -Identity 'string' -Properties NameOfProperty*

Може да се попълват атрибути чрез *Set-ADUser "user.name" -NameOfAttribute "value_as_a_string"*
```powershell
Set-ADUser 'user.name' -EmailAddress "user_mail@example.com"
```
> *Get-ADUser 'user.name' -Property "value_as_a_string"*


## 37. Listing AD Users with PowerShell

```powershell
# Import the active directory module
Import-Module ActiveDirectory

# List all AD users (limit of 100 users)
Get-ADUser -Filter * -ResultSize 100

# List all AD users (limit of 100 users) and readablle
Get-ADUser -Filter * -ResultSize 100 | Select-Object

# List all AD users (limit of 100 users) and only the names
Get-ADUser -Filter * -ResultSize 100 | Select-Object Name

# List all AD users (limit of N users) and selected attribute
Get-ADUser -Filter * -ResultSize N | Select-Object AttributeName, AttributeName, ...

# List all AD users (limit of N users), selected property and selected attribute
Get-ADUser -Filter * -ResultSize N -Properties PropertyName | Select-Object AttributeName, AttributeName, ...

# List all AD users from selected OU
Get-ADUser -Filter * -SearchBase "OU=OU name, OU=sub folder name, DC=domain, DC=com/bg/net"

# List all AD users from selected OU and pretty
Get-ADUser -Filter * -SearchBase "OU=OU name, OU=sub folder name, DC=domain, DC=com/bg/net" | Select-Object Name (example, it can be every attribute)

# List all AD users from selected group and pretty
Get-ADGroupMember 'Group Name' | Select-Object AttributeName, AttributeName, ...

# List all AD users from selected group, pretty and export to csv file
Get-ADGroupMember 'Group Name' | Select-Object AttributeName, AttributeName, ... | Export-Csv "Location:\to\write\the\file"
```
Може да се вземе *-SearchBase* за опреден OU и от *Active Directory Users and Computers --> domain --> OU --> OU -> Propreties*


## 38. Configuring User Roaming Profile Path with PowerShell
*Active Directory Users and Computers --> domain --> OU --> Domain Users -> Propreties*

```powershell
# Inport AD module
Import-Module ActiveDirectory

# Get all members of the roaming profile group
Get-ADGroupMember 'Roaminig Profile Users' |
    # Loop through each user
    ForEach-Object {
        # Do this for each member
        # $_. <--> object looping through
        # ("path" + sam account name) <--> to be considered as one unit
        # -Identity $_.SamAccountName <--> set a user indentity for the current object, same as sam account name
        Set-ADUser -Identity $_.SamAccountName -ProfilePath ("\\PCNAME\Profiles$\" + $_.SamAccountName)
    }
```

## 43. Creating an Active Directory System State Backup

1. За да може да се направи Backup (ако не е инсталирано при създаването на сървъра):
    * *Add Roles and Features --> Features -> Windows Server Backup (install)*
2. За създаване на Backup:
    * *Tools --> Windows Server Backup -> Local Backup(Schedule/Once) -> Backup Options -> Backup Configuration(Custom) -> Add items(System State) -> Remote shared folder(All premistions allowed)*

## 44. Restoring an Active Directory Backup
За възстановяване на АД, трябва да стартираме на сървъра в режим DRM mode (Directory Services Restore Mode). 
За целта:
1. чрез търсачката/старт менюто отваряме *msconfig* файла.
2. Избираме от табовете на *System Confiuration* таб *Boot* и избираме *Safe boot* *Active Directory repair*.
3. Докато се рестартира сървъра се натиска F8(?)
4. Избиране от *Advance Boot Options*: *Directory Services Repair Mode* 
5. Влизане с локалния администратор *.\administrator*
6. В CMD команда: *wbadmin get versions*, за да ни покаже версиите, които имаме.
7. Копираме *Version identifier*
8. Команда: *wbadmin start systemstaterecovery -version:**Version identifier** -authsysvol* (-authsysvol при множество домейни) 
9. 8/9 x Yes
