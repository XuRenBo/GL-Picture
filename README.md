# gl-sdk4-macclone
## MAC地址克隆介绍
  MAC地址是由48位二进制数字表示，是网络设备（如计算机、路由器等）唯一的标识符，用于区分和识别网络设备。MAC地址克隆是将一个设备的MAC地址复制到另外一个设备中，从而使得两个设备在同一个网络中具有相同的MAC地址。
## MAC地址克隆的作用
```
  MAC地址克隆的主要作用是在网络环境中隐藏或者更改真实的MAC地址，从而达到某些目的。    
  
  1.避免网络访问限制：某些网络可能会根据设备的MAC地址来进行访问控制和限制该设备，此时克隆一个已经获得访问权限的设备的MAC地址，则可以绕过这些限制，例如智能手机已在网上注册了，则可以将该客户端的MAC地址克隆到路由器上，从而使得路由器也可以连接上网； 

  2.进行新旧设备的替换：当在更换设备后，如果想要新设备和旧设备保持相同的网络权限时，可以使用MAC地址克隆将旧设备的MAC地址克隆到新设备上，从而使得新设备可以获得和旧设备相同的权限，以便于继续使用网络服务；  

  3.隐藏真实的MAC地址：在某些情况下，例如在连接到公共热点时，可能需要将真实的MAC地址隐藏，从而增加匿名性和安全性，这些特性通过MAC地址克隆可以很好的实现。
```
## 路由器上MAC地址的三个模式
### 出厂默认

