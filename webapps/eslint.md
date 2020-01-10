## install 
```bash
# 會安裝基本上需要的所有 eslint package(airbnb)、plugin
npx install-peerdeps --dev eslint-config-airbnb
```

## eslintrc
[參考](https://www.robinwieruch.de/react-eslint-webpack-babel/)
### step 1
npm --save-dev install eslint-loader

```bash
# webpack.config.js
...
module: {
  rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader']
    },
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['eslint-loader']
    }
  ]
},
...
# 會報錯，No ESLint configuration found
```
### step 2
```bash
# 先在根目錄下新增 .eslintrc 檔案
# .eslintrc
{
  "rules": {
  }
}
# 再度報錯，The keyword 'import' is reserved，因為 eslint 不知道ES6語法
```
### step 3
npm install --save-dev babel-loader

```bash
# webpack.config.js
...
module: {
  rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader']
    },
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader', 'eslint-loader']
    }
  ]
},
...
```

npm install --save-dev babel-eslint

```bash
# .eslintrc
# 讓 eslint 能夠調整 es6 語法
{
  parser: "babel-eslint",
  "rules": {
  }
}
```

## rules
[eslint 所有 rules](https://eslint.org/docs/rules/)
```bash
# 設定單行長度不得超過幾個字元
...
"rules": {
  "max-len": [1, 70, 2, {ignoreComments: true}]
}
...
```

## eslint + react
npm --save-dev install eslint-plugin-react
```bash
# .eslintrc
{
  parser: "babel-eslint",
  "plugins": [
    "react"
  ],
  "rules": {
    "max-len": [1, 120, 2, {ignoreComments: true}],
    "prop-types": [2]
  }
}
```

## use airbnb eslint rules
```bash
# 使用最前面提到的指令安裝 airbnb 所需套件
# .eslintrc
{
  parser: "babel-eslint",
  "rules": {
    "max-len": [1, 120, 2, {ignoreComments: true}],
    "prop-types": [2]
  },
  "extends": ["airbnb-base"]
}
```

## eslint integrate with git
[pre-commit](https://larrylu.blog/improve-code-quality-using-eslint-742cf1f384f1)
