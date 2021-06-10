# Delegate Pattern

In software engineering, the delegation pattern is an object-oriented design pattern that allows object composition to achieve the same code reuse as inheritance.
The delegate is a **helper object**, but with the original context.
With language-level support for delegation, this is done implicitly by having self in the delegate refer to the original (sending) object, not the delegate (receiving object).
In the delegate pattern, this is instead accomplished by explicitly passing the original object to the delegate, as an argument to a method.

> *В софтуерното инженерство моделът на делегиране е обектно-ориентиран модел на проектиране, който позволява на композицията на обекта да постигне същото повторно използване на кода като наследяването.*
> *Делегатът е помощен обект, но с оригиналния контекст.*
> *С поддръжката на делегиране на езиково ниво, това се прави имплицитно, като самостоятелно в делегата се позовава на оригиналния (изпращащ) обект, а не на делегата (приемащия обект).*
> *Това се постига чрез изрично предаване на оригиналния обект на делегата, като аргумент на метод.*
> *За разлина от Интерфейса е, че в Делегата може да има (минимална) логика.*


Delegation is not exactly a 'design pattern' in the sense used in the GoF book. 
It is useful in a number of scenarios, and is a base for other patterns:

* When you want to perform some additional actions before/after you delegate (that's the Decorator pattern, but it's based on delegation). 
    * For example, Collections.synchronizedList(..) creates a new collection that delegates to the original one, but has its methods synchronized.

* When you have incompatible interfaces and you want to adapt one to the other (the adapter pattern). You get the original object and delegate to it from methods that conform to the desired interface.
    * For example, there's the EnumerationIterator class, that adapts enumerations to the Iterator interface. The class has a hasNext() method which delegates to enumeration.hasMoreElements().
    
* When you want to hide some complexity from the user of your class, you can have methods that delegate to different actual workers.
    * For example, a Car can have start(), openWindow() and brake(), but each of these methods will actually delegate to the engine, el.windows and braking system.

> *Делегирането не е точно „модел на проектиране“ в смисъла, използван в книгата на GoF.*
> *Той е полезен в редица сценарии и е основа за други модели:*
> * *Когато искате да извършите някои допълнителни действия преди / след като делегирате (това е моделът на декоратора, но се основава на делегиране).*
>   * *Например, Collections.synchronizedList (...) създава нова колекция, която делегира към оригиналната, но има синхронизирани методи.*
> * *Когато имате несъвместими интерфейси и искате да адаптирате единия към другия (модела на адаптера). Получавате оригиналния обект и го делегирате от методи, които съответстват на желания интерфейс.*
>   * *Например има клас EnumerationIterator, който адаптира изброяванията към интерфейса на Iterator. Класът има метод hasNext (), който делегира на enumeration.hasMoreElements().*
> * *Когато искате да скриете известна сложност от потребителя на вашия клас, можете да имате методи, които да делегират на различни действителни работници.*
>   * *Например, една кола може да има start(), openWindow() и brake(), но всеки от тези методи всъщност ще делегира на двигателя, елекртрическите стъкла и спирачната система.*


### Helper Object
In object-oriented programming, a helper class is used to assist in providing some functionality, which isn't the main goal of the application or class in which it is used.

> *При обектно-ориентираното програмиране помощният клас се използва, за да подпомогне предоставянето на някаква функционалност, което не е основната цел на приложението или класа, в който се използва.*
