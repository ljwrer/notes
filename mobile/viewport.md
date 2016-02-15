1. 移动设备可用布局宽度 = 浏览器的视口宽度（设备屏幕大小）/ 缩放值；
2.

		<style>
		    @import url("main.css") screen; /* 针对屏幕加载main.css */
		    @media print {
		        /* 针对打印机的样式 */
		    }
		</style>

		<head>
		    <link rel="stylesheet" href="main.css" media="screen" />
		    <link rel="stylesheet" href="paper.css" media="print" />
		    <link rel="stylesheet" href="tiny.css" media="handheld"/>
		</head>
		<style>
		    @media all and (min-width:500px) { … }
		    @media (orientation: portrait) { … }
		</style>
		<head>
		    <link rel="stylesheet" href="wide.css" media="screen and (min-width:1024px)" />
		    <link rel="stylesheet" href="mobile.css" media="screen and (max-width:320px)" />
		</head>