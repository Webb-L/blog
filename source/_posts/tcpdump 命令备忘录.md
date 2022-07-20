---
title: tcpdump 命令备忘录
date: 2022-07-19 22:57:45
tags:
---

## 介绍

Tcpdump 打印出网络接口上数据包内容的描述。

## 使用

```bash
tcpdump [options] [proto] [dir] [type]

# options
[-aAbdDefhHIJKlLnNOpqStuUvxX#] [ -B size ] [ -c count ]
[ -C file_size ] [ -E algo:secret ] [ -F file ] [ -G seconds ]
[ -i interface ] [ -j tstamptype ] [ -M secret ] [ --number ]
[ -Q in|out|inout ]
[ -r file ] [ -s snaplen ] [ --time-stamp-precision precision ]
[ --immediate-mode ] [ -T type ] [ --version ] [ -V file ]
[ -w file ] [ -W filecount ] [ -y datalinktype ] [ -z postrotate-command ]
[ -Z user ] [ expression ]

# proto
tcp udp icmp
ip ipv6
arp rarp
ether wlan

# dir
src dst

# type
host net port portrange
```

### 简单使用

```bash
root@webb:~# tcpdump 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:01:20.103144 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 999682989:999683185, ack 3034309012, win 501, options [nop,nop,TS val 3742754932 ecr 544450561], length 196
15:01:20.103268 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 196, win 501, options [nop,nop,TS val 544450627 ecr 3742754932], length 0
...
15:01:20.620795 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 364648, win 998, options [nop,nop,TS val 544451144 ecr 3742755450], length 0
15:01:20.620861 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 364648:365132, ack 1, win 501, options [nop,nop,TS val 3742755450 ecr 544451144], length 484
15:01:20.621140 IP 192.168.111.1.43116 > webb.ssh: Flags [P.], seq 1:37, ack 364648, win 998, options [nop,nop,TS val 544451144 ecr 3742755450], length 36
2079 packets captured
2079 packets received by filter
0 packets dropped by kernel
```

解析信息。

| 时:分:秒.毫秒        | 网络协议 | 发送方的IP地址.端口/主机名.协议（如果这里的端口能够对应上协议的端口就显示协议）> 接收方IP地址.端口/主机名.协议 |     | 协议的信息                                                                                                   |
|-----------------|------|---------------------------------------------------------------|:----|---------------------------------------------------------------------------------------------------------|
| 15:01:20.621140 | IP   | 192.168.111.1.43116 > webb.ssh                                |     | Flags [P.], seq 1:37, ack 364648, win 998, options [nop,nop,TS val 544451144 ecr 3742755450], length 36 |

## 过滤使用

### 过滤特定IP地址数据包

```bash
root@webb:~# tcpdump host 192.168.1.1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:34:03.881935 IP webb > gw.gcableconfig.local: ICMP echo request, id 4, seq 1, length 64
15:34:03.884549 IP gw.gcableconfig.local > webb: ICMP echo reply, id 4, seq 1, length 64
15:34:04.883253 IP webb > gw.gcableconfig.local: ICMP echo request, id 4, seq 2, length 64
15:34:04.884774 IP gw.gcableconfig.local > webb: ICMP echo reply, id 4, seq 2, length 64
15:34:05.892408 IP webb > gw.gcableconfig.local: ICMP echo request, id 4, seq 3, length 64
15:34:05.921905 IP gw.gcableconfig.local > webb: ICMP echo reply, id 4, seq 3, length 64
15:34:06.913191 IP webb > gw.gcableconfig.local: ICMP echo request, id 4, seq 4, length 64
15:34:06.920496 IP gw.gcableconfig.local > webb: ICMP echo reply, id 4, seq 4, length 64
8 packets captured
10 packets received by filter
0 packets dropped by kernel

```

### 过滤发送方IP地址数据包

```bash
root@webb:~# tcpdump src 192.168.1.1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:40:51.123117 IP gw.gcableconfig.local > webb: ICMP echo reply, id 7, seq 1, length 64
15:40:52.125865 IP gw.gcableconfig.local > webb: ICMP echo reply, id 7, seq 2, length 64
15:40:53.127900 IP gw.gcableconfig.local > webb: ICMP echo reply, id 7, seq 3, length 64
3 packets captured
3 packets received by filter
0 packets dropped by kernel
```

### 过滤接收方IP地址数据包

```bash
root@webb:~# tcpdump dst 192.168.1.1
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:36:04.468257 IP webb > gw.gcableconfig.local: ICMP echo request, id 5, seq 1, length 64
15:36:05.469379 IP webb > gw.gcableconfig.local: ICMP echo request, id 5, seq 2, length 64
15:36:06.471280 IP webb > gw.gcableconfig.local: ICMP echo request, id 5, seq 3, length 64
3 packets captured
3 packets received by filter
0 packets dropped by kernel
```

### 过滤某个网段数据包

