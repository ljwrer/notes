#自适应布局#
---------------------------
##viewport##
 一般默认为980，然后缩放到屏幕

	<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />
width设为设备宽度  
初始缩放为1  
用户缩放禁用

##布局##
窄边定宽  
其余自适应

##@media##
实现布局变化，显式/隐藏  
css应该覆盖

	@media screen and (max-width: 320px) {
		
	}
	@media screen and (min-width: 769px){
		
	}
	@media screen and (min-width: 769px) and (max-width: 1000px) {
		
	}