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
7. Что будет в консоли и почему?:
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
