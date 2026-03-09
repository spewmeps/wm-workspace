# dump\dump_info\LogDump 模块日志分析指南

## 1. 模块概述
iBMC核心转储与Web服务日志

## 2. 典型故障分析思路
- **Web访问失败**: 检查 `3rdDump` 下的 `error_log`。
- **系统崩溃**: 分析 `dump_info` 中的调试信息。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | bmccom.dat | BMC串口日志 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 | 
 | fdm_pfae_log | FDM预告警日志 | dump\dump_info\LogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | PD_SMART_INFO_C* | 硬盘的SMART日志，*为RAID控制器的编号 | dump\dump_info\LogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | third_party_file_bak.log | 第三方文件备份日志记录 | dump\dump_info\LogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