```bash
root@webb:~# tcpdump net 192.168.1.0/24
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:43:20.125194 IP webb > gw.gcableconfig.local: ICMP echo request, id 8, seq 1, length 64
15:43:20.127335 IP gw.gcableconfig.local > webb: ICMP echo reply, id 8, seq 1, length 64
15:43:21.147335 IP webb > gw.gcableconfig.local: ICMP echo request, id 8, seq 2, length 64
15:43:21.149613 IP gw.gcableconfig.local > webb: ICMP echo reply, id 8, seq 2, length 64
15:43:22.128009 IP webb > gw.gcableconfig.local: ICMP echo request, id 8, seq 3, length 64
15:43:22.130053 IP gw.gcableconfig.local > webb: ICMP echo reply, id 8, seq 3, length 64
15:43:24.042801 IP webb > android-0000000000: ICMP echo request, id 9, seq 1, length 64
...
15:43:27.629869 IP webb > 192.168.1.3: ICMP echo request, id 10, seq 1, length 64
15:43:28.648935 IP webb > 192.168.1.3: ICMP echo request, id 10, seq 2, length 64
15:43:29.673008 IP webb > 192.168.1.3: ICMP echo request, id 10, seq 3, length 64
15:43:30.696968 IP webb > 192.168.1.3: ICMP echo request, id 10, seq 4, length 64
15:43:30.698285 IP 192.168.1.68 > webb: ICMP host 192.168.1.3 unreachable, length 92
15:43:30.698285 IP 192.168.1.68 > webb: ICMP host 192.168.1.3 unreachable, length 92
15:43:30.698285 IP 192.168.1.68 > webb: ICMP host 192.168.1.3 unreachable, length 92
15:43:30.698285 IP 192.168.1.68 > webb: ICMP host 192.168.1.3 unreachable, length 92
20 packets captured
20 packets received by filter
0 packets dropped by kernel
```

### 过滤某个端口或某个范围端口数据包

```bash
root@webb:~# tcpdump port 80 / tcpdump port http / tcpdump portrange 8080-8888
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
...
15:51:32.684946 IP webb.56630 > gw.gcableconfig.local.http: Flags [.], ack 1, win 64240, length 0
15:51:32.685142 IP webb.56630 > gw.gcableconfig.local.http: Flags [P.], seq 1:76, ack 1, win 64240, length 75: HTTP: GET / HTTP/1.1
15:51:32.685408 IP gw.gcableconfig.local.http > webb.56630: Flags [.], ack 76, win 64240, length 0
15:51:32.688526 IP gw.gcableconfig.local.http > webb.56630: Flags [P.], seq 1:2897, ack 76, win 64240, length 2896: HTTP: HTTP/1.0 200 OK
15:51:32.688618 IP webb.56630 > gw.gcableconfig.local.http: Flags [.], ack 2897, win 62780, length 0
15:51:32.696002 IP gw.gcableconfig.local.http > webb.56630: Flags [FP.], seq 2897:5303, ack 76, win 64240, length 2406: HTTP
...
17 packets captured
17 packets received by filter
0 packets dropped by kernel
```

### 过滤某个协议数据包

不能过滤应用层协议。

```bash
root@webb:~# tcpdump icmp
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
15:59:40.866349 IP webb > gw.gcableconfig.local: ICMP echo request, id 12, seq 1, length 64
15:59:40.884372 IP gw.gcableconfig.local > webb: ICMP echo reply, id 12, seq 1, length 64
15:59:41.868101 IP webb > gw.gcableconfig.local: ICMP echo request, id 12, seq 2, length 64
15:59:41.870006 IP gw.gcableconfig.local > webb: ICMP echo reply, id 12, seq 2, length 64
15:59:42.868916 IP webb > gw.gcableconfig.local: ICMP echo request, id 12, seq 3, length 64
15:59:42.874483 IP gw.gcableconfig.local > webb: ICMP echo reply, id 12, seq 3, length 64
6 packets captured
6 packets received by filter
0 packets dropped by kernel
```

## 常见参数

### -i 监听网卡

| 用法       | 说明     |
|----------|--------|
| -i ens32 | 监听某块网卡 |
| -i any   | 监听所有网卡 |

```bash
root@webb:~# tcpdump -i ens32
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:05:23.250173 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1004681477:1004681593, ack 3034329096, win 501, options [nop,nop,TS val 3746598079 ecr 548293704], length 116
16:05:23.250277 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 116, win 1056, options [nop,nop,TS val 548293754 ecr 3746598079], length 0
16:05:23.250300 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 116:152, ack 1, win 501, options [nop,nop,TS val 3746598080 ecr 548293704], length 36
...
7884 packets captured
8045 packets received by filter
161 packets dropped by kernel
```

### -w 将捕获的数据包保存到文件

```bash
root@webb:~# tcpdump -w filename.pcap
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
6 packets captured
6 packets received by filter
0 packets dropped by kernel

root@webb:~# file filename.pcap 
filename.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4 (Ethernet, capture length 262144)

root@webb:~# ls -lh filename.pcap
-rw-r--r-- 1 tcpdump tcpdump 708 Jul 19 16:11 filename.pcap
```

### -r 从文件中读取数据包

```bash
root@webb:~# tcpdump -r filename.pcap 
reading from file filename.pcap, link-type EN10MB (Ethernet)
16:11:35.279466 IP webb > gw.gcableconfig.local: ICMP echo request, id 13, seq 1, length 64
16:11:35.281550 IP gw.gcableconfig.local > webb: ICMP echo reply, id 13, seq 1, length 64
16:11:36.302235 IP webb > gw.gcableconfig.local: ICMP echo request, id 13, seq 2, length 64
16:11:36.330271 IP gw.gcableconfig.local > webb: ICMP echo reply, id 13, seq 2, length 64
16:11:37.296484 IP webb > gw.gcableconfig.local: ICMP echo request, id 13, seq 3, length 64
16:11:37.299137 IP gw.gcableconfig.local > webb: ICMP echo reply, id 13, seq 3, length 64
```

### -n 不把IP地址转化为主机名，直接显示IP地址。

