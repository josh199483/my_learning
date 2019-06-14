# async/await 
## with for loop
[for loop and async in sequence or in parallel](https://blog.lavrton.com/javascript-loops-how-to-handle-async-await-6252dd3c795)

## axios => ajax request
[axios tutorial](https://ykloveyxk.github.io/2017/02/25/axios%E5%85%A8%E6%94%BB%E7%95%A5/)

## event loop and event emitter
[diff between event loop and event emitter](https://www.eebreakdown.com/2016/09/nodejs-eventemitter.html)

## promise detail
[promise detail](http://liubin.org/promises-book/#not-throw-use-reject)

## promise construction error!!!
[promise constructor antipattern](https://stackoverflow.com/questions/23803743/what-is-the-explicit-promise-construction-antipattern-and-how-do-i-avoid-it)

```js
// 1. 如果 getOtherPromise 發生錯誤會處理不了，使promise pending住，甚至導致 memory leak
// 2. promise 好處就是讓你使用 chain這個概念串聯下去
const access = () => {
  return new Promise((resolve, reject) => {
    getOtherPromise()
      .then(res => {
        resolve(res);
      });
  });
};
// convert to
const access = () => {
  return getOtherPromise()
    .then(res => {
      return res
  });
};
```

[promise in constructor...](https://stackoverflow.com/questions/24398699/is-it-bad-practice-to-have-a-constructor-function-return-a-promise)
