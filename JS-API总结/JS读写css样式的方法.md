[TOC]

##DOM节点对象的style对象(cssstyledeclaration对象)

##ELement对象的getAtrribute、setAttribute、removeAttribute

	div.setAttribute(
	  'style',
	  'background-color:red;' + 'border:1px solid black;'
	);

注意点：
* background-color可以写成backgroundColor
* css属性名是js保留字，规则名之前加上csscssFloat
* width = width + ‘px’注意px


##cssstyleDeclaration对象的cssText
读写或删除整个样式
	var divStyle = document.querySelector('div').style;

	divStyle.cssText = 'background-color: red;'
	  + 'border: 1px solid black;'
	  + 'height: 100px;'
	  + 'width: 100px;';
##检测css

	function isPropertySupported(property){
	  if (property in document.body.style) return true;
	  var prefixes = ['Moz', 'Webkit', 'O', 'ms', 'Khtml'];
	  var prefProperty = property.charAt(0).toUpperCase() + property.substr(1);
	
	  for(var i = 0; i < prefixes.length; i++){
	    if((prefixes[i] + prefProperty) in document.body.style) return true;
	  }
	
	  return false;
	}
	
	isPropertySupported('background-clip')
	// true

##window.getComputedStyle()

行内样式具有最高优先级，改变行内样式，会立即反应出来，获取最终样式

getComputedStyle(DOM节点).backgroundColor

* 还接受第二个伪元素，

   var result = window.getComputedStyle(div, ':before');