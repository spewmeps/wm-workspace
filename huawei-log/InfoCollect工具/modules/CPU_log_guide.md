# CPU 模块日志分析指南

## 1. 模块概述
本模块主要涉及 CPU 使用率、负载及硬件特性的日志分析。

## 2. 典型故障分析思路
- **高负载**: 结合 `top` 输出，区分是用户态 (us) 高还是内核态 (sy) 高。sy 高通常意味着 IO 问题或锁竞争。
- **软锁死**: 在日志中搜索 `soft lockup`，通常意味着 CPU 被内核死循环占用。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| cpuinfo.txt | cat /proc/cpuinfo | CPU型号、频率等信息 | CPU硬件信息，包括型号、频率、缓存大小、指令集支持（如VT-x）。 | 系统负载高、计算任务慢、死机、虚拟化支持检查 |
| cpu_dmidecode.txt | dmidecode -t processor | 执行dmidecode命令获取到的CPU信息 | CPU相关信息：执行dmidecode命令获取到的CPU信息 | 系统负载高、计算任务慢、死机 |
| cpu_usage.txt | top -b -n 60 | CPU占用率 | 记录CPU各核心的使用率分布（User/Sys/Idle/Wait），识别消耗资源的进程。 | 系统负载高、计算任务慢、死机 |
