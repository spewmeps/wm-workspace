# iBMC 模块日志分析指南

## 1. 模块概述
本模块主要涉及服务器带外管理、硬件传感器及底层事件的日志分析。

## 2. 典型故障分析思路
- **SEL日志**: 系统事件日志 (System Event Log) 是判断硬件故障最直接的依据，关注 `Critical` 级别事件。
- **传感器**: 检查温度传感器读数是否接近告警阈值。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| fru.txt | ipmitool fru list | FRU部件信息 | 现场可替换单元(FRU)信息，记录主板、电源、背板的序列号和资产信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高、资产盘点 |
| bmc_lan.txt | ipmitool lan print 1 | iBMC网卡信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| mc_info.txt | ipmitool mc info | iBMC版本信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| sensor.csv | ipmitool sensor list | 所有传感器信息 | 传感器实时读数，包括温度、电压、风扇转速等。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| sdr.csv | ipmitool sdr elist | SDR日志 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| sel.csv | ipmitool sel elist | iBMC SEL日志 | 系统事件日志(SEL)，记录BIOS/BMC检测到的硬件底层错误（如电压异常、温度告警）。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| version_cpld.txt | ipmitool raw0x30 0x90 0x08 0 2 0x00 0x10 | CPLD版本信息 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
| time.txt | ipmitool sel time get<br>date<br>hwclock | iBMC、BIOS、OS时间 | 带外管理卡(BMC)相关信息。 | 硬件告警（黄灯/红灯）、风扇狂转、无法开机、温度过高 |
