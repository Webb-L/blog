---
title: smbclient 命令备忘录
date: 2022-05-30 16:58:38
tags:
---

> 使用该命令的前提您的目标设备必须要开启smb服务和设置共享文件。

## 语法：

```bash
smbclient [选项] 服务 <密码>
```


## 参数：

| 参数                         | 描述                |
|----------------------------|-------------------|
| -M, --message=HOST         | 发信息               |
| -I, --ip-address=IP        | 使用此 IP 连接到        |
| -E, --stderr               | 将消息写入标准错误而不是标准输出  |
| -L, --list=HOST            | 获取主机上可用的共享列表      |
| -T, --tar=\<c/x\>IXFvgbNan | 命令行tar            |
| -D, --directory=DIR        | 从某个目录开始           |
| -c, --command=STRING       | 执行命令              |
| -b, --send-buffer=BYTES    | 更改发送/发送缓冲区        |
| -t, --timeout=SECONDS      | 更改每个操作的超时         |
| -p, --port=PORT            | 要连接的端口            |
| -g, --grepable             | 产生 grepable 输出    |
| -q, --quiet                | 禁止显示帮助消息          |
| -B, --browse               | 使用 DNS 浏览 SMB 服务器 |

## 命令

| 命令                                                                     | 描述                                                           |
|------------------------------------------------------------------------|--------------------------------------------------------------|
| ?/help 命令                                                              | 查看某个命令帮助信息                                                   |
| allinfo \<file>                                                        | 显示所有可用信息                                                     |
| altname \<file>                                                        | 显示替代名称                                                       |
| archive \<level>                                                       | 0=忽略存档位<br/>1=只获取存档文件<br/>2=仅获取存档文件并重置存档位<br/>3=获取所有文件并重置存档位 |
| backup                                                                 | 切换备份意图状态                                                     |
| blocksize \<number> (默认 20)                                            | 数据块大小                                                        |
| cancel \<jobid>                                                        | 取消打印队列条目                                                     |
| case_sensitive                                                         | 将区分大小写的标志切换到服务器                                              |
| cd \[directory]                                                        | 更改/报告远程目录                                                    |
| chmod \<src> \<mode>                                                   | chmod 使用 UNIX 权限的文件                                          |
| du \<mask>                                                             | 计算当前目录的总大小                                                   |
| echo \<num> \<data>                                                    | ping 服务器                                                     |
| exit\q\quit                                                            | 注销服务器                                                        |
| get  \<remote name> \[local name]                                      | 获取文件                                                         |
| getfacl \<file name>                                                   | 获取文件的 POSIX ACL（仅限 UNIX 扩展）                                  |
| geteas \<file name>                                                    | 获取文件的 EA 列表                                                  |
| hardlink \<src> \<dest>                                                | 创建 Windows 硬链接                                               |
| history                                                                | 显示命令历史                                                       |
| iosize \<number> (default 64512)                                       | io 大小                                                        |
| lcd \[directory]                                                       | 更改/报告本地当前工作目录                                                |
| link \<oldname> \<newname>                                             | 创建 UNIX 硬链接                                                  |
| lock \<fnum> \[r/w] \<hex-start> \<hex-len>                            | 设置 POSIX 锁                                                   |
| lowercase                                                              | 切换获取文件名的小写                                                   |
| ls/l \<mask>                                                           | list the contents of the current directory                   |
| mask \<mask>                                                           | 屏蔽所有文件名                                                      |
| md/mkdir \<directory>                                                  | 创建一个目录                                                       |
| mget \<mask>                                                           | 获取所有匹配的文件                                                    |
| more \<remote name>                                                    | 查看远程文件                                                       |
| mput \<mask>                                                           | 把所有匹配的文件                                                     |
| newer \<file>                                                          | 仅 mget 比指定本地文件更新的文件                                          |
| notify \<file>                                                         | 获取有关目录更改的通知                                                  |
| open \<mask>                                                           | 打开一个文件                                                       |
| posix                                                                  | 打开所有 POSIX 功能                                                |
| posix_encrypt \<domain> \<user> \<password>                            | 启动传输加密                                                       |
| posix_open \<name> 0\<mode>                                            | open_flags 模式使用 POSIX 接口打开文件                                 |
| posix_mkdir \<name> 0\<mode>                                           | 使用 POSIX 接口创建目录                                              |
| posix_rmdir \<name>                                                    | removes a directory using POSIX interface                    |
| posix_unlink \<name>                                                   | 使用 POSIX 接口删除文件                                              |
| posix_whoami                                                           | 使用 POSIX 接口返回登录用户信息                                          |
| print \<file name>                                                     | 打印文件                                                         |
| prompt                                                                 | 切换提示输入 mget 和 mput 的文件名                                      |
| put \<local name> \[remote name]                                       | 上传一个文件                                                       |
| pwd                                                                    | 显示当前远程目录（与没有参数的 'cd' 相同）                                     |
| queue                                                                  | 显示打印队列                                                       |
| readlink \<filename>                                                   | 对符号链接执行 UNIX 扩展 readlink 调用                                  |
| rd/rm/rmdir \<directory>                                               | 删除目录                                                         |
| recurse                                                                | 切换 mget 和 mput 的目录递归                                         |
| reget \<remote name> \[local name]                                     | 获取在本地文件末尾重新启动的文件                                             |
| rename \<src> \<dest>                                                  | 重命名一些文件                                                      | 
| reput \<local name> \[remote name]                                     | 将文件重新启动到远程文件的末尾                                              |
| showacls                                                               | 切换是否显示 ACL                                                   |
| setea \<file name> \<eaname> \<eaval>                                  | 设置文件的 EA                                                     |
| setmode \<file name> \<setmode string>                                 | 更改文件模式                                                       |
| scopy \<src> \<dest>                                                   | 服务器端拷贝文件                                                     |
| stat \<file name>                                                      | 对文件执行 UNIX 扩展 stat 调用                                        |
| symlink \<oldname> \<newname>                                          | 创建一个 UNIX 符号链接                                               |
| tar \<c/x>\[IXFvbgNan]                                                 | 当前目录到/从 \<文件名>                                               |
| tarmode \<full/inc/reset/noreset>                                      | tar 对归档位的行为                                                  |
| timeout \<number>                                                      | 以秒为单位设置每个操作的超时时间（默认 20）                                      |
| translate                                                              | 切换用于打印的文本翻译                                                  |
| unlock \<fnum> \<hex-start> \<hex-len>                                 | 移除 POSIX 锁                                                   |
| volume                                                                 | print the volume name                                        |
| vuid                                                                   | change current vuid                                          |
| wdel \<attrib> \<mask>                                                 | 通配符删除所有匹配的文件                                                 |
| logon \<username> \[\<password>]                                       | 建立新的登录                                                       |
| listconnect                                                            | 列出打开的连接                                                      |
| showconnect                                                            | 显示当前活动连接                                                     |
| tcon \<sharename>                                                      | 连接到共享                                                        |
| tdis                                                                   | 断开共享                                                         |
| tid                                                                    | 显示或设置当前 tid (tree-i)                                         |
| utimes \<file name> \<create_time> \<access_time> \<mod_time> \<ctime> | 设定时间                                                         |  
| logoff                                                                 | 注销（关闭会话）                                                     |

## 例子：

+ 显示目标主机可用的共享列表。

```bash
smbclient -L //<ip> -U <用户名>%<密码>

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      远程管理
	C               Disk      
	C$              Disk      默认共享
	IPC$            IPC       远程 IPC
SMB1 disabled -- no workgroup available
```

+ 连接某个共享列表。

```bash
smbclient //<ip>/共享名称 -U <用户名>%<密码>
Try "help" to get a list of possible commands.
smb: \> 
```

+ 设置连接的Windows目录。

```bash
smbclient //<ip>/共享名称 -D Windows  -U <用户名>%<密码>
Try "help" to get a list of possible commands.
smb: \Windows\> 
```

+ 连接成功以后执行上传文件命令.

```bash
smbclient //<ip>/共享名称/ -c "put <文件>" -D <目录> -U <用户名>%<密码>
putting file <文件> as \<目录>\<文件> (71.4 kb/s) (average 71.4 kb/s)
# 没有权限
smbclient //<ip>/共享名称/ -c "put <文件>" -D <目录>  -U <用户名>%<密码>
NT_STATUS_ACCESS_DENIED opening remote file \<目录>\<文件>
```