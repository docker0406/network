## Linux系统性能分析工具

常用系统性能分析命令为vmstat、sar、iostat、netstat、free、ps、top、iftop等。  
常用系统性能组合分析命令如下：  
* vmstat、sar、iostat：检测是否是CPU瓶颈。
* free、vmstat：检测是否是内存瓶颈。
* iostat：检测是否是磁盘I/O瓶颈。
* netstat、iftop：检测是否是网络带宽瓶颈.

## 性能主要是这么4大方面的因素
1）硬件层面

计算(CPU)、存储(Storage/IO)、网络(Network)、内存（RAM），计算机硬件资源也主要是这3方面的资源，现在流行的云计算也主要是这3大资源的虚拟化

2）系统层面

操作系统(Operating sytstem)是大部分应用离不开的一个平台，目前前后端主流的操作系统是Linux,Windows,Android,iOS。同一种操作系统，不同的发行版本对性能的影响也是比较大。

3）中间件、数据库

这一层不是每个应用都会涉及，但大多数复杂的后台应用系统都会涉及到。比如很多web server会用到middleware  Tomcat、Nginx, 会用到数据库MySQL、Oracle. 

4）应用程序

最后这个就是我们直接接触，直接测试的系统本身了，对性能影响最大的因素毫无疑问就是应用程序本身了。  

*TCP连接建立，也就是三次握手阶段
*TCP慢启动
*TCP延迟确认
*Nagle算法(小包)
*TIME_WAIT积累与端口耗尽
*服务端端口耗尽
*服务端HTTP进程打开文件数量达到最大
×HTTPs
*分片
