# DtbtNginx
分布式的负载均衡`正在研发`

利用Raft协议解决单点问题
===
每个节点会监听3个端口
---
集群内部节点通信、集群和server通信、集群和外界client通信
用protobuf进行通信
---
利用进程池和epoll进行高并发
===
Raft 和 Consistent Hash进行分布式
===
自己写的一致性hash算法，用来选择一个后台服务器进程处理
---
使用进程池的好处：
===
1.程序的健壮性很强，一个进程挂了，再创建一个新的就好，不会把整个程序给拖垮
---
2.通过增加CPU，就可以容易扩充性能
---
3.可以尽量减少线程加锁/解锁的影响
---
4.从资源的占有量说比线程池大很多
---