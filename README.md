# xcx
小程序开发指南  

### 文件结构
主文件app文件和pages页面组成，app页面包含了三个基本文件：  
（1）app.json  小程序公共设置，比如页面的路径配置，要显示或者跳转的路径必须在app.json对象的pages对象里面。（在这里配置的颜色值一定要是#加6位数字的颜色值，不能省略成三位数字）  
（2）app.js    小程序逻辑，比如登录，获取用户信息  
（3）app.wxss  小程序公共样式表  

一个pages里面的某个具体页面由四个文件组成，app.wxml,app.js,app,json,app.wxss,这个四个文件的名字必须一致。

微信框架被分为：  
（1）视图层描述语言：wxml，wxss；  
（2）逻辑层描述语言：js。  
在视图层与逻辑层间提供了数据传输和事件系统。   

框架的核心是一个响应的数据绑定系统。框架可以让数据和视图非常简洁的保持同步，当数据修改的时候，只要在逻辑层修改数据，视图层就会做相应更新。     

开发者通过框架将逻辑层的数据和视图层的数据进行绑定，当触发视图层的事件，视图层会发送相应的事件给逻辑层，逻辑层进行相应的处理变更数据，当逻辑层的数据发生变化的时候，由于逻辑层数据和视图层的数据已经进行了绑定，所以逻辑层数据的变化会同步到视图层数据的变化。   

### 模块化：可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 module.exports 或者 exports 才能对外暴露接口。
通过require来引入公共代码，require暂时不支持绝对路径。

### app()
app()函数用来注册一个小程序，接收一个object参数：  
（1）onLaunch：当小程序初始化完成时，会触发onLaunch，且只会触发一次；  
（2）onShow:当小程序启动，或者从后台进入到前台时启动；  
（3）onHide:当小程序从前台进入到后台触发；  
（4）onError：当小程序脚本错误或者api调用失败以后触发，附带错误信息。  
#### getApp()
全局的 getApp() 函数可以用来获取到小程序实例。
```
// other.js
var appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```
（1）App() 必须在 app.js 中注册，且不能注册多个。  
（2）不要在定义于 App() 内的函数中调用 getApp() ，使用 this 就可以拿到 app 实例。  
（3）不要在 onLaunch 的时候调用 getCurrentPages()，此时 page 还没有生成。  
（4）通过 getApp() 获取实例之后，不要私自调用生命周期函数。  
### page
Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等。  
（1）data对象：当前页面的数据源  
生命周期函数：    
（2）onLoad:--监听页面加载   
（3）onReady:--监听页面初次渲染完成  
（4）onShow:--监听页面显示  
（5）onHide:--监听页面隐藏  
（6）onUpload:-- 监听页面卸载  
监听相关事件处理函数：  
（7）onPullDownRefresh:--监听用户下拉动作  
（8）onReachBottom：--页面上拉触底的事件处理函数  
（9）onShareAppMessage:--用户点击右上角转发  
（10）onPageScroll:--页面滚动时触发事件的处理函数  

还包括用户的点击等事件，由用户自定义名称.  
可以通过Page.prototype.setData()来把数据从逻辑层发送到视图层（异步），同时改变this.data的值（同步）。  

setData()可以有两个参数，一个object（以key：value形式存在），和一个回调函数callback。  
object的key不需要在this.data中预先定义。    

注意点：  
（1）直接修改 this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致  
（2）单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据  

### 事件
bind绑定的事件不能阻止事件冒泡，catch绑定的事假可以阻止事件冒泡。  
捕获阶段监听事件时，可以采用capture-bind、capture-catch关键字。  

dataSet  
在组件中可以定义数据，这些数据将会通过事件传递给 SERVICE。 书写方式： 以data-开头，多个单词由连字符-链接，不能有大写(大写会自动转成小写)如data-element-type，最终在 event.currentTarget.dataset 中会将连字符转成驼峰elementType。
```
<view data-alpha-beta="1" data-alphaBeta="2" bindtap="bindViewTap"> DataSet Test </view>
```
```
Page({
  bindViewTap:function(event){
    event.currentTarget.dataset.alphaBeta === 1 // - 会转为驼峰写法
    event.currentTarget.dataset.alphabeta === 2 // 大写会转为小写
  }
})
```

#### wx:key
如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 input 中的输入内容, switch 的选中状态），需要使用 wx:key 来指定列表中项目的唯一的标识符。  

wx:key 的值以两种形式提供  :

（1）字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。  
（2）保留关键字 *this 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字，如：  
当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。  
如不提供 wx:key，会报一个 warning， 如果明确知道该列表是静态，或者不必关注其顺序，可以选择忽略。  

#### wx-if vs hidden
类似vue里面的v-if vs v-show  
（1）wx-if是惰性加载，如果首次v-if的值是false是不渲染的。v-if的状态切换会导致条件块销毁或者重新渲染，比较消耗性能。  
（2）hidden不管值为false还是true，首次总会渲染，只是显示不显示的区别，切换状态也不会销毁或者重新渲染。  
（3）如果有频繁的切换操作行为，使用hidden较好。  

#### WXML 模板
使用 name 属性，作为模板的名字。然后在<template/>内定义代码片段，如：
```
<template name="msgItem">
  <view>
    <text> {{index}}: {{msg}} </text>
    <text> Time: {{time}} </text>
  </view>
</template>
```



#### WXS 模块
WXS 代码可以编写在 wxml 文件中的 <wxs> 标签内，或以 .wxs 为后缀名的文件内。  
（1）每一个 .wxs 文件和 <wxs> 标签都是一个单独的模块。  
（2）每个模块都有自己独立的作用域。即在一个模块里面定义的变量与函数，默认为私有的，对其他模块不可见。  
（3）一个模块要想对外暴露其内部的私有变量与函数，只能通过 module.exports 实现。  
（4）在.wxs模块中引用其他 wxs 文件模块，可以使用 require 函数。  
（5）每个 wxs 模块均有一个内置的 module 对象。通过exports属性对外共享本模块的私有变量或者函数。  
    a. 只能引用 .wxs 文件模块，且必须使用相对路径。  
    b. wxs 模块均为单例，wxs 模块在第一次被引用时，会自动初始化为单例对象。多个页面，多个地方，多次引用，使用的都是同一个 wxs 模块对象。  
    c. 如果一个 wxs 模块在定义之后，一直没有被引用，则该模块不会被解析与运行。  