```bash
root@webb:~# tcpdump -n
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:17:49.617171 IP 192.168.111.129 > 192.168.1.1: ICMP echo request, id 14, seq 1, length 64
16:17:49.619181 IP 192.168.1.1 > 192.168.111.129: ICMP echo reply, id 14, seq 1, length 64
16:17:50.703854 IP 192.168.111.129 > 192.168.1.1: ICMP echo request, id 14, seq 2, length 64
16:17:50.734139 IP 192.168.1.1 > 192.168.111.129: ICMP echo reply, id 14, seq 2, length 64
16:17:51.679695 IP 192.168.111.129 > 192.168.1.1: ICMP echo request, id 14, seq 3, length 64
16:17:51.690580 IP 192.168.1.1 > 192.168.111.129: ICMP echo reply, id 14, seq 3, length 64
6 packets captured
6 packets received by filter
0 packets dropped by kernel
```

### -nn 不把IP地址转化为主机名和不把端口号转换成协议，直接显示IP地址和端口号。

```bash
root@webb:~# tcpdump -nn
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:20:44.830171 IP 192.168.111.129.22 > 192.168.111.1.43116: Flags [P.], seq 1006131257:1006131453, ack 3034339648, win 501, options [nop,nop,TS val 3747519659 ecr 549215216], length 196
16:20:44.830292 IP 192.168.111.1.43116 > 192.168.111.129.22: Flags [.], ack 196, win 1056, options [nop,nop,TS val 549215281 ecr 3747519659], length 0
16:20:44.830478 IP 192.168.111.129.22 > 192.168.111.1.43116: Flags [P.], seq 196:584, ack 1, win 501, options [nop,nop,TS val 3747519660 ecr 549215281], length 388
16:20:44.830627 IP 192.168.111.1.43116 > 192.168.111.129.22: Flags [.], ack 584, win 1056, options [nop,nop,TS val 549215281 ecr 3747519660], length 0
...
2550 packets captured
2552 packets received by filter
0 packets dropped by kernel
```

### -N 只显示host或域名开头部分（192.ssh=192.168.1.1.ssh）。

```bash
root@webb:~# tcpdump -N
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:23:15.312084 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1006622321:1006622437, ack 3034339996, win 501, options [nop,nop,TS val 3747670141 ecr 549365686], length 116
16:23:15.312187 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 116, win 3446, options [nop,nop,TS val 549365757 ecr 3747670141], length 0
16:23:15.312200 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 116:152, ack 1, win 501, options [nop,nop,TS val 3747670142 ecr 549365686], length 36
16:23:15.312258 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 152, win 3446, options [nop,nop,TS val 549365757 ecr 3747670142], length 0
...
658 packets captured
825 packets received by filter
161 packets dropped by kernel
```

### -t 每行不输出时间

```bash
root@webb:~# tcpdump -t
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1006738817:1006738933, ack 3034340408, win 501, options [nop,nop,TS val 3747773458 ecr 549468995], length 116
IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 116, win 3446, options [nop,nop,TS val 549469070 ecr 3747773458], length 0
IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 116:152, ack 1, win 501, options [nop,nop,TS val 3747773458 ecr 549469070], length 36
IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 152, win 3446, options [nop,nop,TS val 549469070 ecr 3747773458], length 0
...
1049 packets captured
1176 packets received by filter
96 packets dropped by kernel
```

### -tt 每行输出时间戳

```bash
root@webb:~# tcpdump -tt
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
1658248690.215415 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1006906525:1006906721, ack 3034341024, win 501, options [nop,nop,TS val 3748565045 ecr 550260577], length 196
1658248690.215543 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 196, win 3446, options [nop,nop,TS val 550260641 ecr 3748565045], length 0
1658248690.249773 IP kazooie.canonical.com.http > webb.60558: Flags [P.], seq 4222572013:4222573441, ack 3325110471, win 64240, length 1428: HTTP
1658248690.249834 IP webb.60558 > kazooie.canonical.com.http: Flags [.], ack 1428, win 65535, length 0
1658248690.278397 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 196:568, ack 1, win 501, options [nop,nop,TS val 3748565108 ecr 550260641], length 372
...
1583 packets captured
1587 packets received by filter
0 packets dropped by kernel
```

### -ttt 每两行输出时间戳

```bash
root@webb:~# tcpdump -ttt
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
 00:00:00.000000 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1007674849:1007675045, ack 3034341740, win 501, options [nop,nop,TS val 3748642834 ecr 550337390], length 196
 00:00:00.000438 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 196, win 3446, options [nop,nop,TS val 550338430 ecr 3748642834], length 0
 00:00:00.008378 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 196:232, ack 1, win 501, options [nop,nop,TS val 3748642843 ecr 550338430], length 36
 00:00:00.000311 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 232, win 3446, options [nop,nop,TS val 550338439 ecr 3748642843], length 0
 ...
1063 packets captured
1068 packets received by filter
0 packets dropped by kernel
```

### -tttt 输出日期时间

```bash
root@webb:~# tcpdump -tttt
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
2022-07-19 16:41:04.792018 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1007865805:1007866001, ack 3034341928, win 501, options [nop,nop,TS val 3748739621 ecr 550435157], length 196
2022-07-19 16:41:04.792158 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 196, win 3446, options [nop,nop,TS val 550435216 ecr 3748739621], length 0
2022-07-19 16:41:04.895337 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 196:584, ack 1, win 501, options [nop,nop,TS val 3748739725 ecr 550435216], length 388
2022-07-19 16:41:04.895583 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 584, win 3446, options [nop,nop,TS val 550435319 ecr 3748739725], length 0
...
2184 packets captured
2187 packets received by filter
0 packets dropped by kernel
```

