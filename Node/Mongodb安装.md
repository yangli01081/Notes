# Mongodb安装

## 一.安装MongoDB


[使用Homebrew安装](https://github.com/mongodb/homebrew-brew)

### 设置


您可以使用以下方法在MacOS终端会话中添加自定义轻点：

```
$ brew tap mongodb/brew
```


### 安装方式：


```other
$ brew install mongodb-community
```


## 二.启动mongodb-community服务器


### 将`mongod`作为服务运行


要立即启动`mongod`，并在登录时重新启动，请使用：

```other
$ brew services start mongodb-community
```


如果您将`mongod`管理为服务，它将使用上面列出的默认路径。要停止服务器实例，请使用：

```other
$ brew services stop mongodb-community
```


## 三.卸载mongodb-community服务器


如果您需要卸载MongoDB服务器，请使用：

```other
$ brew uninstall mongodb-community
```


## 四.安装客户端软件


[MongoDB官网](https://www.mongodb.com/try/download/compass)，下载**Compass** 软件，在启动服务器后再开启连接

命令行：

show:展示数据库，或者集合（show dbs , show collections）

use:进入数据库或者集合

db.users.find(): 查询users集合下所有文档数据

db.users.find({name:"a"}): 查询users集合下name=a的所有文档数据

db.users.insert({name:"a"}): 在users集合下插入一条文档数据

db.users.find().sort({name:1}):排序（1：正序，-1：倒叙）



