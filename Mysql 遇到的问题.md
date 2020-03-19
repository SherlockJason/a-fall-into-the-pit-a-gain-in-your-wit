# Mysql 遇到的问题

## 问题1: 如何查看mysql版本

查看mysql版本

```
select version();
```

也可以用status查看

```
status；
```

## 问题2：在导入csv文件时，出现提示“The MySQL server is running with the --secure-file-priv option so it cannot execute this statement”.

**mysql在启动时没有指定配置文件时会使用/etc/my.cnf配置文件**



查询变量

```
show variables like '%secure%' ;
```

发现secure-file-priv的value为NULL

```
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| require_secure_transport | OFF   |
| secure_file_priv         | NULL  |
+--------------------------+-------+
```

如果是一个文件路径的话，导入/出的文件路径放在这个路径下就可以，但如果是NULL，就要对secure-file-priv进行设置。

首先打开Finder，按住command+Shift+G，可以前往指定文件夹，输入：/usr/local/etc，即可前往etc文件夹。也可以按住command+空格，输入：/usr/local/etc。（要输入自己电脑的etc路径）

查看my.cnf文件是否存在（本人的就不存在）

如果my.cnf文件不存在，则需新建该文件，步骤如下：
①输入：cd /usr/local/etc
②输入：sudo vim my.cnf
③输入电脑的登录密码

④英文键盘下输入a，即可开始编辑。接着复制如下代码到该文件（my.cnf）：

```
# Example MySQL config file for medium systems.  
  #  
  # This is for a system with little memory (32M - 64M) where MySQL plays  
  # an important part, or systems up to 128M where MySQL is used together with  
  # other programs (such as a web server)  
  #  
  # MySQL programs look for option files in a set of  
  # locations which depend on the deployment platform.  
  # You can copy this option file to one of those  
  # locations. For information about these locations, see:  
  # http://dev.mysql.com/doc/mysql/en/option-files.html  
  #  
  # In this file, you can use all long options that a program supports.  
  # If you want to know which options a program supports, run the program  
  # with the "--help" option.  
  # The following options will be passed to all MySQL clients  
  [client]
  default-character-set=utf8
  #password   = your_password  
  port        = 3306  
  socket      = /tmp/mysql.sock   
  # Here follows entries for some specific programs  
  # The MySQL server  
  [mysqld]
  character-set-server=utf8
  init_connect='SET NAMES utf8
  port        = 3306  
  socket      = /tmp/mysql.sock  
  skip-external-locking  
  key_buffer_size = 16M  
  max_allowed_packet = 1M  
  table_open_cache = 64  
  sort_buffer_size = 512K  
  net_buffer_length = 8K  
  read_buffer_size = 256K  
  read_rnd_buffer_size = 512K  
  myisam_sort_buffer_size = 8M  
  character-set-server=utf8  
  init_connect='SET NAMES utf8' 
# Don't listen on a TCP/IP port at all. This can be a security enhancement,  
# if all processes that need to connect to mysqld run on the same host.  
# All interaction with mysqld must be made via Unix sockets or named pipes.  
# Note that using this option without enabling named pipes on Windows  
# (via the "enable-named-pipe" option) will render mysqld useless!  
#   
#skip-networking  

  # Replication Master Server (default)  
  # binary logging is required for replication  
  log-bin=mysql-bin  

    # binary logging format - mixed recommended  
    binlog_format=mixed  

      # required unique id between 1 and 2^32 - 1  
      # defaults to 1 if master-host is not set  
      # but will not function as a master if omitted  
      server-id   = 1  

    # Replication Slave (comment out master section to use this)  
    #  
    # To configure this host as a replication slave, you can choose between  
    # two methods :  
    #  
    # 1) Use the CHANGE MASTER TO command (fully described in our manual) -  
    #    the syntax is:  
    #  
    #    CHANGE MASTER TO MASTER_HOST=<host>, MASTER_PORT=<port>,  
    #    MASTER_USER=<user>, MASTER_PASSWORD=<password> ;  
    #  
    #    where you replace <host>, <user>, <password> by quoted strings and  
    #    <port> by the master's port number (3306 by default).  
    #  
    #    Example:  
    #  
    #    CHANGE MASTER TO MASTER_HOST='125.564.12.1', MASTER_PORT=3306,  
    #    MASTER_USER='joe', MASTER_PASSWORD='secret';  
    #  
    # OR  
    #  
    # 2) Set the variables below. However, in case you choose this method, then  
    #    start replication for the first time (even unsuccessfully, for example  
    #    if you mistyped the password in master-password and the slave fails to  
    #    connect), the slave will create a master.info file, and any later  
    #    change in this file to the variables' values below will be ignored and  
    #    overridden by the content of the master.info file, unless you shutdown  
    #    the slave server, delete master.info and restart the slaver server.  
    #    For that reason, you may want to leave the lines below untouched  
    #    (commented) and instead use CHANGE MASTER TO (see above)  
    #  
    # required unique id between 2 and 2^32 - 1  
    # (and different from the master)  
    # defaults to 2 if master-host is set  
    # but will not function as a slave if omitted  
    #server-id       = 2  
    #  
    # The replication master for this slave - required  
    #master-host     =   <hostname>  
    #  
    # The username the slave will use for authentication when connecting  
    # to the master - required  
    #master-user     =   <username>  
    #  
    # The password the slave will authenticate with when connecting to  
    # the master - required  
    #master-password =   <password>  
    #  
    # The port the master is listening on.  
    # optional - defaults to 3306  
    #master-port     =  <port>  
    #  
    # binary logging - not required for slaves, but recommended  
    #log-bin=mysql-bin  

      # Uncomment the following if you are using InnoDB tables  
      #innodb_data_home_dir = /usr/local/mysql/data  
      #innodb_data_file_path = ibdata1:10M:autoextend  
      #innodb_log_group_home_dir = /usr/local/mysql/data  
      # You can set .._buffer_pool_size up to 50 - 80 %  
      # of RAM but beware of setting memory usage too high  
      #innodb_buffer_pool_size = 16M  
      #innodb_additional_mem_pool_size = 2M  
      # Set .._log_file_size to 25 % of buffer pool size  
      #innodb_log_file_size = 5M  
      #innodb_log_buffer_size = 8M  
      #innodb_flush_log_at_trx_commit = 1  
      #innodb_lock_wait_timeout = 50  

        [mysqldump]  
        quick  
        max_allowed_packet = 16M  

          [mysql]  
          no-auto-rehash  
          # Remove the next comment character if you are not familiar with SQL  
          #safe-updates  
          default-character-set=utf8   

 [myisamchk]  
        key_buffer_size = 20M  
        sort_buffer_size = 20M  
        read_buffer = 2M  
        write_buffer = 2M  

          [mysqlhotcopy]  
          interactive-timeout
          
secure_file_priv=''
[mysqld]
local-infile=1
[mysql]
local-infile=1

[mysqld]
bind-address=127.0.0.1
secure-file-priv='/data/mysql'
```

