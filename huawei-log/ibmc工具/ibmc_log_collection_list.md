# iBMC 一键收集信息说明

| 分类/路径 | 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| dump\dump_info | dump_app_log | iBMC收集结果列表 | dump\dump_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info | dump_log | 一键收集结果列表 | dump\dump_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\3rdDump | access_log | Nginx访问日志 | 记录所有HTTP/HTTPS请求的来源IP、请求URL和响应状态码。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | access_log.1 | Nginx访问日志备份文件 | 记录所有HTTP/HTTPS请求的来源IP、请求URL和响应状态码。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | error_log | Nginx错误日志 | 记录Nginx进程启动错误、模块加载失败及请求处理异常。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | error_log.1 | Nginx错误日志备份文件 | 记录Nginx进程启动错误、模块加载失败及请求处理异常。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx.conf | Nginx基础配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_http_server.conf | Nginx http server配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_https_server.conf | Nginx https server配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_https_server_ext.conf | Nginx扩展配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_https_defualt_server_ext.conf | Nginx https defualt server扩展配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_ssl_ciphersuite.conf | Nginx https协议加密套件配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_ssl_protocol.conf | Nginx https协议版本配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| dump\dump_info\3rdDump | nginx_ssl_verify_client.conf | Nginx双因素认证配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 |
| agentless | agentless_dfl.log | Agentless模块管理对象的信息 | agentless 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| BIOS | BIOS_dfl.log | BIOS模块管理对象的信息 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BIOS | bios_info | BIOS配置信息 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BIOS | currentvalue.json | 当前设置的BIOS项 | 当前生效的BIOS配置项列表。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BIOS | registry.json | BIOS的注册文件，显示所有的BIOS项信息 | BIOS支持的所有配置项及其属性定义。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BIOS | result.json | 通过redfish设置的BIOS项结果 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BIOS | setting.json | 通过redfish设置但尚未生效的BIOS项 | 记录BIOS的配置参数、启动过程日志及与iBMC的交互信息。 | 系统自检(POST)卡住、BIOS设置不生效、启动顺序错误 |
| BMC | BMC_dfl.log | iBMC模块管理对象的信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | fruinfo.txt | FRU电子标签信息 | 现场可替换单元(FRU)信息，包含主板、背板等硬件的序列号和制造信息。 | iBMC重启、管理口无法ping通、IPMI命令无响应、资产盘点失败 |
| BMC | lldp_info.txt | LLDP配置及报文统计信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | mcinfo.txt | iBMC的辅助固件信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | nandflash_info.txt | NAND flash信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | net_info.txt | 网口配置信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | ntp_info.txt | NTP同步失败时的错误信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | psu_info.txt | 服务器上配置的电源信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| BMC | time_zone.txt | iBMC时区信息 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| card_manage | card_info | 服务器上配置的扣卡信息 | card_manage 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| card_manage | card_manage_dfl.log | Card_Manage模块管理对象的信息 | card_manage 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| card_manage | sdi_card_cpld_info | SDI V3卡的CPLD寄存器信息 | card_manage 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| cooling_app | cooling_app_dfl.log | Cooling模块管理对象的信息 | cooling_app 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| cooling_app | fan_info.txt | 风扇型号、转速等详细信息 | cooling_app 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| CpuMem | cpu_info | 服务器配置的CPU参数的详细信息 | CpuMem 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| CpuMem | CpuMem_dfl.log | CpuMem模块管理对象的信息 | CpuMem 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| CpuMem | mem_info | 服务器配置的内存参数的详细信息 | CpuMem 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| ddns | ddns_dfl.log | Ddns模块管理对象的信息 | ddns 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dfm | dfm.log | DFM模块管理对象的信息 | dfm 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| Dft | Dft_dfl.log | DFT模块管理对象的信息 | Dft 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| diagnose | diagnose_dfl.log | Diagnose模块管理对象的信息 | diagnose 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| diagnose | diagnose_info | Port 80的故障诊断信息以及IFMM模块内存占用信息 | diagnose 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| discovery | discovery_dfl.log | Discovery模块管理对象的信息 | discovery 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| FileManage | FileManage_dfl.log | FileManage模块管理对象的信息 | FileManage 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| ipmi_app | ipmbeth_info.txt | 管理系统的IPMI通道状态 | ipmi_app 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| ipmi_app | ipmi_app_dfl.log | IPMI模块管理对象的信息 | ipmi_app 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| kvm_vmm | kvm_vmm_dfl.log | KVM_VMM模块管理对象的信息 | 远程虚拟控制台(KVM)及虚拟媒体(Virtual Media)日志。 | 远程桌面无法连接、虚拟光驱挂载失败、KVM花屏 |
| Lcd | Lcd_dfl.log | LCD模块管理对象的信息 | Lcd 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| LicenseMgnt | alm_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| LicenseMgnt | first_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| LicenseMgnt | LicenseMgnt_dfl.log | 管理对象信息 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| LicenseMgnt | lm_info | License的状态、设备ESN等信息 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| LicenseMgnt | second_protected.persist | License管理组件ALM的持久化文件 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| MaintDebug | MaintDebug_dfl.log | MaintDebug模块管理对象的信息 | MaintDebug 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| net_nat | net_nat_dfl.log | Net_NAT模块管理对象的信息 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| PcieSwitch | PcieSwitch_dfl.log | PCIeSwitch模块管理对象的信息 | PcieSwitch 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| PcieSwitch | RetimerRegInfo | Retimer芯片寄存器信息 | PcieSwitch 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| PowerMgnt | power_statistics.csv | 功率统计信息 | 历史功率消耗统计数据。 | 服务器意外掉电、电源模块故障、功率封顶导致性能下降 |
| PowerMgnt | power_bbu_info.log | BBU模块日志（仅针对支持BBU模块的服务器） | 电源模块(PSU)状态、功率统计及电源封顶(Capping)策略日志。 | 服务器意外掉电、电源模块故障、功率封顶导致性能下降 |
| PowerMgnt | PowerMgnt_dfl.log | PowerMgnt模块管理对象的信息 | 电源模块(PSU)状态、功率统计及电源封顶(Capping)策略日志。 | 服务器意外掉电、电源模块故障、功率封顶导致性能下降 |
| redfish | component_uri.json | 部件URI列表 | Redfish RESTful API接口日志，记录外部管理软件的调用请求和响应。 | eSight/ManageOne对接失败、自动化脚本执行报错 |
| redfish | redfish_dfl.log | Redfish模块管理对象的信息 | Redfish RESTful API接口日志，记录外部管理软件的调用请求和响应。 | eSight/ManageOne对接失败、自动化脚本执行报错 |
| rimm | rimm_dfl.log | RIMM模块管理对象的信息 | rimm 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sensor_alarm | current_event.txt | 服务器当前健康状态和告警事件 | 当前未清除的活动告警列表。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | cache_event_log.db | 未上报的订阅事件数据库文件 | 硬件健康状态记录，包括温度、电压、风扇转速传感器读数及系统事件日志(SEL)。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | LedInfo | 服务器当前LED灯的显示状态 | 硬件健康状态记录，包括温度、电压、风扇转速传感器读数及系统事件日志(SEL)。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sel.db | sel数据库文件 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sel.tar | 当前sel信息和历史sel信息打包文件 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_dfl.log | Sensor_Alarm模块管理对象的信息 | 硬件健康状态记录，包括温度、电压、风扇转速传感器读数及系统事件日志(SEL)。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin | sel原始记录文件 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.bak | sel原始记录备份文件 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.bak.md5 | sel原始记录备份文件完整性校验码 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.bak.sha256 | sel原始记录备份文件完整性校验码 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.md5 | sel原始记录文件完整性校验码 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.sha256 | sel原始记录文件完整性校验码 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_alarm_sel.bin.tar.gz | sel历史记录打包文件 | 系统事件日志(System Event Log)，是硬件故障排查的最核心依据。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| sensor_alarm | sensor_info.txt | 服务器所有传感器信息列表 | 硬件健康状态记录，包括温度、电压、风扇转速传感器读数及系统事件日志(SEL)。 | 面板亮红灯/黄灯、服务器异常下电、温度过高告警、风扇全速运转 |
| Snmp | Snmp_dfl.log | Snmp模块管理对象的信息 | Snmp 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| StorageMgnt | RAID_Controller_Info.txt | 当前RAID控制器/逻辑盘/硬盘的信息 | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| StorageMgnt | StorageMgnt_dfl.log | StorageMgnt模块管理对象的信息 | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| switch_card | phy_register_info | 后插板phy寄存器信息 | switch_card 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| switch_card | port_adapter_info | 后插板接口器件信息 | switch_card 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| switch_card | switch_card_dfl.log | Switch_Card模块管理对象的信息 | switch_card 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| UPGRADE | UPGRADE_dfl.log | Upgrade模块管理对象的信息 | UPGRADE 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| UPGRADE | upgrade_info | iBMC相关器件的版本信息 | UPGRADE 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| User | User_dfl.log | User模块管理对象的信息 | License授权状态及用户信息。 | 高级功能无法使用（如KVM）、License过期 |
| usb_mgmt | usb_mgmt_dfl.log | usb_mgmt模块管理对象的信息 | usb_mgmt 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\CoreDump | core-*（以“core-”开头的文件） | 内存转储文件，根据系统运行情况可能产生一个或者多个文件，为应用程序core dump文件。 | dump\dump_info\CoreDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | cmdline | iBMC内核的命令行参数 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | cpuinfo | iBMC内核的CPU芯片信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | devices | iBMC系统的设备信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | df_info | iBMC分区空间的使用信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | diskstats | iBMC的磁盘信息 | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| sysinfo | filesystems | iBMC的文件系统信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | free_info | iBMC的内存使用概况 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | interrupts | iBMC的中断信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | ipcs_q | iBMC的进程队列信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | ipcs_q_detail | iBMC的进程队列详细信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | ipcs_s | iBMC的进程信号量信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | ipcs_s_detail | iBMC的进程信号量详细信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | loadavg | iBMC系统运行负载情况 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | locks | iBMC内核锁住的文件列表 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | lsof_info | 当前iBMC中所有打开的文件列表 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | meminfo | iBMC的内存占用详细信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | modules | iBMC的模块加载列表 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | mtd | iBMC的配置分区信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | partitions | iBMC所有设备分区信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | ps_info | ps -elf | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | slabinfo | iBMC内核内存管理slab信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | stat | iBMC的CPU利用率 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | top_info | top -bn 1 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | uname_info | uname -a | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | uptime | iBMC系统运行时间 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | version | iBMC当前的RTOS版本 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| sysinfo | vmstat | iBMC虚拟内存统计信息 | sysinfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | ibmc_revision.txt | iBMC版本编译节点信息 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | app_revision.txt | iBMC版本信息 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | build_date.txt | iBMC版本构建时间 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | fruinfo.txt | FRU电子标签信息 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | RTOS-Release | RTOS版本信息 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | RTOS-Revision | RTOS版本标记号 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| versioninfo | server_config.txt | 服务器当前的配置信息 | versioninfo 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| networkinfo | ifconfig_info | 网络信息，执行ifconfig的结果 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | ipinfo_info | iBMC配置的网络信息 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | _data_var_dhcp_dhclient.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6_eth0.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6_eth1.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6_eth2.leases | DHCP租约文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient_ip.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | dhclient6_ip.conf | DHCP配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | resolv.conf | DNS配置文件 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | netstat_info | netstat -an | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | route_info | route | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| networkinfo | services | 服务端口信息 | iBMC网络接口配置，包括IP地址、VLAN、DNS及LLDP信息。 | iBMC网络不可达、跨网段通信失败 |
| other_info | extern.conf | BMC日志文件配置 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | remotelog.conf | syslog定制配置文件 | other_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | ssh | SSH服务配置 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | sshd_config | SSHD服务配置文件 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | logrotate.status | logrotate状态记录文件 | other_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | login | login PAM登录规则 | other_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | sshd | SSH PAM登录规则 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | pam_tally2 | 登录iBMC失败的锁定规则 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | datafs_log | data检测日志 | other_info 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | ntp.conf | NTP服务配置 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| other_info | vsftpd | FTP PAM登录规则 | other_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | dmesg_info | 系统启动信息，执行dmesg的结果 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | lsmod_info | 当前加载驱动模块信息 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | kbox_info | kbox信息 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | edma_drv_info | edma驱动信息 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | cdev_drv_info | 字符设备驱动信息 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| driver_info | veth_drv_info | 虚拟网卡驱动信息 | driver_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| i2c_info | *_info | I2C设备的寄存器/存储区信息 | i2c_info 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| storage | IODeterioration.db | 硬盘IO慢盘监测数据 | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| storage | SubhealthyStatus.db | 硬盘亚健康状态数据 | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| storage | drivelog | 所有SAS和SATA硬盘的日志信息文件夹，其子目录根据硬盘名称命名，如： | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| storage | phy | 所有RAID卡和该卡下Expander的PHY误码日志信息文件夹，其子目录根据RAID卡名称命名，如： | RAID控制卡日志及物理磁盘状态信息。 | RAID组降级、硬盘无法识别、硬盘亮黄灯 |
| dump\dump_info\LogDump | bmccom.dat | BMC串口日志 | iBMC核心模块日志，记录管理系统的运行状态、用户操作及内部进程通信。 | iBMC重启、管理口无法ping通、IPMI命令无响应 |
| dump\dump_info\LogDump | fdm_pfae_log | FDM预告警日志 | dump\dump_info\LogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\LogDump | PD_SMART_INFO_C* | 硬盘的SMART日志，*为RAID控制器的编号 | dump\dump_info\LogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\LogDump | third_party_file_bak.log | 第三方文件备份日志记录 | dump\dump_info\LogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\OSDump | *.rep | 业务侧屏幕自动录像文件 | dump\dump_info\OSDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\OSDump | img*.jpeg | 业务侧最后一屏图像 | dump\dump_info\OSDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\OSDump | systemcom.tar | SOL串口信息 | dump\dump_info\OSDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\OSDump | video_caterror_rep_is_deleted.info | 删除过大的caterror录像的提示 | dump\dump_info\OSDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\Register | cpld_reg_info | CPLD寄存器信息 | dump\dump_info\Register 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\Register | cpu_reg_info | CPU寄存器信息 | dump\dump_info\Register 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | config | 配置导出备份文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | deviceinfo.json | 服务器资产信息 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | diagnose | 硬件诊断日志 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | dmesg.log | 小系统dmesg日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | eSightCfglog.tar.gz | eSightCfg日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | filepatchup_debug.log | 极速部署文件打包日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | images.log | 极速部署克隆日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | images_restore.log | 极速部署还原日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | maintainlog.csv | SP维护日志。带时间戳的maintainlog文件为之前收集的日志。 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | operatelog.csv | SP运行日志。带时间戳的operatinglog文件为之前收集的日志。 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | ping6.log | 网络通信日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | quickdeploy_debug.log | 极速部署日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | sp_upgrade_info.log | SP自升级日志 | dump\dump_info\SpLogDump 模块的运行日志，记录模块初始化及运行时的详细调试信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | upgrade | SP固件升级日志 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | version.json | SP版本配置文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
| dump\dump_info\SpLogDump | version.json.*.sha | SP版本配置文件的校验文件 | dump\dump_info\SpLogDump 模块的相关配置文件或状态信息。 | 该模块功能异常、状态查询失败或配置不生效 |
