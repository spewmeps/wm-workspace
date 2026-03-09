# RAID 模块日志分析指南

## 1. 模块概述
本模块主要涉及RAID控制卡配置、磁盘阵列状态及电池备份单元(BBU)的日志分析。

## 2. 典型故障分析思路
- **VD状态**: 检查Virtual Drive状态是否为Optimal，Degraded表示有盘故障。
- **PD状态**: 检查Physical Drive状态，Failed或Unconfigured Bad表示物理盘故障。
- **BBU**: 检查电池状态，学习周期(Learn Cycle)期间写策略可能变为WriteThrough导致性能下降。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| sashbalog.txt | sas3ircu_arm组合命令集 | LSI SAS3008 RAID卡日志（arm） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| sashbalog.txt | sas3ircu sas3flash hbacli组合命令集 | LSI SAS3008 RAID卡日志（x86） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| sashbalog.txt | sas2ircu sas2flash hbacli组合命令集 | LSI SAS2308 RAID卡日志（x86） | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| sasraidlog.txt | storcli组合命令集 | LSI SAS2208、LSI SAS3108、Avago SAS3408、Avago SAS3508或Avago SAS3516 RAID卡日志 | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
| diskmap.txt | lsscsi<br>opt/MegaRAID/storcli/storcli c* show | LSI SAS2208、LSI SAS3108、Avago SAS3408或Avago SAS3508 RAID卡的逻辑磁盘与物理盘的对应关系 | 列出SCSI设备列表及其属性，用于确认系统是否识别到物理磁盘或逻辑卷。 | 磁盘丢失、新盘无法识别 |
| /md/mdinfo.txt | - | 软RAID md信息 | RAID卡控制器日志，记录RAID级别、磁盘状态（Online/Failed）、电池状态及重构进度。 | RAID降级、磁盘亮黄灯、RAID卡电池故障 |
