# licodeinstall

## install ubuntu development docker
### data-root
```
vi /etc/docker/daemon.json
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"],
  "storage-driver": "devicemapper", 
  "data-root": "/home/allcom/docker.old"
}
only needed if the diretory is mounted by NFS, should use "storage-driver": "devicemapper", 
```
### docker container 
```
UBLIC_IP=192.168.0.113 MIN_PORT=30000; MAX_PORT=30050; sudo docker run --name licodedev -p  3000:3000 -p $MIN_PORT-$MAX_PORT:$MIN_PORT-$MAX_PORT/udp -p 3001:3001  -p 8080:8080 -e "MIN_PORT=$MIN_PORT" -e "MAX_PORT=$MAX_PORT" -e "PUBLIC_IP=$PUBLIC_IP" -e "NETWORK_INTERFACE=eth0" -e HOST_IP=102.168.0.113 -t -i -v /home/allcom/sfu:/sfu ubuntu /usr/bash
```
### insode docker
```
#apt-get update
#apt-get install vim curl wget git
# vi ~/.wgetrc
proxy=http://192.168.0.163:10809
#vi ~/.curlrc
#socks5
socks5 = "127.0.0.1:1080"
#或者HTTP代理
proxy = "127.0.0.1:9999"

#scripts/installUbuntuDeps
modify script file
licode mongodb Cannot change ownership to uid 1000, gid 1000
tar --no-same-owner

tar --no-same-owner -zxvf /sfu/licode/scripts/../build/libdeps/mongodb-linux-x86_64-ubuntu2004-4.4.4.tgz -C /sfu/licode/scripts/../build/libdeps

```
