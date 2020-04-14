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

核心配置demo webpack.config.js 
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const webpack = require("webpack");
// 将CSS提取为独立的文件的插件，对每个包含css的js文件都会创建一个CSS文件，支持按需加载css和sourceMap
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const devMode = process.env.NODE_ENV !== "production";
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');

const config = {
	entry: "./src/index.js",
	output: {
		filename: "bundle.js",
		path: path.resolve(__dirname, "dist", "assets"),
		publicPath: "/assets/",
	},
	optimization: {
	    minimizer: [
	      new TerserPlugin({
		cache: true,
		parallel: true,
		sourceMap: true, // Must be set to true if using source-maps in production
		terserOptions: {
		  // https://github.com/webpack-contrib/terser-webpack-plugin#terseroptions
		}
	      }),
	    ],
	},
	module: {
		rules: [
			{
				test: /\.(sa|sc|c)ss$/,
				use: [
					{
						loader: devMode ? "style-loader" : MiniCssExtractPlugin.loader,
						options: {
							// 这里可以指定一个 publicPath
							// 默认使用 webpackOptions.output中的publicPath
							publicPath: "/assets/css/",
						},
					},
					{ loader: "css-loader" }, // 将 CSS 转化成 CommonJS 模块
					{ loader: "sass-loader" }, // 将 Sass 编译成 CSS
					{ loader: "postcss-loader" }, // 将 Sass 编译成 CSS
				],
			},
			// html中引用的静态资源在这里处理,默认配置参数attrs=img:src,处理图片的src引用的资源.
			{
				test: /\.html$/,
				loader: "html-loader",
				options: {
					// 除了img的src,还可以继续配置处理更多html引入的资源(不能在页面直接写路径,又需要webpack处理怎么办?先require再js写入).
					attrs: ["img:src", "img:data-src", "audio:src"],
					minimize: false,
					removeComments: true,
					collapseWhitespace: false,
				},
			},
			{
				test: /\.(png|jpg|jpeg|gif|svg)$/,
				use: [
					{
						loader: "url-loader",
						options: {
							limit: 5000, //小于该值的图片会使用base64编码，5000/1024k大约5kb
							name: "[name].[hash:8].[ext]", //打包后的图片名称 [ext]指图片格式
							outputPath: "/assets/img/", //这个是图片输出地址，图片路径不对时候需要注意
						},
					},
					{
						loader: "img-loader", // 图片压缩
						options: {
							plugins: [
								require("imagemin-mozjpeg")({
									//这个是处理图片的格式jpeg，需要处理哪些格式都需要在此配置，
									quality: "80", // 压缩质量，这个是压缩图片80%
								}),
								require("imagemin-pngquant")({
									floyd: 0.5,
									speed: 2,
								}),
							],
						},
					},
				],
			},
			{
				test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/, // 字体文件
				use: [
					{
						loader: "url-loader",
						options: {
							name: "[name].[hash:8].[ext]",
							limit: 5000, // fonts file size <= 5KB, use 'base64'; else, output svg file
							outputPath: "/assets/fonts/", //文字输出路径
						},
					},
				],
			},
			{
			      test: /\.m?js$/,
			      exclude: /(node_modules|bower_components)/,
			      use: {
				loader: 'babel-loader',
				options: {
				  presets: ['@babel/preset-env'],
				  plugins: [require('@babel/plugin-transform-object-rest-spread')]
				}
			      }
			},
		],
	},
	devServer: {
		hot: true,
	},
	plugins: [
		// 该插件将为你生成一个 HTML5 文件， 其中包括使用 script 标签的 body 中的所有 webpack 包。
		new HtmlWebpackPlugin({
			filename: "index.html",
			template: "template.html",
		}),
		new MiniCssExtractPlugin({
			filename: "[name].css",
			chunkFilename: "[id].css",
		}),
		new UglifyJsPlugin({
		    parallel: true,
		    uglifyOptions: {
			output: {
			    beautify: true,
			    comments: false
			},
			mangle: {
			    // keep_fnames: true
			},
			compress: {
			    warnings: false,
			    // keep_fnames: true,
			    negate_iife: false // we need this for lazy v8
			}
		    }
		}),
		new OptimizeCssAssetsPlugin({
		    assetNameRegExp: /\.css\.*(?!.*map)/g,
		    cssProcessor: require('cssnano'),
		    cssProcessorOptions: {
			discardComments: { removeAll: true },
			// 避免 cssnano 重新计算 z-index
			safe: true
		    },
		    canPrint: true
		}),
		new CopyWebpackPlugin([]),
		new webpack.DefinePlugin({
			"process.env.NODE_ENV": JSON.stringify(process.env.NODE_ENV),
		}),
		new webpack.HotModuleReplacementPlugin(),
	],
};

module.exports = config;

```
