# js事件

js中事件的三要素:事件源，事件，事件处理程序三部分组成

## 事件流

### 概念

描述从页面中接受事件的顺序。

### 事件捕获

事件在最外层就被捕获，然后沿着DOM树依次向下传播到事件的实际

### 事件冒泡

事件从事件发生源一直向上传递

具体实现:IE9、Firefox、Chrome 和 Safari 则将事件一直泡会跳过 <html> 元素(从 <body> 直接跳到 document )冒泡到 window 对象


### DOM事件流

先捕获然后在目标上触发，最后冒泡到document文档

## 事件处理

### HTML事件处理

添加到HTML结构中

```html
<button onclick="alert('hello');">按钮</button>
```

### DOM0级

把函数赋值给一个事件处理程序，被认为元素的方法，即该处理程序是在元素的作用域上进行的，也就是说this等于引用当前元素

```js
obj.onclick = function(event){ ....};
```
移除事件
```js
btn.onclick = null;
//删除事件处理程序
```

注意:
1. 为一个元素注册一个事件


#### 常用的事件

事件名 | 事件说明
------|--------
onlick| 鼠标单击
ondbclick| 鼠标双击
onkeyup| 按下并释放键盘上的一个键时触发
onchange| 文本内容或下拉菜单中的选项发生改变
onfocus| 获取焦点
onblur| 失去焦点
onload| 文档加载事件
onunload| 网页关闭时
onsubmit| 表单提交
onreset | 重置表单时

### DOM2级

```js
obj.addEventListener('事件名'，'事件处理函数'，'布尔值')
obj.removeEventListener('事件名'，'事件处理函数'，'布尔值') // 移除的参数要与添加时一致，若是事件处理函数为匿名，那么就无法移除了
//true：捕获
//false：冒泡
```
### IE处理

由于 IE8 及更早版本只支持事件冒泡,所以通过attachEvent() 添加的事件处理程序都会被添加到冒泡阶段。

注意:
1. 事件名前有on
2. 在全局作用域中进行事件处理程序，this为window
3. 虽然和DOM2一样为一个元素添加多个事件处理程序，不同的是，DOM2是按添加顺序执行的，而它相反

```js
obj.attachEvent("on事件名", "事件处理函数")
obj.detachEvent("on事件名", "事件处理函数")
```

### 解决事件兼容性问题

重点在事件冒泡

EventUtil 的对象的方法可以处理浏览器的差异

#### 方法

```js
var EventUtil = {
addHandler: function(element, type, handler){
if (element.addEventListener){
element.addEventListener(type, handler, false);
} else if (element.attachEvent){
element.attachEvent("on" + type, handler);
} else {
element["on" + type] = handler;
}
},
removeHandler: function(element, type, handler){
if (element.removeEventListener){
element.removeEventListener(type, handler, false);
} else if (element.detachEvent){
element.detachEvent("on" + type, handler);
} else {
element["on" + type] = null;
}
}
};
```
```js
//使用
var btn = document.getElementById("myBtn");
var handler = function(){
alert("Clicked");
};
EventUtil.addHandler(btn, "click", handler);
//这里省略了其他代码
EventUtil.removeHandler(btn, "click", handler);
```

## 事件对象 event

### 必有属性/方法（可读）

1. bubbles:是否冒泡，布尔值
2. cancelable:是否可以取消默认行为,布尔值
3. defaultPrevented:为 true表示已经调用preventDefault(),布尔值
4. detail:与事件相关法细节信息,int类型
5. eventPhase:调用事件处理程序的阶段:1表示捕获阶段,2表示“处于目标”,3表示冒泡阶段,int类型
6. stopImmediatePropagation():取消事件的进一步捕获或冒泡,同时阻止任何事件处理程序被调用
7. stopPropagation()：取消事件的进一步捕获或冒泡。如果 bubbles为 true ,则可以使用这个方法
8. target：事件源
9. trusted：为 true 表示事件是浏览器生成的。为 false 表示 事 件 是 由 开 发 人 员 通 过 JavaScript 创 建 的
10. type：事件发生类型

### IE中的event对象

属性和方法 | 类型 | 读写 | 说明
---------|------|-----|-----
cancelBubble | Boolean | 读/写 | 默认值为 false ,但将其设置为 true 就可以取消事件冒泡
returnValue | Boolean | 读/写 | 默认值为 true ,但将其设置为 false 就可以取消事件默认行为
srcElement | Element | 读 | 事件目标
type | String | 读| 事件类型

### 阻止默认行为和冒泡/捕获

#### 阻止冒泡/捕获
event.stopPropagation()

IE:event.cancelBubble = true(IE不支持事件捕获，所以只是取消冒泡)

#### 阻止默认行为

cancelable 属性设置为 true 的事件

event.preventDefault()

IE:event.returnValue = false

## DOM3级事件

### 焦点事件

焦点事件 | 是否冒泡 | 说明
---------|------|-----
blur | no | 在元素失去焦点时触发，所有浏览器都支持
focus | no | 在元素获得焦点时触发，所有浏览器都支持
focusin | yes | 在元素获取焦点时触发，
focusout | yes | 在元素失去焦点时触发，

### 滚动事件

坐标名 | 关系 | 说明 | 是否随滚动条而变化
---------|------|-----| ------
clientX/clientY |  ..  | 客户区的坐标，即鼠标位置到浏览器的可视窗口的距离 | no
screenX/screenY |  ..  | 屏幕区的坐标，即鼠标位置到屏幕左边缘和上边缘的距离 | no
pageX/pageY |  = clientX/Y + scrollleft/top| 页面的坐标，即鼠标位置到页面上边和左边的距离 | yes
offsetX/offsetY |  ..  | 即鼠标位置到触发事件对象的X，Y距离 | no
x/y | ... |  设置或获取鼠标指针位置相对于父文档的 x/y 像素坐标。|

混杂模式:document.body

标准模式:document.documentElement

scrollleft/scrolltop :滚动条距离左边和上边的距离

