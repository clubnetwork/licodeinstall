# licodeinstall
https://licode.readthedocs.io/en/master/from_source/

https://github.com/lynckia/licode

https://licode.readthedocs.io/en/stable/docker/

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
### inside docker
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

#### Attentions
- scripts/installNuve.sh will change the licode_config.js every time with a new new service ID and service KEY
- erizo will use the ID and KEY to access nuve. If not correct, the basicExample will stuck at the N.API call and will not show listen on port 30001

```
config.nuve.superserviceID = '60b90bf49a80e18de6fe0bf5'; // default value: ''
config.nuve.superserviceKey = '1168'; // default value: ''

```
- minport and maxport must be consistent with the -p when create the docker container

```
//note, this won't work with all versions of libnice. With 0 all the available ports are used
config.erizo.minport = 30000; // default value: 0
config.erizo.maxport = 30050; // default value: 0
```
- public IP, turn server and SSL
```
config.erizoController.iceServers = [{'url': 'stun:stun.l.google.com:19302'}]; // default value: [{'url': 'stun:stun.l.google.com:19302'}]

// Default and max video bandwidth parameters to be used by clients for both published and subscribed streams
config.erizoController.defaultVideoBW = 300; //default value: 300
config.erizoController.maxVideoBW = 300; //default value: 300

// Public erizoController IP for websockets (useful when behind NATs)
// Use '' to automatically get IP from the interface
config.erizoController.publicIP = '192.168.0.183'; //default value: ''
config.erizoController.networkinterface = 'eth0'; //default value: ''

// This configuration is used by the clients to reach erizoController
// Use '' to use the public IP address instead of a hostname
config.erizoController.hostname = 'sfu.callpass.cn'; //default value: ''
config.erizoController.port = 8080; //default value: 8080
// Use true if clients communicate with erizoController over SSL
config.erizoController.ssl = true; //default value: false

// This configuration is used by erizoController server to listen for connections
// Use true if erizoController listens in HTTPS.
config.erizoController.listen_ssl = true; //default value: false
config.erizoController.listen_port = 8080; //default value: 8080

// Custom location for SSL certificates. Default located in /cert
//config.erizoController.ssl_key = '/full/path/to/ssl.key';
config.erizoController.ssl_key = '/sfu/licode/cert/callpass.cn.key';
config.erizoController.ssl_cert = '/sfu/licode/cert/fullchain.cer';

config.erizo.stunserver = 'XXXXX'; // default value: ''
config.erizo.stunport = 3478; // default value: 0

//TURN server IP address and port to be used by the server.
//Please note this is not needed in most cases, setting TURN in erizoController for the clients
//is the recommended configuration
//if '' is used, no relay for the server is used
config.erizo.turnserver = 'XXXXX'; // default value: ''
config.erizo.turnport = 3478; // default value: 0
config.erizo.turnusername = 'XXXX';
config.erizo.turnpass = 'XXXXX';
config.erizo.networkinterface = 'eth0'; //default value: ''

```
