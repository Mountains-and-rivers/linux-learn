# 抓包工具的使用
### 安装网络分析工具

```
yum install epel-release -y
yum install nethogs 
或者
yum install iftop -y
```

### 主机信息

```
wlp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.31.240  netmask 255.255.255.0  broadcast 192.168.31.255
        inet6 fe80::50fa:6bd6:d626:4d06  prefixlen 64  scopeid 0x20<link>
        ether 08:d4:0c:e6:58:d9  txqueuelen 1000  (Ethernet)
        RX packets 9615489  bytes 6535903123 (6.0 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4335921  bytes 1563104797 (1.4 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

```
[root@node ~]# ps -ef|grep nginx
root       15562       1  0 4月11 ?       00:00:00 nginx: master process nginx
nginx      15563   15562  0 4月11 ?       00:00:00 nginx: worker process
nginx      15564   15562  0 4月11 ?       00:00:00 nginx: worker process
nginx      15565   15562  0 4月11 ?       00:00:00 nginx: worker process
nginx      15566   15562  0 4月11 ?       00:00:00 nginx: worker process
root     2165387 2107099  0 20:32 pts/1    00:00:00 grep --color=auto nginx
```

```
[root@node ~]# netstat -nlp|grep 80         
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      15562/nginx: master 
tcp6       0      0 :::80                   :::*                    LISTEN      15562/nginx: master 
```

### 浏览器访问

图片1

### nethogs工具

```
nethogs wlp2s0
```

图片2

### iftop工具

```
iftop -i wlp2s0 -P
```

图片3



### **使用tcpdum命令进行抓包**

```
[root@iZ23kick03xZ ~]# tcpdump tcp port 80 -nnei eth0 -w web.pcap
简单点写就是：
[root@iZ23kick03xZ ~]# tcpdump -i eth0 -w web.pcap
```

备注：
-w 是将输出内容保存到文件，而web.pcap是一个**二进制文件**，不能直接打开，可以通过 **wireshark** 软件进行分析

这个是在wireshark中打开的web.pcap文件

### 安装并打开Wireshark

打开wireshark，在"文件"->“打开”，选择weatherservice.cap，如下图：

图片4

选择ipv4

图片5