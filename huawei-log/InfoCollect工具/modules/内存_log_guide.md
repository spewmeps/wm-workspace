# 内存 模块日志分析指南

## 1. 模块概述
本模块主要涉及物理内存状态、虚拟内存交换及硬件错误的日志分析。

## 2. 典型故障分析思路
- **ECC错误**: 检查 `edac` 或 `mcelog` 确认是否有内存纠错记录 (CE/UE)。
- **Swap使用**: 频繁的 Swap 换入换出 (si/so) 会严重拖慢系统性能。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| meminfo | /proc/meminfo | 内存信息 | 详细的内存使用统计，包括Total、Free、Buffers、Cached、Swap及大页内存状态。 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| mem_ecc.txt | mcelog --client | 内存ECC信息（默认为空） | 内存硬件错误记录，如ECC纠错计数，用于发现故障内存条。 | 系统蓝屏(PSOD)、随机重启、硬件告警 |
| memory_dmidecode.txt | dmidecode -t memory | 物理内存信息 | 内存相关信息：物理内存信息 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| hwdiag_mem.txt | hwdiag -t mem -d | 通过iBMA收集内存的诊断数据（适用于系统已经安装iBMA） | 内存相关信息：通过iBMA收集内存的诊断数据（适用于系统已经安装iBMA） | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| swapon_info.txt | swapon --bytes --show | 显示交换分区设备的信息 | 内存相关信息：显示交换分区设备的信息 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
