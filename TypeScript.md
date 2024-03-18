## TypeScript  
* Типизация 
    * Вывод типов
    * Совместимость типов
    * User-Defined Type Guards
* Классы;
    * Наследование
    * Модификаторы доступа
    * Абстрактные классы
* Enum'ы
* Интерфейсы - использование, отличие от интерфейсов в "классичиских" ЯП
* Дженерики (обобщения)
* Декораторы: создание, примеры использования
* async await
* Partial, Pick, Omit. 
* The keyof Operator.
* Intersection Types (пересечения)
* Union Types
* Cужение типов in, is.

### Типизация  
TypeScript является строго типизированным языком, и каждая переменная и константа в нем имеет определенный тип. При этом в отличие от javascript мы не можем динамически изменить ранее указанный тип переменной.
            
### Вывод типов  
##### Boolean  
const isFetching: boolean = true

##### Number  
const int: number = 42
const float: number = 4.2
const num: number = 3e10
const message: string = ’TypeScript’

##### Array  
```javascript
const numberArray: number[ ] = [1,2,3,4,5] 
const numberArray<number> = [1,2,3,4,5] 
```
В скобках указывается тип

##### String  
const words: string[ ] = [‘Hello’, ‘World’]

##### Tuple   
const contact: [string, number] = [’Sergey’, 7519747]

##### Any
let variable: any = 42;

##### Unknown
Тип unknown — это надмножество всех доступных типов. Он позволяет присвоить переменной значение произвольного типа.
Может показаться, что тип unknown работает так же, как any. Однако между ними есть различие. Тип any по сути отключает проверку типов и позволяет выполнять любые операции со значением, например, обращаться к свойствам переменной. Тип unknown запрещает это и требует предварительной проверки типа переменной, либо приведения к нужному типу.
```javascript
function f11(x: unknown) {
  x.foo; // Error
  x[5]; // Error
  x(); // Error
  new x(); // Error
}
```
```javascript
// Only equality operators are allowed with unknown
function f10(x: unknown) {
  x == 5;
  x !== 10;
  x >= 0; // Error
  x + 1; // Error
  x * 2; // Error
  -x; // Error
  +x; // Error
}
```

**void** - функция ничего не вернет.
```javascript
function sayMyName(name: string):void{
console.log(name)
}
```
**never** - когда функция возвращает ошибку. Значение которое не должно быть возвращено.
```javascript
function throwError(message: string):never {
    throw new Error(message)
}
```
**Именованные типы**
type Login = string
const login: Login = ‘admin'
type ID = string | number
const id1: ID = 1234
const id2: ID = '1234'
const id3: ID = true // Так сделать не получиться.

type SomeType = string | null | undefined

### Совместимость типов  
В TypeScript совместимость типов основана на структурной совместимости.  
```javascript
interface Monster {
    health : number;
}

class Daemon {
    health : number;
    name : String;
}

class Goblin {
    health : number;
}

class Ghaul {
   flying : boolean;
}

let v1:Monster = new Daemon(); // OK

let v2:Monster = new Goblin();  // OK

let v3:Monster = new Ghaul(); // Ошибка, так как в Ghaul нет поля health;
```
В Daemon есть дополнительный атрибут name, но это нам не мешает присвоить его переменной типа Monster, так как в Daemon есть все поля из Monster. Но мы не можем присвоить переменной типа Monster экземпляр Ghaul, так как в нём нет атрибута health.
```javascript
let x = function(arg0:number):void {
    alert(arg0);
}

let y = function(arg0:number, arg1 : string):void {
    alert(arg1);
}

y = x;
x = y;  // Ошибка, так как в x количество аргументов меньше.
```
При сравнении двух функций тоже действует совместимость типов. Мы можем присвоить переменной с типом функции, в которой больше элементов, ссылку на функции с меньшим числом элементов, так как в JavaScript последние параметры функции могут опускаться.

### User-Defined Type Guards  
Guards - Некоторые вспомогательные конструкции которые позволяют удобно работать с типами 
type AlertType = 'success' | 'danger' | ‘warning’    //Type Guards

function setAlertType(type: AlertType) {
  //....
}

setAlertType(type: 'success')

setAlertType(type: 'warning')

setAlertType(type: 'default') // is not assingnable to parameter of type 'Alert Type'
            
### Классы
 Создаются также как es6 но с некоторыми отличиями 
class Typescript {
  varsion: string  //Поля
  
  constructor(varsion: string) {
    this.version = version
  }
  
  info(name: string) {
    return `[${name}]: Typescript version is ${this.version}`
  }  
}
Все тоже самое только добавили типы.

*** 

```javascript
class Сar {
  readonly moodel: string
  readonly numberOfWheels: number = 4  // Хорошоей практикой считается определять поля до конструктора
  
  constructor(theModel: string) {
    this.model = theModel // model можно перезаписать только внутри конструктора
  }
}
```

