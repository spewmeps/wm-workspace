# 硬盘 模块日志分析指南

## 1. 模块概述
本模块主要涉及物理磁盘、RAID控制器及相关存储子系统的日志分析。

## 2. 典型故障分析思路
- **S.M.A.R.T分析**: 重点关注 `Reallocated_Sector_Ct` (05) 和 `Current_Pending_Sector` (C5)，非零值通常意味着物理损坏。
- **IO性能**: 使用 `iostat` 分析 `%util` (利用率) 和 `await` (响应时间)。若 `%util` 接近 100% 且 `await` 高，说明磁盘可能是瓶颈。
- **RAID状态**: 检查 RAID 卡日志确认是否有磁盘离线 (Offline)、降级 (Degraded) 或正在重构 (Rebuild)。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| disk_smart.txt | smartctl -a /dev/sd*<br>smartctl -a --device=sat+megaraid,0 /dev/sd*<br>smartctl -a --device=megaraid,0 /dev/sd*<br>hwdiag -t disk -s<br>nvme -smart-log /dev/nvmeXX<br>nvme intel smart-log-add /dev/nvmeXX<br>nvmecli -smart-log /dev/nvmeXX | 硬盘的SMART信息（需要安装smartctl工具，NVMe盘需安装nvme、nvmecli工具） | 记录硬盘的S.M.A.R.T健康数据，包括重映射扇区、通电时间、温度、读写错误率等关键寿命指标。 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载、预测硬盘寿命、慢盘分析 |
| phy_info.txt | ls -l /dev/disk/by-path | 硬盘phy信息 | 存储子系统相关信息：硬盘phy信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| sys_block.txt | ls -l /sys/block/sd* | 硬盘名称 | 存储子系统相关信息：硬盘名称 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| parted_disk.txt | parted /dev/sda print | 物理硬盘的分区信息 | 显示磁盘分区表结构，确认分区类型、大小和对齐情况。 | 分区丢失、无法挂载、扩容失败 |
| hwdiag_hdd.txt | hwdiag -t disk -d | 通过iBMA硬盘的打分数据（适用于系统已经安装iBMA） | 存储子系统相关信息：通过iBMA硬盘的打分数据（适用于系统已经安装iBMA） | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| es3000_v2.txt | hio_info -d --device=hioa,/dev/hio*<br>hio_label -d --device=hioa,/dev/hio*<br>hio_log -d --device=hioa,/dev/hio*<br>hio_temperature -d --device=hioa,/dev/hio* | ES3000 V2 PCIe SSD信息 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
| es3000.txt | modinfo nvme<br>hioadm info -d device=nvme0/pd0/sda,hioadm info<br>hioadm info -d device=nvme/pd/sd,hioadm info -s<br>hioadm info -d device=nvme/pd/sd,hioadm info -a<br>hioadm log -d device=nvme0/pd0/sda,hioadm info -a | ES3000 V3/V5/V6 SAS SSD和NVME PCIE SSD日志信息 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
| udisk.txt | ./udisk -v | 查询uDisk版本 | 存储子系统相关信息：查询uDisk版本 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| udisk.txt | ./udisk -l all | 查询硬盘的基本信息 | 存储子系统相关信息：查询硬盘的基本信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| udisk.txt | ./udisk -s all | 查询硬盘的健康状态 | 存储子系统相关信息：查询硬盘的健康状态 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| udisk.txt | ./udisk -m all | 查询硬盘的事件 | 存储子系统相关信息：查询硬盘的事件 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| udisk.txt | ./udisk -d all | 查询硬盘的smart信息 | 存储子系统相关信息：查询硬盘的smart信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| udisk.txt | ./udisk -p al | 查询硬盘的phy信息 | 存储子系统相关信息：查询硬盘的phy信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| scheduler.txt | cat /sys/block/*/queue/scheduler | 操作系统的硬盘I/O调度器信息 | 存储子系统相关信息：操作系统的硬盘I/O调度器信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| blktrace/${disk}/blktrace_log.txt | blktrace -d /dev/${disk} -w 30 -D logPath<br>blkparse -i ${disk} -D logPath -d logPath/blktrace.bin<br>cd logPath &amp;&amp; btt -i blktrace.bin -l d2c_latency -q q2c_latency -B offset &gt; blktrace.txt | 磁盘blktrace信息 | 存储子系统相关信息：磁盘blktrace信息 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| disk_other_info.txt | sg_logs -a /dev/sd*<br>sg_modes -a -a /dev/sd*<br>lspci -vvvxxx -s $BDF | 硬盘其他信息收集 | 存储子系统相关信息：硬盘其他信息收集 | 硬盘读写慢、I/O报错、RAID掉盘、系统卡顿、存储无法挂载 |
| /opt/hio/operationLogs/operations.log | cp /opt/hio/operationLogs/operations.log | hioadm工具运行日志 | PCIe SSD (ES3000/NVMe) 专用日志，包含寿命、温度、错误计数和固件版本。 | NVMe盘掉盘、性能下降、寿命预警 |
