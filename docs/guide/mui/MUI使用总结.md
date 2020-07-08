# MUI使用总结

最近项目中需要使用MUI做一个视频播放的小功能。我就花时间研究了一下MUI。

MUI是一个使用JavaScript开发Android和IOS应用的前端框架。这篇文章将以知识树的形式对MUI的使用做一个总结，一些官方文档中的东西我都只大致提一下，如果需要详细了解可以进入官方文档了解详情。

## UI组件

组件部分不多说，详情可以看[官方文档](http://dev.dcloud.net.cn/mui/ui/)。

小技巧：Dialog 组件正常情况下是无法解析HTML内容的，如果需要对Dialog 组件的内容进行定制可以将Dialog 的最后一个参数type设置为'div'。

## MUI选择器

MUI的选择器类似Jquery，主要有#id选择器、.class选择器 标签选择器，组合选择器。

```javascript
mui('#id')
mui('.class')
mui('input')
mui('p.class')
```

和Jquery一样，如果想从mui选择器选中的元素中取出原生的DOM元素，只需取出`mui('#id')[0]`即可。

## MUI的常用方法

MUI并没有像Jquery一样丰富的方法，常用的方法并不多。

### 事件相关的方法

#### MUI对象方法

`on(event, selector, handler)` 批量绑定事件

`one(event, selector, handler)` 批量绑定事件但是只生效一次

`off([event][, selector])` 删除事件

#### MUI静态方法

`trigger(element, event, data)` 触发事件

`fire(target, event, data)` 触发自定义事件

#### 原生事件监听方法

`addEventListener(event, handler)` 单个DOM节点绑定事件

##### [手势事件](http://dev.dcloud.net.cn/mui/event/#gesture)

### 页面相关方法

##### [init(options)](http://dev.dcloud.net.cn/mui/util/#init)

页面初始化设置。目前支持在mui.init方法中配置的功能包括：[创建子页面](http://dev.dcloud.net.cn/mui/window/#subpage)、[关闭页面](http://dev.dcloud.net.cn/mui/window/#closewindow)、[手势事件配置](http://dev.dcloud.net.cn/mui/event/#gesture)、[预加载](http://dev.dcloud.net.cn/mui/window/#preload)、[下拉刷新](http://dev.dcloud.net.cn/mui/pulldown/#pullrefresh-down)、[上拉加载](http://dev.dcloud.net.cn/mui/pullup/#pullrefresh-up)、[设置系统状态栏背景颜色](http://dev.dcloud.net.cn/mui/util/#statusbar)。

##### [openWindow(options)](http://dev.dcloud.net.cn/mui/window/#openwindow)

打开新页面

##### [back()](http://dev.dcloud.net.cn/mui/window/#closewindow)

关闭当前页面

### 其他工具方法

此部分官方文档都写得非常详细，如果哪个方法不懂可以直接点击连接跳转至官方文档详细查看。

#### MUI对象方法

##### [each(handler)](http://dev.dcloud.net.cn/mui/util/#each)

遍历

#### MUI静态方法

##### [each(obj, handler)](http://dev.dcloud.net.cn/mui/util/#each)

遍历

##### [extend([deep, ]target, obj1[,objN])](http://dev.dcloud.net.cn/mui/util/#extend)

合并多个对象

##### [later(func,delay)](http://dev.dcloud.net.cn/mui/util/#later)

setTimeOut封装

##### [scrollTo(ypos[,duration][, handler])](http://dev.dcloud.net.cn/mui/util/#scrollTo)

滚动窗口屏幕到指定位置，该方法是对`window.scrollTo()`方法在手机端的增强实现，可设定滚动动画时间及滚动结束后的回调函数;鉴于手机屏幕大小，该方法仅可实现屏幕纵向滚动。

##### [os](http://dev.dcloud.net.cn/mui/util/#os)

我们经常会有通过`navigator.userAgent`判断当前运行环境的需求,mui对此进行了封装,通过调用mui.os.XXX即可

- plus(可以访问的参数为:)

  **.plus：**返回是否在 5+ App(包括流应用)运行

  **.stream**：返回是否为流应用

- Android(可以访问的参数为:)

  **.android：**返回是否为安卓手机

  **.version**：安卓版本号

  **.isBadAndroid**：android非Chrome环境

- iOS(可以访问的参数为:)

  **.ios**：返回是否为苹果设备

  **.version**：返回手机版本号

  **.iphone：**返回是否为苹果手机

  **.ipad：**返回是否为ipad

- Wechat(可以访问的参数为:)

  **.wechat：**返回是否在微信中运行

### AJAX方法

类似JQuery主要由`ajax(options)、post(url,params,callback)、get(url,params,callback)`,详情可参考[官方文档](http://dev.dcloud.net.cn/mui/ajax/#ajax)。

### MUI插件方法

示例1：跳转到图片轮播的第二张图片

```javascript
mui('.mui-slider').slider().gotoItem(1);
```

示例2：重新开启上拉加载

```javascript
mui('#pullup-container').pullRefresh().refresh(true);
```
