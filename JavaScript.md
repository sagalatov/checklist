## JavaScript
* this, call, apply, bind
* prototype, рассказ что это и как работает
* ООП в js: наследование, инкапсуляция  
* ES6 - const и let, arrow function, классы, шаблонные строки, spread operator, rest parameters, деструктивное присваивание объектов/массивов 
* как устроена асинхронность в js. Event loop 
* работа с асинхронностью; promise - основные методы, создание промиса из функции с колбэком


### this, call, apply, bind.  
#### this  
Указывает на текущий контекст выполнения функции. Задается следующими способами:  
1. Функция вызвана через new, this - новый объект.  
`const bar = new Foo()`.  
2. Функция вызвана с call, aplly или bind.  
`const bar = foo.call(obj)` - где this указывает на obj.  
3. Функция вызвана с контекстом ( метод объекта ).  
`const bar = obj.foo()`.  
4. Без явно заданного контекста.  
  В режиме 'use strict' - undefined иначе объект global.  
  ` const bar = foo()`.  
 
  Стрелочные функции не содержат собственный контекст this, а используют значение this окружающего контекста.  
  ```javascript
  function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // `this` указывает на объект Person
  }, 1000);
}

var p = new Person();
```

#### bind  
Встроенные метод который возвращает функцию с новым контекстом, контекст передается 1 аргументом. Также можно передать последующие аргументы которые 
в дальнейшем будут использоваться.

#### call  
Встроенные метод который возвращает и вызывает функцию с новым контекстом, контекст передается 1 аргументом. Также можно передать последующие аргументы которые в дальнейшем будут использоваться.

#### applay  
Встроенные метод который возвращает и вызывает функцию с новым контекстом, принимает 2 обязательных параметра это контекст и массив из аргументов.

```
function hello() {
console.log(this)
}

hello()
const person = {
name: 'Sergey',
age: 27,
seyHello: hello,
seyHelloWindow: hello.bind(window), // window
logInfo: function (phone, method) {
console.log(`Name is ${this.name}`)
console.log(`Age is ${this.age}`)
console.log(`Phone is ${phone}`)
console.log(`Method is ${method}`)
}
}
const alina = {
name: 'Alina',
age: 27
}

person.seyHelloWindow() // window
```

Bind (Возвращает функцию и мы вызываем ее когда необходимо)
```
const fnAlinaInfoLogBind = person.logInfo.bind(alina, '8-800-888-80-80', 'bind');
fnAlinaInfoLogBind();
```

Call (Возвращает функцию и вызывает ее сразу)
```
person.logInfo.call(alina, '8-800-888-80-80', 'call');
```

Applay (Поведение аналогично call, разница только в том что принимает вторым аргумент массив из параметров)
```
person.logInfo.apply(alina, ['8-800-888-80-80', 'apply']);
```

### prototype, рассказать что это и как работает.
При вызове функции у не всегда создается свойство prototype, изначально ссылается на практически пустой объект у которого есть __proto__ которая указывает на object prototype и конструктор ссылающейся на самом функцию и этот объект начинает работать когда мы вызываем функцию с помощью оператора new.

При создании через оператор new есть три отличия от стандартного поведения
1. Создается новый объект и этот новый объект передается функции в качестве this сама функция тоже вызывается но this-ом является новый объект.  
2. Новый объект неявно возвращается из функции (возвращается this  а точнее вновь созданный объект).  
3. Ссылка __proto__ нового объекта будет ссылаясь на тот объект который лежит в свойстве  prototype в функции.
Таким образом методы созданные через функцию конструктор, будут взяты у вновь созданных объектов через прототип.  
Пример расширения через prototype:
```
const array = [1, 2, 3, 4, 5]
Array.prototype.multBy = function(n) {
  return this.map(function(i) {
    return i * n
  })
}
console.log(array.multBy(2)) // [2, 4, 6, 8, 10]
```
            
### ООП в js: наследование, инкапсуляция

#### Наследование
Любой объект в JavaScript имеет второй связанный с ним объект который является его прототипом объект наследуют свойство своего прототипа или все свойства которые есть у прототипа будут доступны через дочерний объект. 
Это называется prototype inheritance (наследование основное на прототипах), это единственный тип наследования который есть в языке JavsScript.
(В ООП под наследованием понимают наследование дочерними классами свойств и методов родительских классов.)
Работает это следующим образом: создается объект который должен быть прототипам

```
var ObjectProto = {
  name: 'Sergey',
 }
 //и на основе его создаются дочерние объекты при помощи 
 
 //Object.create(Prototype)
 var object = Object.create(ObjectProto)
 console.log(object.name) // Sergey

 //Пример использования, создания однотипных объектов
 var Person = {
 constructor: function(name, age) {
     this.name = name;
     this.age = age;
     
     return this;
   },
     greet: function () {
     console.log('Hi, my name is' + this.name)
   }
 }
 
 var firstPerson, secondPerson
 firstPerson = Object.create(Person).constructor('Sergey', 28);
 secondPerson = Object.create(Person).constructor('Alina', 27);
 console.log(firstPerson.name) //Sergey
 secondPerson.greet() //Hi my name is Alina 
 ```

