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
