# LicenseMgnt 模块日志分析指南

## 1. 模块概述
通用模块日志

## 2. 典型故障分析思路
- 请根据日志内容进行分析。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | alm_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 | 
 | first_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 | 
 | LicenseMgnt_dfl.log | 管理对象信息 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 | 
 | lm_info | License的状态、设备ESN等信息 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 | 
 | second_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 | 
