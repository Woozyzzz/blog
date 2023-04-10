# 工程化

## 1 常见 loader 和 plugin 有哪些？二者的区别是什么？

**记忆题**

1. 常见 loader
   1. `babel-loader` 把高级 JS/TS 转译为低级 JS，高级 JS 写着爽，低级 JS 兼容性好
   2. `ts-loader` 把 TS 转译为 JS，并提示类型错误
   3. `markdown-loader` 把 Markdown 转译为 JS 字符串，最终展示为 HTML
   4. `html-loader` 把 HTML 转译为 JS 字符串
   5. `sass-loader` 把 SASS/SCSS 转译为 CSS
   6. `css-loader` 把 CSS 转移为 JS 字符串
   7. `style-loader` 把 JS 字符串转译为 style 标签
   8. `postcss-loader` 把 CSS 变为更优化的 CSS，最好放在 `css-loader` 之前
   9. `vue-loader` 把单文件组件（SFC）变为 JS 模块
   10. `thread-loader` 用于多进程打包
2. 常见 plugins
   1. `html-webpack-plugin` 用于创建 HTML 页面并自动引入 JS 和 CSS
   2. `clean-webpack-plugin` 用于清理之前打包的残余文件
   3. `mini-css-extract-plugin` 用于将 JS 中的 CSS 抽离成单独的 CSS 文件
   4. `SplitChunksPlugin` 用于代码分包（Code Split）
   5. `DllPlugin` + `DllReferencePlugin` 用于避免大依赖被频繁重新打包，大幅降低打包时间
   6. `eslint-webpack-plugin` 用于检查代码中的错误
   7. `DefinePlugin` 用于在 webpack config 里添加全局变量
   8. `copy-webpack-plugin` 用于拷贝静态文件到 dist
3. 二者的区别
   1. loader 是文件加载器
      1. 功能：能够对文件进行编译、优化、混淆等，比如 `babel-loader` / `vue-loader`
      2. 运行时机：在创建最终产物之前运行
   2. plugin 是 webpack 插件
      1. 功能：能实现更多功能，比如：定义全局变量、代码分包、加速编译等
      2. 运行时机：在整个打包过程（以及前后）都可以运行

## 2 webpack 如何解决开发时的跨域问题？

- devServer
  - proxy
    - target
    - secure: false 忽略 HTTPS 报错
    - changeOrigin: true

## 3 如何实现 tree-shaking？

**记忆题**

1. 是什么
   1. tree-shaking 就是让没有用到的 JS 代码不打包，以减小包的体积
2. 怎么删
   1. 使用 ES Modules 语法（即 ES6 的 import 和 export 关键字）
   2. CommonJS 语法无法 tree-shaking（即 require 和 exports 语法）
      1. 需要给 babel-loader 添加 `modules: false` 选项
   3. 引用时候只引用需要的模块
      1. 要写 `import {cloneDeep} from "lodash-es"` ，方便 tree-shaking
      2. 不要写 `import _ from "lodash"` ，会导致无法 tree-shaking 无用模块
3. 怎么不删
   1. 在 package.json 中配置 sideEffects 数组，防止某些文件被删掉
      1. 比如：import 某个 JS 文件，其中包含 window.x = {}
      2. 比如：所有被 import 的 CSS 都要放在 sideEffects 里
4. 怎么开启
   1. 在 webpack config 中将 mode 设置为 production（开发环境没必要 tree-shaking）
   2. `mode: production` 给 webpack 加了非常多优化

## 4 如何提高 webpack 构建速度？

**记忆题**

1. 使用 `DllPlugin` 将不常变化的代码提前打包并复用，如 Vue、React
2. 使用 `thread-loader` 或 `HappyPack` (很久没人维护了)进行多进程打包
3. 处于开发环境时，在 webpack config 中将 cache 设为 true，也可用 `cache-loader`（过时）
4. 处于生产环境时，关闭不必要的环节，比如：可以关闭 sourceMap
5. 网传的 `HardSourceWebpackPlugin` 很久没更新了

## 5 webpack 与 vite 的区别是什么？

**记忆题**

1. 开发环境区别
   1. vite 不对代码打包，充分利用浏览器对 `<script type=module>` 的支持
      1. 假设 main.js 引入了 Vue
      2. 该 server 会把 `import { createApp } from "vue"` 改为 `import { createApp } from "/node_modules/.vite/vue.js"` 让浏览器知道依赖路径
   2. webpack-dev-server 常用 babel-loader 基于内存打包，比 vite 慢很多很多
      1. 该 server 会把 vue.js 的代码递归地打包进 main.js
2. 生产环境区别
   1. vite 使用 rollup + esbuild 来打包 JS 代码
   2. webpack 使用 babel 来打包 JS 代码，比 esbuild 慢非常多
      1. webpack 使用 esbuild 的配置很麻烦
3. 文件处理时机
   1. vite 只会在你请求某个文件的时候处理该文件
   2. webpack 会提前打包好 main.js，你请求时直接输出打包好的 JS
4. 目前已知 vite 的缺点：
   1. 热更新经常失败
   2. 有些功能 rollup 不支持，需要自己写 rollup 插件
   3. 不支持非现代浏览器

## 6 webpack 怎么配置多页应用？

- entry
  - JS 入口
- plugins
  - new HtmlWebpackPlugin({filename:"admin.html",chunks:["admin"]})
- 重复打包，都引入了 vue.js
  - 需要使用 optimization.splitChunks 将共同依赖单独打包成 common.js (HtmlWebpackPlugin 会自动引入 common.js)

## 7 swc、esbuild 是什么？

**记忆题**

- 二者对标 babel
- swc（由小年轻开发）
  - 实现语言：Rust
  - 功能：编译、打包 JS/TS
  - 优势：比 babel 快 20 倍以上
  - 能否集成进 webpack：能
  - 使用者：Next.js、Parcel、Deno、Vercel、ByteDance、Tencent、Shopify
  - 做不到：
    - TS 代码类型检查（用 tsc 可以）
    - 打包 CSS、SVG
- esbuild（由 CTO 开发）
  - 实现语言：Go
  - 功能：编译、打包 JS/TS
  - 优势：比 babel 快 10-100 倍以上
  - 能否集成进 webpack：能
  - 使用者：vite、vuepress、snowpack、umijs、blitz.js...
  - 做不到：
    - TS 代码类型检查（用 tsc 可以）
    - 打包 CSS、SVG