### -v 输出详细的信息（TTL，ID标识...）

```bash
root@webb:~# tcpdump -v
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:43:43.939027 IP (tos 0x10, ttl 64, id 23830, offset 0, flags [DF], proto TCP (6), length 184)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x607e (incorrect -> 0xf338), seq 1008299485:1008299617, ack 3034342512, win 501, options [nop,nop,TS val 3748898768 ecr 550594296], length 132
16:43:43.939166 IP (tos 0x0, ttl 64, id 27031, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.111.1.43116 > webb.ssh: Flags [.], cksum 0x8ddc (correct), ack 132, win 3446, options [nop,nop,TS val 550594359 ecr 3748898768], length 0
16:43:43.939182 IP (tos 0x10, ttl 64, id 23831, offset 0, flags [DF], proto TCP (6), length 184)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x607e (incorrect -> 0xa60c), seq 132:264, ack 1, win 501, options [nop,nop,TS val 3748898768 ecr 550594296], length 132
...
4815 packets captured
5238 packets received by filter
420 packets dropped by kernel
```

### -vv 显示比-v更多的信息（NFS回应包中的附加域，SMB数据包会完全解码）

```bash
root@webb:~# tcpdump -vv
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:44:22.612405 IP (tos 0x10, ttl 64, id 26920, offset 0, flags [DF], proto TCP (6), length 184)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x607e (incorrect -> 0x2bbc), seq 1009762505:1009762637, ack 3034342880, win 501, options [nop,nop,TS val 3748937442 ecr 550632388], length 132
16:44:22.613047 IP (tos 0x0, ttl 64, id 29220, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.111.1.43116 > webb.ssh: Flags [.], cksum 0x0b49 (correct), seq 1, ack 132, win 3446, options [nop,nop,TS val 550633029 ecr 3748937442], length 0
16:44:22.614194 IP (tos 0x10, ttl 64, id 26921, offset 0, flags [DF], proto TCP (6), length 184)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x607e (incorrect -> 0x546b), seq 132:264, ack 1, win 501, options [nop,nop,TS val 3748937443 ecr 550633029], length 132
...
1006 packets captured
1013 packets received by filter
0 packets dropped by kernel
```

### -vvv 显示比-vv更多的信息（telnet使用的SB，SE选项会被打印）

```bash
root@webb:~# tcpdump -vvv
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:44:59.706570 IP (tos 0x10, ttl 64, id 27541, offset 0, flags [DF], proto TCP (6), length 104)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x602e (incorrect -> 0xaad1), seq 1010065545:1010065597, ack 3034343176, win 501, options [nop,nop,TS val 3748974536 ecr 550670053], length 52
16:44:59.706685 IP (tos 0x10, ttl 64, id 27542, offset 0, flags [DF], proto TCP (6), length 168)
    webb.ssh > 192.168.111.1.43116: Flags [P.], cksum 0x606e (incorrect -> 0xf794), seq 52:168, ack 1, win 501, options [nop,nop,TS val 3748974536 ecr 550670053], length 116
16:44:59.706729 IP (tos 0x0, ttl 64, id 29641, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.111.1.43116 > webb.ssh: Flags [.], cksum 0x48e2 (correct), seq 1, ack 52, win 3446, options [nop,nop,TS val 550670120 ecr 3748974536], length 0
16:44:59.706730 IP (tos 0x0, ttl 64, id 29642, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.111.1.43116 > webb.ssh: Flags [.], cksum 0x486e (correct), seq 1, ack 168, win 3446, options [nop,nop,TS val 550670120 ecr 3748974536], length 0
...
1200 packets captured
1352 packets received by filter
151 packets dropped by kernel
```

### -c 限制数据包次数

```bash
root@webb:~# tcpdump -c 20
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
16:52:01.575503 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 1010430833:1010431029, ack 3034344492, win 501, options [nop,nop,TS val 3749396405 ecr 551091899], length 196
16:52:01.575643 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 196, win 3446, options [nop,nop,TS val 551091961 ecr 3749396405], length 0
16:52:01.630839 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 196:552, ack 1, win 501, options [nop,nop,TS val 3749396460 ecr 551091961], length 356
16:52:01.631805 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 2144:2628, ack 1, win 501, options [nop,nop,TS val 3749396461 ecr 551092017], length 484
...
16:52:01.631863 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 2628, win 3446, options [nop,nop,TS val 551092017 ecr 3749396461], length 0
16:52:01.631874 IP webb.ssh > 192.168.111.1.43116: Flags [P.], seq 2628:3128, ack 1, win 501, options [nop,nop,TS val 3749396461 ecr 551092017], length 500
16:52:01.631914 IP 192.168.111.1.43116 > webb.ssh: Flags [.], ack 3128, win 3446, options [nop,nop,TS val 551092017 ecr 3749396461], length 0
20 packets captured
24 packets received by filter
0 packets dropped by kernel
```

### -C 限制数据包文件大小

需要配合-w使用。如果数据包超过限制的大小就会生成一个新的文件用来保存数据包。

