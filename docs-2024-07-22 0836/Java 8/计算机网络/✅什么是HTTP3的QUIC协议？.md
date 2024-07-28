# 典型回答

我们知道，HTTP/2之所以"被弃用"，是因为他使用的传输层协议仍然是TCP，所以HTTP/3首要解决的问题就是绕开TCP。

[✅HTTP/2存在什么问题，为什么需要HTTP/3？](https://www.yuque.com/hollis666/fo22bm/pg5ika?view=doc_embed)

那么如果研发一种新的协议，同样还是会因为受到中间设备僵化的影响，导致无法被大规模应用。所以，研发人员们想到了一种基于UDP实现的方式。

于是，Google是最先采用这种方式并付诸于实践的，他们在2013年推出了一种叫做QUIC的协议，全称是**Quick UDP Internet Connections**。

**从名字中可以看出来，这是一种完全基于UDP的协议。**

在设计之初，Google就希望使用这个协议来取代HTTPS/HTTP协议，使网页传输速度加快。2015年6月，QUIC的网络草案被正式提交至互联网工程任务组。2018 年 10 月，互联网工程任务组 HTTP 及 QUIC 工作小组正式将基于 QUIC 协议的 HTTP（英语：HTTP over QUIC）重命名为HTTP/3。

所以，**我们现在所提到的HTTP/3，其实就是HTTP over QUIC，即基于QUIC协议实现的HTTP。**

那么，想要了解HTTP/3的原理，只需要了解QUIC就可以了。

QUIC协议有以下特点：

- **基于UDP**的传输层协议：它使用UDP端口号来识别指定机器上的特定服务器。
- **可靠性**：虽然UDP是不可靠传输协议，但是QUIC在UDP的基础上做了些改造，使得他提供了和TCP类似的可靠性。它提供了数据包重传、拥塞控制、调整传输节奏以及其他一些TCP中存在的特性。
- **实现了无序、并发字节流**：QUIC的单个数据流可以保证有序交付，但多个数据流之间可能乱序，这意味着单个数据流的传输是按序的，但是多个数据流中接收方收到的顺序可能与发送方的发送顺序不同！
- **快速握手**：QUIC提供0-RTT和1-RTT的连接建立
- **使用TLS 1.3传输层安全协议**：与更早的TLS版本相比，TLS 1.3有着很多优点，但使用它的最主要原因是其握手所花费的往返次数更低，从而能降低协议的延迟。

那么，QUIC到底属于TCP/IP协议族中的那一层呢？我们知道，QUIC是基于UDP实现的，并且是HTTP/3的所依赖的协议，那么，按照TCP/IP的分层来讲，他是属于传输层的，也就是和TCP、UDP属于同一层。

如果更加细化一点的话，因为QUIC不仅仅承担了传输层协议的职责，还具备了TLS的安全性相关能力，所以，可以通过下图来理解QUIC在HTTP/3的实现中所处的位置。

![](https://cdn.nlark.com/yuque/0/2023/jpeg/5378072/1692971332899-97e63d46-c1a7-4c8e-ba16-927c202851f1.jpeg#averageHue=%23f8f7e5&clientId=u4d112527-9d21-4&from=paste&id=u06b9ba42&originHeight=553&originWidth=941&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u040566a7-835d-43e7-8512-c45bdcad7e5&title=)



