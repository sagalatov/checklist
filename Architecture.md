* Шаблоны (понимание, обоснование применения): Издатель-подписчик (Publish–subscribe), Одиночка,Фабричный метод, Абстрактная фабрика  
* MVC, MVP, MVVM - Отношения между моделью, вьюхой и контроллером 
* СОЛИД, первые четыре принципа (поверхностное представление)
* Feature slice design методология.
* Atomic design 

***

## Шаблоны (понимание, обоснование применения) Издатель-подписчик (Publish–subscribe), Одиночка, Фабричный метод, Абстрактная фабрика.
#### Шаблон проектирования, или паттерн.  
В разработке программного обеспечения — повторяемая архитектурная конструкция, представляющая собой решение проблемы проектирования, в рамках некоторого часто возникающего контекста.

#### Одиночка
Обеспечивает тот факт, что создаваемый объект является единственным объектом своего класса.  
Вообще шаблон одиночка признан антипаттерном, необходимо избегать его чрезмерного использования. Он необязательно плох и может иметь полезные применения, но использовать его надо с осторожностью, потому что он вводит глобальное состояние в ваше приложение.  

#### Фабричный метод
Порождающий шаблон проектирования, предоставляющий подклассам интерфейс для создания экземпляров некоторого класса. В момент создания наследники могут определить, какой класс создавать. Иными словами, данный шаблон делегирует создание объектов наследникам родительского класса. Это позволяет использовать в коде программы не специфические классы, а манипулировать абстрактными объектами на более высоком уровне.

#### Абстрактная фабрика
Порождающий шаблон проектирования, предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не специфицируя их конкретных классов. Шаблон реализуется созданием абстрактного класса Factory, который представляет собой интерфейс для создания компонентов системы (например, для оконного интерфейса он может создавать окна и кнопки). Затем пишутся классы, реализующие этот интерфейс.  

#### Издатель-подписчик (англ. PubSub)
Поведенческий шаблон проектирования передачи сообщений, в котором отправители сообщений, именуемые издателями (англ. publishers), напрямую не привязаны программным кодом отправки сообщений к подписчикам(англ. subscribers). Вместо этого сообщения делятся на классы и не содержат сведений о своих подписчиках, если таковые есть. Аналогичным образом подписчики имеют дело с одним или несколькими классами сообщений, абстрагируясь от конкретных издателей..

***

## MVC - Отношения между моделью, вьюхой и контроллером.
#### Model View Controller
![mvc](/img/mvc.png)  
Подход к проектированию приложения, который предполагает выделение кода в блоки модель, представление и контроллер.   
* Контроллер обеспечивает взаимодействие с системой: обрабатывает действия пользователя, проверяет полученную информацию и передает ее модели. Контроллер определяет, как приложение будет реагировать на действия пользователя. Также контроллер может отвечать за фильтрацию данных и авторизацию.  
* Модель - это основная логика приложения. Отвечает за данные, методы работы с ними и структуру программы. Модель реагирует на команды из контроллера и выдает информацию и/или изменяет свое состояние. Она передает данные в представление 
* Представление — визуализирует иформацию, которую оно получает от модели. View отображает данные на уровне пользовательского интерфейса. Представление определяет внешний вид приложения и способы взаимодействия с ним.

#### Model View Presenter
MVP — это улучшение по сравнению с MVC, целью которого является решение проблемы сложности контроллера. Presenter действует как интерфейс между Моделью и Представлением, опосредуя их связь. Представление становится более пассивным, но делегирует пользовательский ввод и события презентатору, который соответствующим образом обновляет модель и представление. Это разделяет представление и модель, делая систему более удобной в обслуживании и тестировании.

#### Model View ViewModel
MVVM основывается на шаблоне MVP, уделяя особое внимание разделению задач при разработке пользовательского интерфейса. ViewModel — это интерфейс между моделью и представлением, которые связаны через привязки данных. Это позволяет проектировать представление декларативно, в разметке, при этом ViewModel отвечает за предоставление данных и манипулирование ими в соответствии с представлением. Этот шаблон упрощает разработку пользовательского интерфейса, повышает тестируемость и позволяет лучше разделить логику, связанную с пользовательским интерфейсом.

***

