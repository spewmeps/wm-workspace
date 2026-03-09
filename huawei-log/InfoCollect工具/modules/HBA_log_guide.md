# HBA 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| lspci_fc.txt | lspci | grep Fibre | FC型号 | 收集项说明：grep Fibre | 辅助排查相关模块的配置或状态异常 |
| firmware_fc.txt | cat /sys/class/scsi_host/host*/fwrev<br>cat /sys/class/scsi_host/host*/fw_version | FC HBA卡的固件信息 | 收集项说明：FC HBA卡的固件信息 | 辅助排查相关模块的配置或状态异常 |
| driver_fc.txt | cat /sys/class/scsi_host/host*/lpfc_drvr_version<br>cat /sys/class/scsi_host/host*/driver_version | FC HBA卡的驱动信息 | 收集项说明：FC HBA卡的驱动信息 | 辅助排查相关模块的配置或状态异常 |
| wwpn.txt | cat /sys/class/fc_host/host*/port_name<br>cat /sys/class/fc_host/host*/node_name | FC HBA卡的WWPN和WWNN信息 | 收集项说明：FC HBA卡的WWPN和WWNN信息 | 辅助排查相关模块的配置或状态异常 |
| bdf.txt | ls –l /sys/class/fc_host/<br>ls –l /sys/class/iscsi | Iscsi和FC HBA卡的BDF信息 | 收集项说明：Iscsi和FC HBA卡的BDF信息 | 辅助排查相关模块的配置或状态异常 |
| error_frame_crc.txt | cat /sys/class/fc_host/host*/statistics/error_frames<br>cat<br>/sys/class/fc_host/host*/statistics/invalid_crc_count | FC HBA卡的端口误码和错帧信息 | 收集项说明：FC HBA卡的端口误码和错帧信息 | 辅助排查相关模块的配置或状态异常 |
| port_state.txt | cat /sys/class/fc_host/host*/port_state | FC HBA卡的端口状态信息 | 收集项说明：FC HBA卡的端口状态信息 | 辅助排查相关模块的配置或状态异常 |
| 1822_fc_info.tar.gz | ./collect_log_prd.sh | 收集1822FC HBA卡日志信息 | 收集项说明：收集1822FC HBA卡日志信息 | 辅助排查相关模块的配置或状态异常 |
