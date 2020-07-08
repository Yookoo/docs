# maven私服配置
  
### 1.下载

1. 私服采用的是nexus oss 版本号为 ```Nexus Repository Manager 3``` 下载路径：[https://www.sonatype.com/download-oss-sonatype](https://www.sonatype.com/download-oss-sonatype)
2. 下载window版本的压缩包，将压缩包放到指定目录下例:```D:/nexus``` 

### 2.启动

#### 2.1直接启动

cmd转到解压缩文件bin 目录下，输入命令 nexus.exe /run,等待项目启动完成，在浏览器中输入:[http://localhost:8081](http://localhost:8081)

#### 2.2注册成service服务
  
1. 以管理员权限启动cmd，并转到解压包bin目录下，执行```nexus.exe /install nexus```注册服务
2. 启动服务```nexus.exe /start nexus```（注意服务启动是需要一定时间的）
3. 停止服务```nexus.exe /stop nexus```
4. 卸载服务```nexus.exe /uninstall nexus```

### 3.创建私有仓库

1. 登录网站，```username:admin``` ```password:admin123```                  
2. 点击create repository 并且选择maven2(proxy)
3. 填写仓库名```Name:maven-proxy``` 
4. 远程仓库连接```Remote storage URL:```  [https://repo1.maven.org/maven2](https://repo1.maven.org/maven2)
5. 点击create repository即可创建新的远程仓库

### 4.本地maven配置

配置远程仓库以及releases授权信息（用来发布到远程仓库中）
 
    <settings>
     <servers>
       <server>
         <id>releases</id>  
         <username>admin</username>  
         <password>admin123</password>  
  	   </server>
     </servers>
      <mirrors>
         <mirror>
            <id>nexus</id>
            <mirrorOf>*</mirrorOf>
            <url>http://localhost:8081/repository/maven-proxy/</url>
         </mirror>
      </mirrors>
    </settings>

pom.xml 对应配置--server的id与repository的id需要对应
	
    <distributionManagement>
        <snapshotRepository>
            <id>snapshots</id>
            <name>User Porject Snapshot</name>
            <url>http://localhost:8081/repository/maven-snapshots/</url>;
            <uniqueVersion>true</uniqueVersion>
        </snapshotRepository>
        <repository>
            <id>releases</id>
            <name>User Porject Release</name>
            <url>http://localhost:8081/repository/maven-releases/</url>;
        </repository>
    </distributionManagement>

### 5.发布项目到远程仓库

1. 项目发布到远程仓库 `mvn clean deploy -X -Dmaven.test.skip=true`
2. 发布指定包到远程仓库中

        mvn deploy:deploy-file 
			-DgroupId=com.oracle 
            -DartifactId=ojdbc14 
            -Dversion=1.0.RELEASES
            -Dpackaging=jar 
            -Dfile=D:\ojdbc14.jar 
            -Durl=http://localhost:8081/repository/maven-releases/ 
            -DrepositoryId=maven-repository-releases
