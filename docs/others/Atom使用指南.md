# Atom 使用指南
使用 Atom 有些时间了，还算是比较喜欢这款编辑器的； Atom 从外观界面 和 插件支持来讲，都还算很棒的。
安装插件的方式无论是在界面通过 settings >> Install 还是 通过命令行方式安装，都是通过 apm 来下载安装的。
而 apm 的安装源总是被墙。此时可以切换成淘宝源：

1. 在 C:\Users\用户名\.atom 目录下，新建文件名为 .apmrc 的文本文件 ( 注意：不是 C:\Users\用户名\.atom\.apm 目录下的.apmrc 文件 )， 然后添加如下内容:
registry=https://registry.npm.taobao.org/
strict-ssl=false
添加文件后，发现安装插件还是报错，那是因为 apm 最终还是走的 npm 源，所以还需要更改 npm 源。
2. 通过命令行：npm config set registry https://registry.npm.taobao.org

最后通过 apm install --check // 检查安装环境
运行结果要是为：
Checking for native build tools done //表示可以顺利安装插件了
！！！此刻就已经解决 Atom 安装插件被墙的问题了！！！
