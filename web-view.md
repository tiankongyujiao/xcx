## web-view
设置-开发设置-业务域名，新增配置域名模块。目前小程序内嵌网页能力暂不开放给个人类型帐号和海外类型帐号。  
（1）每次配置均需管理员扫码验证身份。  
（2）配置业务域名时需要严格按照提示要求配置    
限制说明：    
1）每个小程序帐号仅支持配置最多20个域名；
2）每个域名仅支持绑定最多20个小程序；
3）每个小程序一年内最多支持修改域名50次；
4）公众平台后台域名配置成功后，才可使用web-view组件。

#### 使用方法
在小程序页面里面使用：
```
//wxml
<view class="h5">
  <text bindtap="navToH5">把健康带回家</text>
</view>
//js
navToH5: function(){
  go2Page({
    url: "../h5Test/h5Test",//该页面的文件夹和跳转的h5嵌入的页面的文件夹在同一个目录下
 });
}
```
h5页面： 
```
<web-view src="https://m.tuniu.com/event/topic/coarseGrains/index">把健康带回家</web-view>
```
