# Windows下MongDB的安装

**安装**
1. 到官网根据你的系统下载 32 位或 64 位的 .msi 文件，下载后双击该文件，安装过程中，你可以通过点击 `Custom(自定义)` 按钮来设置你的安装目录。
2. 下一步安装 `install mongoDB compass` 不勾选，否则可能要很长时间都一直在执行安装，MongoDB Compass 是一个图形界面管理工具，我们可以在后面自己到官网下载安装。我使用的是`Robo 3T`

**配置**
注意：请根据你安装的位置执行相应的命令

1. 创建一个 data 的目录然后在 data 目录里创建 db 目录。(你也可以通过 window 的资源管理器中创建这些目录，而不一定通过命令行。)
```
c:\>cd c:\

c:\>mkdir data

c:\>cd data

c:\data>mkdir db

c:\data>cd db

c:\data\db>
```

2. 从命令提示符下运行 MongoDB 服务器，你必须从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件：`C:\mongodb\bin\mongod --dbpath c:\data\db`
3. 我们可以在命令窗口中运行 mongo.exe 命令即可连接上 MongoDB，执行如下命令：`C:\mongodb\bin\mongo.exe`
