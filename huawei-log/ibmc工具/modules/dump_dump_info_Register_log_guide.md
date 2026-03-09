# dump\dump_info\Register 模块日志分析指南

## 1. 模块概述
iBMC核心转储与Web服务日志

## 2. 典型故障分析思路
- **Web访问失败**: 检查 `3rdDump` 下的 `error_log`。
- **系统崩溃**: 分析 `dump_info` 中的调试信息。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | cpld_reg_info | CPLD寄存器信息 | dump\dump_info\Register 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
 | cpu_reg_info | CPU寄存器信息 | dump\dump_info\Register 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 | 
