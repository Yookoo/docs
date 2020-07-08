# MUI 页面跳转及传值操作

### 1。首先了解MUI创建页面的方式或者说是页面跳转方式

#### 1.1预加载页面

所谓的预加载技术就是在用户尚未触发页面跳转时，提前创建目标页面，这样当用户跳转时，就可以立即进行页面切换，节省创建新页面的时间，提升app使用体验。

预加载分为两种方式

// 方式1  在页面初始化的时候进行预加载

```
mui.init({  
    preloadPages: [{  
        url: prelaod - page - url,  
        id: preload - page - id,  
        styles: {}, //窗口参数  
        extras: {}, //自定义扩展参数  
        subpages: [{}, {}] //预加载页面的子页面  
    }]  
});  
```

// 方式2  

```
var page = mui.preload({  
    url: new - page - url,  
    id: new - page - id, //默认使用当前页面的url作为id  
    styles: {}, //窗口参数  
    extras: {} //自定义扩展参数  
}); 
```



------------------------------------------------------------------------------------

#### 1.2直接打开新页面

```
mui.openWindow({  
    url: new - page - url,  
    id: new - page - id,  
    styles: {  
        top: newpage - top - position, //新页面顶部位置  
        bottom: newage - bottom - position, //新页面底部位置  
        width: newpage - width, //新页面宽度，默认为100%  
        height: newpage - height, //新页面高度，默认为100%  
        ......  
    },  
    extras: {  
        ..... //自定义扩展参数，可以用来处理页面间传值  
    }  
    show: {  
        autoShow: true, //页面loaded事件发生后自动显示，默认为true  
        aniShow: animationType, //页面显示动画，默认为”slide-in-right“；  
        duration: animationTime //页面动画持续时间，Android平台默认100毫秒，iOS平台默认200毫秒；  
    },  
    waiting: {  
        autoShow: true, //自动显示等待框，默认为true  
        title: '正在加载...', //等待对话框上显示的提示内容  
        options: {  
            width: waiting - dialog - widht, //等待框背景区域宽度，默认根据内容自动计算合适宽度  
            height: waiting - dialog - height, //等待框背景区域高度，默认根据内容自动计算合适高度  
            ......  
        }  
    }  

 } )  
```



-----------------------------------------------------------------------------------------------------------

#### 1.3 初始化时创建的子页面

```
mui.init({  
    subpages: [{  
        url: your subpage url, //子页面HTML地址，支持本地地址和网络地址  
        id: your subpage id, //子页面标志  
        styles: {  
            top: subpage  top position, //子页面顶部位置  
            bottom: subpage  bottom  position, //子页面底部位置  
            width: subpage width, //子页面宽度，默认为100%  
            height: subpage height, //子页面高度，默认为100%  
            ......  
        },  
        extras: {} //额外扩展参数  
    }]

});
```

##### 子页面使用场景

1. 子页面适用于侧滑菜单
2. 子页面适用于频繁切换的情况
3. 子页面适用于下拉刷新和上拉加载

### 2。传参方式 

#### 2.1 通过extras属性带参数

```
// A页面打开B页面时传递参数
mui.openWindow({
	url:'B.html',
	extras:{
		id:'100'
	}
});	
```

// B页面接收参数的方式

```
var self=plus.webview.currentWebview();*//获取当前窗体对象*
var receiveID=self.id;*//接收A页面传入的id参数值*
```

#### 2.2 通过fire()传参

fire()方法是可以触发目标窗口的自定义事件的方法，关于MUI的自定义事件及该方法的详细使用请参照官方文档 http://dev.dcloud.net.cn/mui/event/#customevent

```
// 传递参数 ( 要触发其他页面的自定义监听事件必须要先进行页面的预加载)
mui.init({
     preloadPages:[{
       id:'B.html',
       url:'B.html'
      }]
 });
mui.fire(plus.webview.getWebviewById('pageid'), "pageRefresh",{
    name: 'zhangsan'

})
```

```
//  接收参数
window.addEventListener("pageRefresh", function (e) {
//获得事件参数
var id = e.detail.id;
});
```

