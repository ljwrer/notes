#第9章 客户端检测
##9.1 能力检测

	function isHostMethod(object, property) {
        var t = typeof object[property];
        return t == 'function' ||
                (!!(t == 'object' && object[property])) ||
                t == 'unknown';
    }

###能力检测，不是浏览器检测
在实际开发中，应该将能力检测作为确定下一步解决方案的依据，而不是用它来
判断用户使用的是什么浏览器。

    //确定浏览器是否支持 Netscape 风格的插件
    var hasNSPlugins = !!(navigator.plugins && navigator.plugins.length);
    //确定浏览器是否具有 DOM1 级规定的能力
    var hasDOM1 = !!(document.getElementById && document.createElement && document.getElementsByTagName);

##9.2 怪癖检测
仅检测那些对你有直接影响的“怪癖”，而且最好在脚本一开始就执行此类检测

	var hasDontEnumQuirk = function(){
        var o = { toString : function(){} };
        for (var prop in o){
            if (prop == "toString"){
                return false;
            }
        }
        return true;
    }();
	var hasEnumShadowsQuirk = function () {
        var o = {
            toString: function () {
            }
        };
        var count = 0;
        for (var prop in o) {
            if (prop == "toString") {
                count++;
            }
        }
        return (count > 1);
    }();

##9.3 用户代理检测
优先级排在能力检测和（或）怪癖检测之后
###9.3.1 用户代理字符串的历史
####IE
IE7及之前

	Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)

IE8
	
	Mozilla/4.0 (compatible; MSIE 版本号; 操作系统; Trident/Trident 版本号)
	Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0)
	// 兼容模式
	Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0)

IE9

	Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)
	// 兼容模式
	Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; Trident/5.0)

####Firefox
Firefox4以后

	Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox 4.0.1

####WebKit
Safari3以后

	Mozilla/5.0 (Macintosh; U; PPC Mac OS X; en) 
	AppleWebKit/522.15.5 (KHTML, like Gecko) Version/3.0.3 Safari/522.15.5

####Chrome

	Mozilla/5.0 ( 平台; 加密类型; 操作系统或 CPU; 语言) 
	AppleWebKit/AppleWebKit 版本号 (KHTML,like Gecko) Chrome/ Chrome 版本号 Safari/ Safari 版本
	// Chrome 7
	Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/534.7 (KHTML,like Gecko) Chrome/7.0.517.44 Safari/534.7 

#### iOS 和 Android
	
	Mozilla/5.0 (平台; 加密类型; 操作系统或 CPU like Mac OS X; 语言) 
	AppleWebKit/AppleWebKit 版本号 (KHTML, like Gecko) Version/浏览器版本号 Mobile/移动版本号 Safari/Safari 版本号
	//IOS
	Mozilla/5.0 (iPhone; U; CPU iPhone OS 3_0 like Mac OS X; en-us)
	AppleWebKit/528.18 (KHTML, like Gecko) Version/4.0 Mobile/7A341 Safari/528.16
	//Android
	Mozilla/5.0 (Linux; U; Android 2.2; en-us; Nexus One Build/FRF91)
	AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1

