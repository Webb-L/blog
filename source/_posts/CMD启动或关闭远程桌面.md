---
title: CMD启动或关闭远程桌面
date: 2022-05-12 21:45:53
tags:
---

+ 启动或者关闭远程桌面需要**管理员**权限。

## 启动

```cmd
:: 修改注册表
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

:: 防火墙开放远程桌面
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes

:: 查看是否启动成功
C:\Windows\system32>netstat -anot | findstr "3389"
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING       5900     InHost
  TCP    [::]:3389              [::]:0                 LISTENING       5900     InHost
  UDP    0.0.0.0:3389           *:*                                    5900
  UDP    [::]:3389              *:*                                    5900
```

## 关闭

```cmd
:: 修改注册表
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 1 /f

:: 防火墙开放远程桌面
netsh advfirewall firewall set rule group="remote desktop" new enable=No

:: 查看是否启动成功
C:\Windows\system32>netstat -anot | findstr "3389"
```