Короткая запись
```javascriptjavascript
class Сar {
readonly numberOfWheels: number = 4
constructor(readonly model: string) {
  
  }
}
```
Поскольку мы указываем модификатор внутри конструкторта в таком случае typescript создаст readonly поля model в классе и в контсрукторе определит его на входящее параметр model.

### Наследование  
Наследование интерфейсов 
```javascript
Interface RectWithArea extends Rect {
getArea: () => number
}

const rect5: RectWithArea = {
id: ‘123’,
size: {
    width: 10,
    height: 5
    },
    getArea(): number {
return this.size.width + this.size.height
}
}

//Класс наследует интерфейс

Interface IClock {
    time: Date
    setTime(date: Date): void
}
class Clock implements Clock {
    time: Date = new Date()
    setTime(date: Date): void {
    this.time = date
    }
}
```

### Модификаторы доступа
Одной из ключевых концепций объектно-ориентированного программирования является инкапсуляция, подразумевающая сокрытие от внешнего доступа к состоянию объекта 
и управление доступом к этому состоянию.   
В TypeScript три модификатора: public, protected и private
Если к свойствам и функциям классов не применяется модификатор, то такие свойства и функции расцениваются как будто они определены с модификатором public
Если же к свойствам и методам применяется модификатор private, то к ним нельзя будет обратиться извне при создании объекта данного класса.
Модификатор protected во многом аналогичен private - свойства и методы с данным модификатором не видны из вне, но к ним можно обратиться из классов-наследников:  

```javascript
class User {
    private name: string;
    protected age: number;
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    public displayInfo(): void {
        console.log("name: " + this.name + "; age: " + this.age);
    }
}

class Employee extends User {
    private company: string;
    constructor(name: string, age: number, company: string) {
        super(name, age);
        this.company = company;
    }
    public showData(): void {
        console.log("Age: " + this.age);
        //console.log("Name: " + this.name); // не работает, так как name - private
    }
}
```

Используя модификаторы в параметрах конструктора, нам больше не надо явно создать свойства для этих параметров. Свойства создаются автоматически, называются они по имени параметров и имеют те же модификаторы, что и параметры
class User {
    constructor(private name: string, private age: number) {
    }
    public displayInfo(): void {
        console.log("name: " + this.name + "; age: " + this.age);
    }
}

### Абстрактные классы  
Не во что не компелируются, нужны на этапе разработки чтоб бы от них наследоваться  

```javascript
abstract class Component {
  abstract render():void
  abstract info():string
}

class AppComponent extends Component {
  render():void {
    console.log("Component on render")
  }
  info():void {
    console.log("This is info)
  }
}
```

При компиляции в es5 Будет преобразование в функции, при компиляции в es6 абстрактных классов мы не увидим.

### Enum'ы  

Вспомогательная сущность которая позволяет лучше структурировать код если присутствуют однотипные элементы.

```javascript
enum Membership {
    Simple,
    Standart,
    Premium
}
```
const membership = Membership.Standart //1
//По умолчанию присваиваются индексы
const membershipReverse = Membership[2] // Premium 
//Логика отличается от массива

Если мы присваиваем енуму строчку то на выходе мы ее и получаем. 

```javascript
enum SocialMedia {
VK = ‘VK'
FACEBOOK = ‘FACEBOOK’,
INSTAGRAM = ‘INSTAGRAM'
}
const social = SocialMedia.VK
console.log(social) // VK
```


### Интерфейсы - использование, отличие от интерфейсов в "классичиских" ЯП
```javascript
interface Rect {
readonly id: string
color?: string
    size: {
    width: 20,
    height: 30
    },
color: ‘#ccc'
}     
        
const rect1: Rect = {
id: ‘1234’,
    size: {
    width: 10,
    height: 5
    }
}

rect1.color = ‘black’
```
Так как мы в конечном счете работаем с JS мы можем менять внутреннее состояние объектов или массива но только внутреннее. Записать новое значение в переменную мы не можем.
Возможно указывать к какому типу относиться объект 
```javascript
const rect3 = {} as Rect
const rect4 = <Rect> {} //Альтернативная старая запись.
```
Ситуация кода нужно создать интерфейс для объекта у которого будет много динамических ключей 

```javascript
Interface Styles {
    [key: string]: string
}
const css = {
    border: ‘1px solid red’,
    marginTop: ‘2px’,
    bordRadius: ‘5px’
}
```
Для того что бы не прописывать все типы свойств в интерфейсе.
[key: string] относиться к свойству.
string к значению.

### Generics (обобщения)  
Обобщения (англ. generics) или дженерики - это инструмент, который позволяет писать на TypeScript компоненты, способные работать с различными типами данных.
```javascript
const arrayOfNumbers: Array<number> = [1,2,3,4,5]
const arrayOfStrings: Array<string> = [‘Hello’, ’Sergey’]
```
<> - из чего будет состоять

