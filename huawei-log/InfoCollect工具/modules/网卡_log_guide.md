# 网卡 模块日志分析指南

## 1. 模块概述
本模块主要涉及网络接口、驱动、协议栈及物理链路状态的日志分析。

## 2. 典型故障分析思路
- **丢包分析**: 检查 `ifconfig`/`ip -s link` 输出中的 `dropped` 和 `errors` 计数。
- **链路状态**: 使用 `ethtool` 确认物理链路是否协商在预期的速率 (Speed) 和双工模式 (Duplex)。
- **驱动报错**: 在 `dmesg` 或 `/var/log/messages` 中搜索 `eth` 或驱动名，查找 `reset` 或 `hang` 关键字。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| lspci_eth.txt | lspci | grep Ethernet | 网卡清单 | 网络子系统信息：grep Ethernet | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| /etc/modprobe.d | cp /etc/modprobe.d/ | 网卡的配置文件 | 网络子系统信息：网卡的配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| /etc/sysconfig/network/* | cp /etc/sysconfig/network/* | SLES系统下的网卡配置文件 | 网络子系统信息：SLES系统下的网卡配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| /etc/sysconfig/network-scripts/* | cp /etc/sysconfig/network-scripts/* | RHEL、CentOS和Euler系统下的网卡配置文件 | 网络子系统信息：RHEL、CentOS和Euler系统下的网卡配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /bin/date | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /usr/bin/uptime | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | dmidecode | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | biosdecode | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | lspci -tv | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ifconfig -a | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 接口配置信息，包括IP地址、MAC地址、MTU和接口错误统计(errors/dropped)。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /sbin/ip link show | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 接口配置信息，包括IP地址、MAC地址、MTU和接口错误统计(errors/dropped)。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /sbin/route -n | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| mezz_info.tar.gz | ethtool -i ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| mezz_info.tar.gz | ethtool ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ethtool -a ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ethtool -l ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ethtool -d ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ethtool -e ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | ethtool -S ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| mezz_info.tar.gz | ethtool -g ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| mezz_info.tar.gz | ethtool -k ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| mezz_info.tar.gz | /sbin/lspci -xxx -s XX:XX.X | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | lspci -vvvv -xxx -s XX:XX.X | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /bin/netstat -nlp | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| mezz_info.tar.gz | /bin/netstat -nr | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| mezz_info.tar.gz | /bin/netstat -i | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| mezz_info.tar.gz | /sbin/arp -v | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | /usr/sbin/iptables -t | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | filter -nvL | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| mezz_info.tar.gz | x86tool -r pci/bXX:XX.X-0xXXXXX+4-bX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| bonding.txt | cp /proc/net/bonding/ | 网卡绑定信息 | Bonding绑定状态，显示主备网卡状态、协商速率和切换记录。 | 链路冗余失效、Bonding切换导致丢包 |
| bdf.txt | ls –l /sys/class/net | 网卡BDF信息 | 网络子系统信息：网卡BDF信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| error_frame_crc.txt | cat /sys/class/net/*/statistics/rx_crc_errors<br>cat /sys/class/net/*/statistics/rx_frame_errors | 网卡错帧和端口误码信息 | 网络子系统信息：网卡错帧和端口误码信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 1822_nic_info.tar.gz | ./hinic_log_collect.sh -a | 收集1822网卡日志信息 | 网络子系统信息：收集1822网卡日志信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| routeInfo.txt | route -n | 网卡路由信息 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| iptablesInfo.txt | iptables -t filter -nvL | 防火墙信息 | 网络子系统信息：防火墙信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| trafficControl.txt | tc -s qdisc | 显示／维护流量控制设置 | 网络子系统信息：显示／维护流量控制设置 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| network_adapter_numa_node.txt | cat /sys/class/net/*/device/numa_node | 网卡信息 | 网络子系统信息：网卡信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | ll /sys/class/net | 网卡PCI接口信息 | 网络子系统信息：网卡PCI接口信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/device | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/vendor | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | cat/sys/class/net/ethX/device/subsystem_vendor | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/subsystem_device | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| nic/nic_pci_info.txt | /var/log/boot.msg | 网卡驱动日志 | 网络子系统信息：网卡驱动日志 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/device | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_vendor | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_device | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/vendor | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/symbolic_name | CNA卡firmware版本 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| cna/cna_fcoe_info.txt | ethtool -i ethY | CNA卡firmware版本 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/port_id | CNA卡端口FC_ID信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/port_type | CNA卡端口类型信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/speed | CNA卡端口速率信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/fabric_name | CNA卡端口连接的Switch的WWN信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| cna/cna_fcoe_info.txt | iostat -mx [interval] [count] | 磁盘/LUN性能统计信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/vendor | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/device | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_vendor | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_device | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | rpm -qa | grep lpfc (Emulex)<br>modinfo lpfc | grep version (Emulex)<br>modinfo qla2xxx | grep version (QLogic) | HBA卡驱动版本 | 网络子系统信息：grep lpfc (Emulex)<br>modinfo lpfc | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/port_id | HBA卡端口FC_ID信息 | 网络子系统信息：HBA卡端口FC_ID信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/port_type | HBA卡端口类型信息 | 网络子系统信息：HBA卡端口类型信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/speed | HBA卡端口速率信息 | 网络子系统信息：HBA卡端口速率信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/fabric_name | HBA卡端口连接的Switch的WWN信息 | 网络子系统信息：HBA卡端口连接的Switch的WWN信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| hba/hba_pci_info.txt | iostat -mx [interval] [count] | 磁盘/LUN性能统计信息 | 网络子系统信息：磁盘/LUN性能统计信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
