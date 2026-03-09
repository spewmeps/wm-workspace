| 模块 | 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- | --- |
| 公共 | version.txt | - | 工具版本和发布时间 | 收集项说明：工具版本和发布时间 | 辅助排查相关模块的配置或状态异常 |
| 公共 | filelist.txt | - | 收集到的文件清单 | 收集项说明：收集到的文件清单 | 辅助排查相关模块的配置或状态异常 |
| BIOS | version_bios.txt | dmidecode -t bios | BIOS版本信息 | BIOS版本和配置设置，影响硬件初始化和引导选项。 | 硬件兼容性问题、启动引导问题、性能调优 |
| BigData | selfdiag_log.txt | /opt/DA200/compress_tools/tools/dcadm selfdiag | DA200 selfdiag信息（需要安装DA200工具） | 大数据组件特定日志，可能包含HDFS、Yarn等服务的运行状态。 | 大数据作业失败、集群状态异常 |
| BigData | Lspci_log.txt | lspci | grep d501 | 型号信息 | 大数据组件特定日志，可能包含HDFS、Yarn等服务的运行状态。 | 大数据作业失败、集群状态异常 |
| BigData | /var/log/DA200/ | cp /var/log/DA200/ | DA200卡日志信息 | 大数据组件特定日志，可能包含HDFS、Yarn等服务的运行状态。 | 大数据作业失败、集群状态异常 |
| BigData | /var/log/bigdata/ | cp /var/log/bigdata/ | DA200卡日志信息 | 大数据组件特定日志，可能包含HDFS、Yarn等服务的运行状态。 | 大数据作业失败、集群状态异常 |
| iBMC | fru.txt | ipmitool fru list | FRU部件信息 | 现场可替换单元(FRU)信息，记录主板、电源、背板的序列号和资产信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高、资产盘点 |
| iBMC | bmc_lan.txt | ipmitool lan print 1 | iBMC网卡信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | mc_info.txt | ipmitool mc info | iBMC版本信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | sensor.csv | ipmitool sensor list | 所有传感器信息 | 传感器实时读数，包括温度、电压、风扇转速等。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | sdr.csv | ipmitool sdr elist | SDR日志 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | sel.csv | ipmitool sel elist | iBMC SEL日志 | 系统事件日志(SEL)，记录BIOS/BMC检测到的硬件底层错误（如电压异常、温度告警）。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | version_cpld.txt | ipmitool raw0x30 0x90 0x08 0 2 0x00 0x10 | CPLD版本信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMC | time.txt | ipmitool sel time get<br>date<br>hwclock | iBMC、BIOS、OS时间 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| iBMA | common.log | cat /opt/huawei/ibma/log/common.log | 通用日志 | 收集项说明：通用日志 | 辅助排查相关模块的配置或状态异常 |
| iBMA | http_server.log | cat /opt/huawei/ibma/log/http_server.log | http服务器日志 | 收集项说明：http服务器日志 | 辅助排查相关模块的配置或状态异常 |
| iBMA | iBMC_event.log | cat /opt/huawei/ibma/log/iBMC_event.log | iBMC事件日志 | 收集项说明：iBMC事件日志 | 辅助排查相关模块的配置或状态异常 |
| iBMA | nicErrorStatistics.log | cat /opt/huawei/ibma/log/nicErrorStatistics.log | 网卡故障数据 | 收集项说明：网卡故障数据 | 辅助排查相关模块的配置或状态异常 |
| iBMA | runlog | cat /opt/huawei/ibma/log/runlog | 操作相关运行日志 | 收集项说明：操作相关运行日志 | 辅助排查相关模块的配置或状态异常 |
| iBMA | service.log | cat /opt/huawei/ibma/log/service.log | iBMA服务相关日志 | 收集项说明：iBMA服务相关日志 | 辅助排查相关模块的配置或状态异常 |
| CPU | cpuinfo.txt | cat /proc/cpuinfo | CPU型号、频率等信息 | CPU硬件信息，包括型号、频率、缓存大小、指令集支持（如VT-x）。 | 系统负载高、计算任务慢、死机、虚拟化支持检查 |
| CPU | cpu_dmidecode.txt | dmidecode -t processor | 执行dmidecode命令获取到的CPU信息 | CPU相关信息：执行dmidecode命令获取到的CPU信息 | 系统负载高、计算任务慢、死机 |
| CPU | cpu_usage.txt | top -b -n 60 | CPU占用率 | 记录CPU各核心的使用率分布（User/Sys/Idle/Wait），识别消耗资源的进程。 | 系统负载高、计算任务慢、死机 |
| 硬盘 | disk_smart.txt | smartctl -a /dev/sd*<br>smartctl -a --device=sat+megaraid,0 /dev/sd*<br>smartctl -a --device=megaraid,0 /dev/sd*<br>hwdiag -t disk -s<br>nvme -smart-log /dev/nvmeXX<br>nvme intel smart-log-add /dev/nvmeXX<br>nvmecli -smart-log /dev/nvmeXX | 硬盘的SMART信息（需要安装smartctl工具，NVMe盘需安装nvme、nvmecli工具） | 记录硬盘的S.M.A.R.T健康数据，包括重映射扇区、通电时间、温度、读写错误率等关键寿命指标。 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载、预测硬盘寿命、慢盘分析 |
| 硬盘 | phy_info.txt | ls -l /dev/disk/by-path | 硬盘phy信息 | 存储子系统相关信息：硬盘phy信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | sys_block.txt | ls -l /sys/block/sd* | 硬盘名称 | 存储子系统相关信息：硬盘名称 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | parted_disk.txt | parted /dev/sda print | 物理硬盘的分区信息 | 显示磁盘分区表结构，确认分区类型、大小和对齐情况。 | 分区丢失、无法挂载、扩容失败 |
| 硬盘 | hwdiag_hdd.txt | hwdiag -t disk -d | 通过iBMA硬盘的打分数据（适用于系统已经安装iBMA） | 存储子系统相关信息：通过iBMA硬盘的打分数据（适用于系统已经安装iBMA） | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | es3000_v2.txt | hio_info -d --device=hioa,/dev/hio*<br>hio_label -d --device=hioa,/dev/hio*<br>hio_log -d --device=hioa,/dev/hio*<br>hio_temperature -d --device=hioa,/dev/hio* | ES3000 V2 PCIe SSD信息 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
| 硬盘 | es3000.txt | modinfo nvme<br>hioadm info -d device=nvme0/pd0/sda,hioadm info<br>hioadm info -d device=nvme/pd/sd,hioadm info -s<br>hioadm info -d device=nvme/pd/sd,hioadm info -a<br>hioadm log -d device=nvme0/pd0/sda,hioadm info -a | ES3000 V3/V5/V6 SAS SSD和NVME PCIE SSD日志信息 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
| 硬盘 | udisk.txt | ./udisk -v | 查询uDisk版本 | 存储子系统相关信息：查询uDisk版本 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | udisk.txt | ./udisk -l all | 查询硬盘的基本信息 | 存储子系统相关信息：查询硬盘的基本信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | udisk.txt | ./udisk -s all | 查询硬盘的健康状态 | 存储子系统相关信息：查询硬盘的健康状态 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | udisk.txt | ./udisk -m all | 查询硬盘的事件 | 存储子系统相关信息：查询硬盘的事件 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | udisk.txt | ./udisk -d all | 查询硬盘的smart信息 | 存储子系统相关信息：查询硬盘的smart信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | udisk.txt | ./udisk -p al | 查询硬盘的phy信息 | 存储子系统相关信息：查询硬盘的phy信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | scheduler.txt | cat /sys/block/*/queue/scheduler | 操作系统的硬盘I/O调度器信息 | 存储子系统相关信息：操作系统的硬盘I/O调度器信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | blktrace/${disk}/blktrace_log.txt | blktrace -d /dev/${disk} -w 30 -D logPath<br>blkparse -i ${disk} -D logPath -d logPath/blktrace.bin<br>cd logPath &amp;&amp; btt -i blktrace.bin -l d2c_latency -q q2c_latency -B offset &gt; blktrace.txt | 磁盘blktrace信息 | 存储子系统相关信息：磁盘blktrace信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | disk_other_info.txt | sg_logs -a /dev/sd*<br>sg_modes -a -a /dev/sd*<br>lspci -vvvxxx -s $BDF | 硬盘其他信息收集 | 存储子系统相关信息：硬盘其他信息收集 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| 硬盘 | /opt/hio/operationLogs/operations.log | cp /opt/hio/operationLogs/operations.log | hioadm工具运行日志 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
| 主板 | dmidecode.txt | dmidecode | 硬件信息 | 收集项说明：硬件信息 | 辅助排查相关模块的配置或状态异常 |
| 主板 | lspci.txt | lspci | PCI设备列表 | 收集项说明：PCI设备列表 | 辅助排查相关模块的配置或状态异常 |
| 主板 | lspci.txt | lspci -xxx -vvv -t | 详细PCI设备信息 | 收集项说明：详细PCI设备信息 | 辅助排查相关模块的配置或状态异常 |
| 主板 | lspci.txt | lspci -xxxx -vvv -b | 以总线为中心查看PCI和IRQ设备信息 | 收集项说明：以总线为中心查看PCI和IRQ设备信息 | 辅助排查相关模块的配置或状态异常 |
| 网卡 | lspci_eth.txt | lspci | grep Ethernet | 网卡清单 | 网络子系统信息：grep Ethernet | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | /etc/modprobe.d | cp /etc/modprobe.d/ | 网卡的配置文件 | 网络子系统信息：网卡的配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | /etc/sysconfig/network/* | cp /etc/sysconfig/network/* | SLES系统下的网卡配置文件 | 网络子系统信息：SLES系统下的网卡配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | /etc/sysconfig/network-scripts/* | cp /etc/sysconfig/network-scripts/* | RHEL、CentOS和Euler系统下的网卡配置文件 | 网络子系统信息：RHEL、CentOS和Euler系统下的网卡配置文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /bin/date | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /usr/bin/uptime | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | dmidecode | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | biosdecode | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | lspci -tv | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ifconfig -a | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 接口配置信息，包括IP地址、MAC地址、MTU和接口错误统计(errors/dropped)。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /sbin/ip link show | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 接口配置信息，包括IP地址、MAC地址、MTU和接口错误统计(errors/dropped)。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /sbin/route -n | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| 网卡 | mezz_info.tar.gz | ethtool -i ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| 网卡 | mezz_info.tar.gz | ethtool ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ethtool -a ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ethtool -l ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ethtool -d ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ethtool -e ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | ethtool -S ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| 网卡 | mezz_info.tar.gz | ethtool -g ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| 网卡 | mezz_info.tar.gz | ethtool -k ethX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| 网卡 | mezz_info.tar.gz | /sbin/lspci -xxx -s XX:XX.X | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | lspci -vvvv -xxx -s XX:XX.X | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /bin/netstat -nlp | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| 网卡 | mezz_info.tar.gz | /bin/netstat -nr | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| 网卡 | mezz_info.tar.gz | /bin/netstat -i | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| 网卡 | mezz_info.tar.gz | /sbin/arp -v | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | /usr/sbin/iptables -t | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | filter -nvL | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | mezz_info.tar.gz | x86tool -r pci/bXX:XX.X-0xXXXXX+4-bX | collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络子系统信息：collect_info/network.txt：网卡信息<br>mezz.log：收集脚本自身运行日志文件 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | bonding.txt | cp /proc/net/bonding/ | 网卡绑定信息 | Bonding绑定状态，显示主备网卡状态、协商速率和切换记录。 | 链路冗余失效、Bonding切换导致丢包 |
| 网卡 | bdf.txt | ls –l /sys/class/net | 网卡BDF信息 | 网络子系统信息：网卡BDF信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | error_frame_crc.txt | cat /sys/class/net/*/statistics/rx_crc_errors<br>cat /sys/class/net/*/statistics/rx_frame_errors | 网卡错帧和端口误码信息 | 网络子系统信息：网卡错帧和端口误码信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | 1822_nic_info.tar.gz | ./hinic_log_collect.sh -a | 收集1822网卡日志信息 | 网络子系统信息：收集1822网卡日志信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | routeInfo.txt | route -n | 网卡路由信息 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| 网卡 | iptablesInfo.txt | iptables -t filter -nvL | 防火墙信息 | 网络子系统信息：防火墙信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | trafficControl.txt | tc -s qdisc | 显示／维护流量控制设置 | 网络子系统信息：显示／维护流量控制设置 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | network_adapter_numa_node.txt | cat /sys/class/net/*/device/numa_node | 网卡信息 | 网络子系统信息：网卡信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | ll /sys/class/net | 网卡PCI接口信息 | 网络子系统信息：网卡PCI接口信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/device | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/vendor | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | cat/sys/class/net/ethX/device/subsystem_vendor | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | cat /sys/class/net/ethX/device/subsystem_device | 网卡PCI信息 | 网络子系统信息：网卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | nic/nic_pci_info.txt | /var/log/boot.msg | 网卡驱动日志 | 网络子系统信息：网卡驱动日志 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/device | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_vendor | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_device | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | cna/cna_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/vendor | CNA卡PCI信息 | 网络子系统信息：CNA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/symbolic_name | CNA卡firmware版本 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | cna/cna_fcoe_info.txt | ethtool -i ethY | CNA卡firmware版本 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| 网卡 | cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/port_id | CNA卡端口FC_ID信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/port_type | CNA卡端口类型信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/speed | CNA卡端口速率信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | cna/cna_fcoe_info.txt | cat /sys/class/fc_host/hostX/fabric_name | CNA卡端口连接的Switch的WWN信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | cna/cna_fcoe_info.txt | iostat -mx [interval] [count] | 磁盘/LUN性能统计信息 | FC HBA卡（光纤网卡）信息，包括WWN号、链路状态、误码率。 | SAN存储连接中断、多路径切换、FC链路不稳定 |
| 网卡 | hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/vendor | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/device | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_vendor | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/devices/pci0000:xx/[root port]/[bus/device/function]/subsystem_device | HBA卡PCI信息 | 网络子系统信息：HBA卡PCI信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | rpm -qa | grep lpfc (Emulex)<br>modinfo lpfc | grep version (Emulex)<br>modinfo qla2xxx | grep version (QLogic) | HBA卡驱动版本 | 网络子系统信息：grep lpfc (Emulex)<br>modinfo lpfc | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/port_id | HBA卡端口FC_ID信息 | 网络子系统信息：HBA卡端口FC_ID信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/port_type | HBA卡端口类型信息 | 网络子系统信息：HBA卡端口类型信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/speed | HBA卡端口速率信息 | 网络子系统信息：HBA卡端口速率信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | cat /sys/class/fc_host/hostX/fabric_name | HBA卡端口连接的Switch的WWN信息 | 网络子系统信息：HBA卡端口连接的Switch的WWN信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| 网卡 | hba/hba_pci_info.txt | iostat -mx [interval] [count] | 磁盘/LUN性能统计信息 | 网络子系统信息：磁盘/LUN性能统计信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| HBA | lspci_fc.txt | lspci | grep Fibre | FC型号 | 收集项说明：grep Fibre | 辅助排查相关模块的配置或状态异常 |
| HBA | firmware_fc.txt | cat /sys/class/scsi_host/host*/fwrev<br>cat /sys/class/scsi_host/host*/fw_version | FC HBA卡的固件信息 | 收集项说明：FC HBA卡的固件信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | driver_fc.txt | cat /sys/class/scsi_host/host*/lpfc_drvr_version<br>cat /sys/class/scsi_host/host*/driver_version | FC HBA卡的驱动信息 | 收集项说明：FC HBA卡的驱动信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | wwpn.txt | cat /sys/class/fc_host/host*/port_name<br>cat /sys/class/fc_host/host*/node_name | FC HBA卡的WWPN和WWNN信息 | 收集项说明：FC HBA卡的WWPN和WWNN信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | bdf.txt | ls –l /sys/class/fc_host/<br>ls –l /sys/class/iscsi | Iscsi和FC HBA卡的BDF信息 | 收集项说明：Iscsi和FC HBA卡的BDF信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | error_frame_crc.txt | cat /sys/class/fc_host/host*/statistics/error_frames<br>cat<br>/sys/class/fc_host/host*/statistics/invalid_crc_count | FC HBA卡的端口误码和错帧信息 | 收集项说明：FC HBA卡的端口误码和错帧信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | port_state.txt | cat /sys/class/fc_host/host*/port_state | FC HBA卡的端口状态信息 | 收集项说明：FC HBA卡的端口状态信息 | 辅助排查相关模块的配置或状态异常 |
| HBA | 1822_fc_info.tar.gz | ./collect_log_prd.sh | 收集1822FC HBA卡日志信息 | 收集项说明：收集1822FC HBA卡日志信息 | 辅助排查相关模块的配置或状态异常 |
| 驱动 | lsmod.txt | lsmod | 加载的模块信息 | 内核模块加载列表及详细信息，确认驱动是否正确加载及版本。 | 硬件无法使用、功能缺失、驱动冲突 |
| 驱动 | modinfo.txt | modinfo模块名称 | 各模块的驱动信息 | 内核模块加载列表及详细信息，确认驱动是否正确加载及版本。 | 硬件无法使用、功能缺失、驱动冲突 |
| 内存 | meminfo | /proc/meminfo | 内存信息 | 详细的内存使用统计，包括Total、Free、Buffers、Cached、Swap及大页内存状态。 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| 内存 | mem_ecc.txt | mcelog --client | 内存ECC信息（默认为空） | 内存硬件错误记录，如ECC纠错计数，用于发现故障内存条。 | 系统蓝屏(PSOD)、随机重启、硬件告警 |
| 内存 | memory_dmidecode.txt | dmidecode -t memory | 物理内存信息 | 内存相关信息：物理内存信息 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| 内存 | hwdiag_mem.txt | hwdiag -t mem -d | 通过iBMA收集内存的诊断数据（适用于系统已经安装iBMA） | 内存相关信息：通过iBMA收集内存的诊断数据（适用于系统已经安装iBMA） | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| 内存 | swapon_info.txt | swapon --bytes --show | 显示交换分区设备的信息 | 内存相关信息：显示交换分区设备的信息 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| RAID | sashbalog.txt | sas3ircu_arm组合命令集 | LSI SAS3008 RAID卡日志（arm） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| RAID | sashbalog.txt | sas3ircu sas3flash hbacli组合命令集 | LSI SAS3008 RAID卡日志（x86） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| RAID | sashbalog.txt | sas2ircu sas2flash hbacli组合命令集 | LSI SAS2308 RAID卡日志（x86） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| RAID | sasraidlog.txt | storcli组合命令集 | LSI SAS2208、LSI SAS3108、Avago SAS3408、Avago SAS3508或Avago SAS3516 RAID卡日志 | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| RAID | diskmap.txt | lsscsi<br>opt/MegaRAID/storcli/storcli c* show | LSI SAS2208、LSI SAS3108、Avago SAS3408或Avago SAS3508 RAID卡的逻辑磁盘与物理盘的对应关系 | 列出SCSI设备列表及其属性，用于确认系统是否识别到物理磁盘或逻辑卷。 | 磁盘丢失、新盘无法识别 |
| RAID | /md/mdinfo.txt | - | 软RAID md信息 | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| 系统 | runlog | 工具运行后台记录 | 工具本身的运行日志信息 | 系统基础信息：工具本身的运行日志信息 | 系统级故障排查 |
| 系统 | /var/log/secure | cp /var/log/secure* | 安全日志 | 安全认证日志，记录SSH登录、sudo提权、用户创建等安全事件。 | 登录失败、被黑客攻击、权限审计 |
| 系统 | /var/log/mcelog | cp /var/log/mcelog* | 定位MCE错误需要的日志信息 | 系统基础信息：定位MCE错误需要的日志信息 | 系统级故障排查 |
| 系统 | /var/log/message | cp /var/log/message* | 常规系统日志文件 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| 系统 | /var/log/cron | cp /var/log/cron* | 定制任务日志文件 | 定时任务调度日志，记录crontab任务的执行时间和状态。 | 定时任务未执行或执行失败 |
| 系统 | /var/log/boot.log | cp /var/log/boot.log | 本次启动日志，log格式 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /var/log/boot.msg | cp /var/log/boot.msg | 本次启动日志，msg格式 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /var/log/boot.omsg | cp /var/log/boot.omsg | 启动日志（本次启动的前一次过程日志） | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /etc/syslog.conf | cp /etc/syslog.conf | syslogd进程的配置文件 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| 系统 | /etc/resolv.conf | cp /etc/resolv.conf | 网络代理的配置文件 | 系统基础信息：网络代理的配置文件 | 系统级故障排查 |
| 系统 | /etc/hosts | cp /etc/hosts | 主机信息 | 系统基础信息：主机信息 | 系统级故障排查 |
| 系统 | /etc/services | cp /etc/services | 启动服务 | 系统基础信息：启动服务 | 系统级故障排查 |
| 系统 | /etc/sysctl.conf | cp/etc/sysctl.conf | 系统内核参数配置文件，涉及网络功能，例如转发等 | 内核参数配置，控制网络、内存、文件系统等核心行为。 | 性能调优、参数配置错误导致的问题 |
| 系统 | /etc/inittab | cp /etc/inittab | 初始化，涉及系统配置 | 系统基础信息：初始化，涉及系统配置 | 系统级故障排查 |
| 系统 | /etc/ntp.conf | cp /etc/ntp.conf | NTP配置文件 | 系统基础信息：NTP配置文件 | 系统级故障排查 |
| 系统 | /etc/ntp/step-tickers | cp /etc/ntp/step-tickers | NTP配置文件 | 系统基础信息：NTP配置文件 | 系统级故障排查 |
| 系统 | /etc/ntp/ntpservers | cp /etc/ntp/ntpservers | NTP服务器列表 | 系统基础信息：NTP服务器列表 | 系统级故障排查 |
| 系统 | /etc/yp.conf | cp /etc/yp.conf | ypbind配置文件 | 系统基础信息：ypbind配置文件 | 系统级故障排查 |
| 系统 | /etc/fstab | cp /etc/fstab | 文件系统列表 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| 系统 | /proc/interrupts | cp /proc/interrupts | 中断数量 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/filesystems | cp /proc/filesystems | 当前文件系统列表 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /etc/exports | cp /etc/exports | NFS服务器配置 | 系统基础信息：NFS服务器配置 | 系统级故障排查 |
| 系统 | /proc/self/maps | cp /proc/self/maps | 查看进程的虚拟地址空间 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/self/smaps | cp /proc/self/smaps | 单个进程的内存查看 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/self/numa_maps | cp /proc/self/numa_maps | numa进程的内存查看 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/iomap | cp /proc/iomap | I/O映射内存空间 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/ioports | cp /proc/ioports | 显示当时正在使用的I/O端口资源分布 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/swaps | cp /proc/swaps | 交换分区信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/slabinfo | cp /proc/slabinfo | 内核信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/locks | cp /proc/locks | 查看是否有进程锁定 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/modules | cp /proc/modules | 显示各个模块 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/mounts | cp /proc/mounts | 显示当前挂载的设备 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| 系统 | /proc/version | cp /proc/version | 查看内核版本 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/stat | cp /proc/stat | 内核和系统的统计信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/schedstat | cp /proc/schedstat | 内核schedstat信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/zoneinfo | cp /proc/zoneinfo | 内核时区信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/config.gz | cp /proc/config.gz | proc文件系统配置文件集 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/kallsyms | cp /proc/kallsyms | 内核调试文件 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/mtrr | cp /proc/mtrr | 内核内存范围寄存器 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/vmstat | cp /proc/vmstat | 虚拟内存状态 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/buddyinfo | cp /proc/buddyinfo | 内存碎片信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/cmdline | cp /proc/cmdline | 内核启动的命令行 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/devices | cp /proc/devices | 设备驱动程序 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/diskstats | cp /proc/diskstats | 硬盘状态 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /proc/iomem | cp /proc/iomem | 物理地址的分配情况 | 内存相关信息：物理地址的分配情况 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| 系统 | /proc/dma | cp /proc/dma | DMA通道信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| 系统 | /boot/config* | cp /boot/config* | 内核配置文件 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /etc/grub.conf | cp /etc/grub.conf | grub配置文件 | 系统基础信息：grub配置文件 | 系统级故障排查 |
| 系统 | /boot/grub/device.map | cp /boot/grub/device.map | 启动设备映射 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /boot/grub/menu.lst | cp /boot/menu.lst | 启动菜单 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | /boot/grub2/grub.cfg | cp /boot/grub2/grub.cfg | Grub2信息 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| 系统 | dump_list.txt | du -ah /var/crash/ | crash文件清单 | 系统基础信息：crash文件清单 | 系统级故障排查 |
| 系统 | dmesg.txt | dmesg | dmesg文件 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| 系统 | /root/.bash_history | cp /root/.bash_history | 历史执行命令 | 系统基础信息：历史执行命令 | 系统级故障排查 |
| 系统 | command_log.txt | df -liT | 查看剩余空间 | 系统基础信息：查看剩余空间 | 系统级故障排查 |
| 系统 | command_log.txt | df -Th | 文件系统挂载信息 | 系统基础信息：文件系统挂载信息 | 系统级故障排查 |
| 系统 | command_log.txt | mount | 挂载信息 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| 系统 | command_log.txt | /usr/bin/last -xF | grep "reboot\ | shutdown\ | runlevel\ | system" | 重启纪录 | 系统基础信息：grep "reboot\ | 系统级故障排查 |
| 系统 | command_log.txt | nfsstat -cnrs | NFS服务状态 | 系统基础信息：NFS服务状态 | 系统级故障排查 |
| 系统 | command_log.txt | lsof | 已打开文件列表 | 系统基础信息：已打开文件列表 | 系统级故障排查 |
| 系统 | command_log.txt | ulimit -a | 查看系统资源限制 | 系统基础信息：查看系统资源限制 | 系统级故障排查 |
| 系统 | command_log.txt | ipcs -a | 显示进程间通信设施状态 | 系统基础信息：显示进程间通信设施状态 | 系统级故障排查 |
| 系统 | command_log.txt | ipcs -l | 查看共享信息的内存 | 系统基础信息：查看共享信息的内存 | 系统级故障排查 |
| 系统 | command_log.txt | sysctl -a | sysctl设置和显示在“/proc/sys”目录中的内核参数 | 系统基础信息：sysctl设置和显示在“/proc/sys”目录中的内核参数 | 系统级故障排查 |
| 系统 | command_log.txt | uptime | 已经运行时间 | 系统基础信息：已经运行时间 | 系统级故障排查 |
| 系统 | command_log.txt | uname -a | 查看内核 | 系统基础信息：查看内核 | 系统级故障排查 |
| 系统 | command_log.txt | ps auwx | 当前所有进程列表 | 系统基础信息：当前所有进程列表 | 系统级故障排查 |
| 系统 | command_log.txt | last | 历史登录过的用户纪录 | 系统基础信息：历史登录过的用户纪录 | 系统级故障排查 |
| 系统 | command_log.txt | hostid | 登录用户的ID号 | 系统基础信息：登录用户的ID号 | 系统级故障排查 |
| 系统 | command_log.txt | /bin/rpm -qa | 已经安装软件清单 | 系统基础信息：已经安装软件清单 | 系统级故障排查 |
| 系统 | command_log.txt | printenv | 打印环境变量 | 系统基础信息：打印环境变量 | 系统级故障排查 |
| 系统 | command_log.txt | hostname | 主机名信息 | 系统基础信息：主机名信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/dirty_ratio | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/dirty_background_ratio | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/dirty_writeback_centisecs | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/dirty_expire_centisecs | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/vfs_cache_pressure | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /proc/sys/vm/swappiness | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat blockdev --getra /dev/sdx | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/block/sdx/queue/scheduler | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/nr_requests | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_segments | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_sectors_kb | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_hw_sectors_kb | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/hw_sector_size | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | filesystem_config_log.txt | cat /sys/class/block/sdx/device/queue_depth | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| 系统 | devinfo.txt | ls –l /dev | /dev文件夹下的详细信息 | 系统基础信息：/dev文件夹下的详细信息 | 系统级故障排查 |
| 系统 | /var/log/kern* | cp /var/log/kern* | Kern日志信息 | 系统基础信息：Kern日志信息 | 系统级故障排查 |
| 系统 | /var/log/dmesg* | cp /var/log/dmesg* | Dmesg日志信息 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| 系统 | /var/log/syslog* | cp /var/log/syslog* | Syslog日志信息 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| 系统 | /var/log/{hostname}/{date}/ | cp –r/var/log/{hostname}/{date}/ | FusionSphere CNA 6.1系统日志备份 | 系统基础信息：FusionSphere CNA 6.1系统日志备份 | 系统级故障排查 |
| 系统 | /var/backuplog/galaxenginelog/{date}/ | cp -r /var/backuplog/galaxenginelog/{date}/ | FusionSphere CNA6.3~6.5 系统日志备份 | 系统基础信息：FusionSphere CNA6.3~6.5 系统日志备份 | 系统级故障排查 |
| 系统 | /var/log/logdump/ | cp -r /var/log/logdump | FusionSphere openstack系统日志备份 | 系统基础信息：FusionSphere openstack系统日志备份 | 系统级故障排查 |
| 系统 | /var/log/logdump-{num}-{timestamp}.tar.gz | cp -r /var/log/logdump-{num}-{timestamp}.tar.gz | FusionSphere openstack 系统日志备份 | 系统基础信息：FusionSphere openstack 系统日志备份 | 系统级故障排查 |
| 系统 | /var/crash/*/vmcore-dmesg.txt | - | 系统错误信息 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| 系统 | dpkglist.txt | dpkg -l | Ubuntu系统安装的软件包信息 | 系统基础信息：Ubuntu系统安装的软件包信息 | 系统级故障排查 |
| 系统 | vmstat.txt | vmstat 1 60 | 进程，虚拟内存和CPU活动信息 | 系统基础信息：进程，虚拟内存和CPU活动信息 | 系统级故障排查 |
| 系统 | iostat.txt | iostat -xct 1 60 | 磁盘和CPU活动统计信息 | 系统基础信息：磁盘和CPU活动统计信息 | 系统级故障排查 |
| 系统 | command_log.txt | ps -eo pid,lstart,cmd | 等待资源的进程数 | 系统基础信息：等待资源的进程数 | 系统级故障排查 |
| SLES | nts_$(hostname)_$(time).tbz | supportconfig（SLES系统） | SLES系统定位用日志收集套件 | 收集项说明：SLES系统定位用日志收集套件 | 辅助排查相关模块的配置或状态异常 |
| SLES | nts_$(hostname)_$(time).tbz.md5 | supportconfig（SLES系统） | SLES系统定位用日志收集套件的校验码 | 收集项说明：SLES系统定位用日志收集套件的校验码 | 辅助排查相关模块的配置或状态异常 |
| sosreport（需要安装sosreport软件） | sosreport-$(hostname)_$(time).tar.xz | sosreport --batch 或者<br>sosreport -n networking --batch<br> | RHEL、CentOS和Ubuntu系统定位用日志收集套件。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 收集项说明：RHEL、CentOS和Ubuntu系统定位用日志收集套件。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 辅助排查相关模块的配置或状态异常 |
| sosreport（需要安装sosreport软件） | sosreport-$(hostname)_$(time).tar.xz.md5 | sosreport --batch 或者<br>sosreport -n networking --batch | RHEL、CentOS和Ubuntu系统定位用日志收集套件的校验码。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 收集项说明：RHEL、CentOS和Ubuntu系统定位用日志收集套件的校验码。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 辅助排查相关模块的配置或状态异常 |
| hpc | opainfo_log.txt | opainfo | 网络端口的连接状态 | 收集项说明：网络端口的连接状态 | 辅助排查相关模块的配置或状态异常 |
| hpc | opareport-v_log.txt | opareport –v | 节点端口信息 | 收集项说明：节点端口信息 | 辅助排查相关模块的配置或状态异常 |
| hpc | opareport-slowlinks_log.txt | opareport -o slowlinks | 端口速率问题 | 收集项说明：端口速率问题 | 辅助排查相关模块的配置或状态异常 |
| hpc | opareport-errors_log.txt | opareport -o errors | 网口error统计数 | 收集项说明：网口error统计数 | 辅助排查相关模块的配置或状态异常 |
| hpc | opareport-linear_log.txt | opareport -o linear | 路由情况 | 收集项说明：路由情况 | 辅助排查相关模块的配置或状态异常 |
| hpc | opasaquery-swinfo_log.txt | opasaquery -o swinfo | 交换机信息 | 收集项说明：交换机信息 | 辅助排查相关模块的配置或状态异常 |
| hpc | opahfirev_log.txt | opahfirev | 网卡SN、PCIE号等硬件信息 | 收集项说明：网卡SN、PCIE号等硬件信息 | 辅助排查相关模块的配置或状态异常 |
| hpc | opasaquery-nodeinfo_log.txt | opasaquery -o node | 节点详细信息 | 收集项说明：节点详细信息 | 辅助排查相关模块的配置或状态异常 |
| hpc | ibstat_log.txt | ibstat | IB/OPA 网络设备的基本状态 | 收集项说明：IB/OPA 网络设备的基本状态 | 辅助排查相关模块的配置或状态异常 |
| atlas | dlog/ | cp /var/dlog | Atlas NPU日志 | 收集项说明：Atlas NPU日志 | 辅助排查相关模块的配置或状态异常 |
| atlas | /tmp/atlas300_smartkit.log | cp /tmp/atlas300_smartkit.log | Atlas300 OS巡检脚本执行日志。 | 收集项说明：Atlas300 OS巡检脚本执行日志。 | 辅助排查相关模块的配置或状态异常 |
| atlas | ascend-dmi.txt | ascend-dmi -v<br>ascend-dmi -i -dt<br>ascend-dmi -topo<br>ascend-dmi -c | Atlas NPU相关信息 | 收集项说明：Atlas NPU相关信息 | 辅助排查相关模块的配置或状态异常 |
| atlas | ascend-log-collect.tar.gz | ascend-dmi -c | Ascend日志收集 | 收集项说明：Ascend日志收集 | 辅助排查相关模块的配置或状态异常 |
| hiarm | hiarm_log_xxx.tar.gz | hiarm_log_collect.sh -a -p -l /tmp -z {log_dir} | Hi1620芯片日志及其它相关日志 | 收集项说明：Hi1620芯片日志及其它相关日志 | 辅助排查相关模块的配置或状态异常 |
| block | blk_list.txt | lsblk | 列出块设备 | 存储子系统相关信息：列出块设备 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| block | blk_list.txt | lsblk -t | 列出块设备的拓扑 | 存储子系统相关信息：列出块设备的拓扑 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| block | blk_list.txt | fdisk -l | 列出所有块设备的分区信息(Legacy) | 显示磁盘分区表结构，确认分区类型、大小和对齐情况。 | 分区丢失、无法挂载、扩容失败 |
| block | initramfs.txt | lsinitrd | 显示initramfs镜像的内容 | 存储子系统相关信息：显示initramfs镜像的内容 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| block | file_directory.txt | ls -lanR /boot | 列出/boot目录以及子目录的文件 | 存储子系统相关信息：列出/boot目录以及子目录的文件 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| cgroups | /proc/cgroups | /proc/cgroups | 显示当前内核支持的cgroup子系统以及是否使能 | 收集项说明：显示当前内核支持的cgroup子系统以及是否使能 | 辅助排查相关模块的配置或状态异常 |
| cgroups | /sys/fs/cgroup | /sys/fs/cgroup | 收集所有cgroup文件系统的文件 | 收集项说明：收集所有cgroup文件系统的文件 | 辅助排查相关模块的配置或状态异常 |
| cgroups | cgroup.txt | systemd-cgls --no-pager | 递归显示 cgroup 的内容 | 收集项说明：递归显示 cgroup 的内容 | 辅助排查相关模块的配置或状态异常 |
| cron | scheduled_task.txt | crontab -l -u &lt;user&gt; | 列出user用户设定的定时任务 | 收集项说明：列出user用户设定的定时任务 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/cron.monthly | /etc/cron.monthly | 每月运行一次的定时任务配置目录 | 收集项说明：每月运行一次的定时任务配置目录 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/cron.weekly | /etc/cron.weekly | 每周运行一次的定时任务配置目录 | 收集项说明：每周运行一次的定时任务配置目录 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/cron.daily | /etc/cron.daily | 每天运行一次的定时任务配置目录 | 收集项说明：每天运行一次的定时任务配置目录 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/cron.hourly | /etc/cron.hourly | 每小时运行一次的定时任务配置目录 | 收集项说明：每小时运行一次的定时任务配置目录 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/cron.d | /etc/cron.d | 定时任务配置目录 | 收集项说明：定时任务配置目录 | 辅助排查相关模块的配置或状态异常 |
| cron | /etc/crontab | /etc/crontab | 系统级定时任务，使用root权限执行 | 收集项说明：系统级定时任务，使用root权限执行 | 辅助排查相关模块的配置或状态异常 |
| grub2 | grub2.txt | grub2-mkconfig | 导出当前的grub配置文件 | 收集项说明：导出当前的grub配置文件 | 辅助排查相关模块的配置或状态异常 |
| hardware | udevadm.txt | udevadm info -e | 导出udev数据库的内容 | 收集项说明：导出udev数据库的内容 | 辅助排查相关模块的配置或状态异常 |
| hardware | /proc/irq | /proc/irq | 每个中断的单独的信息 | 收集项说明：每个中断的单独的信息 | 辅助排查相关模块的配置或状态异常 |
| kernel | kpatch_list.txt | kpatch list | 列出所有已安装的内核热补丁模块 | 收集项说明：列出所有已安装的内核热补丁模块 | 辅助排查相关模块的配置或状态异常 |
| last | OS_restart_info.txt | last reboot | 最近一次OS重启的记录 | 收集项说明：最近一次OS重启的记录 | 辅助排查相关模块的配置或状态异常 |
| last | OS_shutdown_info.txt | last shutdown | 最近一次OS关机的记录 | 收集项说明：最近一次OS关机的记录 | 辅助排查相关模块的配置或状态异常 |
| sar | /var/log/sa | /var/log/sa | sa日志，用来分析性能问题 | 收集项说明：sa日志，用来分析性能问题 | 辅助排查相关模块的配置或状态异常 |
| scsi | scsi_info.txt | lsscsi | 列出scsi设备以及其属性 | 列出SCSI设备列表及其属性，用于确认系统是否识别到物理磁盘或逻辑卷。 | 磁盘丢失、新盘无法识别 |
| scsi | scsi_sg_mapping.txt | sg_map -x | 列出sg设备和scsi设备的映射关系 | 列出SCSI设备列表及其属性，用于确认系统是否识别到物理磁盘或逻辑卷。 | 磁盘丢失、新盘无法识别 |
| cyclicCollection | memory_free.txt | free | 查看耗用和剩余的内存 | 详细的内存使用统计，包括Total、Free、Buffers、Cached、Swap及大页内存状态。 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| cyclicCollection | ifconfig.txt | ifconfig -a | 所有网卡配置 | 收集项说明：所有网卡配置 | 辅助排查相关模块的配置或状态异常 |
| cyclicCollection | ethtool.txt | ethtool ethX | 网卡的设置 | 查看网卡物理参数：速率、双工模式、链路状态、Wake-on-LAN设置等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cyclicCollection | ethtool.txt | ethtool -i ethX | 网卡的设置 | 显示网卡驱动版本、固件版本和总线信息。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、驱动兼容性排查 |
| cyclicCollection | ethtool.txt | ethtool -S ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| cyclicCollection | ethtool.txt | ethtool -g ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| cyclicCollection | ethtool.txt | ethtool -k ethX | 网卡的设置 | 网卡底层统计计数(Ring Buffer/Offload)，包括收发包数、CRC错误、FIFO溢出、丢包计数等。 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down、定位物理链路质量问题、性能调优 |
| cyclicCollection | netstat.txt | netstat -i -n | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| cyclicCollection | netstat.txt | netstat -avn | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| cyclicCollection | netstat.txt | netstat -s | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| cyclicCollection | netstat.txt | netstat -rvn | 网络端口状态 | 网络连接状态表，显示TCP/UDP连接、监听端口及路由表。 | 服务无法访问、端口冲突、连接数过多（DDoS） |
| cyclicCollection | stat.txt | cat /proc/stat | 系统状态 | 收集项说明：系统状态 | 辅助排查相关模块的配置或状态异常 |
| cyclicCollection | routeInfo.txt | route -n | 路由信息 | 内核路由表，决定数据包的发送路径。 | 网络不可达、跨网段通信失败 |
| cyclicCollection | iptablesInfo.txt | iptables -t filter -nvL | iptables信息 | 收集项说明：iptables信息 | 辅助排查相关模块的配置或状态异常 |
| cyclicCollection | trafficControl.txt | tc -s qdisc | 流量控制排队规则 | 收集项说明：流量控制排队规则 | 辅助排查相关模块的配置或状态异常 |
| cyclicCollection | command_log.txt | sysctl -a | 内核信息 | 收集项说明：内核信息 | 辅助排查相关模块的配置或状态异常 |
| cyclicCollection | network_adapter_numa_node.txt | cat /sys/class/net/${netname}/device/numa_node | 网卡numa节点信息 | 网络子系统信息：网卡numa节点信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cyclicCollection | network_scripts.txt | ls /etc/sysconfig/network-scripts/<br>cat /etc/sysconfig/network-scripts/${netname} | 网络脚本信息 | 网络子系统信息：网络脚本信息 | 网络丢包、延迟高、连接中断、吞吐量低、网卡down |
| cyclicCollection | interrupts.txt | cat /proc/interrupts | 系统中断信息 | 收集项说明：系统中断信息 | 辅助排查相关模块的配置或状态异常 |
| gpu | nvidia-smi.txt | nvidia-smi -q | nvidia smi信息 | GPU状态、驱动加载情况、显存使用率和XID错误。 | GPU掉卡、AI训练中断、显示异常 |
| gpu | nvidia-installer.log | cp -rf /var/log/nvidia-installer.log | nvidia安装程序日志 | GPU状态、驱动加载情况、显存使用率和XID错误。 | GPU掉卡、AI训练中断、显示异常 |
| gpu | nvidia-uninstall.log | cp -rf /var/log/nvidia-uninstall.log | nvidia卸载日志 | GPU状态、驱动加载情况、显存使用率和XID错误。 | GPU掉卡、AI训练中断、显示异常 |
| gpu | Xorg.*.log | cp -rf /var/log/Xorg.*.log | Xorg日志 | GPU状态、驱动加载情况、显存使用率和XID错误。 | GPU掉卡、AI训练中断、显示异常 |
| trustZone | /var/log/tee/* | /var/log/tee/* | TrustZone日志 | 收集项说明：TrustZone日志 | 辅助排查相关模块的配置或状态异常 |
