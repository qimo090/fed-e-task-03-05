# 基本

概念

- Vite 是一个面向现代浏览器的一个更轻、更快的 Web 应用开发工
  具
- 它基于 ECMAScript 标准原生模块系统（ES Modules）实现

项目依赖

- Vite
- @vue/compiler-sfc

基本使用

- vite serve
- vite build

vite serve

![vite serve](https://tva1.sinaimg.cn/large/007S8ZIlly1gj6vbxaddej31li0kutcc.jpg)

vue-cli-service serve

![vue-cli-service serve](https://tva1.sinaimg.cn/large/007S8ZIlly1gj6vbq2wcfj31ey0nodjp.jpg)

HMR

- Vite HMR
  - 立即编译当前所修改的文件
- Webpack HMR

  会自动以这个文件为入口重写 build 一次，所有的涉及到的依赖也都会被加载一遍

Build

- vite build
  - Rollup
  - Dynamic import
    - Polyfile

打包 or 不打包

- 使用 Webpack 打包的两个原因:
  - 浏览器环境并不支持模块化
  - 零散的模块文件会产生大量的 HTTP 请求

浏览器对 ES Module 的支持

![caniuse-es modules](https://tva1.sinaimg.cn/large/007S8ZIlly1gj6vc10f0ij321i0u011n.jpg)

开箱即用

- TypeScript - 内置支持
- less/sass/stylus/postcss - 内置支持（需要单独安装）
- JSX
- Web Assembly

Vite 特性

- 快速冷启动
- 模块热更新
- 按需编译
- 开箱即用

# 核心功能

- 静态 Web 服务器
- 编译单文件组件
  - 拦截浏览器不识别的模块，并处理
- HMR
