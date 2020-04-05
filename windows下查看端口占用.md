# windows下查看端口占用

1. 打开命令窗口（win+R）

2. 输入命令：

   ```
   netstat -ano
   ```

   该命令列出所有端口的使用情况。

3. ### 查看被占用端口对应的 PID

   输入命令：

   ```
   netstat -aon|findstr "8080"
   ```

4. ### 查看指定 PID 的进程

   继续输入命令：

   ```
   tasklist|findstr "(pid)"
   ```

5. ### 结束进程

   强制（/F参数）杀死 pid 为 9088 的所有进程包括子进程（/T参数）：

   ```
   taskkill /T /F /PID 9088 
   ```

