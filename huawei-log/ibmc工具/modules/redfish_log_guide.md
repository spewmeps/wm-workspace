# redfish 模块日志分析指南

## 1. 模块概述
通用模块日志

## 2. 典型故障分析思路
- 请根据日志内容进行分析。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | component_uri.json | 部件URI列表 | Redfish RESTful API接口日志，记录外部管理软件的调用请求和响应。 | eSight/ManageOne对接失败、自动化脚本执行报错 | 
 | redfish_dfl.log | Redfish模块管理对象的信息 | Redfish RESTful API接口日志，记录外部管理软件的调用请求和响应。 | eSight/ManageOne对接失败、自动化脚本执行报错 | 
