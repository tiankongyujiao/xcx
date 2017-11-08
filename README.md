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