![](https://github.com/XuRenBo/GL-Picture/blob/master/gl-macclone/%E5%87%BA%E5%8E%82%E9%BB%98%E8%AE%A41.png?raw=true)

![](https://github.com/XuRenBo/GL-Picture/blob/master/gl-macclone/%E5%87%BA%E5%8E%82%E9%BB%98%E8%AE%A42.png?raw=true)

```
出厂默认模式--在不使用MAC地址克隆功能时，路由器的真实MAC地址。
```
### 克隆

![](https://github.com/XuRenBo/GL-Picture/blob/master/gl-macclone/%E5%85%8B%E9%9A%861.png?raw=true)

```
克隆模式--将当前客户端（即连接到路由器上的设备）的MAC地址克隆到路由器上。
```
### 手动设置

![](https://github.com/XuRenBo/GL-Picture/blob/master/gl-macclone/%E6%89%8B%E5%8A%A8%E8%AE%BE%E7%BD%AE1.png?raw=true)

```
手动设置模式--可以手动输入或者点击随机一个MAC地址，并将该MAC地址克隆到路由器上。
```
### 注意
```
  在使用路由器中继功能时，会发现MAC地址的模式中会出现两个MAC地址，在出厂默认模式下的MAC地址都是路由器的真实MAC地址。  
  
  因为路由器是多WAN口设备，路由器连接以太网的WAN口，用于与互联网提供商（ISP）的设备进行通信，当路由器作为中继器时，路由器连接上级路由器的WAN口，用于与上级路由器设备进行通信。

  在使用路由器中继功能后，再使用MAC地址克隆，路由器的两个WAN口的MAC地址都会变为克隆的地址，如果没有使用中继功能，此时使用MAC地址克隆，只会改变路由器和以太网连接的WAN口的MAC地址，而另一个WAN口因为没有启用，则不会改变。
```
## MAC地址克隆的相关API
### set_mac
```
当进入MAC地址功能界面，选择模式点击应用后，Web界面会调用set_mac，它的功能是配置路由器后台相关的配置文件，下面来看一下它调用时的请求：

1.出厂默认模式
{
    "jsonrpc": "2.0",
    "id": 41,
    "method": "call",
    "params": [
        "kOwMhgyNDFmY9bhJuOabavmiiWEvugps",
        "macclone",
        "set_mac",
        {
            "mode": 0,
            "mac": "94:83:C4:24:6C:88"
        }
    ]
}

2.克隆模式
{
    "jsonrpc": "2.0",
    "id": 56,
    "method": "call",
    "params": [
        "kOwMhgyNDFmY9bhJuOabavmiiWEvugps",
        "macclone",
        "set_mac",
        {
            "mode": 1,
            "mac": "08:26:AE:3D:C9:B6"
        }
    ]
}

3.手动设置模式
{
    "jsonrpc": "2.0",
    "id": 68,
    "method": "call",
    "params": [
        "kOwMhgyNDFmY9bhJuOabavmiiWEvugps",
        "macclone",
        "set_mac",
        {
            "mode": 2,
            "mac": "CE:AB:2E:35:BD:A7"
        }
    ]
}
```
### get_mac
```
get_mac的功能是显示路由器后台文件配置信息，它在set_mac调用成功后调用，下面来看一下它的调用后的响应：

1.出厂默认模式
{
    "id": 155,
    "jsonrpc": "2.0",
    "result": {
        "mac": "94:83:C4:24:6C:88",
        "secondwan_mac": "",
        "repeater_mac": "9E:83:C4:24:6C:8A",
        "mode": 0,
        "remote_mac": "08:26:AE:3D:C9:B6",
        "factory_mac": "94:83:C4:24:6C:88"
    }
}

2.克隆模式
{
    "id": 161,
    "jsonrpc": "2.0",
    "result": {
        "mac": "08:26:AE:3D:C9:B6",
        "secondwan_mac": "",
        "repeater_mac": "08:26:AE:3D:C9:B6",
        "mode": 1,
        "remote_mac": "08:26:AE:3D:C9:B6",
        "factory_mac": "94:83:C4:24:6C:88"
    }
}

3.手动设置模式
{
    "id": 173,
    "jsonrpc": "2.0",
    "result": {
        "mac": "CE:AB:2E:35:BD:A7",
        "secondwan_mac": "",
        "repeater_mac": "CE:AB:2E:35:BD:A7",
        "mode": 2,
        "remote_mac": "08:26:AE:3D:C9:B6",
        "factory_mac": "94:83:C4:24:6C:88"
    }
}
```
### set_wan_mac
```
  set_wan_mac的功能是设置wan口的MAC地址，无论是出厂默认模式、克隆模式还是手动设置模式，在应用时，都需要去设置WAN口的MAC地址以此来达到对应的目的，在出厂默认模式下，需要恢复到真实的MAC地址，在克隆模式下，则需要将WAN口的MAC地址设置为当前客户端的MAC地址，在手动设置模式下，则需要将WAN口的MAC地址设置为手动输入或者随机的MAC地址，set_wan_mac会在set_mac中被调用。
```
### valid_mac
```
  valid_mac的功能是用来检验MAC地址的有效性，判断MAC地址是否有效，它在set_mac和get_mac中被调用，因为set_wan_mac的mac地址是由set_mac传递的，因此它不需要调用valid_mac。
```
## MAC地址克隆的uci配置
```
1.模式配置--cat /etc/config/gl_macclone
  config macclone 'macclone'
      option mode '0'
用于设置MAC地址克隆功能的模式，0表示出厂默认模式，1表示克隆模式，2表示手动设置模式

2.参数配置--cat /etc/config/glconfig（只截不同的部分）
<1>出厂默认模式
  config service 'general'
        option macclone '0'
<2>克隆模式
  config service 'general'
        option macclone_addr '当前客户端的MAC地址'
        option macclone '1'
<3>手动设置模式
  config service 'general'
        option macclone '1'
        option macclone_addr '手动设置的MAC地址'
  当启用MAC地址克隆功能即选择克隆模式和手动设置模式时，glconfig配置文件中的名为'general'的配置服务的macclone选项的值被设置为1，同时增加macclone_addr选项，macclone_addr选项的值在克隆模式下是当前客户端的MAC地址，在手动设置模式下是手动写入的一个MAC地址；
  当关闭MAC地址克隆功能即选择出厂默认模式时，glconfig配置文件中的名为'general'的配置服务下仅有一个macclone选项，此时它的值被设置为0。

3.WAN口参数配置--cat /etc/config/network
<1>出厂默认模式
  config device
        option name 'eth0'
        option macaddr '真实的MAC地址'
<2>克隆模式
  config device
        option name 'eth0'
        option macaddr '当前客户端的MAC地址'
<3>手动设置模式
  config device
        option name 'eth0'
        option macaddr '手动设置的MAC地址'
无论是哪种模式，在应用后都需要去重新设置MAC地址
```
## MAC地址克隆的使用步骤
```
1.网页上输入路由器的IP地址进入路由器的Web界面
2.点击网络
3.点击MAC地址
4.选择需要的模式，如果不想使用MAC地址克隆，选择出厂默认选项即可
```

## MAC地址克隆后的效果（这里开启了中继功能以便于看出实际应用效果）

```
ifconfig
1.出厂默认模式
apclix0   Link encap:Ethernet  HWaddr 9E:83:C4:24:6C:8A  
          inet addr:192.168.8.207  Bcast:192.168.8.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3646 errors:0 dropped:1 overruns:0 frame:0
          TX packets:17047 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:369193 (360.5 KiB)  TX bytes:988264 (965.1 KiB)
          
eth0      Link encap:Ethernet  HWaddr 94:83:C4:24:6C:88  
          inet addr:192.168.18.158  Bcast:192.168.18.255  Mask:255.255.255.0
          inet6 addr: fe80::9683:c4ff:fe24:6c88/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:199856 errors:0 dropped:769 overruns:0 frame:0
          TX packets:37122 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:37915790 (36.1 MiB)  TX bytes:9289294 (8.8 MiB)
          Interrupt:75 
          
2.克隆模式
apclix0   Link encap:Ethernet  HWaddr 08:26:AE:3D:C9:B6  
          inet addr:192.168.8.110  Bcast:192.168.8.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3706 errors:0 dropped:1 overruns:0 frame:0
          TX packets:17235 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:375467 (366.6 KiB)  TX bytes:1001155 (977.6 KiB)
          
eth0      Link encap:Ethernet  HWaddr 08:26:AE:3D:C9:B6  
          inet addr:192.168.18.84  Bcast:192.168.18.255  Mask:255.255.255.0
          inet6 addr: fe80::a26:aeff:fe3d:c9b6/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:201540 errors:0 dropped:773 overruns:0 frame:0
          TX packets:37243 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:38146093 (36.3 MiB)  TX bytes:9307173 (8.8 MiB)
          Interrupt:75 

3.手动设置模式
apclix0   Link encap:Ethernet  HWaddr CE:AB:2E:35:BD:A7  
          inet addr:192.168.8.186  Bcast:192.168.8.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3753 errors:0 dropped:1 overruns:0 frame:0
          TX packets:17345 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:380337 (371.4 KiB)  TX bytes:1009630 (985.9 KiB)

eth0      Link encap:Ethernet  HWaddr CE:AB:2E:35:BD:A7  
          inet addr:192.168.18.26  Bcast:192.168.18.255  Mask:255.255.255.0
          inet6 addr: fe80::ccab:2eff:fe35:bda7/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:201968 errors:0 dropped:775 overruns:0 frame:0
          TX packets:37327 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:38196204 (36.4 MiB)  TX bytes:9319704 (8.8 MiB)
          Interrupt:75 
```

