# webpack 主要功能
- 將 CSS、圖片與其他資源打包
- 打包之前預處理（Less、CoffeeScript、JSX、ES6 等）檔案
- 依 entry 文件不同，把 .js 分拆為多個 .js 檔案
- 整合豐富的 Loader 可以使用（Webpack 本身僅能處理 JavaScript 模組，其餘檔案如：CSS、Image 需要載入不同 Loader 進行處理）

# babel 主要功能
例如:現在用ES6語法開發，但實際上線的瀏覽器環境有些還不支援ES6語法，可使用 babel編譯成ES5語法讓瀏覽器看得懂


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
    filename: 'index_bundle.js',
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
  },
  // devServer 則是 webpack-dev-server 設定
  devServer: {
    inline: true,
    port: 8008,
  },
  // plugins 放置所使用的外掛
  plugins: [HTMLWebpackPluginConfig],
};
```
