---
title: 解决wpscan找不到webrick错误
date: 2022-04-15 11:55:21
tags: wpscan
---

## 错误信息：

```bash
┌──(webb㉿kali)-[~]
└─$ wpscan                                                                                                                                                                                                                             127 ⨯
/usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1401:in `rescue in block in activate_dependencies': Could not find 'webrick' (>= 0) among 222 total gem(s) (Gem::MissingSpecError)
Checked in 'GEM_PATH=/home/webb/.local/share/gem/ruby/3.0.0:/var/lib/gems/3.0.0:/usr/local/lib/ruby/gems/3.0.0:/usr/lib/ruby/gems/3.0.0:/usr/lib/x86_64-linux-gnu/ruby/gems/3.0.0:/usr/share/rubygems-integration/3.0.0:/usr/share/rubygems-integration/all:/usr/lib/x86_64-linux-gnu/rubygems-integration/3.0.0' at: /usr/share/rubygems-integration/all/specifications/xmlrpc-0.3.2.gemspec, execute `gem env` for more information
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1398:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1405:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1405:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:286:in `block in activate_bin_path'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:285:in `synchronize'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:285:in `activate_bin_path'
        from /usr/bin/wpscan:25:in `<main>'
/usr/lib/ruby/vendor_ruby/rubygems/dependency.rb:311:in `to_specs': Could not find 'webrick' (>= 0) among 222 total gem(s) (Gem::MissingSpecError)
Checked in 'GEM_PATH=/home/webb/.local/share/gem/ruby/3.0.0:/var/lib/gems/3.0.0:/usr/local/lib/ruby/gems/3.0.0:/usr/lib/ruby/gems/3.0.0:/usr/lib/x86_64-linux-gnu/ruby/gems/3.0.0:/usr/share/rubygems-integration/3.0.0:/usr/share/rubygems-integration/all:/usr/lib/x86_64-linux-gnu/rubygems-integration/3.0.0' , execute `gem env` for more information
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1399:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1405:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1405:in `block in activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `each'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1387:in `activate_dependencies'
        from /usr/lib/ruby/vendor_ruby/rubygems/specification.rb:1369:in `activate'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:286:in `block in activate_bin_path'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:285:in `synchronize'
        from /usr/lib/ruby/vendor_ruby/rubygems.rb:285:in `activate_bin_path'
        from /usr/bin/wpscan:25:in `<main>'
```

+ 这里我们只需要找出关键信息然后再尝试解决即可。

+ 从上面的内容我们可以看到“ Could not find 'webrick' (>= 0) among 222 total gem(s) (Gem::MissingSpecError)”这句话打开的意思是找不到**webrick**。

## 解决：

+ 使用**gem**安装**webrick**。

```bash
sudo gem install webrick
```

+ 再次运行**wpscan**。

```bash
┌──(webb㉿kali)-[~]
└─$ wpscan                  
One of the following options is required: --url, --update, --help, --hh, --version

Please use --help/-h for the list of available options.
```

## 补充知识：

+ **gem**是什么？
  
  + **gem**是**Ruby**模块的包管理器。