#### Инкапсуляция
Данные и методы связанный логическим контекстом, объединяются в общую сущность, а также доступ к этим данным разграничивается на уровни.

1. Приватная или защищенная переменная которая объявлена через let или const.
2. Публичная которую мы записываем через контекст  this.
Пример инкапсуляции - разграничение на уровни доступа наши переменные и методы.

```
function Product(
  name = 'Default product',
  price = 0,
  category = new Date()
) {
this.name = name; //Публичная
this.price = price; //Публичная
this.category = category; //Публичная
const createdDate = createdAt;  //Приватная
}
```

### ES6 - const и let, arrow function, классы, шаблонные строки, spread operator, rest parameters, деструктивное присваивание объектов/массивов

#### const  
Позволяет создавать константы которые нельзя изменить входе программы.
(Не поднимаются - нельзя использовать до объявления)

#### let  
область видимости внутри блока а именно фигурных скобок { }  
(Не поднимаются - нельзя использовать до объявления)

#### arrow function - стрелочная функция
компактные, иная работа с this, не имеет имени.  
Особенности:

```
let add = (x, y) => x + y // Присвоили для вызова
//С одним параметром скобки не нужны
x => x + y
//Если в функции несколько строк то необходимо указать скобки и return
let multiply = (x, y) => {
  let result = x * y
  return result;
}

console.log(add(2, 3)); //5
console.log(multiply(2, 2));// 4
```

Если возвращаем литерал объекта то необходимо оборачивать в скобки ()
```
let getPerson = ( ) => ( {name : 'Sergey'} );
```

Вызов функции сразу после ее объявления (immediately invoked function expression)
(() => console.log('IIFI'))(); //IIFI

Стрелочную функцию:
* Нельзя использовать как конструктор.
* Нельзя использовать bind call apply - так как в стрелочных функциях мы не можем менять значение this, так как оно автоматически берется из контекста.

Пример перебора:
```
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let sum = 0;
numbers.forEach(num => sum += num);
console.log(sum) // 55
```

Решение проблемы контекста с помощью стрелочной функции 
```
let person = {
    name: 'Sergey',
    greet: function() {
    var that = this; //Сохраняем контекст объекта.
    setTimeout(function() {
        console.log('Hello, my name is' + " " + that.name);
      }, 2000);
    }
};

person.greet() //Hello, my name is Sergey
```

Так как у setTimout контекст window  мы сохранили контекст в переменную что бы c помощью замыкания иметь доступ к контексту объекта.
Данную проблему можно решить с помощью стрелочной функции:
```
let person = {
  name: 'Sergey',
  greet: function () {
    setTimeout(() => {
      console.log('Hello, my name is' + " " + this.name);
    }, 2000);
  }
};

person.greet() //Hello, my name is Sergey
```

### классы
Классы это функции которые создают объекты через ключевое свойство new (синтаксический сахар).
```
class Task {
  constructor(title = Task.getDefaultTitle()) {
    this.title = title
    this._done = false;
    Task.count += 1;
  }
  get done() {
    return this._done === true ? 'Выполнено' : 'Не выполнено';
  }
  set done(value) {
    if (value !== undefined && typeof value === 'boolean') {
      this._done = value;
    } else {
      console.error();
    }
  }
  complete() {
    this.done = true; //Почему done без _ ?
  }

  static getDefaultTitle() {
    return 'Task';
  }
}
let task = new Task('Any');
```

* constructor - особый метод который вызывается в момент создания объекта, он создает свойства и инициализирует его (подготавливает объект к использованию, может быть только один и если его не создать то класс создаст самостоятельно пустой конструктор)  
* свойства - характеристики объекта которые создаются в конструкторе 
* методы - функции объекта //compleate()
* статические свойства -  Task.count
* статические методы - это методы которые принадлежат самому классу а не объектам созданным на его основе, часто используются для создания вспомогательных функций //getDefaultTitle()
* get set (гетеры и сеттеры) внутри класса ведут себя как методы а снаружи как свойства, позволяют получить доступ и присвоить значения "настоящим свойствам" объекта.
(Названия свойств get и set не должны совпадать с названиями свойств объекта)
set принимает параметр который мы можем присвоить свойству предварительно проверив данный параметр.  

Наследование класса

```
class Task {
  constructor(title) {
    this.title = title;
    this.done = false;
  }
  complete() {
    this.done = true;
  }
}
class SubTask extends Task {

}

let task = new Task("Изучить ES5");
console.log(task)

let subtask = new SubTask("Изучить ES6");
console.log(subtask)
```

Если у подкласса нет своего конструктора он воспользуется конструктором родителя.
Метод complete также будет наследован. 