```javascript
function reverse<T>(array: T[]):T[] {
  return array.reverse() //Как пример простой реализации
}
```

Для того что бы одна функция работала с разными типами данных c указанием типов мы используем дженерик типа Т, дженерик тип который будет **динамически подстраиваться** под тот контент который есть в массиве. Дженерики позволяют делать код более гибким и переиспользуемым, но при этом повышают сложность чтения. 

### Декораторы: создание, примеры использования.  
Функция декоратор принимает один параметр: это контсруктор класса к которому данный дкоратор будет применяться
function logger(contstrFn: Function) {
  constructorFN.prototype.log = function(){ //Расширили через prototype
    console.log(this);
  }
}

@logger //Перед классом нужно указать данную конструкцию для закрепления декоратора
class User {
  constructor(public name: string, public age: number) {
  }
}

let user = nes User("Sergey", "27");
(<any>user).log(); //Выведет в консоль обьект.
В итоге все инстансы класса получать методы которые были определены в функции декораторе.

### async await  
```javascript
const delay = ms => {
  return new Promise(r => setTimeout(() => r(), ms))
}
const url = 'https://jsonplaceholder.typicode.com/todos';
//delay(2000).then(() => console.log('2 sec'))
//Вариант на промисах

function fetchTodos() {
  console.log('Fetch todo startes...')
  return delay(2000)
  .then(() => fetch(url))
  .then(response => response.json())
}
fetchTodos()
.then(data => {
  console.log('Data:', data)
})
.catch(e => console.error(e));
```

Вариант с "async await"

Если внутри функции мы используем await то ферхнеуровненвая функция должна быть асинхронной

```javascript
async function fetchAsyncTodos() {
  console.log('Fetch todo startes...')
  try { //try catch для обработки ошибок
    await delay(2000) //Пока не будет зарезолвиза промис задержки интерпритатор не перейдет на следующую строчку. (Возможность await)
    const responce = await fetch(url) //Fetch также возвращает responce поэтому нужно присвоить в переменную
    const data = await responce.json()
    console.log('Data:', data)
  } catch (e) {
    console.error(e)
  } finally {
    // Таже есть возможность выполнить блок finally
  }
}
fetchAsyncTodos()
fetchAsyncTodos().then() // Возможно

```
### `Partial<Type>`
   Делает все свойства типа в *Type* необязательными. 

### `Pick<Type, Keys>`
   Создает тип используя переданные ключи из *Type*  
   `type TodoPreview = Pick<Todo, "title" | "completed">;`
   
### `Omit<Type, Keys>`  
   Создает тип исключая переданные ключи.  
   `type TodoPreview = Omit<Todo, "description">`

### `Record<Keys, Type>`  
   Создает тип объекта с указанным типом ключа и значения.  
   `type myObj = Record<string, unknown>`

### The keyof Operator
Предоставляет более удобную запись чем *union type* для ожидаемого значнния, имя 
которого будет равно одному из ключей указанному после keyof. 
   ```typescript
  interface Todo {
     id: number;
     text: string;
     due: Date;
}

type TodoKeys = keyof Todo; // "id" | "text" | "due"
   ```
### Intersection Types
Перечесения позволяют объединить несколько типов в один, полученный тип должен удовлетворять
условиям всех включенных в *intersection* типов. Во включенных типах не должно быть свойств с одинаковыми именами
и разными типами, это вызовет ошибку `Type 'number' is not assignable to type 'never'.`
```typescript
interface Animal {
  kind: string;
}
interface Person {
  firstName: string;
  lastName: string;
  age: number;
}
interface Employee {
  employeeCode: string;
}
let employee: Animal & Person & Employee = {
  kind: 'human',
  firstName: 'Jane',
  lastName: 'Smith',
  age: 20,
  employeeCode: '123'
}
```
### Union Types
Объединенные типы могут включать все или некоторые свойства перечисленных типов. Однако доступ в объекте
реализующем объединенный тип можно получить только к свойствам перечисленным во всех включенных типах. Если свойства имеют одинаковые имены,
но разные типы, результирующий тип будет вида `number | string`
```typescript
interface Animal {
  kind: string;
}
interface Person {
  kind: string;
  firstName: string;
  lastName: string;
  age: number;
}
interface Employee {
  kind: string;
  employeeCode: string;
}
let employee: Animal | Person | Employee = {
  kind: 'human',
  firstName: 'Jane',
  lastName: 'Smith',
  age: 20,
  employeeCode: '123'
}
console.log(employee.kind)
```
### Cужение типов
**in** - проверка наличия указанного свойства в объекте. TS позволяет использовать данный оператор для сужения поетнциальных типов.
```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
 if ("swim" in animal) {
   return animal.swim();
 }

 return animal.fly();
}
```
**is** - предикат позволяющий сузить переменную до указанного типа.
```typescript
function isFish(pet: Fish | Bird): pet is Fish {
 return (pet as Fish).swim !== undefined
}
```