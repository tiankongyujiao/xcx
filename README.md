# xcx
小程序开发指南  
微信框架被分为：  
（1）视图层描述语言：wxml，wxss；  
（2）逻辑层描述语言：js。  
在视图层与逻辑层间提供了数据传输和事件系统。 
框架的核心是一个响应的数据绑定系统。框架可以让数据和视图非常简洁的保持同步，当数据修改的时候，只要在逻辑层修改数据，视图层就会做相应更新。 
开发者通过框架将逻辑层的数据和视图层的数据进行绑定，当触发视图层的事件，视图层会发送相应的事件给逻辑层，逻辑层进行相应的处理变更数据，当逻辑层的数据发生变化的时候，由于逻辑层数据和视图层的数据已经进行了绑定，所以逻辑层数据的变化会同步到视图层数据的变化。
### page
1.data对象：当前页面的数据源
