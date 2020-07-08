## MUI适用场景说明

为解决HTML5在低端Android机上的性能缺陷，mui引入了原生加速，其中最关键的就是webview控件，因此mui若要发挥其全部能力，需和[5+ App](http://ask.dcloud.net.cn/docs/#http://ask.dcloud.net.cn/article/89)配合适用，若脱离5+ App，mui功能会受限，主要涉及三个部分：

#### webview窗口相关

涉及webview的，除了5+App，其它所有手机浏览器及PC浏览器均无法使用，涉及功能点包括：

- webview模式窗体动画
- 创建子窗口（除了为解决区域滚动的常见双webview场景，还涉及webview模式的选项卡等多webview场景）
- webview模式的侧滑菜单（也有div方式侧滑菜单）
- webview模式的tab选项卡（也有div方式选项卡）
- nativeUI，如原生的警告框、确认框、popover、actionsheet、toast。这些也有HTML5的实现。
- 预加载
- 自定义事件

#### 第三方扩展插件

涉及webview的，除了5+App，其它所有手机浏览器及PC浏览器均无法使用，目前主要包括：语音输入；

#### Touch事件相关（注意pc浏览器没有touch事件）

Touch事件相关的，手机端浏览器均可使用、pc端chrome模拟手机浏览器也可以正常使用。
但普通PC端浏览器因为没有touch事件，可以显示控件但滑动操作功能会受限；涉及功能点包括：

- 手势事件
- mui封装的tap相关处理业务：折叠面板、二级列表、二级选项卡；
- mui封装的swipe、drag相关处理业务：图片轮播、可左右滑动的图文表格、可左右滑动的9宫格、滑动触发列表项菜单、可拖动式侧滑菜单、下拉刷新和上拉加载、可拖动式选项卡
  【备注】：在PC端，大家将tap替换成click，将HTML5默认的Drag事件替换mui 的swipe和drag，就可以解决如上两个问题。

除上述列出的功能点，其它mui功能，均可以在其它手机浏览器及PC服务端使用，所有CSS均不受影响。	