```bash
root@webb:~# tcpdump -C 1 -w abc
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
1765 packets captured
1767 packets received by filter
0 packets dropped by kernel

root@webb:~# ls -lh
total 7.7M
-rw-r--r-- 1 root root 1009K Jul 19 17:15 abc
-rw-r--r-- 1 root root  980K Jul 19 17:15 abc1
-rw-r--r-- 1 root root  993K Jul 19 17:15 abc2
-rw-r--r-- 1 root root  986K Jul 19 17:15 abc3
-rw-r--r-- 1 root root  979K Jul 19 17:16 abc4
-rw-r--r-- 1 root root  983K Jul 19 17:16 abc5
-rw-r--r-- 1 root root  990K Jul 19 17:16 abc6
-rw-r--r-- 1 root root  861K Jul 19 17:16 abc7
```

### -W 限制写入文件次数

如果限定的文件都写满了以后会从第一个文件开始写。

```bash
root@webb:~/test# tcpdump -C 1 -W 3 -w test
tcpdump: listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
^C1882 packets captured
1885 packets received by filter
0 packets dropped by kernel

root@webb:~/test# ls -lh
total 2.1M
-rw-r--r-- 1 root root  123K Jul 19 17:23 test0
-rw-r--r-- 1 root root 1011K Jul 19 17:23 test1
-rw-r--r-- 1 root root  979K Jul 19 17:23 test2
```

### -Q 选择输入或输出方向的数据

参数：in out inout。

+ 输入（下载）

```bash
root@webb:~# tcpdump -Q in
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
17:58:53.985215 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 3183598209, win 648, options [nop,nop,TS val 555104219 ecr 2635667664], length 0
17:58:54.021439 IP _gateway.domain > webb.51745: 10421 NXDomain 0/0/0 (46)
17:58:54.060765 IP _gateway.domain > webb.41578: 23900 NXDomain 0/0/0 (44)
17:58:54.061448 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 197, win 648, options [nop,nop,TS val 555104295 ecr 2635667740], length 0
17:58:54.072872 IP 192.168.111.1.34450 > 239.255.255.250.1900: UDP, length 173
17:58:54.109763 IP _gateway.domain > webb.40561: 33080 NXDomain 0/0/0 (44)
...
12057 packets captured
24298 packets received by filter
155 packets dropped by kernel
```

+ 输出（上传）

```bash
root@webb:~# tcpdump -Q out
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
18:00:37.877483 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 3185769101:3185769217, ack 36164978, win 501, options [nop,nop,TS val 2635771556 ecr 555208041], length 116
18:00:37.877730 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 116:152, ack 1, win 501, options [nop,nop,TS val 2635771557 ecr 555208108], length 36
18:00:37.877873 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 152:268, ack 1, win 501, options [nop,nop,TS val 2635771557 ecr 555208108], length 116
18:00:37.877985 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 268:304, ack 1, win 501, options [nop,nop,TS val 2635771557 ecr 555208108], length 36
18:00:37.878707 IP webb.49487 > _gateway.domain: 20848+ PTR? 1.111.168.192.in-addr.arpa. (44)
...
1636 packets captured
3248 packets received by filter
9 packets dropped by kernel
```

### -q 显示简洁的信息

```bash
root@webb:~# tcpdump -q 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
18:11:47.861165 IP webb.ssh > 192.168.111.1.43848: tcp 196
18:11:47.861317 IP 192.168.111.1.43848 > webb.ssh: tcp 0
18:11:47.862202 IP webb.51236 > _gateway.domain: UDP, length 44
18:11:47.883263 IP _gateway.domain > webb.51236: UDP, length 44
18:11:47.884388 IP webb.60867 > _gateway.domain: UDP, length 46
18:11:47.913492 IP _gateway.domain > webb.60867: UDP, length 46
18:11:47.914114 IP webb.ssh > 192.168.111.1.43848: tcp 164
18:11:47.914307 IP 192.168.111.1.43848 > webb.ssh: tcp 0
18:11:47.914471 IP webb.36870 > _gateway.domain: UDP, length 44
18:11:47.943661 IP _gateway.domain > webb.36870: UDP, length 44
18:11:47.945832 IP webb.ssh > 192.168.111.1.43848: tcp 308
...
1662 packets captured
1662 packets received by filter
0 packets dropped by kernel
```

### -D 显示可以用的网卡

```bash
root@webb:~# tcpdump -D
1.ens32 [Up, Running]
2.docker0 [Up, Running]
3.veth718d71e [Up, Running]
4.lo [Up, Running, Loopback]
5.any (Pseudo-device that captures on all interfaces) [Up, Running]
6.br-2c5c2f3c07b0 [Up]
7.br-8176041516e6 [Up]
8.br-78e4c3004a16 [Up]
9.br-653aa4c4ca24 [Up]
10.br-0b50877f0c80 [Up]
11.br-4a5364f0c91e [Up]
12.br-65553a1d414f [Up]
13.br-b2f360fdd976 [Up]
14.br-388fed18724d [Up]
15.br-67a000c23451 [Up]
16.br-4c01ac860111 [Up]
17.br-aa15e7478112 [Up]
18.bluetooth-monitor (Bluetooth Linux Monitor) [none]
19.nflog (Linux netfilter log (NFLOG) interface) [none]
20.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
```

### -L 显示某个网卡已只数据链路信息

```bash
root@webb:~# tcpdump -L
Data link types for ens32 (use option -y to set):
  EN10MB (Ethernet)
  DOCSIS (DOCSIS) (printing not supported)
```

### -S 指定每个数据包的长度

如果超过设定的大小限制，数据包会被截断，而每行会出现[|协议]这种标识。

