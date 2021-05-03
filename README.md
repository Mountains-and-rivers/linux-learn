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

![image](https://github.com/Mountains-and-rivers/linux-learn/blob/main/images/01.png)

### nethogs工具

```
nethogs wlp2s0
```

![image](https://github.com/Mountains-and-rivers/linux-learn/blob/main/images/02.png)

### iftop工具

```
iftop -i wlp2s0 -P
```

![image](https://github.com/Mountains-and-rivers/linux-learn/blob/main/images/03.png)



### **使用tcpdum命令进行抓包**

```
[root@iZ23kick03xZ ~]# tcpdump tcp port 80 -nnei eth0 -w web.pcap
简单点写就是：
[root@iZ23kick03xZ ~]# tcpdump -i eth0 -w web.pcap
```

备注：
-w 是将输出内容保存到文件，而web.pcap是一个**二进制文件**，不能直接打开，可以通过 **wireshark** 软件进行分析

这个是在wireshark中打开的web.pcap文件。返回ssh窗口，按下ctrl+c，结束抓包。

### 安装并打开Wireshark

打开wireshark，在"文件"->“打开”，选择weatherservice.cap，如下图：

![image](https://github.com/Mountains-and-rivers/linux-learn/blob/main/images/04.png)

选择ipv4

![image](https://github.com/Mountains-and-rivers/linux-learn/blob/main/images/05.png)

### 分析http报文 

```
Hypertext Transfer Protocol 下面的http报文

可以根据 A B 箭头方向选择出入 
```

### tcpdum命令大全

```
Linux 环境下，通常通过 tcpdump 来进行抓包和分析。它是几乎所有 Linux 发行版本预装的数据包抓取和分析工具。

一、tcpdump 的用法

tcpdump [-aAbdDefhHIJKlLnNOpqRStuUvxX] [ -B size ] [ -c count ]
	[ -C file_size ] [ -E algo:secret ] [ -F file ] [ -G seconds ]
	[ -i interface ] [ -j tstamptype ] [ -M secret ]
	[ -P in|out|inout ]
	[ -r file ] [ -s snaplen ] [ -T type ] [ -V file ] [ -w file ]
	[ -W filecount ] [ -y datalinktype ] [ -z command ]
	[ -Z user ] [ expression ]


常见参数说明(区分大小写)：
    -a 　 将网络地址和广播地址转变成名字；
    -d 　 将匹配信息包的代码以人们能够理解的汇编格式给出；
    -dd 　将匹配信息包的代码以c语言程序段的格式给出；
    -ddd  将匹配信息包的代码以十进制的形式给出；
    -e 　 在输出行打印出数据链路层的头部信息；
    -f 　 将外部的Internet地址以数字的形式打印出来；
    -l 　 使标准输出变为缓冲行形式；
    -n 　 不把网络地址转换成名字；
    -t 　 在输出的每一行不打印时间戳；
    -s    设置数据包抓取长度。如果置为0，则表示自动选择合适的长度来抓取数据包。
    -v 　 输出一个稍微详细的信息，例如在ip包中可以包括ttl和服务类型的信息；
    -vv 　输出详细的报文信息；
    -vvv 输出详细的数据交互信息。
    -c 　 在收到指定的包的数目后，tcpdump就会停止；
    -F 　 从指定的文件中读取表达式,忽略其它的表达式；
    -i 　 指定需要监听的接口(网卡)；
    -r 　 从指定的文件中读取包(这些包一般通过-w选项产生)；
    -w 　 直接将包写入文件中，并不分析和打印出来；
    -T 　 将监听到的包直接解释为指定的类型的报文，常见的类型有rpc (远程过程调用)和snmp(简单网络管理协议)
    
    expression 一个正则表达式，用作过滤报文的条件。主要包含如下几类：
        关于类型的关键字：包括 host(主机)，net(网络)，port(端口)。
        确定传输方向的关键字：包括 src(源)，dst(目标)，dst or src(源或目标)，dst and src(源和目标)。
        使用协议的关键字：包括 icmp，ip，arp，rarp，tcp，udp等类型。

二、常见用法
1、显示tcpdump的所有可用接口(网卡)

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -D
1
2、抓取指定网卡指定端口的交互数据

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -s 0 -i eth0 port 22
1
3、抓取指定网卡发送给指定 IP 上指定端口的交互数据，并在控制台输出详细交互信息

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -s 0 -i eth1 -vvv port 22
1
4、抓取发送至指定 IP 的 ping 交互数据，并在控制台输出详细交互信息

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -s 0 -i eth1 -vvv dst 114.57.136.45 and icmp
1
5、捕获特定接口上的TCP数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -i {接口名} tcp
1
6、抓取系统内所有接口数据并保存到指定文件

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -i any -s 0 -w test.cap
1
7、从保存的文件中读取数据包( -r 选项)

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -r test.cap
1
8、捕获带有可读时间戳的数据包( -tttt 选项)

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -c 8 -tttt -i {接口名}
1
9、捕获特定接口上的IP地址数据包( -n 选项)

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -n -i {接口名}
1
10、在特定接口上捕获来自特定源IP地址数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -n -i {接口名} src {IP地址}
1
12、在特定接口上捕获来自特定目的IP地址数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -n -i {接口名} dst {IP地址}
1
13、捕获并保存大小大于N字节的数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -i {接口名} -w test.cap greater 1024
1
14、捕获并保存大小小于N字节的数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -i {接口名} -w test.cap less 1024
1
15、捕获两台主机之间的 TCP 数据包通信
假设我想捕获两台主机 169.144.0.1 和 169.144.0.20 之间的 TCP 数据包，示例如下所示，

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -w two-host-tcp-comm.cap -i eth0 tcp and \(host 169.144.0.1 or host 169.144.0.20\)
1
使用 tcpdump 命令只捕获两台主机之间的 SSH 数据包流，

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -w ssh-comm-two-hosts.cap -i eth0 src 169.144.0.1 and port 22 and dst 169.144.0.20 and port 22
1
16、捕获两台主机之间（来回）的 UDP 网络数据包

语法：tcpdump -w -s -i udp and \(host and host \)
[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -w two-host-comm.pcap -s 1000 -i eth0 udp and \(host 169.144.0.10 and host 169.144.0.20\)
1
2
17、捕获十六进制和 ASCII 格式的数据包
要使用 -A 选项捕获 ASCII 格式的数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump -c 10 -A -i eth0
1
三、表达式介绍
表达式是一个正则表达式，tcpdump利用它作为过滤报文的条件，如果一个报文满足表达式的条件，则这个报文将会被捕获。如果没有给出任何条件，则网络上所有的信息包将会被截获。在表达式中一般如下几种类型的关键字。
第一种是关于类型的关键字，主要包括host，net，port, 例如 host 210.27.48.2，指明 210.27.48.2是一台主机，net 202.0.0.0 指明 202.0.0.0是一个网络地址，port 23 指明端口号是23。如果没有指定类型，缺省的类型是host.

第二种是确定传输方向的关键字，主要包括src , dst ,dst or src, dst and src ,这些关键字指明了传输的方向。举例说明，src 210.27.48.2 ,指明ip包中源地址是210.27.48.2 , dst net 202.0.0.0 指明目的网络地址是202.0.0.0 。如果没有指明方向关键字，则缺省是src or dst关键字。

第三种是协议的关键字，主要包括fddi,ip,arp,rarp,tcp,udp等类型。Fddi指明是在FDDI(分布式光纤数据接口网络)上的特定的网络协议，实际上它是”ether”的别名，fddi和ether具有类似的源地址和目的地址，所以可以将fddi协议包当作ether的包进行处理和分析。其他的几个关键字就是指明了监听的包的协议内容。如果没有指定任何协议，则tcpdump将会监听所有协议的信息包。

除了这三种类型的关键字之外，其他重要的关键字如下：gateway, broadcast,less,greater,还有三种逻辑运算，取非运算是 ‘not ’ ‘! ‘, 与运算是’and’,’&&’;或运算 是’or’ ,‘││’；这些关键字可以组合起来构成强大的组合条件来满足人们的需要。

1、想要截获所有210.27.48.1 的主机收到的和发出的所有的数据包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump host 210.27.48.1
1
2、想要截获主机210.27.48.1 和主机210.27.48.2 或210.27.48.3的通信

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump host 210.27.48.1 and \ (210.27.48.2 or 210.27.48.3 )
1
3、如果想要获取主机210.27.48.1除了和主机210.27.48.2之外所有主机通信的ip包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump ip host 210.27.48.1 and ! 210.27.48.2
1
4、如果想要获取主机210.27.48.1接收或发出的telnet包

[root@iZbp1bhgh25pacaj2ffmp2Z ~]# tcpdump tcp port 23 host 210.27.48.1

```

### IP封锁

```
linux下实用iptables封ip段的一些常见命令
封单个IP的命令是：
iptables -I INPUT -s 106.45.233.109 -j DROP
iptables -I OUTPUT -s 106.45.233.109 -j DROP

封IP段的命令是：
iptables -I INPUT -s 106.45.233.0/24 -j DROP
iptables -I OUTPUT -s 106.45.233.0/24 -j DROP

封整个段的命令是：
iptables -I INPUT -s 106.0.0.0/8 -j DROP
iptables -I OUTPUT -s 106.0.0.0/8 -j DROP

封几个段的命令是：
iptables -I INPUT -s 106.45.233.0/24 -j DROP
iptables -I OUTPUT -s 106.45.233.0/24 -j DROP
iptables -I INPUT -s 60.210.246.0/24 -j DROP
iptables -I OUTPUT -s 60.210.246.0/24 -j DROP
```

