# networkinfo 模块日志分析指南

## 1. 模块概述
网络配置

## 2. 典型故障分析思路
- **连接失败**: 检查 IP/VLAN 配置。
- **丢包**: 分析网络接口统计信息。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | ifconfig_info | 网络信息，执行ifconfig的结果 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | ipinfo_info | iBMC配置的网络信息 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | _data_var_dhcp_dhclient.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6_eth0.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6_eth1.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6_eth2.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient_ip.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | dhclient6_ip.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | resolv.conf | DNS配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | netstat_info | netstat -an | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | route_info | route | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
 | services | 服务端口信息 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 | 
