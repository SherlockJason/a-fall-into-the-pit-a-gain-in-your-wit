# 查看Linux源代码

以ls为例

1. 搜索命令所在位置

   ```
   which ls
   /bin/ls //查询后的地址
   ```

2. 搜索该命令所在软件包

   ```
   dpkg -S /bin/ls
   coreutils: /bin/ls //在coreutils里
   ```

3. 下载源代码

   ```
   sudo apt-get source coreutils
   ```

   进入/usr/src/可以查看其源代码