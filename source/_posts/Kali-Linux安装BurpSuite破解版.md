---
title: Kali Linux安装BurpSuite破解版
date: 2022-04-30 23:42:53
tags:
---

## 解压BurpSuite破解版。

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ unzip BurpSuite\ v2022.3.6专业破解版.zip 
Archive:  BurpSuite v2022.3.6专业破解版.zip
  inflating: BurpCrack.jar           
  inflating: BurpSuite.ico           
  inflating: burpsuite_pro_v2022.3.6.jar  
  inflating: BurpSuiteLoader.bat     
  inflating: BurpSuiteLoader.sh      
  inflating: BurpSuiteLoader.vbs     
  inflating: BurpSuiteLoader_v2022.3.6.jar  
  inflating: 创建桌面快捷方式.bat  
 extracting: 建议：JAVA15环境运行.txt  
  inflating: 微信公众号雾晓安全.jpg  
  inflating: 想获取更多安全资源.html  
  inflating: 最新BurpSuite汉化破解版说明.pdf  
  inflating: 网络安全攻防Clud.jpg  
  inflating: 网络安全攻防WiKi.jpg  

```

## 安装和破解

### 安装

+ 因为Kali Linux默认安装了BurpSuite，我们就不需要再次安装了。
+ 如果没有安装就运行`sudo apt install burpsuite`命令进行安装。

### 破解

+ 查询BurpSuite的安装位置。
```BASH
┌──(webb㉿kali)-[~/temp]
└─$ whereis burpsuite                                                                
burpsuite: /usr/bin/burpsuite /usr/share/burpsuite
```

+ 查看上面两条记录的信息。
```BASH
┌──(webb㉿kali)-[~/temp]
└─$ ll /usr/bin/burpsuite
-rwxr-xr-x 1 root root 184 12月 13 23:50 /usr/bin/burpsuite
                                                                                            
┌──(webb㉿kali)-[~/temp]
└─$ ll /usr/share/burpsuite
总用量 193304
-rw-r--r-- 1 root root 197937091 12月 13 23:50 burpsuite.jar
```

+ 查看第一个文件信息。

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ cat /usr/bin/burpsuite 
#!/usr/bin/env sh

set -e

export JAVA_CMD=java

# Include the wrappers utility script
. /usr/lib/java-wrappers/java-wrappers.sh

run_java -jar /usr/share/burpsuite/burpsuite.jar "$@"
```
+ 修改`BurpSuiteLoader.sh`的权限。

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ chmod +x BurpSuiteLoader.sh 
```

+ 将我们破解文件移动到`/usr/share/burpsuite`。

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ sudo mv * /usr/share/burpsuite/
```

+ 修改`/usr/bin/burpsuite`文件的内容。

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ sudo vim /usr/bin/burpsuite

#!/usr/bin/env sh

set -e

cd /usr/share/burpsuite/

./BurpSuiteLoader.sh
```

## 启动

```BASH
┌──(webb㉿kali)-[~/temp]
└─$ burpsuite
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Your JRE appears to be version 11.0.14.1 from Debian
Burp has not been fully tested on this platform and you may experience problems.
    ____                      _____       _ __          __    ____            __         
   / __ )__  ___________     / ___/__  __(_) /____     / /   / __ \____ _____/ /__  _____
  / __  / / / / ___/ __ \    \__ \/ / / / / __/ _ \   / /   / / / / __ `/ __  / _ \/ ___/
 / /_/ / /_/ / /  / /_/ /   ___/ / /_/ / / /_/  __/  / /___/ /_/ / /_/ / /_/ /  __/ /    
/_____/\__,_/_/  / .___/   /____/\__,_/_/\__/\___/  /_____/\____/\__,_/\__,_/\___/_/     
                /_/                                                                      
Github:https://github.com/x-Ai/BurpSuiteLoader 商业使用请购买正版软件！
```
