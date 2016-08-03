Nginx的信号控制：

选项 作用

TERM, INT Quick shutdown 直接杀死进程（粗暴）



QUIT Graceful shutdown 优雅的关闭进程,即等请求结束后再关闭

HUP Configuration reload ,Start the new worker processes with a new configuration Gracefully shutdown the old worker processes

 （改变配置文件,平滑的重读配置文件）

USR1 Reopen the log files 重读日志,在日志按月\/日分割时有用



USR2 Upgrade Executable on the fly 平滑的升级



WINCH Gracefully shutdown the worker processes 优雅关闭旧的进程\(配合USR2来进行升级\)







具体语法:

 Kill -信号选项 nginx的主进程号

 Kill -HUP 4873

 

 

 Kill -信号控制 \`cat \/xxx\/path\/log\/nginx.pid\`

 

 

 Kil; -USR1 \`cat \/xxx\/path\/log\/nginx.pid\`







测试HUP的方法：

1.修改nginx配置文件，让nginx的默认读取的文件修改为别的文件（test.html）

2.index.html和test.html文件都使用js将自身不断的刷新，

3.执行 kill -HUP xx\(nginx master pid\)，观察浏览器的情况

注意：当执行上面的kill命令时，浏览器不会立刻的看到效果，因为js程序在不断的刷新页面，需要等几秒钟才能看到nginx配置文件修改的效果





测试USR1：

Linux文件系统在向文件写数据的时候，不是根据文件名来做标识的，而是根据 inode，就是如果将nginx的日志文件access.log改了别的名字（access.log.bak），nginx日志还是会写到 access.log.bak文件中，如果使用kill USR1 xx\(nginx master pid\)，nginx会重新的生成新的日志文件，而不再将日志数据写入到access.log.bak文件中





技巧：

在使用kill命令时，如果想将pid用别的方式代替，其实在logs目录下，nginx.pid就是存放nginx master pid，

所以kill -HUP nginx master pid == kill -HUP \`cat nginx.pid\` （注意：这里的引号是单反引号）

