#布局解决方案#
##居中布局##
###水平居中###
宽度不定，父元素宽度不定
####inline-block+text-align####
块级元素默认宽度撑满父元素  
inline-block宽度为内容宽度  
text align:对子元素inline元素有效  
优点：  

- 兼容性好  
- display:inline+zoom：1在IE6,7模拟inline-block   

缺点：  

- child会继承text-align属性，需要单独调整