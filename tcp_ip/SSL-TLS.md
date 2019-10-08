## SSL/TLS协议
http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html    
https://blog.csdn.net/enweitech/article/details/81781405  
SSL(Secure Socket Layer 安全套接层)  
TLS1.0(Transport Layer Security 安全传输层协议)  
在SSL更新到3.0时，IETF对SSL3.0进行了标准化，并添加了少数机制(但是几乎和SSL3.0无差异)，标准化后的IETF更名为TLS1.0(Transport Layer Security 安全传输层协议),可以说TLS就是SSL的新版本3.1，并同时发布“RFC2246-TLS加密协议详解”

## SSL协议的握手过程
第一步，客户端给出协议版本号、一个客户端生成的随机数（Client random），以及客户端支持的加密方法。

第二步，服务端确认双方使用的加密方法，并给出数字证书、以及一个服务器生成的随机数（Server random）。

第三步，客户端确认数字证书有效，然后生成一个新的随机数（Premaster secret），并使用数字证书中的公钥，加密这个随机数，发给服务端。

第四步，服务端使用自己的私钥，获取客户端发来的随机数（即Premaster secret）。

第五步，客户端和服务端根据约定的加密方法，使用前面的三个随机数，生成"对话密钥"（session key），用来加密接下来的整个对话过程。

## TLS与SSL的差异
　　1）版本号：TLS记录格式与SSL记录格式相同，但版本号的值不同，TLS的版本1.0使用的版本号为SSLv3.1。

　　2）报文鉴别码：SSLv3.0和TLS的MAC算法及MAC计算的范围不同。TLS使用RFC-2104定义的HMAC算法。SSLv3.0使用了相似的算法，两者差别在于SSLv3.0中，填充字节与密钥之间采用的是连接运算，而HMAC算法采用的异或运算。但是两者的安全程度是相同的。

　　3）伪随机函数：TLS使用了称为PRF的伪随机函数来将密钥扩展成数据块，是更安全的方式。

　　4）报警代码：TLS支持几乎所有的SSLv3.0报警代码，而且TLS还补充定义了很多报警代码，如解密失败（decryption_failed）、记录溢出（record_overflow）、未知CA（unknown_ca）、拒绝访问（access_denied）等。

　　5）密文族和客户证书：SSLv3.0和TLS存在少量差别，即TLS不支持Fortezza密钥交换、加密算法和客户证书。

　　6）certificate_verify和finished消息：SSLv3.0和TLS在用certificate_verify和finished消息计算MD5和SHA-1散列码时，计算的输入有少许差别，但安全性相当。

　　7）加密计算：TLS和SSLv3.0在计算主密值（master secret）时采用的方式不同。

　　8）填充：用户数据加密之前需要增加的填充字节。在SSL中，填充后的数据长度哟啊达到密文快长度的最小整数倍。而在TLS中，填充后的数据长度可以是密文块长度的任意整数倍（但填充的最大长度为255字节），这种方式可以防止基于对报文长度进行分析的攻击。

### TLS的主要增强内容

　　TLS的主要目标是使SSL更安全，并使协议的规范更精确和完善。TLS在SSL v3.0的基础上，提供了以下增加内容：

　　1）更安全的MAC算法

　　2）更严密的警报

　　3）“灰色区域”规范的更明确的定义

### TLS对于安全性的改进

　　1）对于消息认证使用密钥散列法：TLS使用“消息认证代码的密钥散列法”（HMAC），当记录在开放的网络（如因特网）上传送时，该代码确保记录不会被变更。SSLv3.0还提供键控消息认证，但HMAC比SSLv3.0使用（消息认证代码）MAC功能更安全。

　　2）增强的伪随机功能（PRF）：PRF生成密钥数据。在TLS中，HMAC定义PRF。PRF使用两种散列算法保证其安全性。如果任一算法暴露了，只要第二种算法未暴露，则数据仍然是安全的。

　　3）改进的已完成消息验证：TLS和SSLv3.0都对两个端点提供已完成的消息，该消息认证交换的消息没有被变更。然而，TLS将此已完成消息基于PRF和HMAC值之上，这也比SSLv3.0更安全。

　　4）一致证书处理：与SSLv3.0不同，TLS试图指定必须在TLS之间实现交换的证书类型。

　　5）特定警报消息：TLS提供更多的特定和附加警报，以指示任一会话端点检测到的问题。TLS还对何时应该发送某些警报进行记录。
