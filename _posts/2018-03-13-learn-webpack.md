---
layout: post
title: "Webpack学习笔记"
date: 2018-03-13
excerpt: "Webpack是现在很流行的打包配置工具，前端搭配主流框架使用，可以说是非常普遍了。对于学习来说，整个开发引入很多插件、工具库变得复杂化。对于开发来说，整个流程有了各种复制工具变得简单化。"
tags: [学习]
comments: false
---


## 原课程
imooc上看了一个webpack+vue的教程，老师讲的非常好，记录一下   
[课程地址](https://www.imooc.com/learn/935)

## 工具和安装

工具：node + npm   

初始化命令：npm init   
初始化一个package.json文件，这个文件中描述了项目的基本信息，模块以来还有脚本命令等。   
安装命令：npm i
安装webpack包后创建webpack.config.js文件   

## 初步配置webpack.config.js文件

entry: 项目的入口，一个路径参数。使用绝对路径结合当前项目下主程序入口， __dirname + 'src/index.js'   
output: 把依赖打包的输出文件，需要声明路径参数和文件名。   

```
output:{
	filename: 'bundle.js',
	path: __dirname + 'dist'
}
```
module.rules: 定义编译处理文件的规则和工具依赖
```javascript
module:{
		rules:[
			{
				test:/\.vue$/, //文件名（正则表达式）
				loader:'vue-loader' //所需要的loader
			},
			{
				test:/\.css$/,
				use:[
					'style-loader',
					'css-loader'
				]
			},
			{
				test:/\.(gif|jpg|jpeg|png|svg)$/,
				use:[
					{
						loader:'url-loader',
						options:{ //包含参数的loader
							limit:1024,
							name:'[name].[ext]'
						}
					}
				]
			}
		]
	}
```

配置完loader后需要使用安装一下loader依赖   

### CSS预处理器
使用 **stylus-loader** 文件名为.styl   
或者使用 **less-loader** 和 **sass-loader**   

### webpack-dev-server 
> 开发环境和生产环境   

package.json添加脚本 `"dev":"webpack-dev-server --config webpack.config.js"`   
安装cross-env包 用来跨系统统一环境变量 npm i cross-env   
修改脚本运行   
`"build": "cross-env NODE_ENV=production webpack --config webpack.config.js"` 生产环境   
`"dev":"cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"`  开发环境   
webpack.config.json   
新建变量 isDev 判断是否是开发环境    
`const isDev = process.env.NODE_ENV === 'development'` (true则为开发环境)   
在末尾添加判断语句   
判断是开发环境 则添加一个config.devServer的对象，更改配置   
*注：此处config为前面的整个配置对象*
```javascript
if(isDev){
	config.devServer = {
		port:8000,
		host:'0,0,0,0', //可以在内网通过IP访问 这里我报错了 怀疑权限问题
		overlay:{
			errors:true, //显示错误在页面上
		}
	}
}
```
## 插件
plugins: 项目所需要的插件。安装所需插件，在文件开头引入，并在plugins创建相应的插件对象。   
**html-webpack-plugin**   
`webpack.DefinePlugin({'process.env':NODE_DEV:isDev?'"development"':'"production"'})` 判断环境   
**hot-module-replacement**   







