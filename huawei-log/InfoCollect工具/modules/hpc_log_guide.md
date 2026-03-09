# hpc 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| opainfo_log.txt | opainfo | 网络端口的连接状态 | 收集项说明：网络端口的连接状态 | 辅助排查相关模块的配置或状态异常 |
| opareport-v_log.txt | opareport –v | 节点端口信息 | 收集项说明：节点端口信息 | 辅助排查相关模块的配置或状态异常 |
| opareport-slowlinks_log.txt | opareport -o slowlinks | 端口速率问题 | 收集项说明：端口速率问题 | 辅助排查相关模块的配置或状态异常 |
| opareport-errors_log.txt | opareport -o errors | 网口error统计数 | 收集项说明：网口error统计数 | 辅助排查相关模块的配置或状态异常 |
| opareport-linear_log.txt | opareport -o linear | 路由情况 | 收集项说明：路由情况 | 辅助排查相关模块的配置或状态异常 |
| opasaquery-swinfo_log.txt | opasaquery -o swinfo | 交换机信息 | 收集项说明：交换机信息 | 辅助排查相关模块的配置或状态异常 |
| opahfirev_log.txt | opahfirev | 网卡SN、PCIE号等硬件信息 | 收集项说明：网卡SN、PCIE号等硬件信息 | 辅助排查相关模块的配置或状态异常 |
| opasaquery-nodeinfo_log.txt | opasaquery -o node | 节点详细信息 | 收集项说明：节点详细信息 | 辅助排查相关模块的配置或状态异常 |
| ibstat_log.txt | ibstat | IB/OPA 网络设备的基本状态 | 收集项说明：IB/OPA 网络设备的基本状态 | 辅助排查相关模块的配置或状态异常 |
