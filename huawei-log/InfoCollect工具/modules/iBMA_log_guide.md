# iBMA 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| common.log | cat /opt/huawei/ibma/log/common.log | 通用日志 | 收集项说明：通用日志 | 辅助排查相关模块的配置或状态异常 |
| http_server.log | cat /opt/huawei/ibma/log/http_server.log | http服务器日志 | 收集项说明：http服务器日志 | 辅助排查相关模块的配置或状态异常 |
| iBMC_event.log | cat /opt/huawei/ibma/log/iBMC_event.log | iBMC事件日志 | 收集项说明：iBMC事件日志 | 辅助排查相关模块的配置或状态异常 |
| nicErrorStatistics.log | cat /opt/huawei/ibma/log/nicErrorStatistics.log | 网卡故障数据 | 收集项说明：网卡故障数据 | 辅助排查相关模块的配置或状态异常 |
| runlog | cat /opt/huawei/ibma/log/runlog | 操作相关运行日志 | 收集项说明：操作相关运行日志 | 辅助排查相关模块的配置或状态异常 |
| service.log | cat /opt/huawei/ibma/log/service.log | iBMA服务相关日志 | 收集项说明：iBMA服务相关日志 | 辅助排查相关模块的配置或状态异常 |