```
class Task {
  constructor(title) {
    this.title = title;
    this.done = false;
    console.log("Создание задачи в конструкторе Task")
  }
  complete() {
    this.done = true;
  }
}
class SubTask extends Task {
  constructor(title) {
    super(title)
    console.log("Создание задачи в конструкторе SubTask")
  }
}

let task = new Task("Изучить ES5");
console.log(task)

let subtask = new SubTask("Изучить ES6");
console.log(subtask)
```
Определили подклассу свой конструктор


super - вызывает родительский конструктор.  
super - также перезаписывает родительский метод.  

#### шаблонные строки
поддерживают многосторонность и имеют тип строки а значит есть возможность применять методы строк. toUpperCase() и так далее.  

Пример: 
function greet(name) {
    Console.log(`Првет` + `${name}`)
}
greet('Sergey'); //Привет Sergey

#### spread operator
оператор разворота в виде троеточие, позволяет разворачивать элементы массива для передачи в качестве аргумента функции или другого массива.
```
let staticLanguages = ['c', 'c++', 'java'];
let dynamicLanguages = ['js', 'php', 'ruby']
let languages = [...staticLanguages, 'c#', ...dynamicLanguages, 'python']
console.log(languages) // ['c', 'c++', 'java’, 'c#' , 'js', 'php', 'ruby', 'python']
```

#### rest parameters
```
function add(x, y, z) {
  console.log(x + y + z);
}

let number = [1, 2, 3];
add(...number); // 6
```

#### Деструктивное присваивание массивов
```
let languages = ['JavaScript', 'PHP', 'Phyton', 'Ruby'];
let [js, php, phyton, ruby] = languages;
console.log(js) // JavaScript
```
Значения из масива копируются по порядку.

#### Деструктивное присваивание объектов
Название переменных и свойств в данном случае должны совпадать. 
```
let person = {
  firstName: 'Sergey',
  lastName: 'Sagalatov'
}

let {firstName, lastName} = person;
console.log(firstName, lastName) // Sergey Sagalatov
```

Вариант когда названия переменных не совпадают 
```
let person = {
  firstName: 'Sergey',
  lastName: 'Sagalatov'
}

let { firstName: first, lastName: last } = person;
console.log(first, last) // Sergey Sagalatov
```

### как устроена асинхронность в js. Event loop.
```
console.log(’Start’) //1
console.log(’Start2’) //2
setTimeout(function() {
console.log(‘Inside timeout, after 2000 seconds’)
} //3
console.log(’End’) //4
```
1. Start
2. Start2
3. End
4. Inside timeout, after 2000 seconds
Асинхронность:
Регистрация -> Сторонние API -> Event loop  -> callStack  
Event loop: (очередь которая отдает события поочередно) 
setTimeout - пришла из браузерного api  метод глобального объекта window.  
[Пример Event Loop](latentflip.com/loupe)
        
### работа с асинхронностью; promise - основные методы, создание промиса из функции с колбэком
```
const p = new Promise(function (resolve, reject) {
  setTimeout(() => {
    console.log('Prepering data..')
    const backendData = {
      server: 'aws',
      port: 2000,
      status: 'working'
    }
    resolve()
  }, 2000)
})
p.then(() => {
  console.log("promise resolve");
})
```
После разрешения промиса у нас есть переменная p которая является промисом. 
Так как мы получаем ее из класса Promise, поэтому мы можем использовать у p метод then.  
Конструкция p.then - будет выполнена когда промис разрешится.  

#### промис из функции с колбэком (колбэк)
```
console.log('Request data..')
setTimeout(() => {
  console.log('Preparing data..')
  const backendData = {
    server: 'aws',
    port: 2000,
    status: 'working'
  }
  setTimeout(() => {
    backendData.modified = true
    console.log('Data received', backendData)
  }, 2000)
}, 2000)
```
Итог большая вложенность

```
const p = new Promise(function (resolve, reject) {
  setTimeout(() => {
    console.log("Preparing data...");
    const backendData = {
      server: "aws",
      port: 2000,
      status: "working"
    };
    resolve(backendData);
  }, 2000);
});

p.then(data => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      data.modified = true
      resolve(data)
    }, 2300)
  })
}).then(clientData => {
  clientData.port = 2020
  console.log(clientData)
  return clientData
})
  .catch(err => console.log('Error', err))
  .finally(() => console.log('Finally'))
  .catch //сработает при вызове rejcet
  .finally //сработает в любом случае

//Sleep - решение для установки таймера
const sleep = ms => {
  return new Promise(resolve => {
    setTimeout(() => resolve(), ms)
  })
}

//Метод All - отработает после выполнения всех промисов
Promise.all([sleep(2000), sleep(5000)]).then(() => {
console.log('All promises')
})

//Метод Race - отработает как только завершиться первый провис (позволяет определить какой первый завершается)
Promise.race([sleep(2000), sleep(5000)]).then(() => {
  console.log('Race promises')
})
```
