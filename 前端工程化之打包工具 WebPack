### 前言

什么是前端工程化： 将前端的开发流程、技术选型、代码规范、构建发布等规范化，标准化，以提升开发效果及代码质量。

webpack 属于构建工具中的一种

实践：

1. 组件化 / 脚手架 / 工程 cli 工具
2. Git flow
3. Webpack
4. Jenkins + Docker

#### WebPack 5大核心概念

1. 入口 entry ：指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，例如 ./src
2. 输出 output ：告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，例如 ./dist
3. loader ：处理除了 js 意外的图片，css 等资源文件，webpack 本身是由 js 编写，只能识别 js 文件
4. plugin ：处理 loader 无法解决的问题及功能的扩展
5. 模式/兼容性

webpack.config.js
```
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const CleanWebpackPlugin = require('clean-webpack-plugin');
  const webpack = require('webpack');

  module.exports = {
    entry: {
       app: './src/index.js',
       print: './src/print.js'
       app: './src/index.js'
    },
    devtool: 'inline-source-map',
    devServer: {
      contentBase: './dist',
      hot: true
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement'
      }),
      new webpack.NamedModulesPlugin(),
      new webpack.HotModuleReplacementPlugin()
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```