```bash
root@webb:~# tcpdump -s 50
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 50 bytes
03:25:19.730692 IP webb.ssh > 192.168.111.1.43848: [|tcp]
03:25:19.730844 IP 192.168.111.1.43848 > webb.ssh: [|tcp]
03:25:19.730854 IP webb.ssh > 192.168.111.1.43848: [|tcp]
03:25:19.730912 IP 192.168.111.1.43848 > webb.ssh: [|tcp]
03:25:19.730949 IP webb.ssh > 192.168.111.1.43848: [|tcp]
03:25:19.731011 IP 192.168.111.1.43848 > webb.ssh: [|tcp]
03:25:19.731511 IP webb.53133 > _gateway.domain: [|domain]
03:25:19.758616 IP _gateway.domain > webb.53133: [|domain]
...
2331 packets captured
2333 packets received by filter
0 packets dropped by kernel
```

### -A 数据包的信息用ASCII字符串显示

```bash
root@webb:~# tcpdump -A 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:29:41.935419 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 3187380677:3187380873, ack 36172546, win 501, options [nop,nop,TS val 2637269641 ecr 556706107], length 196
E.....@.@..)..o...o....H.....'......`......
.1..!..;.....;.....y.X........E...}+..O1..rA.L..[       .6...<p..*.........f?'CZ...N..;...).X.Su;.`......ZNX...:.RDxJ..>D.uZ8...!...A./.......V.?......j9....)......U.X6..B>E=e..[^....X..'].>M..&.d...&.....m....
03:29:41.935548 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 196, win 12278, options [nop,nop,TS val 556706161 ecr 2637269641], length 0
E..4\.@.@.~9..o...o..H...'......../........
!..q.1..
03:29:41.936377 IP webb.33513 > _gateway.domain: 6457+ PTR? 1.111.168.192.in-addr.arpa. (44)
E..H6.@.@.....o...o....5.4`..9...........1.111.168.192.in-addr.arpa.....
3 packets captured
13 packets received by filter
0 packets dropped by kernel
```

### -X 数据包的信息用十六进制和ASCII字符串显示

