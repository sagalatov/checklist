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
