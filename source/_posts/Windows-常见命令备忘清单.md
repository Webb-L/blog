---
title: Windows 常见命令备忘清单
date: 2022-04-25 16:20:50
tags:
---

## 防火墙

| 命令                                                                                            | 说明      |
|-----------------------------------------------------------------------------------------------|---------|
| netsh advfirewall show allprofiles                                                            | 防火墙状态   |
| netsh firewall show opmode                                                                    | 防火墙状态   |
| netsh firewall show state                                                                     | 防火墙状态   |
| netsh advfirewall firewall show rule name=all                                                 | 防火墙所有规则 |
| netsh advfirewall firewall show rule name=http                                                | 防火墙某个规则 |
| netsh firewall show config                                                                    | 防火墙配置   |
| netsh firewall set opmode mode = disable                                                      | 关闭防火墙   |
| netsh advfirewall set allprofiles state off                                                   | 关闭防火墙   |
| netsh firewall set opmode mode = enable                                                       | 启动防火墙   |
| netsh advfirewall set allprofiles state on                                                    | 启动防火墙   |
| netsh firewall set portopening tcp 80 enable                                                  | 添加某个端口  |
| netsh advfirewall firewall add rule name="http" dir=in protocol=tcp localport=80 action=allow | 添加某个端口  |
| netsh firewall delete portopening protocol=TCP port=80                                        | 删除某个端口  |
| netsh advfirewall firewall delete rule name=http protocol=tcp localport=80                    | 删除某个端口  |
| netsh firewall show logging                                                                   | 防火墙日志   |
| netsh firewall reset                                                                          | 恢复默认配置  |

## 用户

| 命令                                         | 说明           |
|--------------------------------------------|--------------|
| net user                                   | 查询所有用户 （本机）  |
| net user /domain                           | 查询所有用户 （域控）  |
| whoami /all                                | 查询所有用户 （本机）  |
| wmic useraccount get /ALL /format:csv      | 查询所有用户 （本机）  |
| whoami                                     | 查询当前用户       |
| whoami /logonid                            | 查询当前用户登录ID   |
| whoami /priv                               | 查询当前用户特权信息   |
| whoami /groups                             | 查询当前用户的组     |
| net localgroup administrators              | 查询某个组的成员     |
| quser                                      | 所有当前在线用户     |
| quser administrators                       | 当前某个在线用户     |
| qwinsta                                    | 所有当前在线用户     |
| qwinsta administrators                     | 当前某个在线用户     |
| query user                                 | 所有当前在线用户     |
| query user administrator                   | 当前某个在线用户     |
| net user name password /add                | 添加用户         |
| net user name *                            | 修改某个用户密码     |
| net user name /del                         | 删除用户         |
| net localgroup administrators name /add    | 将某位用户添加到管理员组 |
| net localgroup administrators name /delete | 将某位用户从管理员组删除 |

## 服务

| 命令                                    | 说明         |
|---------------------------------------|------------|
| sc query state= all                   | 查看所有服务     |
| net start                             | 查看所有开启的服务  |
| sc query                              | 查看所有开启的服务  |
| sc query state= active                | 查看所有开启的服务  |
| sc query state= inactive              | 查看所有关闭的服务  |
| sc query type= driver                 | 查看所有驱动的服务  |
| sc query type= service                | 查看所有服务的服务  |
| sc query 服务名                          | 查看某个服务     |
| sc stop 服务名                           | 关闭某个服务     |
| sc start 服务名                          | 开启某个服务     |
| sc pause 服务名                          | 暂停某个服务     |
| sc pause 服务名                          | 暂停某个服务     |
| sc delete 服务名                         | 删除某个服务     |
| sc create 服务名 binPath= 路径             | 创建一个服务     |
| sc create 服务名 binPath= 路径 start= auto | 创建一个自启动的服务 |
| sc config 服务名 binPath= 路径             | 修改某个服务     |
| sc config 服务名 binPath= 路径 start= auto | 修改某个服务手动启动 |
| sc description 描述                     | 修改某个服务的描述  |

## 进程

| 命令                                                                     | 说明           |
|------------------------------------------------------------------------|--------------|
| tasklist                                                               | 查看所有进程       |
| tasklist /v                                                            | 查看所有进程       |
| tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running" | 查看所有正在运行系统进程 |
| tasklist /v /fi "STATUS eq running"                                    | 查看所有正在运行进程   |
| taskkill /pid 进程pid                                                    | 结束进程         |
| taskkill /t /pid 进程pid                                                 | 结束进程并结束子进程   |
| taskkill /f /pid 进程pid                                                 | 强制结束进程       |
| taskkill /pid 2134 /t /fi "username eq administrator"                  | 结束管理员帐户启动的进程 |

## 启动项

| 命令                                                                                                         | 说明       |
|------------------------------------------------------------------------------------------------------------|----------|
| wmic startup                                                                                               | 查看启动项    |
| reg query HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run                                 | 查看启动项    |
| reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v 启动项名称 /t REG_SZ /d 新注册表项的数据 /f | 添加或修改启动项 |
| reg delete HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v 启动项名称 /f                    | 删除启动项    |
