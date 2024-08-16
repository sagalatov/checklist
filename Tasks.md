## Задачи по JS


1. Что выведет консоль и как исправить.  
```javaScript
var a = {
	lol: 'whut',
  kek: function() {
  	console.log(this.lol)
	}
}

var fn = a.kek ? a.kek : a. kek;
fn(); // ?  
```
___
2.  Чему в результате равен x.  
```javaScript
Promise.resolve(123)
  .then(x => x + 1)
  .catch(x => x + 2)
  .then(x => x + 3)
```
___
3. Написать функцию сложения вида sum(1)(2)(3)...:
```javaScript
function sum (num) {
    //...
}
sum(1)(2)(3)(4); // 10
```
<details>
  <summary>answer</summary>
  
  ```javascript
  function sum (num) {
    function fn(val) {
        return sum(num + val)
    }

    fn.valueOf = () => {
        return num
    }

    return fn
  }
  ```

</details>

___
4. Написать функцию с асинхронными запросами:
```javaScript

function getAuth() {} // return Promise (critical) with token

function getUser(token) {} // [return Promise with userId]
function getOrder(token, userId) {} // [return Promise]

async function getData() {
	// return Promise with {auth, order?, promo?} data
}
```
___
5. Что выведет консоль
```javaScript
let a = 1;
let b = { prop: 2 };
let c = { prop: 3 };
 
function foo(d, e, f) {
 d = -1;
 e = { prop: -1 };
 f.prop = -1;
}
 
foo(a, b, c);
 
console.log(a, b, c);
```
___
6. Какая очередность вывода будет в консоле
```javaScript
console.log(1);
 
setTimeout(function() {
  console.log(2);
}, 0);
 
setTimeout(function() {
  console.log(3);
}, 1);
 
console.log(4);
 
Promise.resolve()
  .then(function() {
    console.log(5);
  })
  .then(function() {
    console.log(6);
  });
 
console.log(7);
```
___
7. ⭐ Какая очередность вывода будет в консоле 
```javaScript
setTimeout(() => {
  console.log("timeoOut");
}, 0);

console.log(1);

new Promise((resolve) => {
  console.log("Promise");
  setTimeout(() => {
    console.log("777");
  }, 0);
  resolve();
})
  .then(() => {
    console.log("then1");
  })
  .then(() => {
    console.log("then2");
  });

console.log(4);

setTimeout(() => {
  console.log("timeout2");
}, 0);
```
___
8. Что будет в консоли и почему?:
```javaScript
var a = 5;
function test () {
    a = 10;
    if (false) {
        var a = 20;
    }
}
test();
console.log(a); // ?
```
___
9. Нечётные числа должны отсортироваться по возрастанию, а чётные должны остаться на своих местах:
```javaScript
const nums = [1,9,4,2,3,6,7,1,5]; // [1,1,4,2,3,6,5,7,9]
```
Та же задача с тестами: [codewars](https://www.codewars.com/kata/578aa45ee9fd15ff4600090d)

___
10. Сделать вложенный объект из строки:
```javaScript
const str = 'one.two.three.four.five';

/*{
    one: {
        two: {
            three: {
                ...
            }
        }
    }
}*/
```

<details>
  <summary>answer</summary>

  ```javaScript
   function getObj(str) {
    const arr = str.split(".").reverse();
    let result = {};

    for (let i = 0; i < arr.length; i++) {
      const element = arr[i];

      result = { [element]: result };
    }

     return result;
   }
  ```

</details>

___
11. Написать функцию, которая будет возвращать true если строка (слово) является **палиндромом**, иначе false:
TODO: Найти такую задачу с тестами на https://www.codewars.com/
```javaScript
function isPalindrom (str) {
    //...
}
isPalindrom('казак'); // true
isPalindrom('строка'); // false
isPalindrom('шалаш'); // true
```
___
12. Напишите функцию, проверяющую, являются ли две строки **анаграммами** друг друга (регистр букв не имеет значения). Важны только символы, пробелы или знаки препинания не учитываются:
TODO: Найти такую задачу с тестами на https://www.codewars.com/
```javaScript
function isAnagram (str) {
    //...
}
isAnagram('finder', 'Friend'); // true
isAnagram('hello', 'bye'); // false
```
___
13. Написать полифил для: filter, map, bind.  
  Тесты для [bind](https://codesandbox.io/s/mybind-r2eo1?file=/index.html)
___

14. Что выведет консоль и как исправить.

```javascript
const x = 1;
if (true) {
 console.log(x);
 let x = 2;
}
```

15. Что выведет консоль и почему?
```javascript
const b = [1, 2, 3];
const fn = (a, ...b) => a + b;
console.log(fn(1));
```

16. Что выведет консоль и почему?
```javascript
const a = { prop: 1 };
const b = { prop: 2 };
const c = { ...a, ...b, b };
b.prop = 5;
console.log(c);
```

17. Что выведет консоль и почему?
```javascript
Object.prototype.x = 10
console.log(window.x)
```

18. Написать обертку (example: await promiseTimeout(promise, 1000);)
```javascript
const promiseTimeout = (
  promise: Promise<any>,
  ms: number
): Promise<any> => {
}
```

19. Написать свою реализацию Array flat.
```javascript
const arr = [1, 2, 3, [1, 2], "6", [13, {}, 1, [1, "1"]]];

function arrFlat(array) {}

// console.log(arrFlat(arr));
// [1, 2, 3, 1, 2, 6, 13, {}, 1, 1, 1]
```

19. Написать свою реализацию Promise.All в функции getPromises.
```javascript
const arr = [1, 0, 5, 4, 8];

function fakeApi(ar) {
  return ar.map((item) => {
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve(item);
      }, Math.round(Math.random() * 1000));
    });
  });
}

const promiseAll = getPromises(fakeApi(arr));
promiseAll.then((data) => {
  console.log("data ", data);
});
// 1, 0, 5, 4, 8
```
