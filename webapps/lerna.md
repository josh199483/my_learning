[reference1](https://medium.com/lion-f2e/lerna-js-package-%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-e9ed360d1143)

[referance2](https://blog.logrocket.com/setting-up-a-monorepo-with-lerna-for-a-typescript-project-b6a81fe8e4f8/)

# install
install by yarn
```
yarn add global lerna
mkdir lerna-test
cd lerna-test
lerna init --independent
```

上面使用 independent mode 的原因是，當你的專案下有好幾個 package，p1,p2,p3，總不能你升級其中一個 package 到1.0.1，其他package 也要升到 1.0.1，所以這個模式的 version不會影響到其他package version

接著建立一個測試 module，這邊在命名時可以考慮使用一個前綴(@test)，這樣在以後安裝這些module的時候可以被歸類到同一個資料夾下，大家最有印象的應該就是 "@babel" 了！
```
lerna create @test/module1 -y
```
src
基本環境都設定好後，接著可以安裝一些所有 module 都會用到的 3rd-party library，記得要加 -W
```
yarn add ... -W
```

這個 command 可以把要安裝的 library 統一加到所有的 package.json
```
lerna add ...
```

這邊也可以指定特定 package 才要安裝到 package.json
```sh
lerna add ... --scope=@packages/module1
# 這邊也可以選擇用 --ignore，來忽略某個package
lerna add ... --ignore=@packages/module2

```

如果所有 module 都要安裝同樣的 library 會很肥大，以下 command 可以讓 library 統一在根目錄管理，如果 lerna.json 是使用 yarn 會報錯，可以使用 "useWorkspaces": true，就不需要 --hoist
```
lerna bootstrap --hoist
```

如果要刪除所有專案下的 node_modules
```
lerna clean
```

可以 build 出一個版本，但要先有一個遠端連結的 git repo，並確定 commit 所有變更，如果使使用 independent mode，就會逐一詢問每個 package 的版本
```
lerna version
```