```bash
root@webb:~# tcpdump -X 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:32:53.459045 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 3187382901:3187383097, ack 36173466, win 501, options [nop,nop,TS val 2637461164 ecr 556897624], length 196
        0x0000:  4510 00f8 2f0f 4000 4006 ab0d c0a8 6f81  E.../.@.@.....o.
        0x0010:  c0a8 6f01 0016 ab48 bdfb 9a75 0227 f69a  ..o....H...u.'..
        0x0020:  8018 01f5 60be 0000 0101 080a 9d34 76ac  ....`........4v.
        0x0030:  2131 9558 0000 00b0 cce9 d735 79ce 5a55  !1.X.......5y.ZU
        0x0040:  9f4b 8734 e72f d4f1 a980 261f f5b6 c1bb  .K.4./....&.....
        0x0050:  0294 1623 1ddf 77ff 9a52 1714 8b60 6206  ...#..w..R...`b.
        0x0060:  50ef e4d4 3eb3 1a99 dff8 8eaf 6edd 8d70  P...>.......n..p
        0x0070:  ed4c 257d 2837 cd81 449c f9ea 2569 6fa5  .L%}(7..D...%io.
        0x0080:  f6df 019c c77c 0b5c cf27 662f 787f 53c3  .....|.\.'f/x.S.
        0x0090:  64e0 239f cd91 8d20 bd18 a5b0 76eb 0750  d.#.........v..P
        0x00a0:  190f 1c22 7330 d317 3d68 7068 4cb6 767c  ..."s0..=hphL.v|
        0x00b0:  11b4 4a41 0547 1411 f6b1 8783 9dc3 ec63  ..JA.G.........c
        0x00c0:  38f1 00fb c79b 4bec c243 e609 944e f0e2  8.....K..C...N..
        0x00d0:  f765 3af6 428f 4773 9d1c b8e4 37aa 279f  .e:.B.Gs....7.'.
        0x00e0:  a9d5 06fd 9d62 49aa c640 7fbd 1c6c 2035  .....bI..@...l.5
        0x00f0:  bbb3 4abf 1a3c b009                      ..J..<..
03:32:53.459186 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 196, win 12293, options [nop,nop,TS val 556897684 ecr 2637461164], length 0
        0x0000:  4500 0034 5ce9 4000 4006 7e07 c0a8 6f01  E..4\.@.@.~...o.
        0x0010:  c0a8 6f81 ab48 0016 0227 f69a bdfb 9b39  ..o..H...'.....9
        0x0020:  8010 3005 1ee8 0000 0101 080a 2131 9594  ..0.........!1..
        0x0030:  9d34 76ac                                .4v.
03:32:53.459848 IP webb.46115 > _gateway.domain: 9560+ PTR? 1.111.168.192.in-addr.arpa. (44)
        0x0000:  4500 0048 722f 4000 4011 68a1 c0a8 6f81  E..Hr/@.@.h...o.
        0x0010:  c0a8 6f02 b423 0035 0034 601a 2558 0100  ..o..#.5.4`.%X..
        0x0020:  0001 0000 0000 0000 0131 0331 3131 0331  .........1.111.1
        0x0030:  3638 0331 3932 0769 6e2d 6164 6472 0461  68.192.in-addr.a
        0x0040:  7270 6100 000c 0001                      rpa.....
3 packets captured
15 packets received by filter
0 packets dropped by kernel
```

### -e 显示数据链路层的头部信息（MAC地址、VLAN tag信息）

```bash
root@webb:~# tcpdump -e 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:36:06.230854 00:0c:29:6b:19:3c (oui Unknown) > 00:50:56:c0:00:08 (oui Unknown), ethertype IPv4 (0x0800), length 262: webb.ssh > 192.168.111.1.43848: Flags [P.], seq 3187386301:3187386497, ack 36174278, win 501, options [nop,nop,TS val 2637653936 ecr 557090387], length 196
03:36:06.230950 00:50:56:c0:00:08 (oui Unknown) > 00:0c:29:6b:19:3c (oui Unknown), ethertype IPv4 (0x0800), length 66: 192.168.111.1.43848 > webb.ssh: Flags [.], ack 196, win 12315, options [nop,nop,TS val 557090454 ecr 2637653936], length 0
03:36:06.231603 00:0c:29:6b:19:3c (oui Unknown) > 00:50:56:f3:ae:21 (oui Unknown), ethertype IPv4 (0x0800), length 86: webb.40574 > _gateway.domain: 36789+ PTR? 1.111.168.192.in-addr.arpa. (44)
3 packets captured
10 packets received by filter
0 packets dropped by kernel
```

### -F 使用文件中的过滤规则

```bash
root@webb:~# cat filter_rule 
tcp port 80

root@webb:~# tcpdump -F filter_rule -i ens32
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:43:06.217104 IP webb.43088 > 120.41.45.103.http: Flags [S], seq 1123306280, win 64240, options [mss 1460,sackOK,TS val 278585417 ecr 0,nop,wscale 7], length 0
03:43:06.246672 IP 120.41.45.103.http > webb.43088: Flags [S.], seq 2338310987, ack 1123306281, win 64240, options [mss 1460], length 0
03:43:06.246733 IP webb.43088 > 120.41.45.103.http: Flags [.], ack 1, win 64240, length 0
03:43:06.246819 IP webb.43088 > 120.41.45.103.http: Flags [P.], seq 1:75, ack 1, win 64240, length 74: HTTP: GET /VersionControl.dat HTTP/1.1
03:43:06.246999 IP 120.41.45.103.http > webb.43088: Flags [.], ack 75, win 64240, length 0
03:43:06.286821 IP 120.41.45.103.http > webb.43088: Flags [P.], seq 1:1049, ack 75, win 64240, length 1048: HTTP: HTTP/1.1 200 OK
03:43:06.286897 IP webb.43088 > 120.41.45.103.http: Flags [.], ack 1049, win 63928, length 0
...
14 packets captured
18 packets received by filter
0 packets dropped by kernel
```

### -l 对标准输出进行行缓冲（标准输出遇到一个换行符就马上把这行的内容打印出来）。

```bash
root@webb:~# tcpdump -l | tee data
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:49:41.850910 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 3188656729:3188656925, ack 36189394, win 501, options [nop,nop,TS val 2638469556 ecr 557905972], length 196
03:49:41.851026 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 196, win 12791, options [nop,nop,TS val 557906030 ecr 2638469556], length 0
03:49:42.839861 IP webb.51943 > _gateway.domain: 14084+ PTR? 1.111.168.192.in-addr.arpa. (44)
03:49:42.865887 IP _gateway.domain > webb.51943: 14084 NXDomain 0/0/0 (44)
03:49:42.868939 IP webb.50062 > _gateway.domain: 8041+ PTR? 129.111.168.192.in-addr.arpa. (46)
03:49:42.897498 IP _gateway.domain > webb.50062: 8041 NXDomain 0/0/0 (46)
03:49:42.900392 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 196:552, ack 1, win 501, options [nop,nop,TS val 2638470606 ecr 557906030], length 356
03:49:42.901005 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 552, win 12791, options [nop,nop,TS val 557907080 ecr 2638470606], length 0
03:49:42.902050 IP webb.ssh > 192.168.111.1.43848: Flags [P.], seq 552:588, ack 1, win 501, options [nop,nop,TS val 2638470607 ecr 557907080], length 36
03:49:42.902402 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 588, win 12791, options [nop,nop,TS val 557907082 ecr 2638470607], length 0
10 packets captured
17 packets received by filter
0 packets dropped by kernel
```

## 逻辑运算符

### and/&& 满足所有条件

```bash
root@webb:~# tcpdump src 192.168.1.1 and port 80
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:53:43.308323 IP gw.gcableconfig.local.http > webb.49650: Flags [S.], seq 1487312683, ack 4054784189, win 64240, options [mss 1460], length 0
03:53:43.308918 IP gw.gcableconfig.local.http > webb.49650: Flags [.], ack 76, win 64240, length 0
03:53:43.315012 IP gw.gcableconfig.local.http > webb.49650: Flags [P.], seq 1:2897, ack 76, win 64240, length 2896: HTTP: HTTP/1.0 200 OK
03:53:43.321661 IP gw.gcableconfig.local.http > webb.49650: Flags [FP.], seq 2897:5303, ack 76, win 64240, length 2406: HTTP
03:53:43.322855 IP gw.gcableconfig.local.http > webb.49650: Flags [.], ack 77, win 64239, length 0
5 packets captured
5 packets received by filter
0 packets dropped by kernel
```

### or/|| 只满足一个条件

```bash
root@webb:~# tcpdump tcp port 53 or udp port 53
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:54:53.621757 IP webb.56996 > _gateway.domain: 36025+ A? webb-l.top. (28)
03:54:53.623207 IP webb.47043 > _gateway.domain: 12683+ AAAA? webb-l.top. (28)
03:54:53.627082 IP webb.59208 > _gateway.domain: 45958+ PTR? 2.111.168.192.in-addr.arpa. (44)
03:54:53.680540 IP _gateway.domain > webb.59208: 45958 NXDomain 0/0/0 (44)
03:54:53.681699 IP webb.36977 > _gateway.domain: 37135+ PTR? 129.111.168.192.in-addr.arpa. (46)
03:54:53.711146 IP _gateway.domain > webb.36977: 37135 NXDomain 0/0/0 (46)
03:54:53.711433 IP _gateway.domain > webb.56996: 36025 5/0/0 CNAME webb-l.github.io., A 185.199.109.153, A 185.199.111.153, A 185.199.110.153, A 185.199.108.153 (122)
03:54:53.711433 IP _gateway.domain > webb.47043: 12683 5/0/0 CNAME webb-l.github.io., AAAA 2606:50c0:8000::153, AAAA 2606:50c0:8003::153, AAAA 2606:50c0:8002::153, AAAA 2606:50c0:8001::153 (170)
03:54:53.741361 IP webb.52080 > _gateway.domain: 4388+ PTR? 153.108.199.185.in-addr.arpa. (46)
03:54:53.789243 IP _gateway.domain > webb.52080: 4388 1/0/0 PTR cdn-185-199-108-153.github.com. (90)
10 packets captured
10 packets received by filter
0 packets dropped by kernel
```

### not/! 取反

```bash
root@webb:~# tcpdump not tcp port 22
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:55:30.206918 IP webb > cdn-185-199-108-153.github.com: ICMP echo request, id 1, seq 37, length 64
03:55:30.209607 IP webb.54773 > _gateway.domain: 56005+ PTR? 153.108.199.185.in-addr.arpa. (46)
03:55:30.230574 IP cdn-185-199-108-153.github.com > webb: ICMP echo reply, id 1, seq 37, length 64
03:55:30.239947 IP _gateway.domain > webb.54773: 56005 1/0/0 PTR cdn-185-199-108-153.github.com. (90)
03:55:30.242390 IP webb.37901 > _gateway.domain: 6529+ PTR? 129.111.168.192.in-addr.arpa. (46)
03:55:30.270067 IP _gateway.domain > webb.37901: 6529 NXDomain 0/0/0 (46)
03:55:30.273460 IP webb.47470 > _gateway.domain: 3405+ PTR? 2.111.168.192.in-addr.arpa. (44)
03:55:30.299833 IP _gateway.domain > webb.47470: 3405 NXDomain 0/0/0 (44)
8 packets captured
8 packets received by filter
0 packets dropped by kernel
```

### 多个逻辑运算符组合

```bash
root@webb:~# tcpdump "src 192.168.1.1 and (tcp port 80 or icmp)"
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
03:59:10.994485 IP gw.gcableconfig.local > webb: ICMP echo reply, id 4, seq 1, length 64
03:59:14.370158 IP gw.gcableconfig.local.http > webb.49676: Flags [S.], seq 1740528049, ack 465635601, win 64240, options [mss 1460], length 0
03:59:14.370455 IP gw.gcableconfig.local.http > webb.49676: Flags [.], ack 76, win 64240, length 0
03:59:14.383240 IP gw.gcableconfig.local.http > webb.49676: Flags [P.], seq 1:2897, ack 76, win 64240, length 2896: HTTP: HTTP/1.0 200 OK
03:59:14.390582 IP gw.gcableconfig.local.http > webb.49676: Flags [FP.], seq 2897:5303, ack 76, win 64240, length 2406: HTTP
03:59:14.391307 IP gw.gcableconfig.local.http > webb.49676: Flags [.], ack 77, win 64239, length 0
03:59:20.560951 IP gw.gcableconfig.local > webb: ICMP echo reply, id 5, seq 1, length 64
03:59:21.561663 IP gw.gcableconfig.local > webb: ICMP echo reply, id 5, seq 2, length 64
8 packets captured
8 packets received by filter
0 packets dropped by kernel
```

## 高级过滤

### less 抓取数据包小于xx bytes的数据包

```bash
root@webb:~# tcpdump less 100 -c 3 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
04:02:33.290867 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 3189235577, win 12881, options [nop,nop,TS val 558677448 ecr 2639240996], length 0
04:02:33.290920 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 153, win 12881, options [nop,nop,TS val 558677448 ecr 2639240996], length 0
04:02:33.291073 IP 192.168.111.1.43848 > webb.ssh: Flags [.], ack 189, win 12881, options [nop,nop,TS val 558677449 ecr 2639240996], length 0
3 packets captured
12 packets received by filter
0 packets dropped by kernel
```

### greater 抓取数据包大于xx bytes的数据包

```bash
root@webb:~# tcpdump greater 1000 -c 3
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens32, link-type EN10MB (Ethernet), capture size 262144 bytes
04:04:02.375412 IP gw.gcableconfig.local.http > webb.49690: Flags [P.], seq 2285946385:2285949281, ack 3210337588, win 64240, length 2896: HTTP: HTTP/1.0 200 OK
04:04:02.376102 IP webb.ssh > 192.168.111.1.38728: Flags [P.], seq 1635248512:1635251300, ack 1337887836, win 501, options [nop,nop,TS val 2639330081 ecr 558766503], length 2788
04:04:02.380943 IP gw.gcableconfig.local.http > webb.49690: Flags [FP.], seq 2896:5302, ack 1, win 64240, length 2406: HTTP
3 packets captured
4 packets received by filter
0 packets dropped by kernel
```