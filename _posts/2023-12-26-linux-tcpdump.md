---
layout: post
title: "linux抓包命令tcpdump"
category: linux 
---


## 简介
网络世界，计算机系统被分成不同的层级，比如：OSI 模型（7层）、TCP/IP 模型（四层）、五层模型。其对应系如下图👇：

![Untitled.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f960f359ab9b45809a4e9d5cb8192ed4~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=608&h=295&s=149021&e=png&b=faf6f5){:.ioda}

![theme logo](https://raw.githubusercontent.com/riggraz/no-style-please/master/logo.png){:.ioda}


其中，在每一层都不可缺少的分出了传输层和网络层。网络层用于将数据从一台主机传输到位于不同网络中的另一台主机。而传输层会从网络层获取服务，并向应用层提供服务。

我们要解说的tcpdump（dump the traffic on a network）就是抓取在网络层和传输层流通的数据。tcpdump是依赖于libpcap函数库的linux命令，用来抓取网络中的流量包。


## tcpdump常用参数
### 基础使用
```sh
tcpdump -V ## 版本号
man tcpdump ## 查看帮助文档
tcpdum -D ## 打印所有可用的网络接口列表
```
### 1. tcpdump抓取内容介绍
单独执行`tcpdump`获取默认抓包信息：
```txt
18:17:30.137623 IP x.x.x.x.ndmp > x-x-x-x.ssh: Flags [.], ack 15613, win 811, options [nop,nop,TS val 1760701871 ecr 4166071444], length 0
```
数据包内容按照顺序依次为：操作执行的时间、协议类型、发送方地址、接收方地址、发送内容
### 2. 控制时间格式的显示 -t
不同数量的`t`可显示不同效果
```sh
## 时间显示格式 -t
tcpdump -c 1 -t ## 返回数据没有时间 
tcpdump -c 1 -tt ## 返回秒级别时间戳 
tcpdump -c 1 -ttt ## 返回两次抓包之间的时间间隔 
tcpdump -c 1 -tttt ## 返回2023-11-15 18:50:37.497681格式时间
```
### 3. 抓取指定协议类型
在tcpdump后面直接接协议类型即可过滤
```sh
tcpdump tcp # 抓取tcp协议数据包 
tcpdump icmp 0.0.0.0 # 过滤网络层/传输层协议
```
ps：这里只能过滤传输层、网络层协议，过滤其他层协议是不生效的。比如：`tcpdump http 0.0.0.0`，因为http协议是应用层的。
### 4. 基于源/目标地址做过滤：host、dst、src、-Q、--direction
```sh
## 基于发送/接收双方过滤：使用参数：`host`、`dst`、`src`
tcpdump host x.x.x.x ## 抓取发送方或接收方为主机x.x.x.x的数据
tcpdump dst x.x.x.x ## 抓取接收方为主机x.x.x.x的数据
tcpdump src x.x.x.x ## 抓取发送方为主机x.x.x.x的数据

## 控制流量方向 -Q，同 --direction
## -Q 的option：in、out、all；in-外网回来的流量，out-出去的流量，all-所有；
```

### 5. 基于内容做过滤
```sh
## 控制抓取包的条数 -c
tcpdump -c 10 ## 抓取10条数据后自动停止

## 基于包大小进行过滤
tcpdump less 32 ## 抓取包小于32bytes的包
tcpdump greater 100 ## 抓取包大于100bytes的包

## 限制抓包的数量 -C，和-w 一起使用
## 抓包量满后，会自动停止抓包
tcpdump -C 1 -W 3 -w abc  ## -C后面的1表示抓取1MB大小的文件，-W表示抓取文件的最大数量，-w后面表示抓取文件的名字，该命令会抓取3个1MB大小的文件，文件名分别为：abc1，abc2，abc3
tcpdump -C 1 -w abc ## 没有指定-W，抓取的文件数量会一直增加
## 设置-W后，流量超出就会覆盖写入旧文件

## 打印简洁信息 -q：较少的协议相关信息

## 指定每个包捕获长度 -s 默认：2622144bytes
tcpdump -s 22  ## 抓取22bytes的包流量

## 选择网卡 -i
## linux系统中，我们可以通过`ifconfig`命令查看系统网卡，通过`-i`参数选择指定网卡的数据。
tcpdump -i eth0 ## 抓取从eth0网卡进入的数据

## 打印数据链路层的头部信息 -e
## 显示源&目的的MAC地址、VLAN tag信息
```

#### 6. 过滤信息保存
```sh
## 保存到指定文件夹 -w
tcpdump -w xxx.pcap

## -l 在需要同时观察抓包打印以及保存抓包时使用
tcpdump -l | tee dat ## 抓取数据包内容实时打印并保存到dat文件中
tcpdump -l > dat | & tail -f dat ## 效果同上
```

#### 7. 读取文件
```sh
tcpdump -r xx.cap ## 显示xx.cap 文件内容
tcpdump -A -r xx.cap ## 以ASCII格式打印出所有的分组并读取此文件
tcpdump -x -r xx.cap ## 以十六进制和ASCII格式同时打印出此文件
## -A 和 -x 不可同时使用
```
#### 8. 逻辑运算
逻辑运算符号有：&&、||、| ,也可以直接用英文字母：and、or、not
```sh
tcpdump src xxxx and port 80 ## 获取源地址为xxxx，且端口号为80的流量包
tcpdump tcp port 53 or udp port 53
tcpdump not tcp port 22
```
在逻辑运算中，`and`的优先级大于`or`，如果需要先执行`or`再执行`and`，需要用`括号`
```sh
tcpdump "src xx and (dst port 3389 or 22)"
```

## 通过Wireshark查看tcpdump内容
在抓取网络数据后，通过`-w`命令将数据写入`xx.pcap`文件中。
### Wireshark打开pcap文件
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b643b544499248119b0cb685bbfeff52~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2274&h=1652&s=1729255&e=png&b=f1f1f1)
打开界面如下：
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db58f38bf5da4a2ea77fb7a8261c801d~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2164&h=1592&s=614344&e=png&b=eeeeee)
### Wireshark过滤pcap文件
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d8869c6b225497494a3d202c4b3ec2e~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2112&h=306&s=139169&e=png&b=dddddd)
##### 常用过滤条件
```sh
## 过滤单个ip地址
ip.src == 12.3.1.1  ## 过滤源ip为12.3.1.1的数据
ip.dst == 12.3.1.1  ## 过滤目的ip为12.3.1.1的数据
ip.addr == 12.3.1.1 ## 过滤源ip或者目的ip为12.3.1.1的数据
ip.addr != 12.3.1.1 ## 过滤源ip和目的ip都不是12.3.1.1的数据
!(ip.addr == 12.3.1.1) ## 获取 源ip或者目的ip为12.3.1.1 之外的数据

## 针对协议区分
tcp ## 过滤出tcp协议的数据包
!tcp ## 过滤非tcp协议的数据包
tcp or udp ## 过滤出tcp和udp两种协议的数据包
## 如果.pcap文件中有协议类型为SSH、Kafka的数据包，在tcp命令过滤时，这些数据也会展示出来。因为SSH和Kafka协议属于应用层协议，其传输层的协议依然是tcp协议。

## 端口过滤：端口过滤是依附于协议的，
tcp.port == 22  ## 过滤tcp协议中端口号为22的数据包
tcp.port == 22 || udp.port == 80 ## 过滤tcp协议端口号为22以及udp协议中端口号为80的数据包

```
上面只是列举了一些常用的过滤命令，这里[推荐连接](https://www.cnblogs.com/willingtolove/p/12519490.html#%E6%98%BE%E7%A4%BA%E8%BF%87%E6%BB%A4%E5%99%A8%E5%86%99%E6%B3%95)可以查看更详细的写法。另外，我们也可以在命令行输入的时候通过回车来查看wireshark支持的过滤命令。
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/60f53ee37a254316a5bea65cd43e10bd~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1980&h=884&s=1218607&e=png&b=e4e3fa)

### 解析TCP数据包
下图为tcp数据包内容详解
![企业微信截图_cb022331-ed99-4adc-b07d-5fa84ef4a2ad.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d79faa40e03d45318ae8e1bdfecc1237~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1930&h=738&s=844638&e=png&b=f9f0ea)
数据包内容在wireshark解析结果显示如下：
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9d28bdf440d427e83e37a2668d6a52f~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2228&h=1418&s=779895&e=png&b=fcfcfc)
