## prototype
[這篇很詳細](https://blog.techbridge.cc/2017/04/22/javascript-prototype/)

```js
// 這個範例很清楚
// 一個 instance 的 __proto__ 會指向建構式的 prototype
function Person(name, age) {
  this.name = name;
  this.age = age;
}
  
Person.prototype.log = function () {
  console.log(this.name + ', age:' + this.age);
}
  
var nick = new Person('nick', 18);
  
// nick.__proto__ 會指向 Person.prototype
console.log(nick.__proto__ === Person.prototype) // true
  
// 那 Person.prototype.__proto__ 會指向誰呢？會指向 Object.prototype
console.log(Person.prototype.__proto__ === Object.prototype) // true
  
// 那 Object.prototype.__proto__ 又會指向誰呢？會指向 null，這就是原型鍊的頂端了
console.log(Object.prototype.__proto__) // null
```

## event
[capturing & bubbling](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)

## callback
[callback](http://eddychang.me/blog/javascript/83-js-callback.html)

## 遇到的問題
### Error: *.default is not a constructor
js 用 babel compile 後遇到這問題代表 import or export 出錯
```js
// export default 時其他檔案要直接import module
export default class Index {...}

import Index from './index';

// 若不是 export default 時則要 import {module}
export class Index1 {}
export class Index2 {}

import { Index1, Index2 } from './index';
```
### regeneratorRuntime is not defined
babel 無法 compile async/await 的寫法

npm install babel-plugin-transform-runtime –save-dev

[transform-runtime](https://calpa.me/2018/07/29/regenerator-runtime-is-not-defined)
```bash
# .babelrc
{
  "presets": [
    ["latest", {
      "es2015": { "modules": false }
    }]
  ],
  # add this
  "plugins": ["transform-runtime"]
}
```
## weird parts!!!
[weird part concept detail!](https://pjchender.blogspot.com/2016/07/javascript-frameworklibrary.html)

## memory leak
[memory leak in js](https://dzone.com/articles/eradicating-memory-leaks-in-javascript)

[setTimeout](https://stackoverflow.com/questions/14034107/does-javascript-setinterval-method-cause-memory-leak)

## map vs weakmap(ES6)
[diff between map and weakmap](https://stackoverflow.com/questions/15604168/whats-the-difference-between-es6-map-and-weakmap)

## how to implement private property in js
[private property](https://tw.twincl.com/javascript/*6937)

## axios
[axios advanced](http://coderlt.coding.me/2017/03/21/axios-api-md/)