###9.3.2 用户代理字符串检测技术
不推荐指定具体版本的浏览器

	var client = function(){
	
	    //rendering engines
	    var engine = {            
	        ie: 0,
	        gecko: 0,
	        webkit: 0,
	        khtml: 0,
	        opera: 0,
	
	        //complete version
	        ver: null  
	    };
	    
	    //browsers
	    var browser = {
	        
	        //browsers
	        ie: 0,
	        firefox: 0,
	        safari: 0,
	        konq: 0,
	        opera: 0,
	        chrome: 0,
	
	        //specific version
	        ver: null
	    };
	
	    
	    //platform/device/OS
	    var system = {
	        win: false,
	        mac: false,
	        x11: false,
	        
	        //mobile devices
	        iphone: false,
	        ipod: false,
	        ipad: false,
	        ios: false,
	        android: false,
	        nokiaN: false,
	        winMobile: false,
	        
	        //game systems
	        wii: false,
	        ps: false 
	    };    
	
	    //detect rendering engines/browsers
	    var ua = navigator.userAgent;    
	    if (window.opera){
	        engine.ver = browser.ver = window.opera.version();
	        engine.opera = browser.opera = parseFloat(engine.ver);
	    } else if (/AppleWebKit\/(\S+)/.test(ua)){
	        engine.ver = RegExp["$1"];
	        engine.webkit = parseFloat(engine.ver);
	        
	        //figure out if it's Chrome or Safari
	        if (/Chrome\/(\S+)/.test(ua)){
	            browser.ver = RegExp["$1"];
	            browser.chrome = parseFloat(browser.ver);
	        } else if (/Version\/(\S+)/.test(ua)){
	            browser.ver = RegExp["$1"];
	            browser.safari = parseFloat(browser.ver);
	        } else {
	            //approximate version
	            var safariVersion = 1;
	            if (engine.webkit < 100){
	                safariVersion = 1;
	            } else if (engine.webkit < 312){
	                safariVersion = 1.2;
	            } else if (engine.webkit < 412){
	                safariVersion = 1.3;
	            } else {
	                safariVersion = 2;
	            }   
	            
	            browser.safari = browser.ver = safariVersion;        
	        }
	    } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
	        engine.ver = browser.ver = RegExp["$1"];
	        engine.khtml = browser.konq = parseFloat(engine.ver);
	    } else if (/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){    
	        engine.ver = RegExp["$1"];
	        engine.gecko = parseFloat(engine.ver);
	        
	        //determine if it's Firefox
	        if (/Firefox\/(\S+)/.test(ua)){
	            browser.ver = RegExp["$1"];
	            browser.firefox = parseFloat(browser.ver);
	        }
	    } else if (/MSIE ([^;]+)/.test(ua)){    
	        engine.ver = browser.ver = RegExp["$1"];
	        engine.ie = browser.ie = parseFloat(engine.ver);
	    }
	    
	    //detect browsers
	    browser.ie = engine.ie;
	    browser.opera = engine.opera;
	    
	
	    //detect platform
	    var p = navigator.platform;
	    system.win = p.indexOf("Win") == 0;
	    system.mac = p.indexOf("Mac") == 0;
	    system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);
	
	    //detect windows operating systems
	    if (system.win){
	        if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
	            if (RegExp["$1"] == "NT"){
	                switch(RegExp["$2"]){
	                    case "5.0":
	                        system.win = "2000";
	                        break;
	                    case "5.1":
	                        system.win = "XP";
	                        break;
	                    case "6.0":
	                        system.win = "Vista";
	                        break;
	                    case "6.1":
	                        system.win = "7";
	                        break;
	                    default:
	                        system.win = "NT";
	                        break;                
	                }                            
	            } else if (RegExp["$1"] == "9x"){
	                system.win = "ME";
	            } else {
	                system.win = RegExp["$1"];
	            }
	        }
	    }
	    
	    //mobile devices
	    system.iphone = ua.indexOf("iPhone") > -1;
	    system.ipod = ua.indexOf("iPod") > -1;
	    system.ipad = ua.indexOf("iPad") > -1;
	    system.nokiaN = ua.indexOf("NokiaN") > -1;
	    
	    //windows mobile
	    if (system.win == "CE"){
	        system.winMobile = system.win;
	    } else if (system.win == "Ph"){
	        if(/Windows Phone OS (\d+.\d+)/.test(ua)){;
	            system.win = "Phone";
	            system.winMobile = parseFloat(RegExp["$1"]);
	        }
	    }
	    
	    
	    //determine iOS version
	    if (system.mac && ua.indexOf("Mobile") > -1){
	        if (/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)){
	            system.ios = parseFloat(RegExp.$1.replace("_", "."));
	        } else {
	            system.ios = 2;  //can't really detect - so guess
	        }
	    }
	    
	    //determine Android version
	    if (/Android (\d+\.\d+)/.test(ua)){
	        system.android = parseFloat(RegExp.$1);
	    }
	    
	    //gaming systems
	    system.wii = ua.indexOf("Wii") > -1;
	    system.ps = /playstation/i.test(ua);
	    
	    //return it
	    return {
	        engine:     engine,
	        browser:    browser,
	        system:     system        
	    };
	
	}();

##小结
在决定使用哪种客户端检测方法时，一般应优先考虑使用能力检测。怪癖检测是确定应该如何处理代码的第二选择。而用户代理检测则是客户端检测的最后一种方案，因为这种方法对用户代理字符串具有很强的依赖性。