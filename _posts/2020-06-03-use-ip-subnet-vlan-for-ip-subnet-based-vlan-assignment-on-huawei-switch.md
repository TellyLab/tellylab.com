---
title: 在华为交换机上使用 ip-subnet-vlan 实现基于子网的 VLAN 划分
author: 陈天傲
---

如无特别声明，本文中的「端口」、「接口」皆指以太网交换机的「interface」。

## 背景

### 关于 VLAN 划分方式

最常见的 VLAN 划分方式是基于端口划分，与之对应的最常见的 IP 子网 - VLAN 间的关系是一一对应——也就是一个子网对应一个 VLAN。

有时希望在一个 VLAN（或傻瓜交换机的 LAN）中使用多个 IP 子网，并实现正常的子网间路由（包括 VLAN 内和 VLAN 间），可以在 VLAN 虚接口（三层接口）上配置 sub 地址（华为等）或 secondary 地址（Cisco 等）。

但如果新的网络规划使用最典型的一个子网一个 VLAN，并且子网网关地址已经在对应的 VLAN 虚接口上配置完毕，此时便无法兼容上一自然段所说的做法，因为同一三层设备无法在两个不同的三层接口上配置属于同一子网的地址。要在新架构上兼容历史遗留的一 VLAN 多子网问题，就要把源自不同子网主机的流量划分（发送）到各自对应的新 VLAN。

### 需求

现在新建的网络使用子网 - VLAN 一一对应的模式，并在有条件的接入端口上使用基于接口的 VLAN 划分方式，但有部分边缘端口必须用于接入属于多个不同子网的主机（比如通过下挂傻瓜交换机）。

## 解决方案

使用基于子网的 VLAN 划分方式即可将源自不同子网主机的上行流量送入对应 VLAN，并将来自受允许的 VLAN 的流量根据 MAC 表 untag 发送到对应的边缘端口，实现对主机和傻瓜交换机透明的（也就是不带 tag 的）一端口多 VLAN。

在华为交换机上，可以使用 ip-subnet-vlan 命令实现基于子网的 VLAN 划分。

### 配置思路

首先在 VLAN 视图下为 VLAN 关联 IP 子网。最简命令参考如 `ip-subnet-vlan ip 192.168.10.0 24`，具体命令自己 `ip-subnet-vlan ?` 吧。

然后在以太网接口视图下将用于下联至终端主机的边缘端口配置为 hybrid 类型，并将所有需要放通的 VLAN 配置为 untagged。根据本案的需求，不需要配置 tagged vlan，因为该端口之下再没有能够处理 tag 的设备了。

最后在接口视图下执行命令 `ip-subnet-vlan enable`，为接口使能（启用）基于 IP 子网划分 VLAN 的功能。

此外，可以为 hybrid 接口配置 pvid，这样该端口下联的主机甚至可以使用 DHCP 客户端向缺省 VLAN 请求 DHCP 配置。这样就可以实现办公 PC 自动获取缺省 VLAN 对应子网的地址，打印机继续使用原先手动配置的其他子网的地址。

### 配置参考

```
#
vlan 10
 ip-subnet-vlan 1 ip 192.168.10.0 255.255.255.0
vlan 20
 ip-subnet-vlan 1 ip 192.168.20.0 255.255.255.0
#
interface GigabitEthernet0/0/1
 port link-type hybrid
 port hybrid pvid vlan 10
 port hybrid untagged vlan 10 20
 ip-subnet-vlan enable
#
```

{% include ad.html %}
