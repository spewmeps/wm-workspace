# dump\dump_info\SpLogDump 模块日志分析指南

## 1. 模块概述
iBMC核心转储与Web服务日志

## 2. 典型故障分析思路
- **Web访问失败**: 检查 `3rdDump` 下的 `error_log`。
- **系统崩溃**: 分析 `dump_info` 中的调试信息。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | config | 配置导出备份文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | deviceinfo.json | 服务器资产信息 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | diagnose | 硬件诊断日志 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | dmesg.log | 小系统dmesg日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | eSightCfglog.tar.gz | eSightCfg日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | filepatchup_debug.log | 极速部署文件打包日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | images.log | 极速部署克隆日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | images_restore.log | 极速部署还原日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | maintainlog.csv | SP维护日志。带时间戳的maintainlog文件为之前收集的日志。 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | operatelog.csv | SP运行日志。带时间戳的operatinglog文件为之前收集的日志。 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | ping6.log | 网络通信日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | quickdeploy_debug.log | 极速部署日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | sp_upgrade_info.log | SP自升级日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | upgrade | SP固件升级日志 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | version.json | SP版本配置文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | version.json.*.sha | SP版本配置文件的校验文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
