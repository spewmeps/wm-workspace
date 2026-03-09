# cyclicCollection 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| memory_free.txt | free | 查看耗用和剩余的内存 | 详细的内存使用统计，包括Total、Free、Buffers、Cached、Swap及大页内存状态。 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| ifconfig.txt | ifconfig -a | 所有网卡配置 | 收集项说明：所有网卡配置 | 辅助排查相关模块的配置或状态异常 |
| ethtool.txt | ethtool ethX | 网卡的设置 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| ethtool.txt | ethtool -i ethX | 网卡的设置 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| ethtool.txt | ethtool -S ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| ethtool.txt | ethtool -g ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| ethtool.txt | ethtool -k ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| netstat.txt | netstat -i -n | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| netstat.txt | netstat -avn | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| netstat.txt | netstat -s | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| netstat.txt | netstat -rvn | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| stat.txt | cat /proc/stat | 系统状态 | 收集项说明：系统状态 | 辅助排查相关模块的配置或状态异常 |
| routeInfo.txt | route -n | 路由信息 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| iptablesInfo.txt | iptables -t filter -nvL | iptables信息 | 收集项说明：iptables信息 | 辅助排查相关模块的配置或状态异常 |
| trafficControl.txt | tc -s qdisc | 流量控制排队规则 | 收集项说明：流量控制排队规则 | 辅助排查相关模块的配置或状态异常 |
| command_log.txt | sysctl -a | 内核信息 | 收集项说明：内核信息 | 辅助排查相关模块的配置或状态异常 |
| network_adapter_numa_node.txt | cat /sys/class/net/${netname}/device/numa_node | 网卡numa节点信息 | 网络子系统信息：网卡numa节点信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| network_scripts.txt | ls /etc/sysconfig/network-scripts/<br>cat /etc/sysconfig/network-scripts/${netname} | 网络脚本信息 | 网络子系统信息：网络脚本信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| interrupts.txt | cat /proc/interrupts | 系统中断信息 | 收集项说明：系统中断信息 | 辅助排查相关模块的配置或状态异常 |
