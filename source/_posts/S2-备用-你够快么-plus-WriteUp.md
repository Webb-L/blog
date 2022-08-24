---
title: S2-备用-你够快么-plus WriteUp
date: 2022-08-24 19:58:37
tags:
---

## 题目

+ 从下方的代码可以看出需要我们计算结果发送过去。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>给你一秒你把握住</title>
</head>
<body>
<div style="text-align: center;">听说你<a style="color: green">能力堪比人形计算机</a>？</div>
<div style="text-align: center;">那你<a style="color: blue">肯定</a>能把这个式子算出来发给我吧？</div>
<div style="text-align: center;">4175 + 4391 = ?</div>
<div style="text-align: center;"><form method="post" action="">
    <input type="text" name="math_result"/>
    <input type="submit" value='冲了'>
</form></div>
<div style="text-align: center;"></div>
<div style="text-align: center;"><a href=".">我不服，再来</a></div>

<!--
手速不够可以用工具：
import requests
import re
-->
</body>
</html>
```

## 测试

+ 尝试发送正确答案。

+ 下方的结果不能正确响应应该是我们缺少参数，尝试加入Cookie。

```bash
┌──(kali㉿kali)-[~]
└─$ curl -d "math_result=8566" http://你的靶机ID.ctf.nynusec.com/  
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>500 Internal Server Error</title>
<h1>Internal Server Error</h1>
<p>The server encountered an internal error and was unable to complete your request. Either the server is overloaded or there is an error in the application.</p>
```

+ 能够正常请求但是提示我们答案错误。

```bash
┌──(kali㉿kali)-[~]
└─$ curl -d "math_result=答案" -b="session=Cookie" http://你的靶机ID.ctf.nynusec.com/ 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>给你一秒你把握住</title>
</head>
<body>
<div style="text-align: center;">听说你<a style="color: green">能力堪比人形计算机</a>？</div>
<div style="text-align: center;">那你<a style="color: blue">肯定</a>能把这个式子算出来发给我吧？</div>
<div style="text-align: center;"></div>
<div style="text-align: center;"><form method="post" action="">
    <input type="text" name="math_result"/>
    <input type="submit" value='冲了'>
</form></div>
<div style="text-align: center;">输入的不对啊</div>
<div style="text-align: center;"><a href=".">我不服，再来</a></div>

<!--
手速不够可以用工具：
import requests
import re
-->
</body>
</html>             
```

+ 重新请求靶机的时候发现cookie已经改变。接下来应该需要使用Python。

```bash
┌──(kali㉿kali)-[~]
└─$ curl -v http://你的靶机ID.ctf.nynusec.com/
...
< Set-Cookie: session=eyJjdGltZSI6MTY2MTM0MzE2NSwibWF0aCI6IjE0ODAxIn0.YwYVvQ.3Xzezu9mm7lINTHOaRwp5eblpQw; HttpOnly; Path=/
...
<div style="text-align: center;">7786 + 7015 = ?</div>
...                                                
```

+ 编写Python代码。

```python
import requests
import re

url = "http://你的靶机ID.ctf.nynusec.com/"
// 请求靶机题目和Cookie。
response = requests.get(url)
// 使用正则匹配题目。
topic = re.findall("(\d{2,} . \d{2,})", response.text)[0]
// 计算出答案。
answer = eval(topic)
// 将答案和Cookie发送到靶机。
result = requests.post(
    url,
    data={
        "math_result": answer
    },
    headers={
        "Cookie": "session=" + response.cookies.get("session"),
    }
)
// 匹配flag
flag = re.findall("(flag\{.+\})", result.text)[0]
print(flag)
```

+ 运行脚本

```bash
┌──(kali㉿kali)-[~]
└─$ python main.py 
flag{b281b684-****-****-****-ecb2fce81334}
```
