#Animation

- dojo/_base/fx
	- baseFx.fadeIn
	- baseFx.fadeOut
- dojo/fx
	- fx.slideTo
	- fx.wipeIn

###Animating Properties

	baseFx.animateProperty({
        node: dom.byId('anim8target'),
        properties: {
            borderWidth: 200,
            top:{start:25,end:200},
            left:0,
            opacity:{start:1,end:0},
        },
        duration:2000
    }).play();
	baseFx.fadeIn()
	baseFx.fadeOut()

###Easing(与css3类似)

	baseFx.animateProperty{
		easing:easing.	
	}

###Putting it Together

 - 链式动画
	- fx.chain
- 同时动画
	- fx.combain
- 执行之前回掉
	- aspect.before(foo,'beforeBegin',callback)
- 执行之前回掉
	- on(foo,'End',callback)

---

#Dojo DOM Functions
##Retrieval查询
###dom.byId
 - param:string/node
 - return:dom节点

##Creation
###domConstruct.create
 - param
	 - string// node name
	 - obj// properties
	 - node// parent/sibling
	 - string// position

##Placement
###domConstruct.place
 - param
	 - string(id)/node
	 - node //ref
	 - position //'last'(default)  "before", "after", "replace", "only", "first", and "last".

##Destruction
###domConstruct.destory
本身及子节点
###domConstruct.empry
子节点

---

#Dojo Effects
所有动画函数：
param:

	{
		node:stringID/node
		duration:350(default)
	}
return:

	dojo/_base/fx::Animation
	play, pause, stop, status, and gotoPercent

###Fading
fx.fadeIn/fx.fadeOut

###Wiping
高度
fx.wipeIn/fx.wipeOut

height:auto

###Sliding
fx.slideTo

left,top

###Animation Events
on()

 - beforeBegin
 - onEnd	

将beforeBegin放入动画参数保证执行

###Chaining
fx.chain([obj::anim])

###Combining
fx.combine([obj::anim])

---

#Events with Dojo
##on
on(element,event name,handler)
on.once()
 
 - return handle

handle

 - remove()
	
event	

 - target
 - stopPropagation()
 - preventDefault()

lang.hitch(context,handle)
修改作用域

##NodeList events
node.on(event name,handler)

 - return array::handles

##Event Delegation
on(parent element, "selector:event name",handler)

 - !需声明dojo/query
 - !this指向子节点

##Object Methods
dojo/aspect监听函数事件

##Publish/Subscribe
自定义事件
dojo/topic

topic.subscribe('name',handle1,handle2...)

 - return obj
	 - obj.remove()

topic.publish('name',args)

---

#Keyboard Events with Dojo
onkeydown-->onkeypress-->onkeyup

dojo/keys

 - keys.RIGHT_ARROW;
 - keys.LEFT_ARROW
 - ...

###The KeyboardEvent object
event.keyCode===keys.keyName

dojo.NodeList-traverse
扩展node prev next children

---

#Using dojo/query
query(selector,parentNpde)
###NodeList
return NodeList::Array

NodeList.forEach(function,scope)
map, filter, every, and some

###dojo/NodeList-dom
支持链式操作

style, toggleClass, replaceClass, place, and empty
removeClass,addClass

on

---

#NodeList Extensions
dojo/NodeList
导入nodelist扩展包 dojo/ dojox/ 扩展nodelist方法

###dojo/notelist
map, filter, every, and some

###dojo/NodeList-data

 - node.data()
 - node.removeData()

### dojo/NodeList-fx
auto: true返回节点，不设置返回动画对象

###  dojo/NodeList-traverse 
parents, siblings, and children 
closest(), prev(), and next()

### Manipulating Elements
dojo/NodeList-manipulate
clone() prepenf() appendTo()

#Advanced Content Injection
 dojo/NodeList-html

html.set(node, cont, params)