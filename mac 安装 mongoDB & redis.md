# mac 安装 mongoDB & redis

## 安装redis

直接到官网下载https://redis.io/download， 选取稳定版本

✓ https://redis.io/download ✓ 请使用默认端口号6379 ✓ redis-server ✓ redis-cli

### 1. 安装

使用以下命令下载，提取和编译Redis：

```
$ wget http://download.redis.io/releases/redis-5.0.8.tar.gz
$ tar xzf redis-5.0.8.tar.gz
$ cd redis-5.0.8
$ make
```



`src` 目录 中现在提供了已编译的二进制文件 。使用以下命令运行Redis：

```
$ src/redis-server
```

可以使用内置客户端与Redis进行交互：

```
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```



## 安装mongoDB

https://www.mongodb.com/download-center/community

请使用默认端口号27017 ✓ mongo：MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境

**官网下载太慢啦，这里采用的是curl的方式来安装。**

### 1、安装

- 先打开mac终端，cd 到user/local下

```
cd /usr/local
```

- 下载 mongodb的包

```
sudo curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.4.2.tgz
```

- 解压刚刚下载的包

```
sudo tar -zxvf mongodb-osx-x86_64-3.4.2.tgz
```

- 将文件名 mongodb-osx-x86_64-3.4.2 重命名为简单的 mongodb

```
sudo mv mongodb-osx-x86_64-3.4.2 mongodb
```

- 配置环境变量

  - 打开本地的 .bash_profile

    ```
    open -e .bash_profile
    ```

  - 在文件的最后添加下面一行代码：

    ```
    export PATH=${PATH}:/usr/local/MongoDB/bin
    ```

  - 让刚刚输入的代码生效：

    ```
    source .bash_profile
    ```

- 验证是否安装成功

  终端输入

  ```
  mongod -version  		
  ```

  如果出现本版好则证明已经安装成功了

  ```
  $ mongod -version  
  db version v3.4.2
  git version: 3f76e40c105fc223b3e5aac3e20dcd026b83b38b
  allocator: system
  modules: none
  build environment:
      distarch: x86_64
      target_arch: x86_64
  ```

  

  

## 2、根目录新建 Data文件

在终端输入

```
sudo mkdir -p /data/db
```

其中 -p表示如果没有data文件夹，就先建立data文件夹，然后在data文件夹下面建立db文件夹，避免报错。
当然这一步也可以手动去建立，也是ok的，建完之后可以查看一下，如下：

### 3、启动并运行

- 在终端输入如下命令启动服务：

  ```
  sudo mongod
  ```

  

  如上图所示，就表示启动成功了。这时候打开浏览器，输入 localhost:27017 ,会出现如下字，那就ok了

  ```
  It looks like you are trying to access MongoDB over HTTP on the native driver port.
  ```

  

  或者输入http://127.0.0.1:27017/ 也是一样的。

  再打开一个终端，输入

```
mongo
```

这样就可以对数据库进行一些操作了。
比如查看当前表：

```
show dbs
```

## 退出

当你要退出的时候，注意要用正确的姿势退出，不然下次再连接的话可能会出现一些问题

```
use admin
db.shutdownServer()
```

即可完成退出操作.

这个时候再刷新 http://localhost:27017/ 或者 http://127.0.0.1:27017/
就会发现无法访问此网站了。