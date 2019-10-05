## 几个命令
linux查看tcp的状态命令：
1. netstat -nat  查看TCP各个状态的数量
2. lsof  -i:port  可以检测到打开套接字的状况
3. sar -n SOCK 查看tcp创建的连接数
4. tcpdump -iany tcp port 9000 对tcp端口为9000的进行抓包

## 网络测试常用命令
1. ping:检测网络连接的正常与否,主要是测试延时、抖动、丢包率。
           但是很多服务器为了防止攻击，一般会关闭对ping的响应。所以ping一般作为测试连通性使用。ping命令后，会接收到对方发送的回馈信息，其中记录着对方的IP地址和TTL。TTL是该字段指定IP包被路由器丢弃之前允许通过的最大网段数量。TTL是IPv4包头的一个8 bit字段。例如IP包在服务器中发送前设置的TTL是64，你使用ping命令后，得到服务器反馈的信息，其中的TTL为56，说明途中一共经过了8道路由器的转发，每经过一个路由，TTL减1。

2. traceroute：raceroute 跟踪数据包到达网络主机所经过的路由工具
   traceroute hostname
3. pathping：是一个路由跟踪工具，它将 ping 和 tracert 命令的功能与这两个工具所不提供的其他信息结合起来，综合了二者的功能
   pathping www.baidu.com
4. mtr：以结合ping nslookup tracert 来判断网络的相关特性
5. nslookup:用于解析域名，一般用来检测本机的DNS设置是否配置正确。

## 状态机
### LISTENING 侦听来自远方的TCP端口的连接请求
  首先服务端需要打开一个socket进行监听，状态为LISTEN。
  有提供某种服务才会处于LISTENING状态，TCP状态变化就是某个端口的状态变化，提供一个服务就打开一个端口，例如：提供www服务默认开的是80端口，提供ftp服务默认的端口为21，当提供的服务没有被连接时就处于LISTENING状态。FTP服务启动后首先处于侦听（LISTENING）状态。处于侦听LISTENING状态时，该端口是开放的，等待连接，但还没有被连接。就像你房子的门已经敞开的，但还没有人进来。
  看LISTENING状态最主要的是看本机开了哪些端口，这些端口都是哪个程序开的，关闭不必要的端口是保证安全的一个非常重要的方面，服务端口都对应一个服务（应用程序），停止该服务就关闭了该端口，例如要关闭21端口只要停止IIS服务中的FTP服务即可。关于这方面的知识请参阅其它文章。
  如果你不幸中了服务端口的木马，木马也开个端口处于LISTENING状态。

### SYN-SENT：客户端SYN_SENT状态：
  在发送连接请求后等待匹配的连接请求:客户端通过应用程序调用connect进行active open.于是客户端tcp发送一个SYN以请求建立一个连接.之后状态置为SYN_SENT. /*The socket is actively attempting to establish a connection. 在发送连接请求后等待匹配的连接请求 */
  当请求连接时客户端首先要发送同步信号给要访问的机器，此时状态为SYN_SENT，如果连接成功了就变为ESTABLISHED，正常情况下SYN_SENT状态非常短暂。例如要访问网站http://www.baidu.com,如果是正常连接的话，用TCPView观察IEXPLORE.EXE（IE）建立的连接会发现很快从SYN_SENT变为ESTABLISHED，表示连接成功。SYN_SENT状态快的也许看不到。
  如果发现有很多SYN_SENT出现，那一般有这么几种情况，一是你要访问的网站不存在或线路不好，二是用扫描软件扫描一个网段的机器，也会出出现很多SYN_SENT，另外就是可能中了病毒了，例如中了"冲击波"，病毒发作时会扫描其它机器，这样会有很多SYN_SENT出现。