由于之前处于编辑模式，故现在需要退出编辑模式：
①按Esc键，退出编辑模式
②输入：`：wq`并回车即可保存文件并退出。

**在系统偏好设置中，最下一栏中的mysql，点击，选择配置文件，将my.cnf选为配置文件**

（在网上查了半天，最后发现这里没有选择配置文件，如果没有默认是选择/etc中的配置文件，还是自己定义下比较清楚）

重启mysql，也可以直接重启电脑。

打开mysql开始导入数据！/开心
①打开terminal.app终端
②输入：mysql -uroot -p，然后输入你的数据库密码

```
PATH=$PATH:/usr/local/mysql/bin/
mysql -u root -p
```

③选择一个数据库（不要忘了哦！！！）
④开始导入数据：
load data local infile '/Users/elainezhang/Desktop/data/employee.csv' into table employee

重启命令

```
sudo /usr/local/mysql-8.0.19-macos10.15-x86_64/support-files/mysql.server restart
```

## 问题3 ERROR! MySQL server PID file could not be found!解决方案

首先怀疑是有僵尸mysqld的存在，首先查看进程

```
ps -ef|grep mysqld 
```

 然后用
kill  进程号杀死进程

重启mysql，但是问题并没有得到解决。

再然后可以通过mysql的配置文件my.cnf查看一下mysql的数据存储目录位置，查看一下目录权限。其实不看也没事，可以直接给mysql一个高权限，直接777最干脆。

```
chmod 777 
```