## СОЛИД, первые четыре принципа (поверхностное представление).
#### S: Single Responsibility
Каждый класс должен иметь только одну причину для изменения. Это означает, что класс должен быть ответственным только за одну конкретную функцию или задачу. Этот принцип помогает сделать классы более связанными, легко понятными и поддерживаемыми.  

#### O: Open-Closed Principle
Программные сущности, такие как классы, модули и функции, должны быть открыты для расширения, но закрыты для модификации. Вместо изменения существующего кода, следует добавлять новый код для внесения изменений. Это позволяет создавать более стабильные и гибкие системы.

#### L: Liskov Substitution Principle
 Объекты в программе должны быть заменяемыми экземплярами их базовых типов, не нарушая корректность программы. Это означает, что код, который работает с базовым типом, должен работать и с любым его подтипом, не вызывая ошибок или неожиданного поведения. Этот принцип обеспечивает согласованность в использовании наследования и полиморфизма. 

#### I: Interface Segregation Principle  
Клиенты не должны зависеть от интерфейсов, которые они не используют. Вместо создания общих интерфейсов следует создавать специфические интерфейсы, предназначенные для конкретных клиентов. Это позволяет избежать излишней связности между компонентами системы и улучшить модульность. 

#### D: Dependency Inversion Principle
(Принцип инверсии зависимостей).  
Позволяет направить зависимости от низкоуровниевых сущностей (логгер) к более высокооуровневым (бизнес логика) с большим приоритетом. Например используя интерфейсы в высокоуровненых сущностях.  
Это необходимо для предотвращения необходимости вносить изменения в бизнес логику при изменении мение важных частей системы.  
[Подробное видео на это тему](https://www.youtube.com/watch?v=u6gAVCEJjQ4)

***
## Feature slice design
Методалогия организации frontend приложения.  
![fsd](/img/fsd2.png)  
Проект на FSD состоит из слоев (layers), каждый слой состоит из слайсов (slices) и каждый слайс состоит из сегментов (segments).
Слои стандартизированы во всех проектах и расположены вертикально. Модули на одном слое могут взаимодействовать лишь с модулями, находящимися на слоях строго ниже. На данный момент слоев семь (снизу вверх):

**shared** — переиспользуемый код, не имеющий отношения к специфике приложения/бизнеса. (например, UIKit, libs, API)  
**entities** (сущности) — бизнес-сущности. (например, User, Product, Order)  
**features** (фичи) — взаимодействия с пользователем, действия, которые несут бизнес-ценность для пользователя. (например, SendComment, AddToCart, UsersSearch)  
**widgets** (виджеты) — композиционный слой для соединения сущностей и фич в самостоятельные блоки (например, IssuesList, UserProfile).  
**pages** (страницы) — композиционный слой для сборки полноценных страниц из сущностей, фич и виджетов.  
**processes** (процессы, устаревший слой) — сложные сценарии, покрывающие несколько страниц. (например, авторизация)  
**app** — настройки, стили и провайдеры для всего приложения.

**Слайсы** - это второй уровень в организационной иерархии Feature-Sliced Design. Их основное назначение - группировать код по его значению для продукта, бизнеса или просто приложения.

**Сегменты** — это папки в слайсах, и они могут иметь произвольные названия, описывающие их цель, но некоторые цели настолько распространены, что существует несколько общепринятых названий:

📂 api/ для взаимодействия с бэкендом  
📂 ui/ для кода, отвечающего за отображение и внешний вид  
📂 model/ для хранения данных и бизнес-логики  
📂 config/ для фиче-флагов, переменных окружения и других форм конфигурации  

## Atomic design 
Методология проектирования, компоновки и организация компонентов frontend приложений и дизайн систем. По AD сущности в проекте делятся на 5 основных категорий.  

![fsd](/img/atomicDesign.png)  
* **Атомы** - html тэги, кнопки, поля ввода.
* **Молекулы** - комбинация атомов, например поле ввода с кнопкой и тултипом.
* **Организмы** - формы, хэдер, футер.
* **Шаблоны** - Объединение организмов. Форма разбитая на шаги.
* **Страницы** - Объеденяет все структуру шаблонов и кнотента.

Пример организации папок:  

![fsd](/img/atomicDesignFolders.png) 