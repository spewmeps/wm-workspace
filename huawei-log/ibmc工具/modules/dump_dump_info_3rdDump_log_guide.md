# dump\dump_info\3rdDump 模块日志分析指南

## 1. 模块概述
iBMC核心转储与Web服务日志

## 2. 典型故障分析思路
- **Web访问失败**: 检查 `3rdDump` 下的 `error_log`。
- **系统崩溃**: 分析 `dump_info` 中的调试信息。

## 3. 详细日志清单
| 收集项文件 | 内容说明 | 详细作用 | 关联故障场景 |
| --- | --- | --- | --- |
 | access_log | Nginx访问日志 | 记录所有HTTP/HTTPS请求的来源IP、请求URL和响应状态码。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | access_log.1 | Nginx访问日志备份文件 | 记录所有HTTP/HTTPS请求的来源IP、请求URL和响应状态码。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | error_log | Nginx错误日志 | 记录Nginx进程启动错误、模块加载失败及请求处理异常。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | error_log.1 | Nginx错误日志备份文件 | 记录Nginx进程启动错误、模块加载失败及请求处理异常。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx.conf | Nginx基础配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_http_server.conf | Nginx http server配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_https_server.conf | Nginx https server配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_https_server_ext.conf | Nginx扩展配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_https_defualt_server_ext.conf | Nginx https defualt server扩展配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_ssl_ciphersuite.conf | Nginx https协议加密套件配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_ssl_protocol.conf | Nginx https协议版本配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
 | nginx_ssl_verify_client.conf | Nginx双因素认证配置文件 | Nginx配置文件，决定了Web服务的端口、SSL协议版本及加密套件。 | Web界面无法访问、HTTPS证书报错、Web操作超时、双因素认证失败 | 
