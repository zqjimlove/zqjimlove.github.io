title: 混合应用的JavaScript Bridge（三）
date: 2016-4-5 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: hybrid-app-javascript-bridge-3
---
上一篇说到如何设计一个统一的跨平台的实现方式，那么今天就详细的说说各个平台是如何实现的自己的私有的代码。在开始之前可以回顾下第一篇的文章，通过分析两平台之间实现的差异可以更容易理解本篇文章。

<!--more-->

### Android 平台下发送到客户端的实现

其实根据第一篇文章中的介绍，Android实现起来并没有什么难度。因为Android既可以对window注入，也可以调用前端的js方法。所以在实际的编码过程中并没有太多复杂的东西。只要客户端对WEBView注入方法`AndroidBridge`便可以了，但需要**注意**的是，在调用`AndroidBridge`的时候必须采用call的方法，并且把调用对象（第一个参数）设置为AndroidBridge。不然在某些android版本上会报错。

```javascript
//对于回调函数我们暂时不考虑
send:function(api,params,successCallback,failCallback){
   var callbackId = gCallbackId();
   if(isAndroid){
      AndroidBridge.call(AndroidBridge,api,params,callbackId); 
   }
}
```

### IOS 平台下发送到客户端的实现

由于ios的平台限制比Andoird的复杂得多，实现起来也难得多。由于ios没有办法注入方法到前端当中（只针对uiwebview）所以我们在前端只能通过改变url来通知客户端应该来获取数据了。

```javascript
iosSendMsgArray:[], //ios纪录接口请求
frame:null,
send:function(api,params,successCallback,failCallback){
   var callbackId = gCallbackId();
   if(isAndroid){
      
   }else{
   	 this._iosSend(api,params,callbackId);
   }
},
_iosSend:function(api,params,callbackId){
	var frame = this.frame;
	if(!frame){
		this.frame= document.createElement('iframe');
		frame = this.frame;
    	frame.style.display = 'none';
    	document.documentElement.appendChild(frame);
	}

	this.iosSendMsgArray.push({
		api:api,
		params:params,
		callbackId:callbackId
	});
	frame.src = 'inhu://send';  
	//以上代码可以优化，每隔一段时间再去发起一次通知不必每次发起。
	//但这里我们还是以最简单的方式来展示。不做多余的代码。
},
IOSGetMsg:function(){
	var resultStr = this.iosSendMsgArray.toString();
	this.iosSendMsgArray.length=0;
	return resultStr;
}
```

通过以上代码，我们可以知道当前端要调用后台提供的借口时`send()`，如果是IOS的话我们会先暂时存放到一个数组里面然后修改iFrame的链接以实现通知IOS，IOS接到通知后便调用前端的`inhuBridge.IOSGetMsg`获得一组数据。然后IOS去解释字符串便可。


### 回调的实现

我们简单的实现了发送消息到客户端的代码，当然这里只是简单的实现方式而已可能在实际使用过程中也存在一些缺陷。但这理暂时不做讨论，我们只关心如何客户端和前端是如何通讯的。

我们上篇文章说过要有一个`CallBackId`，我们每发起一次发送请求之前都需要把成功回调和失败回调缓存起来，并且把callBackId作为信息的一部分发送到客户端而客户端发起返回数据时同时也要把CallBackId作为参数返回并从缓存中取出相关的回调。

```javascript
callBackCache:{},
send:function(api,params,successCallback,failCallback){
   var callbackId = gCallbackId();
   this.callBackCache[callbackId] = function(_resultData){
   		if(_resultData.status){
   			successCallback(_resultData);
   		}else{
   			failCallback(_resultData);
   		}
   };

   if(isAndroid){
      
   }else{

   }
},
receive:function(respond){
	this.callBackCache[respond.callbackId](respond);
}
```

至于IOS和Android之间如何实现客户端调用前端的方法，前篇文章已经说过了。这里就不再说了。

## 总结

这篇文章算不上完整的解决方案，只是提供了大家一个思路方案仅此而已。为了简单明了的用代码展示出如何实现的所以代码过程中忽略了很多应有的判断和方式。

根据上面的思路方式可以延伸出更多的功能，如会话模式、事件通知等。
