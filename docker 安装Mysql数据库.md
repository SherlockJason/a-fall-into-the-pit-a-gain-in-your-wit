# docker 安装Mysql数据库

1. 首先安装mysql，默认latest

```
docker pull mysql
```

2. 创建并启动mysql服务容器

   ```
   docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest
   ```

   参数说明

   ```
   --name  给容器起别名，可选，不指定，则自动生成不规则的字符串
   -p  3306:3306 将本机3306 端口映射到docker 的3306端口
   -v  ~/mysql/data:/var/lib/mysql 是将本机的/mysql/data目录映射到docker的/var/lib/mysql 文件夹
       自动创建~/mysql/data，用于存放容器的mysql数据库文件
   -e  MYSQL_ROOT_PASSWORD=123456 初始化密码为123456
   -d  确定唯一镜像，这里只有一个mysql也有必要要tag， 仓库名:标签
   
   
   ```

   连接mysql数据库

   ```
   docker exec -it mysql bash
   bash# mysql -u root -p 123456
   mysql>
   ```

   

