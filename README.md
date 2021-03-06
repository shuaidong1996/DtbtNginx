# DtbtNginx
分布式的负载均衡
---
通过DtbtNginx我们可以对数据进行分流，
比如一个网页有很多请求，像身份验证，请求html，请求图片，播放视频等。
我们可以依靠DtbtNginx分别把消息发给不同的服务器去处理，
假设图片服务器压力过大，我们可以多放几台，
DtbtNginx根据一个hash算法均衡的负载到后台的几个服务器上，
如果要做数据同步也可以利用DtbtNginx。
利用Raft协议解决单点问题

功能：实现了web服务器和负载均衡器两种功能，但实际上我写的是一种框架，逻辑业务可以写在相应的位置
---

每个节点会监听3个端口<br/>
集群内部节点通信、集群和server通信、集群和外界client通信<br/>
用protobuf进行通信<br/>
利用红黑树实现消息回调<br/>
利用进程池和epoll进行高并发<br/>
Raft 和 Consistent Hash进行分布式<br/>
自己写的一致性hash算法，用来选择一个后台服务器进程处理<br/>

一致性Hash算法
---
默认是一致性hash算法，当然我写的时候做到了可拓展，传进去任何hash算法都行<br/>
github: https://github.com/shuaidong1996/Consistent-Hashing

使用进程池的好处：
---
1.程序的健壮性很强，一个进程挂了，再创建一个新的就好，不会把整个程序给拖垮<br/>
2.通过增加CPU，就可以容易扩充性能<br/>
3.可以尽量减少线程加锁/解锁的影响<br/>
4.从资源的占有量说比线程池大很多<br/>

与Nginx的压力测试:
===
环境：腾讯云 Centos 2.6.32 x86_64 1核 1GB 1Mbps<br/>
![dong](https://raw.githubusercontent.com/shuaidong1996/DtbtNginx/master/html/images/webServerTest.png)