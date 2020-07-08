## SVN命令

### 服务端命令

创建本版库

```shell
svnadmin create svnroot
```

删除版本库

```shell
rm -rvf svnroot
```

版本库配置及权限分组

配置文件位于`/path/repos/conf/`

authz -- 配置用户组以及用户组权限

passwd -- 配置用户名和密码

svnserve.conf -- 配置默认权限、权限配置文件及密码配置文件

svnserve.conf

```properties
 # 没有权限不允许操作(默认可读)
 anon-access = none
 # 有权限时的操作
 auth-access = write
 
 # 用户名密码配置的路径
 password-db = passwd
 # 权限路径配置文件位置
 authz-db = authz
```

passwd

```properties
imooc = 123456
```

authz

```properties
[groups]
# 组名 = 用户名(多个用户用,隔开)
pm = imooc

[/]
@pm = rw
imooc2 = r
# * 所有用户
# * = r
```

SVN版本库访问

运行SVN

```shell
# svn版本库路径
svnserve -d -r /imooc
```



访问svn（**客户端命令**）

```shell
# 检出版本库
mkdir svntest
cd svntest
svn co/checkout svn://192.168.0.130 (--username imooc --password 123456)

```

SVN服务自启动

编辑 /etc/rc.local(ubuntu)

```shell
vim /etc/rc.local
svnserve -d -r /imooc
```

常见SVN术语与文件状态

checkout检出和export导出

```shell
svn co/checkout -r 2 //检出版本2
svn co/export -r 3 //导出版本2
```





### 客户端命令

```shell
svn add - 添加到版本控制（--non-recursive）/ (* --force)
svn commit/ci - 提交修改到服务端（创建新版本号）(-m "xxxx") 
svn update/up - 更新工作副本(-r 1)
svn delete/del/remove/rm - 从版本库中删除文件或目录(-m "")
```



```shell
svn diff/di - 版本差异比较(-r 1)
svn mkdir - 创建目录并增加到版本控制
svn cat - 不检出工作副本直接查看指定文件
```



```shell
svn revert - 工作副本还原 （--recursive） [filename|*]
```

二进制冲突与树冲突

```shell

svn resolve [filename] - 处理冲突
svn resolved [filename] - 标记冲突已处理
```



锁定与解锁(没什么用)

```shell
svn lock - 锁定文件，防止其他成员对文件进行提交
svn unlock - 解锁文件（提交之后会自动解锁）（svn ci --no-unlock）
```



## svn进阶应用

```shell
svn list/ls - 列出当前目录下处于版本控制的所有文件(--recursive -v)
svn status/st - 列出工作副本中文件（夹）的状态
? - 无版本控制
D - 已被标记从版本库中删除
M - 已被编辑过
A - 已被标记添加到版本库中
R - 文件被替换
C - 文件存在冲突
！ - 文件缺失

svn log - 查看提交日志（来自svn ci的-m参数）
svn info - 工作副本及文件（夹）的详细信息 （--xml>>info.xml）
```

## svn高级应用

```shell
# 复制工作副本并添加到当前版本库
svn copy/cp [source] [target] - 复制单个文件 (-r 4)
svn cp index.html about.html ./temp 批量操作

# 复制工作副本并直接提交到当前版本库(不可跨库)
svn cp index.html svn://192.168.0.130/imooc/target.html -m ""

# 复制当前线上版本库文件到工作副本 （可以跨库）
svn cp svn://192.168.0.130/imooc/target.html index.html 

# 复制当前线上版本库文件到其他版本库 (不可跨库)
svn cp svn://192.168.0.130/imooc/ svn://192.168.0.130/imooc2/ -m ""
```

主干版本与分支版本

```shell
# 复制当前线上版本库文件到其他版本库 (不可跨库)
svn cp svn://192.168.0.130/imooc/ svn://192.168.0.130/imooc2/ -m ""

```

hooks 钩子



版本库精简

```shell
killall svnserve
svnadmin dump /svnroot/imooc/ -r 6:16 > ~/imooc.repo

svnadmin create /svnroot/newimooc/

svnadmin load /svnroot/newimooc/ < ~/imooc.repo
# 复制配置文件

```

























