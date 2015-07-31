#第十一章 HTML5#
 - `<canvas>`
 - `<video> <audio>`
 - `<form>`

检测函数：
	
	function inputSupportsType(type){
		if(!document.createElement("input")) return false;
		var input=document.createElement("input");
		input.setAttribute("type",type);
		if(input.type==="text"&&type!=="text") return false;
		return true;
	}
	function elementSupportAttribute(ele,att){
		if(!document.createElement(ele)) return false;
		var element=document.createElement(ele);
		return (att in element);
	}