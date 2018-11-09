# install 
[參考](https://codeburst.io/getting-started-with-flow-and-nodejs-b8442d3d2e57)

yarn add -D flow-bin

```json
// package.json
"scripts": {
  "flow": "flow",
  "flow:check": "flow check ./src/"
}
```

```bash
# .flowconfig
[ignore]
.*/node_modules/.* # 忽略那些檔案

[include]

[libs] # 使用 3-party lib
flow-typed

[options] # all=true，會 check 目錄下所有檔案，不然就只會check //@flow開頭的檔案
# all=true
```

# test
## step 1
```js
// @flow
type Point2d = {|
  x: number,
  y: number
|};
const myPoint: Point2d = {
  X: 1,
  y: 2
};
console.log(myPoint.x, myPoint.y);
```
yarn flow:check 

會報錯，因為 Point2d 只有小寫 x,y!!

## step 2
但實際 run 的時候，不會成功，因為這些 type 不會被 nodejs 編譯

所以有兩種方式解決

- 使用 babel
- 安裝 flow-removes-type，手動刪除這些 type

yarn add -D flow-removes-type
```json
// package.json
"flow:build": "flow-remove-types ./src/ -d ./lib/ --all --pretty",
```
yarn flow:build

## step3
使用 3-party lib

yarn add -D flow-typed
```json
// package.json
// 安裝需要套件
"flow:deps": "flow-typed install",
```
yarn add lodash

yarn flow:deps

```js
const _ = require('lodash');
type Point2d = {|
  x: number,
  y: number
|};
const points: Array<Point2d> = [
  { x: 1, y: 2},
  { x: -1, y: 2}
]
const numbers = [2, 3, 4];
function testFunc(point: Point2d):bool {
  return point.x > 0 && point.y > 0;
}
_.some(numbers, testFunc);
```
yarn flow:check 

會報錯!!

## special symbol meaning
The '+' symbol means the property is read only and

'-' means the property is write only and

If does not have any '+' or '-' symbol it means the property have both read/write access.

It can be used while defining interface property or type property

