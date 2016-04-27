#grunt

#npm

	npm install <packgename>
	npmjs.org
	找不到则去package.json下去找dependencies

###yeoman

 - 脚手架工具
 - 生成器
 - 代码校验，测试，压缩

###bower

 - Web包管理
 - Web站点组成：框架，库，公共部分
 - 版本自动下载

###grunt

 - build tool
 - 自动化
	 - 压缩
	 - 编译
	 - 单元测试
	 - 代码校验

---

#yeoman
模具:GENERATOR

	npm install -g generator-<name>
	下载generator
	yo <generator name> <project name>
	生成目录
	!空格选择

##package.json
<pre>
{
	//应用名
  "name": "learnangular",
  "private": true,
//开发环境依赖包
  "devDependencies": {
	//^ 宽松的版本限制,第一位相同即可
	//如^5.2.1兼容5.3.1 不兼容 6.1.1
	//npm会自动更新兼容依赖包
	//~ 严格的版本限制,只允许最小版本号更新
	//如~5.2.2兼容5.2.5 不兼容 5.3.1
    "autoprefixer-core": "^5.2.1",// 
    "grunt": "^0.4.5",
    "grunt-angular-templates": "^0.5.7",
    "grunt-concurrent": "^1.0.0",
    "grunt-contrib-clean": "^0.6.0",
    "grunt-contrib-compass": "^1.0.0",
    "grunt-contrib-concat": "^0.5.0",
    "grunt-contrib-connect": "^0.9.0",
    "grunt-contrib-copy": "^0.7.0",
    "grunt-contrib-cssmin": "^0.12.0",
    "grunt-contrib-htmlmin": "^0.4.0",
    "grunt-contrib-imagemin": "^0.9.2",
    "grunt-contrib-jshint": "^0.11.0",
    "grunt-contrib-uglify": "^0.7.0",
    "grunt-contrib-watch": "^0.6.1",
    "grunt-filerev": "^2.1.2",
    "grunt-google-cdn": "^0.4.3",
    "grunt-jscs": "^1.8.0",
    "grunt-newer": "^1.1.0",
    "grunt-ng-annotate": "^0.9.2",
    "grunt-postcss": "^0.5.5",
    "grunt-svgmin": "^2.0.0",
    "grunt-usemin": "^3.0.0",
    "grunt-wiredep": "^2.0.0",
    "jit-grunt": "^0.9.1",
    "time-grunt": "^1.0.0",
    "jshint-stylish": "^1.0.0"
  },
	//制定项目所需的node版本 
  "engines": {
    "node": ">=0.10.0"
  },
	//可使用npm直接运行的脚本命令
  "scripts":{
	//npm:test  = grunt test
	 "test":"grunt test",
	 "install":""
	}
}

</pre>
##node_modules
存放开发过程中所依赖的node包

##Gruntfile.js
grunt配置文件

##bower.json
值定bower拉取的框架和组件

##.travis.xml
为开源打造的集成环境

##.jshintrc
jshint配置文件

##.gitignore
忽略当前不上传到git的文件

##.gitattributes
git配置项

##.editorconfig
统一的编辑器配置项，缩进，空格，需要插件

##.bowerrc
bower本身的配置项
<pre>
{
	//指定bower拉取存放的目录
  "directory": "bower_components"
}
</pre>

##test
自动化测试

##node modules
node包存放目录

##bower_compomemts
bower拉取的存放目录

##app
项目目录，源文件

views angular-view

#bash

	pwd
	当前路径
	mkdir
	生成目录	
	ls -al
	列出详细目录
	:wq
	退出
	rm -rf
	删除文件 递归删除 不需要二次确认

