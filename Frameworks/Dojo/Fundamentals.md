#Creating Builds
###Modules and Packages
package-->package.json

package.js  Dojo-specific build information
###dojoConfig
###Layers

	layers: {
		"dojo/myGrid": {
			include: [  
						"dojo/json","dojo/data/ObjectStore", 
	
                        "dojox/grid/enhanced/plugins/Pagination", 

                        "dojox/grid/enhanced/plugins/Filter", 

                        "dojox/grid/EnhancedGrid", 

                        "dojo/store/JsonRest", 

                        "dojo/domReady" ],
			customBase: true,
			boot: true
		}
	}

---
##Laying Out Your Application

	src:{
			dojo
			dijit
			dojox
			app
		}

##packages
main application package directory:

 - package.json
	 - 位于package根目录
	 - http://wiki.commonjs.org/wiki/Packages/1.0

			{
			   "name": "mypackage",
			   "version": "0.7.0",
			   "description": "Sample package for CommonJS. This package demonstrates the required elements of a CommonJS package.",
			   "keywords": [
			       "package",
			       "example" 
			   ],
			   "maintainers": [
			       {
			           "name": "Bill Smith",
			           "email": "bills@example.com",
			           "web": "http://www.example.com" 
			       } 
			   ],
			   "contributors": [
			       {
			           "name": "Mary Brown",
			           "email": "maryb@embedthis.com",
			           "web": "http://www.embedthis.com" 
			       } 
			   ],
			   "bugs": {
			       "mail": "dev@example.com",
			       "web": "http://www.example.com/bugs" 
			   },
			   "licenses": [
			       {
			           "type": "GPLv2",
			           "url": "http://www.example.org/licenses/gpl.html" 
			       } 
			   ],
			   "repositories": [
			       {
			           "type": "git",
			           "url": "http://hg.example.com/mypackage.git" 
			       } 
			   ],
			   "dependencies": {
			       "webkit": "1.2",
			       "ssl": {
			           "gnutls": ["1.0", "2.0"],
			           "openssl": "0.9.8" 
			       } 
			   },
			   "implements": ["cjs-module-0.3", "cjs-jsgi-0.1"],
			   "os": ["linux", "macos", "win"],
			   "cpu": ["x86", "ppc", "x86_64"],
			   "engines": ["v8", "ejs", "node", "rhino"],
			   "scripts": {
			       "install": "install.js",
			       "uninstall": "uninstall.js",
			       "build": "build.js",
			       "test": "test.js" 
			   },
			   "directories": {
			       "lib": "src/lib",
			       "bin": "local/binaries",
			       "jars": "java" 
			   } 
			}
 - <package.name>.profile.js
	 - 可改为package.js
	 - 位于包根目录
	 - Typically, you'll have just one package containing your entire application, but if you've split your code into multiple packages (for instance, with shared/common modules in a different package), you will need to create both of these files for each package.

##Package Descriptors
package:json

	{
	    "name": "app",
	    "description": "My Application.",
	    "version": "1.0",
	    "keywords": ["JavaScript", "Dojo", "Toolkit", "DojoX"],
	    "maintainers": [{
	        "name": "Kitson Kelly"
	    }],
	    "contributors": [{
	        "name": "Kitson Kelly"
	    },{
	        "name": "Colin Snover"
	    }],
	    "licenses": [{
	        "type": "AFLv2.1",
	        "url": "http://bugs.dojotoolkit.org/browser/dojox/trunk/LICENSE#L43"
	    },{
	        "type": "BSD",
	        "url": "http://bugs.dojotoolkit.org/browser/dojox/trunk/LICENSE#L13"
	    }],
	    "bugs": "https://github.com/example/issues",
	    "repositories": [{
	        "type": "git",
	        "url": "http://github.com/example.git",
	        "path": "packages/app"
	    }],
	    "dependencies": {
	        "dojo": "~1.10.4",
	        "dijit": "~1.10.4",
	        "dojox": "~1.10.4"
	    },
	    "main": "src",
	    "homepage": "http://example.com/",
	    "dojoBuild": "app.profile.js"
	}
	
build属性:

`dojoBuild`:指定`<package.name>.profile.js`

##The Package Build Profile

	var profile = (function(){
	    return {
	        resourceTags: {
				//将此包内所有.js文件添加amd tag
	            amd: function(filename, mid) {
	                return /\.js$/.test(filename);
	            }
	        }
	    };
	})();

 - IIFE
 - 非primary build profile只需resourceTags，检测是否为js文件
 - 通过调用方resourceTags内部函数如 amd ,返回true则加入amd tag

resourceTags:

