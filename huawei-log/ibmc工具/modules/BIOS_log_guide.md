# BIOS 模块日志分析指南

## 1. 模块概述
BIOS配置与状态

## 2. 典型故障分析思路
- **启动卡住**: 检查 `BIOS` 目录下的启动日志。
- **配置丢失**: 对比 `CurrentValue` 与预期配置。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | BIOS_dfl.log | BIOS模块管理对象的信息 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
 | bios_info | BIOS配置信息 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
 | currentvalue.json | 当前设置的BIOS项 | 当前生效的BIOS配置项列表。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
 | registry.json | BIOS的注册文件，显示所有的BIOS项信息 | BIOS支持的所有配置项及其属性定义。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
 | result.json | 通过redfish设置的BIOS项结果 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
 | setting.json | 通过redfish设置但尚未生效的BIOS项 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 | 
