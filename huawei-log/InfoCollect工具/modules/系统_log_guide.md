# 系统 模块日志分析指南

## 1. 模块概述
本模块主要涉及内核消息、服务启动、用户登录及核心系统配置的日志分析。

## 2. 典型故障分析思路
- **关键报错**: 在 `/var/log/messages` 中搜索 `error`, `fail`, `panic`, `warning` 等关键字。
- **内存溢出**: 搜索 `Out of memory` 或 `Kill process` 确认是否触发了 OOM Killer。
- **硬件隐患**: `dmesg` 中的 `Hardware Error` (MCE) 通常预示着硬件故障。

## 3. 详细日志清单
| 文件名称 | 收集命令 | 文件说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- | --- |
| runlog | 工具运行后台记录 | 工具本身的运行日志信息 | 系统基础信息：工具本身的运行日志信息 | 系统级故障排查 |
| /var/log/secure | cp /var/log/secure* | 安全日志 | 安全认证日志，记录SSH登录、sudo提权、用户创建等安全事件。 | 登录失败、被黑客攻击、权限审计 |
| /var/log/mcelog | cp /var/log/mcelog* | 定位MCE错误需要的日志信息 | 系统基础信息：定位MCE错误需要的日志信息 | 系统级故障排查 |
| /var/log/message | cp /var/log/message* | 常规系统日志文件 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| /var/log/cron | cp /var/log/cron* | 定制任务日志文件 | 定时任务调度日志，记录crontab任务的执行时间和状态。 | 定时任务未执行或执行失败 |
| /var/log/boot.log | cp /var/log/boot.log | 本次启动日志，log格式 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /var/log/boot.msg | cp /var/log/boot.msg | 本次启动日志，msg格式 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /var/log/boot.omsg | cp /var/log/boot.omsg | 启动日志（本次启动的前一次过程日志） | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /etc/syslog.conf | cp /etc/syslog.conf | syslogd进程的配置文件 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| /etc/resolv.conf | cp /etc/resolv.conf | 网络代理的配置文件 | 系统基础信息：网络代理的配置文件 | 系统级故障排查 |
| /etc/hosts | cp /etc/hosts | 主机信息 | 系统基础信息：主机信息 | 系统级故障排查 |
| /etc/services | cp /etc/services | 启动服务 | 系统基础信息：启动服务 | 系统级故障排查 |
| /etc/sysctl.conf | cp/etc/sysctl.conf | 系统内核参数配置文件，涉及网络功能，例如转发等 | 内核参数配置，控制网络、内存、文件系统等核心行为。 | 性能调优、参数配置错误导致的问题 |
| /etc/inittab | cp /etc/inittab | 初始化，涉及系统配置 | 系统基础信息：初始化，涉及系统配置 | 系统级故障排查 |
| /etc/ntp.conf | cp /etc/ntp.conf | NTP配置文件 | 系统基础信息：NTP配置文件 | 系统级故障排查 |
| /etc/ntp/step-tickers | cp /etc/ntp/step-tickers | NTP配置文件 | 系统基础信息：NTP配置文件 | 系统级故障排查 |
| /etc/ntp/ntpservers | cp /etc/ntp/ntpservers | NTP服务器列表 | 系统基础信息：NTP服务器列表 | 系统级故障排查 |
| /etc/yp.conf | cp /etc/yp.conf | ypbind配置文件 | 系统基础信息：ypbind配置文件 | 系统级故障排查 |
| /etc/fstab | cp /etc/fstab | 文件系统列表 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| /proc/interrupts | cp /proc/interrupts | 中断数量 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/filesystems | cp /proc/filesystems | 当前文件系统列表 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /etc/exports | cp /etc/exports | NFS服务器配置 | 系统基础信息：NFS服务器配置 | 系统级故障排查 |
| /proc/self/maps | cp /proc/self/maps | 查看进程的虚拟地址空间 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/self/smaps | cp /proc/self/smaps | 单个进程的内存查看 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/self/numa_maps | cp /proc/self/numa_maps | numa进程的内存查看 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/iomap | cp /proc/iomap | I/O映射内存空间 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/ioports | cp /proc/ioports | 显示当时正在使用的I/O端口资源分布 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/swaps | cp /proc/swaps | 交换分区信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/slabinfo | cp /proc/slabinfo | 内核信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/locks | cp /proc/locks | 查看是否有进程锁定 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/modules | cp /proc/modules | 显示各个模块 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/mounts | cp /proc/mounts | 显示当前挂载的设备 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| /proc/version | cp /proc/version | 查看内核版本 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/stat | cp /proc/stat | 内核和系统的统计信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/schedstat | cp /proc/schedstat | 内核schedstat信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/zoneinfo | cp /proc/zoneinfo | 内核时区信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/config.gz | cp /proc/config.gz | proc文件系统配置文件集 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/kallsyms | cp /proc/kallsyms | 内核调试文件 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/mtrr | cp /proc/mtrr | 内核内存范围寄存器 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/vmstat | cp /proc/vmstat | 虚拟内存状态 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/buddyinfo | cp /proc/buddyinfo | 内存碎片信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/cmdline | cp /proc/cmdline | 内核启动的命令行 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/devices | cp /proc/devices | 设备驱动程序 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/diskstats | cp /proc/diskstats | 硬盘状态 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /proc/iomem | cp /proc/iomem | 物理地址的分配情况 | 内存相关信息：物理地址的分配情况 | 内存溢出(OOM)、系统变慢、Swap频繁交换、程序崩溃 |
| /proc/dma | cp /proc/dma | DMA通道信息 | 内核运行时状态快照，包含进程、内存、中断等实时信息。 | 深入分析系统状态、内核调试 |
| /boot/config* | cp /boot/config* | 内核配置文件 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /etc/grub.conf | cp /etc/grub.conf | grub配置文件 | 系统基础信息：grub配置文件 | 系统级故障排查 |
| /boot/grub/device.map | cp /boot/grub/device.map | 启动设备映射 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /boot/grub/menu.lst | cp /boot/menu.lst | 启动菜单 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| /boot/grub2/grub.cfg | cp /boot/grub2/grub.cfg | Grub2信息 | 系统启动过程日志，记录服务启动顺序和状态。 | 启动慢、启动卡住 |
| dump_list.txt | du -ah /var/crash/ | crash文件清单 | 系统基础信息：crash文件清单 | 系统级故障排查 |
| dmesg.txt | dmesg | dmesg文件 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| /root/.bash_history | cp /root/.bash_history | 历史执行命令 | 系统基础信息：历史执行命令 | 系统级故障排查 |
| command_log.txt | df -liT | 查看剩余空间 | 系统基础信息：查看剩余空间 | 系统级故障排查 |
| command_log.txt | df -Th | 文件系统挂载信息 | 系统基础信息：文件系统挂载信息 | 系统级故障排查 |
| command_log.txt | mount | 挂载信息 | 文件系统挂载表和配置，显示挂载点、类型和参数。 | 文件系统只读、挂载失败 |
| command_log.txt | /usr/bin/last -xF | grep "reboot\ | shutdown\ | runlevel\ | system" | 重启纪录 | 系统基础信息：grep "reboot\ | 系统级故障排查 |
| command_log.txt | nfsstat -cnrs | NFS服务状态 | 系统基础信息：NFS服务状态 | 系统级故障排查 |
| command_log.txt | lsof | 已打开文件列表 | 系统基础信息：已打开文件列表 | 系统级故障排查 |
| command_log.txt | ulimit -a | 查看系统资源限制 | 系统基础信息：查看系统资源限制 | 系统级故障排查 |
| command_log.txt | ipcs -a | 显示进程间通信设施状态 | 系统基础信息：显示进程间通信设施状态 | 系统级故障排查 |
| command_log.txt | ipcs -l | 查看共享信息的内存 | 系统基础信息：查看共享信息的内存 | 系统级故障排查 |
| command_log.txt | sysctl -a | sysctl设置和显示在“/proc/sys”目录中的内核参数 | 系统基础信息：sysctl设置和显示在“/proc/sys”目录中的内核参数 | 系统级故障排查 |
| command_log.txt | uptime | 已经运行时间 | 系统基础信息：已经运行时间 | 系统级故障排查 |
| command_log.txt | uname -a | 查看内核 | 系统基础信息：查看内核 | 系统级故障排查 |
| command_log.txt | ps auwx | 当前所有进程列表 | 系统基础信息：当前所有进程列表 | 系统级故障排查 |
| command_log.txt | last | 历史登录过的用户纪录 | 系统基础信息：历史登录过的用户纪录 | 系统级故障排查 |
| command_log.txt | hostid | 登录用户的ID号 | 系统基础信息：登录用户的ID号 | 系统级故障排查 |
| command_log.txt | /bin/rpm -qa | 已经安装软件清单 | 系统基础信息：已经安装软件清单 | 系统级故障排查 |
| command_log.txt | printenv | 打印环境变量 | 系统基础信息：打印环境变量 | 系统级故障排查 |
| command_log.txt | hostname | 主机名信息 | 系统基础信息：主机名信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/dirty_ratio | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/dirty_background_ratio | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/dirty_writeback_centisecs | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/dirty_expire_centisecs | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/vfs_cache_pressure | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /proc/sys/vm/swappiness | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat blockdev --getra /dev/sdx | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/block/sdx/queue/scheduler | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/nr_requests | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_segments | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_sectors_kb | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/max_hw_sectors_kb | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/block/sdx/queue/hw_sector_size | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| filesystem_config_log.txt | cat /sys/class/block/sdx/device/queue_depth | 文件系统配置信息 | 系统基础信息：文件系统配置信息 | 系统级故障排查 |
| devinfo.txt | ls –l /dev | /dev文件夹下的详细信息 | 系统基础信息：/dev文件夹下的详细信息 | 系统级故障排查 |
| /var/log/kern* | cp /var/log/kern* | Kern日志信息 | 系统基础信息：Kern日志信息 | 系统级故障排查 |
| /var/log/dmesg* | cp /var/log/dmesg* | Dmesg日志信息 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| /var/log/syslog* | cp /var/log/syslog* | Syslog日志信息 | 系统核心日志，汇总了内核、服务启动、硬件错误等大部分系统级事件。 | 通用故障排查第一入口、系统报错、服务异常 |
| /var/log/{hostname}/{date}/ | cp –r/var/log/{hostname}/{date}/ | FusionSphere CNA 6.1系统日志备份 | 系统基础信息：FusionSphere CNA 6.1系统日志备份 | 系统级故障排查 |
| /var/backuplog/galaxenginelog/{date}/ | cp -r /var/backuplog/galaxenginelog/{date}/ | FusionSphere CNA6.3~6.5 系统日志备份 | 系统基础信息：FusionSphere CNA6.3~6.5 系统日志备份 | 系统级故障排查 |
| /var/log/logdump/ | cp -r /var/log/logdump | FusionSphere openstack系统日志备份 | 系统基础信息：FusionSphere openstack系统日志备份 | 系统级故障排查 |
| /var/log/logdump-{num}-{timestamp}.tar.gz | cp -r /var/log/logdump-{num}-{timestamp}.tar.gz | FusionSphere openstack 系统日志备份 | 系统基础信息：FusionSphere openstack 系统日志备份 | 系统级故障排查 |
| /var/crash/*/vmcore-dmesg.txt | - | 系统错误信息 | 内核环形缓冲区信息，记录从启动到当前的底层硬件和驱动加载消息。 | 启动失败、硬件识别错误、驱动崩溃、OOM Killer |
| dpkglist.txt | dpkg -l | Ubuntu系统安装的软件包信息 | 系统基础信息：Ubuntu系统安装的软件包信息 | 系统级故障排查 |
| vmstat.txt | vmstat 1 60 | 进程，虚拟内存和CPU活动信息 | 系统基础信息：进程，虚拟内存和CPU活动信息 | 系统级故障排查 |
| iostat.txt | iostat -xct 1 60 | 磁盘和CPU活动统计信息 | 系统基础信息：磁盘和CPU活动统计信息 | 系统级故障排查 |
| command_log.txt | ps -eo pid,lstart,cmd | 等待资源的进程数 | 系统基础信息：等待资源的进程数 | 系统级故障排查 |
