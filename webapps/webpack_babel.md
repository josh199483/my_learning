# webpack 主要功能
- 將 CSS、圖片與其他資源打包
- 打包之前預處理（Less、CoffeeScript、JSX、ES6 等）檔案
- 依 entry 文件不同，把 .js 分拆為多個 .js 檔案
- 整合豐富的 Loader 可以使用（Webpack 本身僅能處理 JavaScript 模組，其餘檔案如：CSS、Image 需要載入不同 Loader 進行處理）

# babel 主要功能
例如:現在用ES6語法開發，但實際上線的瀏覽器環境有些還不支援ES6語法，可使用 babel編譯成ES5語法讓瀏覽器看得懂

# 配置 .babelrc
```bash
# 這個設定檔是針對 babel6 的，babel5 預設是全部載入
# presets 就是決定要編譯哪種語法
# es2015是編譯ES6，react是編譯jsx，stage-0是編譯ES7
# stage 又分 stage-0 到 stage-4階段，stage-0包含stage-1~stage-3所有功能
# 詳情可參考 https://www.vanadis.cn/2017/03/18/babel-stage-x/
{
  "presets": [
      "es2015"，
      "react",
      "stage-0"
  ],
  "plugins": []
}
```


# 透過 npm 安裝
```bash
mkdir myWeb # 建立資料夾
cd myWeb # 切換目錄
npm init # 初始化資料夾，會產生 package.json 設定檔
npm install --save-dev babel-core babel-eslint babel-loader babel-preset-es2015 babel-preset-react html-webpack-plugin webpack webpack-cli webpack-dev-server # 安裝需要的套件(webpack-cli 是最新版要裝的)
```
# 使用
```js
module.exports = {
  // 檔案起始點從 entry 進入，因為是陣列所以也可以是多個檔案
  entry: [
    './app/index.js',
  ],
  // output 是放入產生出來的結果的相關參數
  output: {
    path: `${__dirname}/dist`,
    filename: 'bundle.js',
  },
  resolve: {
    /* 設定 extensions 後 import 或 require 路徑只需要給檔名而不用加副檔名 */
    extensions: [ '.js', '.jsx' ],
  },
  module: {
  	// loaders 則是放欲使用的 loaders，在這邊是使用 babel-loader 將所有 .js（這邊用到正則式）相關檔案（排除了 npm 安裝的套件位置 node_modules）轉譯成瀏覽器可以閱讀的 JavaScript。preset 則是使用的 babel 轉譯規則，這邊使用 react、es2015。若是已經單獨使用 .babelrc 作為 presets 設定的話，則可以省略 query
    rules: [{
      test: /\.js$/,
      exclude: /node_modules/,
      use: [{
        loader: 'babel-loader',
      }]
    }],
  }
};
```

# plugin
## html-webpack-plugin
```js
// 這個 plugin 功能是讓 webpack 能自動產生 html 檔並且把前面設定的 output(bundle.js)
// 塞進 html 的script，例:<script type="text/javascript" src="bundle.js"></script>
// 指定 template 就是從現有的 html 複製一份再插入 script
plugins: [
  new HtmlWebpackPlugin({
    template: 'src/index.html'
  })
]
```

## setup env by webpack
[define plugin](https://www.cnblogs.com/tugenhua0707/p/9780621.html)

# babel 7(newest version)
frequently use
```bash
npm install --save-dev @babel/core @babel/preset-env @babel/preset-flow
babel-loader babel-eslint webpack webpack-cli webpack-dev-server
```
```json
"presets": [
  "@babel/preset-env",
  "@babel/preset-flow"
]
```

preset is set of plugin, if needed you can install plugin

for example i want to use this export usage
```js
export module from 'module'
```
you can install @babel/plugin-proposal-export-default-from and add in .babelrc
```json
{
  "plugins": ["@babel/plugin-proposal-export-default-from"]
}
```