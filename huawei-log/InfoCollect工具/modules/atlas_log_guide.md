# atlas 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| dlog/ | cp /var/dlog | Atlas NPU日志 | 收集项说明：Atlas NPU日志 | 辅助排查相关模块的配置或状态异常 |
| /tmp/atlas300_smartkit.log | cp /tmp/atlas300_smartkit.log | Atlas300 OS巡检脚本执行日志。 | 收集项说明：Atlas300 OS巡检脚本执行日志。 | 辅助排查相关模块的配置或状态异常 |
| ascend-dmi.txt | ascend-dmi -v<br>ascend-dmi -i -dt<br>ascend-dmi -topo<br>ascend-dmi -c | Atlas NPU相关信息 | 收集项说明：Atlas NPU相关信息 | 辅助排查相关模块的配置或状态异常 |
| ascend-log-collect.tar.gz | ascend-dmi -c | Ascend日志收集 | 收集项说明：Ascend日志收集 | 辅助排查相关模块的配置或状态异常 |
