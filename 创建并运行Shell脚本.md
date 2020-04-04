# 创建并运行Shell脚本

## 1.写一个脚本

a) 用touch命令创建一个文件：touch my_script

b) 用vim编辑器打开my_script文件：vi my_script

c) 用vim编辑器编辑my_script文件,内容如下：

\#!/bin/bash           告诉shell使用什么程序解释脚本

\#My first script

ls -l .*

 

## 2.允许Shell执行它

chmod 755 my_script

 

## 3.执行my_script脚本

./my_script