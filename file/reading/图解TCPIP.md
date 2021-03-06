[TOC]

## 概述

### 常见协议

![image-20210225162916816](https://i.loli.net/2021/03/09/AG2OKnNWtgwbLQv.png)

#### 分组交换协议

![image-20210225163259771](https://i.loli.net/2021/03/09/CHQETo8nhYAFlmL.png)



#### 协议与分层

![image-20210225163404478](https://i.loli.net/2021/03/09/kM4NyF8UHTxdQAW.png)



#### 通过对话理解协议与分层

![image-20210225163425619](https://i.loli.net/2021/03/09/kM4NyF8UHTxdQAW.png)



### OSI参考模型 即经典的TCP/IP七层

![image-20210225163509903](https://i.loli.net/2021/03/09/5D9vBFRsmipEA36.png)





#### 七层的各自分工

![image-20210225163657123](https://i.loli.net/2021/03/09/l5yeFrJKLGhuRZP.png)

![image-20210225163637218](https://i.loli.net/2021/03/09/hHlof4jEg1Cnb5k.png)

#### 通过七层协议进行通信

![image-20210225163810911](https://i.loli.net/2021/03/09/g1bK6l9nPxmrZBk.png)





![image-20210225163909657](https://i.loli.net/2021/03/09/TS12VB6H5OeDPq3.png)

当我们在写完邮件点击发送后，`应用层`就开始工作了。他会将我们的邮件内容打包成数据包往表示层进行传递。



![image-20210225164448991](https://i.loli.net/2021/03/09/TIsqY1uFznx2SPN.png)

在`表示层`，用户会将上层传过来的数据进行分析，将他转换成网络通信中统一的格式。举例来说，有一台用UTF-8编码的电脑传邮件给Unicode编码的电脑，那么他们需要知道对方的编码格式，`表示层`就负责进行这样的编码，将他们转换成统一的格式。

![image-20210225164736551](https://i.loli.net/2021/03/09/xoldM8wOiNFBDGr.png)

`会话层`主要负责在信息前端加上传输信息，表示传输顺序等。

![image-20210225164858798](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210225164858798.png)

`传输层`主要来控制两个机器之间如何建立连接，以及连接的断开与重发等。其含有两个重要的协议，`tcp`和`udp`。



![image-20210225165052363](https://i.loli.net/2021/03/09/8725C3xLUyi1djM.png)

`网络层`负责将数据进行传输，从主机A把数据传输到数据B。

![image-20210225165339109](https://i.loli.net/2021/03/09/ufeH51ijBRWCgJr.png)

`数据链路层`可以看成是`网络层`的一个部分，主要负责在一个小的链路中进行数据传输。

`物理层`则是最真实的存在，他就是01串和我们现实中的设备。



### 传输方式的分类

#### 面向有连接型和面向无连接型

![image-20210225165535210](https://i.loli.net/2021/03/09/4omCHicT3GKP7UN.png)

#### 分组交换与电路交换

![image-20210225165601866](https://i.loli.net/2021/03/09/iOT5ozCLeqKVYJS.png)

电路交换就是两个电脑通过一根线连接，他们基于这根线进行交互。但是这样会导致有时候线会被占用。

分组交换就是把数据包进行拆分，然后全都往一根线上送，数据包上写了相关的目标等信息，这样就可以发到合适的地方去。

#### 单播、多播、广播、任播

![image-20210225170628907](https://i.loli.net/2021/03/09/FIGQDpwExzeOHuR.png)



#### 地址的唯一性

在通信的每一层中，地址的概念都是不同的。但是即使这样，在同一个网络中，也不应该有同名的地址存在。

![image-20210301214412426](https://i.loli.net/2021/03/09/4D5a6bS2WusgAdR.png)

在这里和我们前面所说的多播、任播等就会出现矛盾，该如何去理解这样一个矛盾呢？简单的说，即使是多播，他多播的对象也是可以看成一类唯一地址的集合。

![image-20210301214559149](https://i.loli.net/2021/03/09/KyF3EoYcTN4mgpZ.png)



#### 地址的层次性

基于我们打电话的区号，我们可以联想到地址也是有层次的。`IP`地址就遵循这种层次性，而`MAC`地址则是无分层地址。

![image-20210301215216543](https://i.loli.net/2021/03/09/qt8WicApEyVrPQC.png)



在这里我们要说明一下，`IP`地址是由接入网来进行分配的，而`MAC`地址则是网卡生产的时候由制造商写在网卡上的。`IP`地址主要由网络号和主机号构成，前者可以看成一个小区，而后者则可以看成是小区内的地址。`MAC`发送依靠`地址转发表`，`IP`转发依靠`路由转发表`。

![image-20210301222104388](https://i.loli.net/2021/03/09/kgcNHspUrJSYanT.png)



### 网络的构成

![image-20210301222209798](https://i.loli.net/2021/03/09/uCmMqAhKyUZfpwQ.png)

![image-20210301222219154](https://i.loli.net/2021/03/09/FScMLxNrfvTX1Hy.png)

#### 通信媒介与链路

![image-20210301222352985](https://i.loli.net/2021/03/09/QxuVgIKyd7wRreh.png)

解释几个名词：

- 传输速率，用来表示两个设备之间数据流动的物理速度
- 吞吐量，主机间实际的传输速率



网卡（网络适配器、网卡、LAN卡）



#### 中继器（OSI第一层）

将电缆传过来的电信号或者光信号通过中继器的波形调整和放大再传给另一个电缆。我们可以用它来延长网络。

![image-20210301222614325](https://i.loli.net/2021/03/09/4p7VmT32iKPqefn.png)

有的中继器有多个端口服务，我们可以把这些堪称是中继集线器。

![image-20210301222843412](https://i.loli.net/2021/03/09/XCBjDGfoZuw26Vx.png)



#### 网桥/2层交换机（OSI第二层）

![image-20210301223600210](https://i.loli.net/2021/03/09/VPmviF5B7Kn3jgo.png)



连接数据链路层的硬件。该硬件通过`FCS`（循环冗余校验码），来检验数据是否正确送达目的地。网桥通过检查这个里面的值，丢弃掉损坏的数据包。此外，其还能通过地址自学机制和过滤功能控制网络流量。

有些网桥能够判断是否将报文转发给响铃的网段，被称为自学式网桥。他们可以记住曾经通过自己转发的所有数据帧的`MAC`地址，装到自己的内存表里。

注意，网桥根据`MAC`地址来处理。

![image-20210301224041667](https://i.loli.net/2021/03/09/cnwpbZDXdAQy8uJ.png)

![image-20210301224107964](https://i.loli.net/2021/03/09/emdDANUjYJz58QC.png)



#### 路由器/三层交换机（ISO模型第三层）

![image-20210301224132436](https://i.loli.net/2021/03/09/loUKQPFp4wguiXt.png)

可以连接两个网络，并且对分组报文进行转发。路由器根据`IP`来处理。其还具有分担网络负荷的功能。



#### 4~7层交换机

![image-20210301224312218](https://i.loli.net/2021/03/09/X8SAkHouPVs9wYN.png)

负责处理OSI模型中从传输层至应用层的数据。以`TCP`协议的传输层和上方的应用层为基础，分析收发数据，对其进行特定的处理。



#### 网关

![image-20210301224445696](https://i.loli.net/2021/03/09/DK96iUjHpqa8uFA.png)

网关是模型中负责将传输层到应用层的数据进行转换和转发的设备。

![image-20210301224530750](https://i.loli.net/2021/03/09/dsKMHXP1LqfyFWm.png)

![image-20210301224541121](https://i.loli.net/2021/03/09/mTjLhpsrabNOU5I.png)



#### 设备和网络分层

![image-20210301224557779](https://i.loli.net/2021/03/09/PrW7DVn9CHqAymN.png)



### 如何联网通信

互联网通信

![image-20210301224834678](https://i.loli.net/2021/03/09/yifr4eRb1cMgKTo.png)

移动通信

![image-20210301224933762](https://i.loli.net/2021/03/09/9afYK6GQUvuS5w8.png)

部署者角度来看互联网的变化（云计算）

![image-20210301225019635](https://i.loli.net/2021/03/09/XCKhm3te5akF827.png)





## TCP/IP基础知识

![image-20210301225556946](https://i.loli.net/2021/03/09/7dbMwlOKxheEgQn.png)

### TCP/IP的含义

一般情况下，我们认为TCP/IP是指一些协议的组合。

![image-20210302212658594](https://i.loli.net/2021/03/09/Q4TWOjioqtznJpg.png)

### 互联网的组成

每个网络都是由骨干网（BackBone）和末端网（Stub）组成。每个网络通过NOC（Network Operation Center，网络操作中心）连接。如果运行商不相同，需要有IX（Internet Exchange，网络交换中心）的支持。

![image-20210302213410172](https://i.loli.net/2021/03/09/o3QVhmIdR8ruOyl.png)

### TCP/IP分层模型

![image-20210302213847805](https://i.loli.net/2021/03/09/CuFhSn3958Bwocj.png)

#### 硬件层

主要负责数据传输。



#### 网络接口层

可以看成是网卡和操作系统之间的接口与桥梁。



#### 互联网层

使用IP协议，基于IP地址转发分包数据。

有IP、ICMP和ARP三个协议。

IP协议能够让数据发往另一台主机，这期间用IP地址作为主机标识。

ICMP是用来诊断网络健康状况的，如果发送失败就给发送端送一个发生异常的通知。

ARP是从分组数据包的IP地址中解析出物理地址的协议。



#### 传输层

![image-20210302214638631](https://i.loli.net/2021/03/09/ZpNLDI8odchCwMQ.png)

传输层主要实现程序之间的通信。包含TCP和UDP两个协议

TCP协议是面向有连接的传输协议，其能够保证两端通信主机之间的通信可达。TCP能够正确处理丢包、数据顺序错乱等问题、但是其为了建立和断开连接，他需要至少七次的发包和收包，会浪费一定流量。

UDP协议面向无连接的传输，他不考虑接收端是否收到，发送了即可，主要在直播、广播等领域实现。



#### 应用层

主要实现应用程序的功能。如WWW，电子邮件等、

![image-20210302214937116](https://i.loli.net/2021/03/09/Vi8eD4gxjWUFoKT.png)

WWW主要协议是Http协议。电子邮件的协议是SMTP协议。FTP协议用来实现文件共享。远程登陆需要TELNET和SSH协议、网络管理需要SNMP协议。



### 数据包

![image-20210302215432328](https://i.loli.net/2021/03/09/imAcPenOr7VEbRK.png)

从协议的上层往下，逐渐加包，从下往上，逐渐解包。

#### 发送数据包

#### 应用程序处理

应用程序将内容转换一下编码，然后发送到下一层。

#### TCP模块处理

根据应用的指示，在数据包前面加一个TCP首部，里面包含了端口号、目标端口号、序号和校验和。然后将该部分发送给IP模块。

#### IP模块

IP将TCP传过来的TCP首部和TCP数据合到一起，然后在前面加上自己的IP首部。之后将该部分发送到驱动程序，让驱动程序们进行向外发送。

#### 网络接口的处理

接收IP传过来的IP包，其给这个包加上以太网首部并发送。以太网首部里包含着接收端MAC地址、发送端MAC地址和标志以太网类型的数据。接着硬件计算FCS添加到包的最后，给接收者判断包是否因为噪声而损坏。

![image-20210302220457245](https://i.loli.net/2021/03/09/VL7Waz4H5wln31E.png)

### 经过数据链路的包

我们给出包的结构：

![image-20210302220825002](https://i.loli.net/2021/03/09/GIAZ9WEsfRe6TJi.png)

### 接收数据包

显然，这是一个解包的过程，我们自底向上的进行解包。

#### 网络接口处理

主机接收到网络包后，首先从以太网首部的接收MAC地址判断是否为发送给自己的，如果不是则丢弃（发送的时候会采取广播的方式），如果是发送给自己的话，就查找以太网数据包里的类型数据判断类型。IP则给IP处理，ARP则给ARP处理。如果无法识别，也丢弃。

#### IP模块处理

IP的操作和网络接口类似，先拆包，如果是自己的则去找协议类型，是TCP还是UDP，如果不是的话，则进行数据的转发。

#### TCP模块处理

TCP模块中，接收数据后悔计算一下校验和，判断是否破坏，然后根据序号来收集数据，数据收集完后检查一下端口号，然后确定具体的应用程序。当数据接收后，其会发一个接收到了的信息给发送方。如果发送方没有接收到的话，就会重复发送。

确定完端口号后，数据会传送给端口号识别的应用程序。

#### 应用程序的处理

接收端接收发送端发送的数据，然后解析数据进行处理。



## 数据链路

在这里，我们主要介绍计算机网络的数据链路层，主要介绍以太网、无线局域网、PPP等。

![image-20210302222635057](https://i.loli.net/2021/03/09/eI2CZzNkJW4KGVu.png)

### 相关技术

#### MAC地址

MAC地址长48位，会被烧入到网卡中。

![image-20210308192152565](https://i.loli.net/2021/03/09/dXirV9xblt37gLs.png)

值得注意的是，在传播中，MAC地址会呈现出小段对齐的形式。也即，例如地址为`0100xxxxxxxx`，在传输中会是如下的情况：

![image-20210308192537549](https://i.loli.net/2021/03/09/xaBJbpVTOXfPYEt.png)

但是，MAC地址可能不是唯一的，例如某一台PC上有多个虚拟机，则会分配一些虚拟数据给虚拟机，这时候就可能出现不唯一的MAC地址。



### 共享介质型网络

共享介质型网络是指多个网络共享一种通信介质的网络。

共享介质型网络有两种访问方式。争用方式和令牌传递方式。

#### 争用方式

和名字一样，是一种先到先得的方式，但是可能会出现冲突。

![image-20210308193100206](https://i.loli.net/2021/03/09/3EyiYxjbVdtRXI1.png)

所以人们尝试改良了一下该方式，如果发生了冲突则停止发送，等待片刻后在尝试占用通道发送。

![image-20210308193154896](https://i.loli.net/2021/03/09/FgGRn5Lvxj7oIlr.png)

#### 令牌传递方式

有点类似微机原理中的异常处理，通过一个令牌的传递来进行资源占用，这样可以保证没有冲突同时每个节点都有机会访问资源，但是问题在于这样会使得资源的利用率不高，因为可能某个节点不需要使用资源但是拿到了令牌。

![image-20210308193433027](https://i.loli.net/2021/03/09/JkXrT9SN8y5BPeU.png)



### 非共享介质网络

该方式指各个节点直接连到交换机，不需要使用同样的资源，有交换机进行数据帧转发。

![image-20210308193800869](https://i.loli.net/2021/03/09/QWswBKqvxMkZj3b.png)



### 根据MAC地址转发

接上文，将多个网络的线合到一个接口里，就形成了交换集线器，也可以叫做以太网交换机。其是一个有多个端口的网桥，其能够根据数据帧的MAC地址来决定从哪里发送/接收数据，其用来判断从哪转发的东西叫转发表。每次转发的时候，路由器都能自己进行学习，记录下转发情况。

值得注意，因为MAC地址是没有层次性的，也即不能像DNS一样一层层的寻找，所以我们要考虑把网络划分成多个链路，就像cache中的分组一样。

![image-20210308194700659](https://i.loli.net/2021/03/09/J3HsAalZ7x2mMEQ.png)

### 环路检测技术

当我们在拓扑中进行数据发送的时候，一旦出现回路，数据就会不停地环绕，导致网络瘫痪。

主要有生成树和源路由两种技术。

![image-20210308195357995](https://i.loli.net/2021/03/09/zRJF8mqAK7Y6lOy.png)

#### 生成树方法

以某个节点作为root，然后对树枝设置权重，根据权重进行转发，这样就不会出现环路了。



#### 源路由法

在转发的时候，将数据记录到RIF中，网桥根据该信息进行转发，这样可以有效地防止出现环。（也即不会绕一圈回到原点）



### VLAN

![image-20210308201129154](https://i.loli.net/2021/03/09/KtsVXWIvLHZMlyo.png)

VLAN可以看成是一种通过路由器来控制的网络，在软件上将一个网络进行划分。在首部加上一个标签，通过标签区分是发给哪个网段的。

![image-20210308201519305](https://i.loli.net/2021/03/09/NsK3HPBqGMxRFQb.png)



### 以太网

#### 以太网的连接形式

![image-20210308202018704](https://i.loli.net/2021/03/09/v7jamP6qHIC8hTR.png)

![image-20210308202026764](https://i.loli.net/2021/03/09/l4t9Hw3BsCQZFTD.png)

#### 以太网帧格式

以太网帧前端有个前导码，表示该帧的开始。前导码末尾是SFD域，值为11，表示结束。后面跟的就是以太网帧的本体。

![image-20210308202252245](https://i.loli.net/2021/03/09/PEg4qJcfF9M6sV7.png)

帧主体开头记录了源MAC地址、目标MAC地址和上层协议类型。

![image-20210308202328050](https://i.loli.net/2021/03/09/QGEuOJrqdNtfzMg.png)

之后跟随的是数据包，也就是主体内容。最后有一个FCS字段（帧检验序列），用来进行校验。

![image-20210308202546867](https://i.loli.net/2021/03/09/tNYTmofr67hjKqD.png)



### 无线通信

![image-20210308202637792](https://i.loli.net/2021/03/09/kPowSeiXshTKcCM.png)



### PPP

PPP是指point to point protocol，一种点对点通信协议。

![image-20210308202804305](https://i.loli.net/2021/03/09/A81kZOacTp7dwuv.png)



#### LCP与NCP

PPP主要包含两个协议，LCP协议与NCP协议。如果上层为ip，那么NCP也叫IPCP。

LCP主要负责建立与断开连接、设置最大接收单元、设置验证协议以及设置是否进行通信质量的监控。

IPCP则负责IP地址设置以及TCP/IP首部压缩。

![image-20210308203107279](https://i.loli.net/2021/03/09/XE9oFSpi7us5aQY.png)

PPP连接时，需要进行一个双向的认证。分为PAP和CHAP两种方法。

PAP通过两次握手来进行连接，但是采取明文传输，不够安全。

CHAP则只进行一次认证，但是会在中途进行定期密码交换。



#### PPP帧格式

通过标志码来区分每一帧。在两个标志码之间，不允许出现连续6个1，如果出现了则在第五个1后面加0，所以接收方接收了以后要去掉0.

![image-20210308203447225](https://i.loli.net/2021/03/09/kjSvn7KaMzR2esd.png)



#### PPPoE

![image-20210308203536896](https://i.loli.net/2021/03/09/VRoUQr2L6PCvdSE.png)



#### 其他链路连接

ATM

![image-20210308203621553](https://i.loli.net/2021/03/09/m2PQZjC34slJpAI.png)

ATM要求先发出请求，建立通话，类似电话通话，但是不同的是其能建立多个连接。

POS


FDDI

分布式光线数据接口。FDDI采用令牌（追加令牌）环的访问方式。令牌环访问方式在网络拥堵的情况下极容易导致网络收敛。

FDDI中的每个站通过光纤连接形成环状，如图3.32所示。FDDI为了防止在环在某处断开时导致整个通信的中断，采用双环的结构。双环中站叫做DAS（Dual Attachment Station，双连站。） ，单环中的站叫做SAS（Single Attachment Station，单连站。） 。

![image-20210308203900917](https://i.loli.net/2021/03/09/X8wIN3Tz9jB4sUH.png)

TOKEN RING

100VG-AnyLAN

光纤通道

HIPPI

IEEE1394

HDMI



### 公共网络

#### 模拟电话线路

![image-20210308204128348](https://i.loli.net/2021/03/09/EgyKRzUL8QciYeN.png)

#### 移动通信服务

#### ADSL

ADSL利用从电话机到电信局的线路，将数字信号和音频信号进行分离，防止产生干扰。

![image-20210308204251063](https://i.loli.net/2021/03/09/u6ALSziDVbmNtrO.png)

#### FTTH

用光纤进行接入。

![image-20210308204323461](https://i.loli.net/2021/03/09/QmkehLtKMDW6Ugl.png)

#### 有线电视

![image-20210308204357973](https://i.loli.net/2021/03/09/Jq7oaCDEwXOHuIU.png)

#### 专线

#### VPN

虚拟专用网络（VPN）用于连接距离较远的地域。这种服务包括IP-VPN和广域以太网。

IP-VPN通过附加标签信息形成一个私有虚拟网络。

![image-20210308204454767](https://i.loli.net/2021/03/09/jVvETp1SHkbL8Cd.png)

#### 公共无线网络

WIFI



## IP协议

### IP网际协议

IP协议位于互联网层，主要由IP协议和ICMP协议构成。

#### 位于OSI位置

位于OSI第三层网络层。网络层主要负责点对点通信或者说实现终端节点之间的通信。链路层主要负责在某一个短的链路内进行数据传输，而网络层则负责一个目的地的甄选。

![image-20210308211410107](https://i.loli.net/2021/03/09/iJzXbdMvwLTtHY9.png)

在这里，我们定义只有IP的为主机，而有IP地址又可以进行路由控制的为路由器。这里可以参考我们家里的路由器。或者也可以称之为网关。



#### 网络层与数据链路层的关系

![image-20210308211545951](https://i.loli.net/2021/03/09/QyxEpr5Zz6n4MX3.png)

我们可以认为，网络层借数据链路层来实现传输。



### IP基础知识

主要分为三部分，IP寻址、路由以及IP分包和组包。



#### IP地址是网络层地址

在网络链路层中，我们通过MAC地址来进行地址识别，在网络层中，我们通过IP地址来识别地址。因此，能进行TCP/IP的主机都需要有一个IP地址，且IP地址保持不变。



#### 路由控制

路由控制可以将数据包成功的送向他的目的地。

![image-20210308211945394](https://i.loli.net/2021/03/09/oTidLkPXVsMIveJ.png)

在路由转发的过程中，我们通过跳这个词来形容一次路由转发，IP路由也被叫做多跳路由。每一跳都会告诉下个路由如何操作，在这样的过程里才能到达目的地。

![](https://i.loli.net/2021/03/09/JxlogRSdebwE6CL.png)

![image-20210308212230134](https://i.loli.net/2021/03/09/skdD5m6T7VfKvwW.png)

每次包到达一个路由器，路由器会查询往哪里发送，然后在进行转发操作。

![image-20210308212327023](https://i.loli.net/2021/03/09/8LdhWa2yOB4m9QH.png)

为了维护这样的转发，每个路由器都有一张路由控制表，其记录着每个IP该往哪里发送。



#### 通信链路具象化

在数据传输的过程中，可能会遇到不同的传输类型引发的最大数据包不同的问题，但是IP协议允许我们把一个大的包进行拆分然后一个个的进行传输，在收到后在进行拼接，这样我们就不用考虑这样的一些传输的具体的问题了。



#### IP是无连接类型

IP只负责发送，而且其不需要先建立连接，发就行了。至于处理发送异常，则需要TCP来进行处理。简单的来看，因为IP已经表明了目的地，所以只需要发送，而发送的目标会通过路由到达。



### IP基础知识

#### 地址定义

IP（v4）由32个bit位组成。其由网络和主机两个标识构成。网络标识一个网络群，而这个网络群内的则通过主机标识进行标志。IP在转发时，通过网络标识进行转发。现在我们往往通过子网掩码来标志32位中哪些是网络标识哪些是主机标识。

![image-20210308213227722](https://i.loli.net/2021/03/09/G3SWnfHZyI7clXM.png)

![image-20210308213237161](https://i.loli.net/2021/03/09/D3m9yvrVWRgM8XG.png)

#### 地址分类

地址根据开始的四位分成ABCD四类，当然还有一个未使用的E类。

![image-20210308213356065](https://i.loli.net/2021/03/09/CBR9saDvStp28yr.png)

值得注意的是，在一个网络内，我们往往不使用全0和全1的主机标识，全1是用来做全局广播的，0则标识主机地址不可获知。所以计算主机数量时需要减去2。



#### 广播地址

主机标识全1表示广播。广播分两种，本地广播和直接广播。

本地广播是将主机标识改成1，从而在本地内进行全部传输。而直接广播则是往别的网络传一个广播，传送给别的网络内的全部主机。

![image-20210308213844886](https://i.loli.net/2021/03/09/uk7XgTOx5ywcq3Q.png)



#### IP多播

多播是指发送给特定范围的一些主机，在单播和广播之间。

![image-20210308213949542](https://i.loli.net/2021/03/09/s1eKC7q9krEhXFn.png)

多播使用D类地址完成，开头4位是1110即为D类地址。

然后通过后28位的变化来实现多播。



#### 子网掩码

为了拓展网络，人们引入了子网掩码。引入子网掩码后，需要用IP地址和子网掩码放一起来表示一个地址。子网掩码用1来表示网络表示，而用0来表示主机表示。IP地址对应1的部分是网络，对应0的部分是主机标识。

![image-20210308214509419](https://i.loli.net/2021/03/09/QaY8IdWlZ5LjhAg.png)

怎么理解子网掩码呢？我们可以认为有了子网掩码，我们可以用它来表示一个网络中主机的上限，比如一个网络只有十台主机，那我们分16位的主机位置显然是浪费的，所以我们就可以用它来拓展网络表示，这样就相当于拓展了网络表示的长度。



#### CIDR和VLSM

CIDR允许几个C类地址合成一个B类地址，而VLSM则可以实现动态的子网掩码。



#### 全部IP和私有IP

![image-20210308215144820](https://i.loli.net/2021/03/09/yLqsAv51ChHZPU9.png)

通过NAT桥接技术来实现一个主IP与多个私有IP。



### 路由控制

数据在转发时，需要路由器根据路由表寻址。该部分由路由协议负责。



#### IP地址与路由控制

![image-20210308215551933](https://i.loli.net/2021/03/09/jRmqtKXLJkMveph.png)

路由控制表中记录着网络地址与下一步应该发送至路由器的地址（在Windows或Unix上表示路由表的方法分别为netstat-r 或netstat-rn。） 。在发送IP包时，首先要确定IP包首部中的目标地址，再从路由控制表中找到与该地址具有相同网络地址的记录，根据该记录将IP包转发给相应的下一个路由器。如果路由控制表中存在多条相同网络地址的记录，就选择一个最为吻合的网络地址。所谓最为吻合是指相同位数最多的意思（也叫最长匹配。） 。



#### 路由控制表的聚合

即使有子网掩码，我们也不考虑他，我们只考虑网络号，这样整个路由表记录的内容就会减少。

![image-20210308215907979](https://i.loli.net/2021/03/09/jhSlgmfiDqEMIaB.png)



#### IP分割处理与再构成处理

在传输过程中，如果某个包较大，我们需要考虑对齐进行分包，比如把一个5000的包分成5个1000的包，这取决与MTU的大小。

![image-20210308220033607](https://i.loli.net/2021/03/09/HNujO9YBPmGElvg.png)



#### 路径MTU技术

假设有一条链路，其一开始的MTU是1000，后来是20，那么在过程中我们需要再分包，这样会出现一些问题。所以引入了路径MTU技术。

![image-20210309094818932](https://i.loli.net/2021/03/09/vSuVcRJ3WCa6dTx.png)

如果是UDP情况下，会先设置禁止分包为1，这样后面的路由接收到包以后如果要分包的话就会直接将其丢弃，然后把新的MTU值返回，如此往复即可。

TCP情况下会根据这些信息计算出MSS（最大段长度）然后进行数据发送。

![image-20210309095212153](https://i.loli.net/2021/03/09/fVw6GQc175Mxjeo.png)



### IPV6(暂时不加入)



### IPv4首部

![image-20210309095454833](https://i.loli.net/2021/03/09/cSMwORtVK7sUkJA.png)

总长度记录整个首部与数据部分总长度。标识用于分片重组，如果是一个包的数据的话他们会是一个标识就可以将他们重组。标志位记录着分片信息。片位移记录这是该包的第几个片。生存时间标识最多经过多少次转发，每次转发值减1，到0就丢弃。协议部分标明是什么协议。首部校验和用来校验。可选字段可以加入安全级别、源路径、时间戳等。填充字段用来使整个首部是32bit的整数倍。





## IP协议相关技术

主要介绍DNS、ARP、ICMP以及DHCP等协议。



### DNS

用户可以通过host来实现本地的域名与ip映射。

![image-20210309100803923](https://i.loli.net/2021/03/09/mf2GaC1dYIUtkA4.png)

但是随着域名的变多，这样的操作已经不可行了，所以引入了DNS机制。

用户通过DNS解析器解析网站获得IP地址然后进行访问：

![image-20210309100938545](https://i.loli.net/2021/03/09/nIdwRs8Qptku1Eo.png)

首先会访问本地解析缓存，不命中则访问host文件，之后访问最近的DNS解析器，如果有缓存就结束了。如果到这里还没找到，则会调用根DNS解析器，然后会返回下一级的解析器，逐步往下。例如taobao.com，则会返回.com的解析器。



### ARP

ARP是一种把IP地址转换为MAC地址的协议。



#### ARP工作机制

ARP是借助ARP请求和ARP响应两种类型的包确定的。

![image-20210309102930966](https://i.loli.net/2021/03/09/trh2ObySfWCkvwi.png)

ARP请求会进行广播，发送给一条链路上的全部主机，目标机收到后会返回相关的MAC地址。然后会把相关的信息进行缓存。



在整个过程中，IP负责整体目的地的把控，而MAC地址则实现短链路内的把控。



### RARP

RARP是一种把MAC地址转换为IP地址的协议。

![image-20210309104424293](https://i.loli.net/2021/03/09/HwQFXtLKdfpelbI.png)



### ICMP

ICMP的主要功能包括，确认IP包是否成功送达目标地址，通知在发送过程当中IP包被废弃的具体原因，改善网络设置等。

ICMP的消息大致可以分为两类：一类是通知出错原因的错误消息，另一类是用于诊断的查询消息。

![image-20210309104506872](https://i.loli.net/2021/03/09/CTd7PgI2t8lLBrc.png)



### DHCP

实现IP设置的即插即用。

![image-20210309104612273](https://i.loli.net/2021/03/10/8V34BRLIJ6M5sKq.png)

工作机制上，需要一台专门的DHCP服务器。

![image-20210309104635810](https://i.loli.net/2021/03/10/RrpLHkvdU2f5m8G.png)

### NAT

NAT（Network Address Translator）是用于在本地网络中使用私有地址，在连接互联网时转而使用全局IP地址的技术。除转换IP地址外，还出现了可以转换TCP、UDP端口号的NAPT（Network Address PortsTranslator）技术，由此可以实现用一个全局IP地址与多个主机的通信。

工作机制：对外使用一个公网，对内通过转换表进行拆分。

![image-20210309110332596](https://i.loli.net/2021/03/10/RQrfmdj2UXwEoh7.png)



### 其他IP技术

#### IP多播



#### IP任播



#### 通信质量控制



#### 显式拥塞通知



#### Mobile IP





## TCP与UDP

本章主要讲述传输层以及传输层的两个重要协议：TCP和UDP。



### 传输层

IP首部会记录传输层使用的是什么协议，通信双方可以根据该协议实现通信。本层主要负责的就是传输，传输某个包到某个电脑。就像寄快递一样，我们寄到一个家以后，我们并不能知道是谁的快递，所以我们需要根据名字来进行区别。因为在传输层，我们会加入一个端口号来表示是和某个主机的某个端口进行通信。

![image-20210309194045282](https://i.loli.net/2021/03/10/RqeLrnvKPMgy7xs.png)



在这里我们先做一个简单的定义，我们自己用的电脑叫客户端，而我们访问的网站那端则成为服务端。

![image-20210309194311207](https://i.loli.net/2021/03/10/RIj2dkFOKmDg3l7.png)




#### TCP与UDP

TCP是面向连接的、可靠的流协议。TCP比较可靠，具有流量控制、堵塞控制、重发等功能。

UDP是不具有可靠性的数据报协议。他能保证发送数据包的大小，但并不保证一定送达，发出去就算成功。

TCP用于在传输层有必要实现可靠传输的情况。由于它是面向有连接并具备顺序控制、重发控制等机制的，所以它可以为应用提供可靠传输。
而UDP发出去就完事了，经常被用来一些视频直播、通话等领域（同步？）。假设我们在视频传输的时候使用了TCP，那么一但卡顿了就会重发，这样就会导致整个视频播放的不流畅。

在使用TCP/UDP的时候，我们常常需要使用套接字。应用程序利用套接字，可以设置对端的IP地址、端口号，并实现数据的发送与接收。

![image-20210309194802174](https://i.loli.net/2021/03/10/M2BDw8sSlgO7TGP.png)



### 端口号

举一个简单的例子，一台电脑部署了五个网站，但是ip是一样的，那么我们访问的时候如何知道访问的是哪一个呢？所以在这里我们就引入了端口号，通过端口来实现访问。简单的说，我们可以把端口看成是一个管子，一台电脑有很多个管子，每次我们访问其中的一个。

![image-20210309194923536](https://i.loli.net/2021/03/10/bPsvpzyHrNZ36fY.png)

#### 只有端口号够吗

但是仅仅凭借端口号，是不能实现通信的。

比如我们开了两个不同的网页，这两个网页都是80端口，如果只看端口那我们就无法区分这两个。因此，TCP/IP或UDP/IP通信中通常采用5个信息来识别（这个信息可以在Unix或Windows系统中通过netstat -n 命令显示。） 一个通信。它们是“源IP地址”、“目标IP地址”、“协议号”、“源端口号”、“目标端口号”。只要其中某一项不同，则被认为是其他通信。

![image-20210309195105208](https://i.loli.net/2021/03/10/A72JXpywoz6xSqr.png)

#### 如何确定端口号

一般有两种方法，第一种是静态方法，第二种是时序分配法。

静态方法就是我们规定好某个东西就是某个端口，比如一般80端口是作为web端口。

而时序分配法则是根据时序来分配端口，也即用户无须care这样的一个存在，其对于用户是透明的，是由操作系统自动分配的东西。



#### 端口号与协议

端口号由其使用的传输层协议决定。因此，不同的传输协议可以使用相同的端口号。数据到达IP层后，会先检查IP首部中的协议号，再传给相应协议的模块。如果是TCP则传给TCP模块、如果是UDP则传给UDP模块去做端口号的处理。即使是同一个端口号，由于传输协议是各自独立地进行处理，因此相互之间不会受到影响。



### UDP

UDP是User Datagram Protocol的缩写。

UDP不提供复杂的控制机制，利用IP提供面向无连接的通信服务。并且它是将应用程序发来的数据在收到的那一刻，立即按照原样发送到网络上的一种机制。其不具备什么重发机制，发完就拉倒，如果需要重发的话，需要那些使用UDP的程序来进行操作。



### TCP

TCP是对“传输、发送、通信”进行“控制”的“协议”。

TCP与UDP的区别相当大。它充分地实现了数据传输时各种控制功能，可以进行丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。此外，TCP作为一种面向有连接的协议，只有在确认通信对端存在时才会发送数据，从而可以控制通信流量的浪费。

TCP的目的就是实现可靠的连接，为了达到这样的目的，TCP通过检验和、序列号、确认应答、重发控制、连接管理以及窗口控制等机制实现可靠性传输。



#### 通过序列号与确认应答提高可靠性

在TCP中，当发送端的数据到达接收主机时，接收端主机会返回一个已收到消息的通知。这个消息叫做确认应答（ACK（ACK（Positive Acknowled-gement）意指已经接收。） ）。就像两个人在讲话一样，我们如何知道对方是否在听我们讲话呢？很简单，观察他或者看他的回答，也即一种反馈。

![image-20210309200019735](https://i.loli.net/2021/03/10/xiurpoJ37W2wVSn.png)

而如果我们发现对方好像没听到我们说的话，我们就会再说一遍，TCP也是这样操作的：

![image-20210309200059001](https://i.loli.net/2021/03/10/C8lAMp1oBYLiSfh.png)

当然，这也可能是因为我们没收到答复而出现的情况。

![image-20210309200135679](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210309200135679.png)

那么问题来了，对于说话的人来说，说的哪句话是什么顺序，我们显然是知道的，但是对于接收的人他可能并不知道说话的顺序。所以我们就要在里面加上某句话是第几句这样的标签，也即序列号。

![image-20210309200346153](https://i.loli.net/2021/03/10/sQldDYBWXJtvCag.png)

![image-20210309200353515](https://i.loli.net/2021/03/10/uOg8EbRTBHmV1Kx.png)



#### 如何确定要重发

上文说到，我们可能需要重发数据包，那么什么时候需要重发呢？

最理想的情况来看，我们知道数据发送过去的时间和返回的时间的总和的话，我们就能比较好的知道这样的一个最小重发时间了。

为此，它在每次发包时都会计算往返时间（Round Trip Time也叫RTT。是指报文段的往返时间。） 及其偏差（RTT时间波动的值、方差。有时也叫抖动。） 。将这个往返时间和偏差相加重发超时的时间，就是比这个总和要稍大一点的值。

![image-20210309200557943](https://i.loli.net/2021/03/10/glLGIdvcD1hbpNZ.png)

#### 连接管理

讲完了数据的发送，我们需要介绍一下连接的管理。在通信的最开始，两个主机并没有连接到一起，虽然他们可能物理上连接在了一起。我们通过SYN信号和ACK信号来实现连接的管理。

![image-20210309200723544](https://i.loli.net/2021/03/10/sl4gQZ6Jbv3yYtS.png)

就比如A要和B讲话一样，A先需要告诉B他想说点啥，B需要表示知道了这样的请求并且表示同意，然后A知道B同意以后也需要回复B一个信号。我们可以把这个看成是一个凡有请求，必有回答的过程。这样的过程，我们成为三次握手。

而一次TCP的建立和断裂，需要至少七次包才能完成。所以如果在直播中采取TCP的话，如果不停的建立和断开，需要花大量的数据包在这方面。



#### TCP以段为单位进行数据发送

在建立TCP连接的同时，也可以确定发送数据包的单位，我们也可以称其为“最大消息长度”（MSS：Maximum Segment Size）。最理想的情况是，最大消息长度正好是IP中不会被分片处理的最大数据长度。

MSS是在三次握手的时候，在两端主机之间被计算得出。两端的主机在发出建立连接的请求时，会在TCP首部中写入MSS选项，告诉对方自己的接口能够适应的MSS的大小，这样协商得到一个MSS的大小即可。

![image-20210309201744396](https://i.loli.net/2021/03/10/HmYdXoevqF4LpfC.png)



#### 利用窗口提高速度

TCP以1个段为单位，每发一个段进行一次确认应答的处理，如图6.14。这样的传输方式有一个缺点。那就是，包的往返时间越长通信性能就越低。

![image-20210309201812442](https://i.loli.net/2021/03/10/LinSYXVDpcbv8Ms.png)

也即我们不能等收到了结果再进行操作，这样的同步并不适合需求高速率的我们。为解决这个问题，TCP引入了窗口这个概念。即使在往返时间较长的情况下，它也能控制网络性能的下降。

![image-20210309201851328](https://i.loli.net/2021/03/10/usc5NdklYXWmZaP.png)

窗口大小就是指无需等待确认应答而可以继续发送数据的最大值。需要用到一块缓冲区来存储。整个流程就是，我们先把一些数据发出去，然后把他们保存下来，防止需要重发的情况出现，如果收到了应答，那么我们就删除相关的缓存。这里有点类似于计组里的流水线，通过流水线来加快指令的执行。

![image-20210309202125022](https://i.loli.net/2021/03/10/Tr9H7divCQG5hcW.png)



#### 窗口控制与重发控制

在窗口控制里，我们已经采取了类似流水的方式加快了包的发送，那么我们自然也就不能根据时间来进行重发管理了。于是我们采取如下的方式进行重发管理：

![image-20210309203235295](https://i.loli.net/2021/03/10/vUkaA4gspiCuxEQ.png)

如果某个包接收端收到了但ACK发送端没收到，这不会出现影响，因为接收端不会发出连着三次的未收到信号，所以无所谓。



#### 流控制

如果接收端处理速度比较慢，在面临大量的数据包的时候就会丢弃掉后面的一些包，而这样又会引发重发，就出现了大量的流量浪费。

所以就需要一种机制来解决这种问题。

TCP提供一种机制可以让发送端根据接收端的实际接收能力控制发送的数据量。这就是所谓的流控制。它的具体操作是，接收端主机向发送端主机通知自己可以接收数据的大小，于是发送端会发送不超过这个限度的数据。该大小限度就被称作窗口大小。

TCP首部中，专门有一个字段用来通知窗口大小。接收主机将自己可以接收的缓冲区大小放入这个字段中通知给发送端。这个字段的值越大，说明网络的吞吐量越高。
不过，接收端的这个缓冲区一旦面临数据溢出时，窗口大小的值也会随之被设置为一个更小的值通知给发送端，从而控制数据发送量。也就是说，发送端主机会根据接收端主机的指示，对发送数据的量进行控制。这也就形成了一个完整的TCP流控制（流量控制）。

简单的来说，我们可以把这个认为是一种协商的窗口大小的机制。

![image-20210309204202834](https://i.loli.net/2021/03/10/rKQuYkgtSMBF93A.png)



#### 拥塞控制

有了TCP的窗口控制，收发主机之间即使不再以一个数据段为单位发送确认应答，也能够连续发送大量数据包。然而，如果在通信刚开始时就发送大量数据，也可能会引发其他问题。尤其是一开始的时候就发送大量的数据，这样就可能会导致系统瘫痪。

于是TCP采取了一种慢启动的机制。

![image-20210309204410654](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210309204410654.png)

也即一开始只发送一个包，然后每收到一次ACK，就加大包的大小，前期可能采取指数增长的形式，但是随着增长，窗口可能过大了，所以我们引入了拥塞窗口阈值这样的概念，也即超出这个阈值后，我们就只能按照某一种比例来加大了。



#### 如何提高网络利用率

##### Nagle

该算法是指发送端即使还有应该发送的数据，但如果这部分数据很少的话，则进行延迟发送的一种处理机制。具体来说，就是仅在下列任意一种条件下才能发送数据。如果两个条件都不满足，那么暂时等待一段时间以后再进行数据发送。

- 已发送的数据都已经收到确认应答时
- 可以发送最大段长度（MSS）的数据时



##### 延迟确认应答

当接收端收到一个包以后，不立即进行应答，可以等收到更多的包后再进行应答。

![image-20210309210005761](https://i.loli.net/2021/03/10/1hl6mtjWIK43Dq9.png)



##### 捎带应答

简单的说，我们发送邮件的时候，会有回执告诉我们是否发送成功，我们的应答信息可以跟这个回执一起返回回来，这样就不用单独的发包了。

![image-20210309210143977](https://i.loli.net/2021/03/10/4pDtgaSN2jrQksC.png)



### UDP格式

![image-20210309210648724](https://i.loli.net/2021/03/10/VL5PEwWAcoMs4Gn.png)



#### TCP首部格式

![image-20210309210852489](https://i.loli.net/2021/03/10/fxGWkZtdCHDlaBX.png)

注意，这里有一个紧急指针，解释下：
该字段长为16位。只有在URG控制位为1时有效。该字段的数值表示本报文段中紧急数据的指针。正确来讲，从数据部分的首位到紧急指针所指示的位置为止为紧急数据。因此也可以说紧急指针指出了紧急数据的末尾在报文段中的位置。
如何处理紧急数据属于应用的问题。一般在暂时中断通信，或中断通信的情况下使用。例如在Web浏览器中点击停止按钮，或者使用TELNET输入Ctrl + C时都会有URG为1的包。此外，紧急指针也用作表示数据流分段的标志。



## 路由协议

为了能让数据包正确达地到达目标主机，路由器必须在途中进行正确地转发。这种向“正确的方向”转发数据所进行的处理就叫做路由控制或路由。

路由器根据路由控制表（Routing Table）转发数据包。它根据所收到的数据包中目标主机的IP地址与路由控制表的比较得出下一个应该接收的路由器。因此，这个过程中路由控制表的记录一定要正确无误。但凡出现错误，数据包就有可能无法到达目标主机。

路由管理分为静态路由管理和动态路由管理。

静态路由是指事先设置好路由器和主机中并将路由信息固定的一种方法。而动态路由是指让路由协议在运行过程中自动地设置路由控制信息的一种方法。如果采用静态路由，那么需要添加新的信息的时候就会很麻烦。

![image-20210310102920343](https://i.loli.net/2021/03/10/Za1NDjpCeYt89ko.png)

动态路由的话，由管理员设置好路由协议即可。

动态路由会给相邻路由器发送自己已知的网络连接信息，而这些信息又像接力一样依次传递给其他路由器，直至整个网络都了解时，路由控制表也就制作完成了。而此时也就可以正确转发IP数据包了

![image-20210310103121851](https://i.loli.net/2021/03/10/gU1uaRjdSE7p8DL.png)



### 路由控制范围

因为网络很大，所以通过一个路由管理全部网络是不可能的，所以人们设置了管理范围。一般有IGP（Interior Gateway Protocol）和EGP（Exterior Gateway Protocol）（EGP是特定的路由协议名称，请不要与其他同名词汇混淆。） 两种类型的路由协议。

在某个大公司内部，其网络部署可以由该公司自己决定，也即这是一种自治系统，这种一般是域内路由协议。而自治系统之间，往往是是域间路由协议，即EGP。

IGP中还可以使用RIP（Routing Information Protocol，路由信息协议）、RIP2、OSPF（Open ShortestPath First，开放式最短路径优先）等众多协议。与之相对，EGP使用的是BGP（Border Gateway Protocol，边界网关协议）协议。



#### 路由算法

一般有距离向量（Distance-Vector）算法和链路状态（Link-State）算法。



##### 距离向量算法

距离向量算法（DV）是指根据距离（代价（Metric是指转发数据时衡量路由控制中距离和成本的一种指标。在距离向量算法中，代价相当于所要经过的路由器的个数。） ）和方向决定目标网络或目标主机位置的一种方法。

![image-20210310104003050](https://i.loli.net/2021/03/10/Dxb6YCG1c5LrH4j.png)

路由器之间可以互换目标网络的方向及其距离的相关信息，并以这些信息为基础制作路由控制表。



##### 链路状态算法

链路状态算法是路由器在了解网络整体连接状态的基础上生成路由控制表的一种方法。该方法中，每个路由器必须保持同样的信息才能进行正确的路由选择。而基于距离的路由算法中，因为路由器并不能取得全部网络的数据，所以会浪费时间。

链路算法也即每个路由器都是一致的，即使有某一个不一致，他也可以很快同步。因此，我们就需要解决如何从网络获取路由表的问题。

![image-20210310104519060](https://i.loli.net/2021/03/10/3VxvsEqPfBaXuQS.png)



#### 主要路由协议

![image-20210310104639819](https://i.loli.net/2021/03/10/EXA4B1HKzMNGlea.png)



### RIP

RIP（Routing Information Protocol）是距离向量型的一种路由协议。



RIP将路由控制信息定期（30秒一次）向全网广播。如果没有收到路由控制信息，连接就会被断开。不过，这有可能是由于丢包导致的，因此RIP规定等待5次。如果等了6次（180秒）仍未收到路由信息，才会真正关闭连接。

![image-20210310105059837](https://i.loli.net/2021/03/10/pBwCtEjADPvsULQ.png)

RIP基于距离向量算法决定路径。距离（Metrics）的单位为“跳数”。跳数是指所经过的路由器的个数。RIP希望尽可能少通过路由器将数据包转发到目标IP地址。根据距离向量生成距离向量表，再抽出较小的路由生成最终的路由控制表。

![image-20210310105136565](https://i.loli.net/2021/03/10/JOWRDKw4CgscalV.png)



#### 使用子网掩码的RIP

RIP虽然不交换子网掩码信息，但可以用于使用子网掩码的网络环境。

![image-20210310105853239](https://i.loli.net/2021/03/10/TsthVorzjyqvxlp.png)



#### RIP中路由变更的处理

RIP的基本行为可归纳为如下两点：

- 将自己所知道的路由信息定期进行广播。
- 一旦认为网络被断开，数据将无法流过此路由器，其他路由器也就可以得知网络已经断开。

但是在出故障的时候，该机制就会出现问题。如下图所示，路由器A连接不上网络A了，但是他仍然能和B通信，由于B收到A的信息得到了B到A网络的距离，A也会根据A和B的距离进行无限的重复计算。

![image-20210310110116206](https://i.loli.net/2021/03/10/a426rJ5GKj9Ocix.png)

为了解决这样的问题，我们采取两种方法，第一种限制最长距离，这样距离就不会一直累加，第二种采取水平分割的方式。不把收到的信息进行原路返回。

![image-20210310110352511](https://i.loli.net/2021/03/10/gye71PfLt8chmr2.png)

但是如果网络本身就出现了回路，那么即使采取了水平分割还是会出现回环的情况，所以人们提出了“毒性逆转”（Poisoned Reverse）和“触发更新”（Triggered Update）两种方法。

![image-20210310110742694](https://i.loli.net/2021/03/10/Vrm1pWwkTyUs89H.png)



毒性逆转是指当网络中发生链路被断开的时候，不是不再发送这个消息，而是将这个无法通信的消息传播出去。即发送一个距离为16的消息。触发更新是指当路由信息发生变化时，不等待30秒而是立刻发送出去的一种方法。有了这两种方法，在链路不通时，可以迅速传送消息以使路由信息尽快收敛。也即将路由变化情况尽可能快的传送出去。

![image-20210310110843061](https://i.loli.net/2021/03/10/ZwkzUdxQICymG3R.png)

但是还是会有一些问题存在，所以人们提出了OSPF。



### OSPF

OSPF（Open Shortest Path First）是根据OSI的IS-IS（Intermediate System to Intermediate System Intra-Domain routing information exchange protocol，中间系统到中间系统的路由选择协议。） 协议而提出的一种链路状态型路由协议。由于采用链路状态类型，所以即使网络中有环路，也能够进行稳定的路由控制。

OSPF支持子网掩码，并且引入了区域的概念。



OSPF是一种链路状态协议，路由器之间交换信息生成路由控制表。

![image-20210310111538154](https://i.loli.net/2021/03/10/3PDdWz2GLO4tyCe.png)

与RIP相比，OSPF能够给网络加权重，从而选择一个权重最小的路径，而RIP则只能选择中间经过路由器最少的路径。

![image-20210310111703674](https://i.loli.net/2021/03/10/NlPaqDeQHBCiJFY.png)



#### OSPF基础

在OSPF中，把连接到同一个链路的路由器称作相邻路由器（Neighboring Router）。在一个比较简单的网络里，我们可以通过一个一个传，但是在复杂网络里，我们需要指定一个作为路由的中心，通过它来传递路由信息。

RIP中包的类型只有一种。它利用路由控制信息，一边确认是否连接了网络，一边传送网络信息。但是随着网络规模的变大，包的内容就变大了，这样会浪费一些带宽。

在OSPF中，包可以分为以下几种：

![image-20210310112313162](https://i.loli.net/2021/03/10/hMFD2PlGQXRK1Ve.png)

通过发送问候（HELLO）包确认是否连接。每个路由器为了同步路由控制信息，利用数据库描述（Database Description）包相互发送路由摘要信息和版本信息。如果版本比较老，则首先发出一个链路状态请求（Link State Request）包请求路由控制信息，然后由链路状态更新（Link State Update）包接收路由状态信息，最后再通过链路状态确认（Link State ACK Packet）包通知大家本地已经接收到路由控制信息。



#### OSPF工作原理

OSPF中进行连接确认的协议叫做HELLO协议。

![image-20210310112551296](https://i.loli.net/2021/03/10/c26JiMHaOsRZVkC.png)

LAN中每10秒发送一个HELLO包。如果没有HELLO包到达，则进行连接是否断开的判断（管理员可以自定义HELLO包的发送间隔和判断连接断开的时间。只是在同一个链路中的设备必须配置相同的值。） 。具体为，允许空等3次，直到第4次（40秒后）仍无任何反馈就认为连接已经断开。之后在进行连接断开或恢复连接操作时，由于链路状态发生了变化，路由器会发送一个链路状态更新包（Link State Update Packet）通知其他路由器网络状态的变化。

链路状态更新包所要传达的消息大致分为两类：一是网络LSA（Network Link State Advertisement，网络链路状态通告。） ，另一个是路由器LSA（Router Link State Advertisement，路由器链路状态通告。） 。网络LSA是以网络为中心生成的信息，表示这个网络都与哪些路由器相连接。而路由器LSA是以路由器为中心生成的信息，表示这个路由器与哪些网络相连接。



#### 区域分层化管理

当网络规模变大的时候，我们存储相关网络拓扑的时间就会上升，所以人们采用了区域分层化管理的方式来解决问题。

简单的来说，我们允许一个区域被划分成多个小区域，这个区域里选择一个中心，其他的块都与该中心相连。

连接区域与主干区域的路由器称作区域边界路由器；而区域内部的路由器叫做内部路由器；只与主干区域内连接的路由器叫做主干路由器；与外部相连接的路由器就是AS边界路由器。

![image-20210313152436233](https://i.loli.net/2021/03/13/cIGZ1tkJRlQ83Nz.png)

每个区域内的路由器都持有本区域网络拓扑的数据库。然而，关于区域之外的路径信息，只能从区域边界路由器那里获知它们的距离。区域边界路由器也不会将区域内的链路状态信息全部原样发送给其他区域，只会发送自己到达这些路由器的距离信息，内部路由器所持有的网络拓扑数据库就会明显变小。

![image-20210313152533836](https://i.loli.net/2021/03/13/Kf9J7knWdLsZTAl.png)

简单地说，就是一个小区只管自己小区的事，只记录自己这的路由表，这样就减少了存储和发送的问题。





### RIP2

RIP2是RIP的升级版本，其多了一些新的特性：

- 使用多播
- 支持子网掩码
- 路由选择域
- 外部路由标志
- 身份验证密钥



### BGP

BGP（Border Gateway Protocol），边界网关协议是连接不同组织机构（或者说连接不同自治系统）的一种协议。



#### BGP和AS号

在RIP和OSPF中利用IP的网络地址部分进行着路由控制，然而BGP则需要放眼整个互联网进行路由控制。BGP的最终路由控制表由网络地址和下一站的路由器组来表示，不过它会根据所要经过的AS个数进行路由控制。

![image-20210313152825087](https://i.loli.net/2021/03/13/YH3eihrpFOoD8bx.png)

简单地说， 一个一个的自治系统（AS），我们对他们进行编号，通过BGP来处理他们之间的通信。



#### BGP是路径向量协议

根据BGP交换路由控制信息的路由器叫做BGP扬声器。BGP扬声器为了在AS之间交换BGP信息，必须与所有AS建立对等的BGP连接。

BGP中数据包送达目标网络时，会生成一个中途经过所有AS的编号列表。这个表格也叫做AS路径信息访问列表（AS Path List）。如果针对同一个目标地址出现多条路径时，BGP会从AS路径信息访问列表中选择一个较短的路由。

在做路由选择时使用的度量，RIP中表示为路由器个数，OSPF中表示为每个子网的成本，而BGP则用AS进行度量标准，也即BGP选择通过AS最小的路径。



### MPLS

现如今，在转发IP数据包的过程中除了使用路由技术外，还在使用标记交换技术。路由技术基于IP地址中最长匹配原则进行转发，而标记交换则对每个IP包都设定一个叫做“标记”的值，然后根据这个“标记”再进行转发。标记交换技术中最具代表性的当属多协议标记交换技术，即MPLS（Multi Protocol Label Switching）。

![image-20210313153133784](https://i.loli.net/2021/03/13/hDc3VlC2pgRwubM.png)

![image-20210313153157027](https://i.loli.net/2021/03/13/qTEDAjCJ8bhpcPR.png)

#### MPLS基本动作

MPLS网络中实现MPLS功能的路由器叫做标记交换路由器（LSR，Label Switching Router）。特别是与外部网路连接的那部分LSR叫做标记边缘路由器（LER，Label Edge Router）。MPLS正是在LER上对数据包进行追加标记和删除标记的操作。

如果一个包没有附加MPLS标记，那么我们就给他加上标记，如果有标记的话，那么就按照这个标记进行转发。

![image-20210313153357601](https://i.loli.net/2021/03/13/V1gklaWNM9vGCse.png)

MPLS中目标地址和数据包（它们被称作FEC（Forward-ing Equivalence Class），是指具有相同特性的报文。） 都要通过由标记决定的同一个路径，这个路径叫做标记交换路径（LSP，Label Switch Path）。



#### MPLS的优点

一：转发速度快

二：利用标记生成虚拟的路径



## 应用协议

主要介绍一些常见的应用层协议。



### 远程登陆

远程登陆主要是为了实现分时系统，也即一台主机多个用户。主要分为SSH和TALNET两种协议。



#### TELNET

TELNET利用TCP的一条连接，通过这一条连接向主机发送文字命令并在主机上执行。本地用户好像直接与远端主机内部的Shell相连着似的，直接在本地进行操作。

![image-20210313155421480](https://i.loli.net/2021/03/13/oXbV4YiyqPznvCG.png)

TELNET有行模式和透明模式两种。

![image-20210313155508825](https://i.loli.net/2021/03/13/VUicdy5DxwZt86O.png)



#### SSH

SSH是加密的远程登录系统。TELNET中登录时无需输入密码就可以发送，容易造成通信窃听和非法入侵的危险。使用SSH后可以加密通信内容。即使信息被窃听也无法破解所发送的密码、具体命令以及命令返回的结果是什么。

SSH有一些比较好的功能，诸如：

- 可以使用更强的认证机制
- 可以转发文件
- 可以使用端口转发功能

![image-20210313155959459](https://i.loli.net/2021/03/13/BsKZuYfJj3TvH6D.png)



### 文件传输

文件传输主要基于FTP协议。

FTP使用两条TCP连接：一条用来控制，另一条用于数据（文件）的传输。

用于控制的TCP连接主要在FTP的控制部分使用。一般使用21端口。

数据传输用的TCP连接通常使用端口20。

控制连接会一直持续，而数据连接在进行一次数据传输后就会结束，下次传输重新连接。

![image-20210313160148733](https://i.loli.net/2021/03/13/5EciltZuR3s9eTF.png)



### 电子邮件

提供电子邮件服务的协议叫做SMTP（Simple Mail Transfer Protocol）。SMTP为了实现高效发送邮件内容，在其传输层使用了TCP协议。

![image-20210313160531666](https://i.loli.net/2021/03/13/F7onVdXK2igWbZw.png)

![image-20210313160541895](https://i.loli.net/2021/03/13/E8Mjx92BsgVutWk.png)

可以这么认为，现在我们使用的邮件，很多都是由第三方邮件服务器进行接收，然后我们再去使用的。

原来邮件只能发送文件，但是随着时间的变化，邮件已经可以发送其他类型的文件了。这主要是通过MIME实现的，MIME通过把数据拆分然后合并来实现传输其他媒体文件。



#### SMTP

SMTP是发送电子邮件的协议。它使用的是TCP的25号端口。SMTP建立一个TCP连接以后，在这个连接上进行控制和应答以及数据的发送。

![image-20210313161927382](https://i.loli.net/2021/03/13/OCZlkboaAQMNgjG.png)

#### POP

随着时代发展，个人主机因为不能长时间开机，所以使得人们不能一直实时传输，所以引入了POP协议。

![image-20210313161758066](https://i.loli.net/2021/03/13/q7XS98ZUB63GyYl.png)

该协议是一种用于接收电子邮件的协议。发送端的邮件根据SMTP协议将被转发给一直处于插电状态的POP服务器。客户端再根据POP协议从POP服务器接收对方发来的邮件。在这个过程中，为了防止他人盗窃邮件内容，还要进行用户验证。

![image-20210313161904045](https://i.loli.net/2021/03/13/52jsdryhuZEBkSO.png)

OP与SMTP一样，也是在其客户端与服务器之间通过建立一个TCP连接完成相应操作。



#### IMAP

IMAP（Internet Message Access Protocol） 与POP类似，也是接收电子邮件的协议。在POP中邮件由客户端进行管理，而在IMAP中邮件则由服务器进行管理。使用IMAP时，可以不必从服务器上下载所有的邮件也可以阅读。



### WWW

万维网（WWW，World Wide Web）是将互联网中的信息以超文本（超文本用以显示文本及与文本相关的内容。） 形式展现的系统。

![image-20210313162402810](https://i.loli.net/2021/03/13/7t4PYZHxER8iuUl.png)



WWW主要由以下三个概念组成：URL、HTTP、HTML



#### HTTP

当用户在浏览器的地址栏里输入所要访问Web页的URI以后，HTTP的处理即会开始。HTTP中默认使用80端口。它的工作机制，首先是客户端向服务器的80端口建立一个TCP连接，然后在这个TCP连接上进行请求和应答以及数据报文的发送。

![image-20210313162531426](https://i.loli.net/2021/03/13/98c5JiN2fKPgqYe.png)



### 网络管理



#### SNMP

随着网络的规模扩大，管理员已经很难靠自己管理网络了。于是人们引入了SNMP协议。

![image-20210313162626097](https://i.loli.net/2021/03/13/r75iHb1AyVUZSj6.png)

在TCP/IP的网络管理中可以使用SNMP（Simple Network Management Protocol）收集必要的信息。它是一款基于UDP/IP的协议。

SNMP中管理端叫做管理器（Manager，网络监控终端），被管理端叫做代理（路由器、交换机等）（SNMPv3中管理器和代理都叫做实体（Entity）。） 。

![image-20210313162738054](https://i.loli.net/2021/03/13/5T6IZCNmbJoBOaG.png)

在SNMP中，MIB是交互的信息，可能看成一个设备的信息，RMON则是一条线路上的信息。





## 网络安全

这一章主要介绍一些网络安全的内容。



### 网络安全要素

![image-20210313163041161](https://i.loli.net/2021/03/13/6dIUCOtpzaiDQ8s.png)



#### 防火墙

组织机构（域）内部的网络与互联网相连时，为了避免域内受到非法访问的威胁，往往会设置防火墙。我们可以为防火墙配置规则，这取决于我们的需求。

![image-20210313163146301](https://i.loli.net/2021/03/13/PCakyE617nHV28r.png)



#### IDS

如果有违背防火墙的信息出现，那么IDS就会起作用。

从功能上看，IDS有定期采集日志、长期监控、通知异常等功能。它可以监控网络上流动的所有数据包。为了确保各种不同系统的安全，IDS可以与防火墙相辅相成，实现更为安全的网络环境。



#### 反病毒/个人防火墙

随着时代发展，个人机越来越多，个人使用的防火墙/杀毒软件等就出现了。



### 加密基础

#### 对称密码体制与公钥密码体制

首先介绍一下加密与解密，两者是一个互逆的过程。

![image-20210313163430632](https://i.loli.net/2021/03/13/hl3xqUkBOYwWd4p.png)

如果加密解密使用的秘钥一致，那么称之为对称加密，如果不一致则成为非对称加密。

对称加密方式包括AES（Advanced Encryption Standard）、DES（Data Encryption Standard）等加密标准，而公钥加密方法中包括RSA、DH（Diffie-Hellman）、椭圆曲线等加密算法。

![image-20210313163518552](https://i.loli.net/2021/03/13/EOZCXpshGFzb7vV.png)



### 安全协议

#### IPsec与VPN

为了防止一些信息外露，人们发明了VPN来构建虚拟局域网。

在构建VPN时，最常被使用的是IPsec。它是指在IP首部的后面追加“封装安全有效载荷”（ESP，Encapsulating Security Payload。） 和“认证首部”（AH，Authentication Header。） ，从而对此后的数据进行加密，不被盗取者轻易解读。在发包的时候附加上述两个首部，可以在收包时根据首部对数据进行解密，恢复成原始数据。由此，加密后的数据不再被轻易破解，即使在途中被篡改，也能够被及时检测。

![image-20210313163955021](https://i.loli.net/2021/03/13/vHnwupRDxGaXQbL.png)



#### TLS/SSL与HTTPS

为了保证网络传输信息的安全，人们尝试通过协议对网络信息进行加密。主要通过SSL证书才实现。

![image-20210313164100633](https://i.loli.net/2021/03/13/Sc8azrNwgukTCit.png)

#### IEEE802.1X

IEEE802.1X是为了能够接入LAN交换机和无线LAN接入点而对用户进行认证的技术。并且它只允许被认可的设备才能访问网络。

![image-20210313164345309](https://i.loli.net/2021/03/13/P7p8JSOzbk2daAE.png)



 