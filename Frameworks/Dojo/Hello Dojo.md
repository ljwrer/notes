---
##plugins

！结尾，放在依赖列表最后
dojo/text

	"dojo/text!./templates/NavBar.html"

dojo/i18n

	dojo/i18n!./nls/common
dojo/has

	"dojo/has!dom-addeventlistener?./events/w3c:./events/ie"
dojo/domReady

	"dojo/domReady!"

---
#Advanced AMD Usage
###require

 1. configuration 可选可忽略 默认undefined,可实时修改
	 - async, tlmSiblingOfDojo，has 不可修改
 2. dependencies 可选 默认[]
 3. callback

		require({
		    baseUrl: "/js/",
		    packages: [
			        { name: "dojo", location: "//ajax.googleapis.com/ajax/libs/dojo/1.10.4/" },
			        { name: "my", location: "my" }
			    ]
		}, [ "my/app" ]);

###define

1. moduleId不用提供
2. dependencies
3. factory 返回构造函数或者单例（ i18n）

###Configuring the loader
####async
	异步加载
	<script data-dojo-config="async: true" src="js/lib/dojo/dojo.js"></script>
####baseUrl,tlmSiblingOfDojo
	baseUrl: "/js/",
    tlmSiblingOfDojo: false,
不设置baseUrl和tlmSiblingOfDojo则从dojo.js的父目录加载

####packages	
支持commonJS加载	

 - name:与目录名一致
 - location:路径 ，baseUrl+相对路径或绝对路径
 - main：注入包名时的js,默认main.js 


###Using portable modules
	
	dojoConfig = {
	    packages: [
	        { name: "dojo16", location: "lib/dojo16" },
	        { name: "dijit16", location: "lib/dijit16" },
	        { name: "dojox16", location: "lib/dojox16" },
	        { name: "dojo", location: "lib/dojo" },
	        { name: "dijit", location: "lib/dijit" },
	        { name: "dojox", location: "lib/dojox" },
	        { name: "myOldApp", location: "myOldApp" },
	        { name: "my", location: "my" }
	    ],
	    map: {
	        myOldApp: {
	            dojo: "dojo16",
	            dijit: "dijit16",
	            dojox: "dojox16"

	        }
	    }
	};

###Writing portable modules

 - 同一包内使用相对路径注入依赖 ../ ./
 - 如不使用./将被作为包名
 - 只能在define内使用相对路径

包与包的依赖需要声明

	var map16 = {
	    dojo: "dojo16",
	    dijit: "dijit16",
	    dojox: "dojox16"
	};
	
	map: {
	        dojo16: map16,
	        dijit16: map16,
	        dojox16: map16,
	        myOldApp: map16
	    }

###Conditionally requiring modules
注入require可以使用相对路径,以免require作为全局变量而使用baseUrl 

可以注入images, templates, CSS

	dom.byId("infoBoxImage").src = require.toUrl("./images/info.png")

###Handling circular dependencies
注入exports

###Loading non-AMD code


 - /
 - http:/https
 - .js结尾

dojo.provide（）


---

#Configuring Dojo with dojoConfig
 - dojoConfig定义在加载dojo.js之前
 - dojo/_base/config记录dojoConfig以备后用
 - data-dojo-config与dojoConfig混淆

###has()配置
特性检测	

	dojoConfig = {
        has: {
            "dojo-amd-factory-scan": false
        }
    };

###Debug/Firebug Lite配置

		dojoConfig = {
	        has: {
	            "dojo-firebug": true,
	            "dojo-debug-messages": true，
 				"dojo-guarantee-console":true,
				"debugContainerId":
				"popup":
	        }
	    };

###Loader 配置
baseUrl:当转化一个模块标识符为地址或者URL时，baseUrl将添加给它
	
	baseUrl:
packages: 一个对象数组，它提供了包裹名字和位置。

	packages: [{
	    name: "myapp",
	    location: "/js/myapp"
	}]
map:映射路径

	map:
	 {
        dijit16: {
            dojo: "dojo16"
        }
    }
paths: 模块名到文件路径

	var dojoConfig = {
	    packages: [
	        "package1",
	        "package2"
	    ],
	    paths: {
	        package1: "../lib/package1",
	        package2: "/js/package2"
	    }
	};
	
	    // ...is equivalent to:
	var dojoConfig = {
	    packages: [
	        { name: "package1", location: "../lib/package1" },
	        { name: "package2", location: "/js/package2" }
	    ]
	};

async: 定义Dojo核心是否异步加载，可以设置为true, false 或 legacyAsync, 这使得loader永久处在legacy跨域模式。

	async: true

parseOnLoad: 如果为true，当DOM和所有初始依赖项(包含在dojoConfig.deps数组中)被载入后使用dojo/parser解析页面。
推荐设置为false(或者省略这个属性)

deps: 一个应该在Dojo载入完成后立即载入的资源路径数组。
deps:["dojo/parser"]

callback: 一个在deps被取出后执行的回调函数。

waitSeconds: 等待加载模块超时时间，默认为0(永远等待)。

cacheBust:如果为true，添加时间值到每个模块URL中作为querystring避免模块缓存。

map--> -->packages-->name-->location-->paths-->baseUrl

locale:
	
	查询参数？locale=''
	dojo.config.locale
	dojoConfig.locale

多语言
	 extraLocale:[]
	 dojo/parser
	 lang=

"dojo/_base/config"
dojo.config

	dojoConfig配置
	mixin
	服务端填充
	cookie填充
	查询参数填充
	data-dojo-config

#Modern Dojo
dojo/on处理dom事件和控件的事件

aspect.after()替换dojo.connetct()处理回调
aspect可以在函数执行之前或之后回调
dojo/topic

#CDN
库文件部署在CDN
其他模块部署在本地 
配置dojoBlankHtmlUrl

	var dojoConfig = {
					dojoBlankHtmlUrl: location.pathname.replace(/\/[^/]+$/, '') + '/blank.html',
					packages: [{
						name: 'custom',
						location: location.pathname.replace(/\/[^/]+$/, '') + '/js/custom'
					}]
				};