filename:文件名，如dojo.js  
mid：模块名 如dojo/dojo

 - amd
	 - The resource is an AMD module.
	 - 不声明也没问题但会报警
 - declarative
	 - The resource uses declarative markup you want to scan for dependencies.
	 - 指定使用声明式加载的页面
	 - dojo/parser 会负责注入依赖
	 - 只会注入依赖不会注入页面，如需注入页面，需使用dojo/text？？？？？？
	 - 使用dojo/text注入页面以后，页面的依赖不会注入
	 - [http://dojotoolkit.org/reference-guide/1.10/build/transforms/depsDeclarative.html](http://dojotoolkit.org/reference-guide/1.10/build/transforms/depsDeclarative.html)

			<div data-dojo-type="dijit/layout/ContentPane">
			  <button type="button" data-dojo-type="dijit/form/Button">Click Me!</button>
			</div>
			
			resourceTags: {
				//给.html或.htm文件添加declarative标签，打包工具会搜索声明式指令并注入依赖
			  declarative: function(filename){
			    return /\.htm(l)?$/.test(filename); // tags any .html or .htm files as declarative
			  }
			}

			layers: {
			  "dojo/dojo": {
			    include: [ "dojo/dojo", "app/index.html" ],
			    customBase: true,
			    boot: true
			  }
			}
			
			packages:[{
			  name: "dojo",
			  location: "../dojo"
			},{
			  name: "dijit",
			  location: "../dijit"
			},{
			  name: "dojox",
			  location: "../dojox"
			},{
				//指定页面所在包的位置
			  name: "app",
			  location: "./app"
			}]
			
			//支持声明式require语法
			<script type="dojo/require">
			  on: "dojo/on"
			</script>

 - test
	 - The resource is part of the test code of the package.
 - copyOnly
	 - The resource should be copied to the destination location and otherwise left unaltered.
 - miniExclude
	 - The resource should not be copied to the destination if the profile property mini is truthy.

			var profile = (function(){
			    var testResourceRe = /^app\/tests\//,
			        // checks if mid is in app/tests directory
			
			        copyOnly = function(filename, mid){
			            var list = {
			                "app/app.profile": true,
			                // we shouldn't touch our profile
			                "app/package.json": true
			                // we shouldn't touch our package.json
			            };
			            return (mid in list) ||
			                (/^app\/resources\//.test(mid)
			                    && !/\.css$/.test(filename)) ||
			                /(png|jpg|jpeg|gif|tiff)$/.test(filename);
			            // Check if it is one of the special files, if it is in
			            // app/resource (but not CSS) or is an image
			        };
			
			    return {
			        resourceTags: {
			            test: function(filename, mid){
			                return testResourceRe.test(mid) || mid=="app/tests";
			                // Tag our test files
			            },
			
			            copyOnly: function(filename, mid){
			                return copyOnly(filename, mid);
			                // Tag our copy only files
			            },
			
			            amd: function(filename, mid){
			                return !testResourceRe.test(mid)
			                    && !copyOnly(filename, mid)
			                    && /\.js$/.test(filename);
			                // If it isn't a test resource, copy only,
			                // but is a .js file, tag it as AMD
			            }
			        }
			    };
			})();

dgrid profile示例：[https://github.com/SitePen/dgrid/blob/master/package.js](https://github.com/SitePen/dgrid/blob/master/package.js)

##The Application Build Profile
在app目录创建app.profile.js 

	var profile = (function(){
	    return {
			//基于app.profile.js所在目录
	        basePath: "./src",
			//build以后的文件目录，基于basePath,且覆盖目录中的文件
	        releaseDir: "../../app",
			//build以后的文件次目录
	        releaseName: "lib",
			//设为release
	        action: "release",
			//同dojoConfig
	        packages:[{
	            name: "dojo",
	            location: "dojo"
	        },{
	            name: "dijit",
	            location: "dijit"
	        },{
	            name: "dojox",
	            location: "dojox"
	        },{
	            name: "app",
	            location: "app"
	        }],
			//每个layer生成一个文件，即多个module合并产生的layer,layer之间的依赖需要使用include声明
	        layers: {
	            "dojo/dojo": {
	                include: [ "dojo/dojo", "dojo/i18n", "dojo/domReady",
	                    "app/main", "app/run" ],
	                customBase: true,
	                boot: true
	            },
	            "app/Dialog": {
	                include: [ "app/Dialog" ]
	            }
	        }
	    };
	})();

##Build Optimization

 - layerOptimize
	 - layer压缩工具
	 - shrinksafe (default)
	 - false (shrinksafe.keeplines)
	 - closure
 - optimize
	 - layer外文件压缩工具
	 - false (layerOptimize)
 - cssOptimize
	 - false(default)
	 - comments(消除注释换行)
 - mini
	 - false(default)
	 - true (丢弃包含miniExclude tag的文件)
 - stripConsole 
	 - normal(只保留console.error和console.warn)
	 - none
	 - warn
	 - all
 - selectorEngine
	 - 选择器引擎，可不设置
 - staticHasFeatures
	 - 其他功能，如dead code path removal with the Closure Complier

##Dead Code Path Removal
staticHasFeatures在build时强制修改配置文件  
[http://dojotoolkit.org/reference-guide/1.10/dojo/has.html](http://dojotoolkit.org/reference-guide/1.10/dojo/has.html)

##Layers

	var profile = {
	    layers: {
	        "app/main": {
	            include: [ "app/main" ],
	            exclude: [ "app/mail", "app/calendar" ]
	        },
	        "app/mail": {
	            include: [ "app/mail" ],
	            exclude: [ "app/main" ]
	        },
	        "app/calendar": {
	            include: [ "app/calendar" ],
	            exclude: [ "app/main" ]
	        }
	    }
	};

请注意，如果在app/main中没有其他不被任何非排除的依赖链要求的共享组件，你需要把它们加入到组件列表中，从而涵盖到这个层面中来。例如，如果邮件和日历组件都用DataGrid，但是在app/main中并没有其他东西提到它， 它需要在主层面中被明确地规定，以防在app/mail和app/calendar中被分别编译。

dojo/main会默认被build进入dojo/dojo

	//自定义dojo.js

	//不包含dojo/main
	 customBase: true,
	//包含AMD加载器
     boot: true

##Default Configuration

	defaultConfig: {
	    hasCache:{
	        "dojo-built": 1,
	        "dojo-loader": 1,
	        "dom": 1,
	        "host-browser": 1,
	        "config-selectorEngine": "lite"
	    },
	    async: 1
	},