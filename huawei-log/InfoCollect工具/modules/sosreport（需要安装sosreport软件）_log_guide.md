# sosreport（需要安装sosreport软件） 模块日志分析指南

## 1. 模块概述
本模块包含特定组件的日志信息，请结合具体文件名进行分析。

## 2. 典型故障分析思路
- **通用建议**: 根据文件名和描述判断日志用途，结合故障现象进行关联分析。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| sosreport-$(hostname)_$(time).tar.xz | sosreport --batch 或者<br>sosreport -n networking --batch<br> | RHEL、CentOS和Ubuntu系统定位用日志收集套件。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 收集项说明：RHEL、CentOS和Ubuntu系统定位用日志收集套件。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 辅助排查相关模块的配置或状态异常 |
| sosreport-$(hostname)_$(time).tar.xz.md5 | sosreport --batch 或者<br>sosreport -n networking --batch | RHEL、CentOS和Ubuntu系统定位用日志收集套件的校验码。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 收集项说明：RHEL、CentOS和Ubuntu系统定位用日志收集套件的校验码。（如果是RHEL 6且安装的sosreport版本低于3.3或者RHEL 7且安装的sosreport版本低于3.6，请使用参数-n networking） | 辅助排查相关模块的配置或状态异常 |
