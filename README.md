# HTTPSQS

HTTPSQS is based on HTTP GET/POST protocol open source simple message queue service, Use Tokyo Cabinet's B+Tree Key/Value database to do data persistence storage。

## Prepare

- openssl: OpenSSL 1.1.1f  31 Mar 2020
- libevent: [2.1.12-stable](https://github.com/libevent/libevent/releases/tag/release-2.1.12-stable)
- tokyo cabinet: [1.4.48](https://github.com/hthetiot/Tokyo-Cabinet)
### openssl
Type `openssl version` in your linux operator system to make sure the version is 1.1.1f, openssl is installed in every linux system


### libevent

```shell
wget https://github.com/libevent/libevent/releases/download/release-2.1.12-stable/libevent-2.1.12-stable.tar.gz
tar tar zxvf libevent-2.0.12-stable.tar.gz
cd tar zxvf libevent-2.0.12-stable
./autegen.sh
./configure
make
make install
```

### toyko cabinet

```shell
git clone https://github.com/hthetiot/Tokyo-Cabinet
cd Tokyo-Cabinet
./configure
make
make install
```

## ln tokyo cabinet

```shell
ln -s /usr/local/tokyocabinet-1.4.48/lib/libtokyocabinet.so.9 /usr/lib/libtokyocabinet.so.9  
```

## httpsqs

```shell
git clone https://github.com/WangTingZheng/httpsqs
make 
make install
```

## check

```shell
httpsqs -h
```
if show this, all done:
```shell
--------------------------------------------------------------------------------------------------
HTTP Simple Queue Service - httpsqs v1.7 (April 14, 2011)

Author: Zhang Yan (http://blog.s135.com), E-mail: net@s135.com
This is free software, and you are welcome to modify and redistribute it under the New BSD License

-l <ip_addr>  interface to listen on, default is 0.0.0.0
-p <num>      TCP port number to listen on (default: 1218)
-x <path>     database directory (example: /opt/httpsqs/data)
-t <second>   keep-alive timeout for an http request (default: 60)
-s <second>   the interval to sync updated contents to the disk (default: 5)
-c <num>      the maximum number of non-leaf nodes to be cached (default: 1024)
-m <size>     database memory cache size in MB (default: 100)
-i <file>     save PID in <file> (default: /tmp/httpsqs.pid)
-a <auth>     the auth password to access httpsqs (example: mypass123)
-d            run as a daemon
-h            print this help and exit

Use command "killall httpsqs", "pkill httpsqs" and "kill `cat /tmp/httpsqs.pid`" to stop httpsqs.
Please note that don't use the command "pkill -9 httpsqs" and "kill -9 PID of httpsqs"!

Please visit "http://code.google.com/p/httpsqs" for more help information.

--------------------------------------------------------------------------------------------------
```

## usage

### start httpsqs service

```shell
ulimit -SHn 65535
httpsqs -d -p 1218 -x /data0/queue
```

### put data

put `你好，https` into queue
```shell
GET: curl "http://localhost:1218/?name=your_queue_name&opt=put&data=%E4%BD%A0%E5%A5%BD%EF%BC%8Chttpsqs&auth=mypass123"
POST: curl -d "%E4%BD%A0%E5%A5%BD%EF%BC%8Chttpsqs" "http://localhost:1218/?name=your_queue_name&opt=put&auth=mypass123"
```
if put successfully, will get `HTTPSQS_PUT_OK`
### get data

get `你好，httpsqs` into queue
```shell
GET: curl "http://localhost:1218/?charset=utf-8&name=your_queue_name&opt=get&auth=mypass123"
POST: curl "http://localhost:1218/?charset=gb2312&name=your_queue_name&opt=get&auth=mypass123"
```
if get successfully, will show `你好，httpsqs`

### stop httpsqs

USE: `killall httpsqs`, `pkill httpsqs`, `kill `cat /tmp/httpsqs.pid`
PS: DO NOT USE `pkill -9 httpsqs` or `kill -9 httpsqs` to aviod lost unsaved data

## Ref

- [URL encoding](https://tool.oschina.net/encode?type=4)
- [Httpsqs的安装以及安装过程错误的解决方法 转](https://www.cnblogs.com/jami918/p/3569743.html)
- [基于HTTP协议的轻量级开源简单队列服务：HTTPSQS](http://zyan.cc/httpsqs/)

## develop log

2021.1.8
- fork this rep
- install httpsqs into wsl
- add more detail in readme
