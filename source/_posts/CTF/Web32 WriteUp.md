---
title: Web33 WriteUp 
date: 2021-05-26 
tags: CTF,Web33,WriteUp
---

### 题目

![index.png](../../images/Web32%20WriteUp/index.png)

### 加密代码：

```php
<?php
 function encrypt(data,key)
 {
     key = md5('ISCC');x = 0;
     len = strlen(data);
     klen = strlen(key);
     for (i=0;i < len;i++) { 
         if (x ==klen)
        {
            x = 0;
        }char .= key[x];
        x+=1;
    }
    for (i=0; i<len; i++) {str .= chr((ord(data[i]) + ord(char[i])) % 128);
    }
    return base64_encode($str);
 }
?>
```

+ `$key = md5('ISCC');` 这里的key=729623334f0aa2784a1599fd374c120d。

```php
for (i=0;i < len;i++) { 
    if (x ==klen)
     {
         x = 0;
     }char .= key[x];
     x+=1;
} // 根据用户传入的data长度执行循环，并且根据x的值在key提取字符串给$char。
```

+ 上面的代码都不是重点。

```php
for (i=0;i < len;i++) {
    str .= chr((ord(data[i]) + ord(char[$i])) % 128);
} // 这里的for循环和上方的for循环一模一样，主要需要我们解密的代码在18行。
```

+ 根据i为索引在data中提取出一个字符然后转换为int型，在加上char中提取出来的int型数据余上128，然后再将得出来的结果转换成char类型并拼接到str中。

**例如：**

```php
'a'=97, '7'=55
chr((ord('a') + ord('7')) % 128)
chr((97 + 55) % 128)
chr((97 + 55) % 128)
chr((97 + 55) % 128)
chr(152%128)
chr(24)
24='取消'
```

+ 为什么要余上128?
  + 答：因为ascii表一共只有128个，如果超出去无法让int型转化成char型。
+ `20| return base64_encode($str);` 将循环结束后的结果进行base64编码并返回给用户。

### 解密代码：

```php
<?php
 decrypt("GA==");
 function decrypt(data)
 {data = base64_decode(data);key = md5('ISCC');
     x = 0;len = strlen(data);klen = strlen(key);
    for (i = 0; i<len; i++) {
        if (x == klen) {x = 0;
        }
        char .=key[x];x += 1;
    }
    for (i = 0;i < len;i++) {
        result = ord(data[i]) - ord(char[i]);
           if (result >= 0) {
           str .= chr(result);
        } else {
           str .= chr(result + 128);
        }
    }
    echo "str = $str<br>";
}
```

+ `5| data = base64_decode(data);` 先把base64加密的数据进行解密，解密出来的int型数据为24，char类型数据为’取消’。
+ `6| $key = md5('ISCC');` 这里和加解密的key都是一样的。key=729623334f0aa2784a1599fd374c120d
+ 其他的代码都是和加密代码一致。
+ 重点在这里。

```php
$result = ord($data[$i]) - ord($char[$i]);
 if ($result >= 0) {
   $str .= chr($result);
} else {
   $str .= chr($result + 128);
}
```

> 第20行用data[i]-char[i]=原来的数，如果>=0那么就可以直接输出字符，如果<=0的话在ASCII表中是不存在所有我们需要加上128。
> 
> data[i]=’取消’=24，char[i]=’7’=55。
> 
> 第21行判断result是否大于等于0，如果大于等于零就跳转到第22行，如果不是就跳转到第24行。
> 
> 第22行将result变量的值转换成char型并字符串拼接到str中。
> 
> 第24行将result变量值小于0的值+128变成ASCII表中存在的数值，并将int型的数据转换成char型数据并把字符串拼接到str中。
> 
> 这里的result=-31很明显它是要跳转到第24行。
> 
> 结果：-32+128=97=‘a’
> 

+ 把GA替换成题目中的描述就可以得到flag。