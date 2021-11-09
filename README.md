## Docker Engine storage location on Host system 

<img src="engine.png">

### Namespaces 

<img src="ns.png">

### Java based Dockerfile  

<img src="java.png">

### checking default openjdk image 

```
 docker run -it --rm  openjdk  bash 
bash-4.4# java -version 
openjdk version "17.0.1" 2021-10-19
OpenJDK Runtime Environment (build 17.0.1+12-39)
OpenJDK 64-Bit Server VM (build 17.0.1+12-39, mixed mode, sharing)
bash-4.4# cat  /etc/os-release 
NAME="Oracle Linux Server"
VERSION="8.4"
ID="ol"
ID_LIKE="fedora"
VARIANT="Server"
VARIANT_ID="server"
VERSION_ID="8.4"
PLATFORM_ID="platform:el8"
PRETTY_NAME="Oracle Linux Server 8.4"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:oracle:linux:8:4:server"
HOME_URL="https://linux.oracle.com/"
BUG_REPORT_URL="https://bugzilla.oracle.com/"

ORACLE_BUGZILLA_PRODUCT="Oracle Linux 8"
ORACLE_BUGZILLA_PRODUCT_VERSION=8.4
ORACLE_SUPPORT_PRODUCT="Oracle Linux"
ORACLE_SUPPORT_PRODUCT_VERSION=8.4
bash-4.4# jps
26 Jps

```

### Dockerfile for sample java code 

```
FROM openjdk 
# standard java based docker image from docker hub
LABEL email=ashutoshh@linux.com 
RUN mkdir /code 
COPY hello.java /code/hello.java 
WORKDIR /code 
# changing folder during image build time 
RUN javac  hello.java 
# javac is compiler -- to compile java program 
CMD ["java","myclass"]
# default parent process for this image 

```

### build image 

```
 ls
Dockerfile  hello.java
[ashu@ip-172-31-17-159 javasamplecode]$ docker build -t  ashujava:v1  . 
Sending build context to Docker daemon  3.072kB
Step 1/7 : FROM openjdk
 ---> deaa5a1a5f98
Step 2/7 : LABEL email=ashutoshh@linux.com
 ---> Running in f3319bd7976d
Removing intermediate container f3319bd7976d
 ---> 5295f8402d74
Step 3/7 : RUN mkdir /code
 ---> Running in fc557c1fc292
Removing intermediate container fc557c1fc292
 ---> 8debeeaafda2
Step 4/7 : COPY hello.java /code/hello.java
 ---> c07775e3a856
 
 ```
 
 ### creating container 
 
 ```
  docker run -itd --name  ashujc1  ashujava:v1 
d5437caf8dad6b60e48d2f91271d80969147d1d9081c6622551dc4116691e157
[ashu@ip-172-31-17-159 javasamplecode]$ docker  ps
CONTAINER ID   IMAGE         COMMAND          CREATED         STATUS         PORTS     NAMES
d5437caf8dad   ashujava:v1   "java myclass"   4 seconds ago   Up 2 seconds             ashujc1
8192541d7678   rajujava      "java myclass"   5 seconds ago   Up 4 seconds             rajujavac1
[ashu@ip-172-31-17-159 javasamplecode]$ 

```

### Image sharing concept 

<img src="share.png">

### Docker hub image storage format 

<img src="hub.png">

## Puhsing image to docker hub 

#### create repo in docker hub and tag that image 

<img src="tag.png">

### login to docker hub from CLient machine 

```
 docker  login  docker.io 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

```

### pushing image and logout 

```
docker  push  docker.io/dockerashu/oraclejava:v1
docker  logout 
Removing login credentials for https://index.docker.io/v1/

```


## Docker networking setup 

<img src="dnet1.png">

### Testing docker networking 

### create sample container 

```
docker  run -itd --name ashuc1  alpine  ping localhost 
c561534f4b080eccdc3e1d47db27004b31be57d4f578481b3e9ba3d3339df5f0
[ashu@ip-172-31-17-159 oracledockerimages]$ 
[ashu@ip-172-31-17-159 oracledockerimages]$ docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED         STATUS         PORTS     NAMES
c561534f4b08   alpine    "ping localhost"   5 seconds ago   Up 3 seconds             ashuc1

```

### docker container inspection 

```
 docker  inspect  ashuc1  --format='{{.Id}}'
c561534f4b080eccdc3e1d47db27004b31be57d4f578481b3e9ba3d3339df5f0
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  ashuc1  --format='{{.State.Status}}'
running
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  ashuc1  --format='{{.NetworkSettings.IPAddress}}'
172.17.0.2
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  ps
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS     NAMES
65afb103e592   vidhipython:v1   "python3 /mycode/vid…"   4 minutes ago   Up 4 minutes             vidhipythonc1
d96ef6666b47   alpine           "ping localhost"         4 minutes ago   Up 4 minutes             anithac1
4e2c3561ab7f   alpine           "ping localhost"         4 minutes ago   Up 4 minutes             dhanuc2
a5f8401c6553   alpine           "ping localhost"         4 minutes ago   Up 4 minutes             advc1
ac66873d9f16   alpine           "ping localhost"         4 minutes ago   Up 4 minutes             abhishc1
d2e5547afb53   alpine           "ping yahoo.com"         4 minutes ago   Up 4 minutes             amitdocnetc1
aea82b5ee777   alpine           "ping localhost"         5 minutes ago   Up 5 minutes             vijc1
96b05a6f2130   alpine           "ping google.com"        5 minutes ago   Up 5 minutes             rajuc1
cb6b55c23f99   alpine           "ping localhost"         5 minutes ago   Up 5 minutes             anushac1
c561534f4b08   alpine           "ping localhost"         5 minutes ago   Up 5 minutes             ashuc1
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect dhanuc2  --format='{{.NetworkSettings.IPAddress}}'
172.17.0.9

```

### COntainer restarting may get diff IP address 

```
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  stop  ashuc1
ashuc1
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  ashuc1  --format='{{.NetworkSettings.IPAddress}}'

[ashu@ip-172-31-17-159 oracledockerimages]$ docker  start  ashuc1
ashuc1
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  ashuc1  --format='{{.NetworkSettings.IPAddress}}'
172.17.0.2

```

### method 2 to get IP address of containers 

```
 docker  exec -it  ashuc1  sh 
/ # 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02  
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:752 (752.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:174 errors:0 dropped:0 overruns:0 frame:0
          TX packets:174 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:14616 (14.2 KiB)  TX bytes:14616 (14.2 KiB)

/ # exit

```

### checking docker networking device 

```
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  network   ls
NETWORK ID     NAME      DRIVER    SCOPE
d46b64d47296   bridge    bridge    local
cb545fe003a5   host      host      local
331a1f83334b   none      null      local

```

### connection of containers networking 

#### container 2 container 

```
 docker  ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS     NAMES
65afb103e592   vidhipython:v1   "python3 /mycode/vid…"   15 minutes ago   Up 8 minutes              vidhipythonc1
d96ef6666b47   alpine           "ping localhost"         15 minutes ago   Up 15 minutes             anithac1
4e2c3561ab7f   alpine           "ping localhost"         15 minutes ago   Up 15 minutes             dhanuc2
a5f8401c6553   alpine           "ping localhost"         15 minutes ago   Up 15 minutes             advc1
ac66873d9f16   alpine           "ping localhost"         16 minutes ago   Up 8 minutes              abhishc1
d2e5547afb53   alpine           "ping yahoo.com"         16 minutes ago   Up 8 minutes              amitdocnetc1
aea82b5ee777   alpine           "ping localhost"         16 minutes ago   Up 16 minutes             vijc1
96b05a6f2130   alpine           "ping google.com"        16 minutes ago   Up 16 minutes             rajuc1
cb6b55c23f99   alpine           "ping localhost"         16 minutes ago   Up 9 minutes              anushac1
c561534f4b08   alpine           "ping localhost"         17 minutes ago   Up 9 minutes              ashuc1
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  ashuc1  --format='{{.NetworkSettings.IPAddress}}'
172.17.0.2
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  inspect  vidhipythonc1  --format='{{.NetworkSettings.IPAddress}}'
172.17.0.11
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  exec -it ashuc1  sh 
/ # ping 172.17.0.11
PING 172.17.0.11 (172.17.0.11): 56 data bytes
64 bytes from 172.17.0.11: seq=0 ttl=255 time=0.165 ms
64 bytes from 172.17.0.11: seq=1 ttl=255 time=0.177 ms
64 bytes from 172.17.0.11: seq=2 ttl=255 time=0.090 ms
^C
--- 172.17.0.11 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.090/0.144/0.177 ms
/ # exit

```

### Container to external world communication 

<img src="cw.png">

```

[ashu@ip-172-31-17-159 oracledockerimages]$ docker  exec -it ashuc1  sh 
/ # 
/ # ping google.com
PING google.com (142.250.188.206): 56 data bytes
64 bytes from 142.250.188.206: seq=0 ttl=108 time=1.107 ms
64 bytes from 142.250.188.206: seq=1 ttl=108 time=1.126 ms
64 bytes from 142.250.188.206: seq=2 ttl=108 time=1.066 ms
64 bytes from 142.250.188.206: seq=3 ttl=108 time=1.053 ms
^C
--- google.com ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 1.053/1.088/1.126 ms
/ # exit

```

### container app can be accessed from outside using 
#### port forwarding 

<img src="portf.png">

### port forwarding 

```
$ docker  run -itd --name ashuweb  -p  1024:80   370cb606c170  
0b37bfd9c183ef7cfb8184e180124715f1a0483218e685cca309aa9097d8a8c5
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  ps
CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                                   NAMES
0b37bfd9c183   370cb606c170    "/bin/sh -c 'httpd -…"   4 seconds ago    Up 2 seconds    0.0.0.0:1024->80/tcp, :::1024->80/tcp   ashuweb
ae6a467f1f74   rajuwebapp:v1   "/bin/sh -c 'httpd -…"   27 seconds ago   Up 25 seconds   0.0.0.0:5555->80/tcp, :::5555->80/tcp   rajuwebc1
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  ps
CONTAINER ID   IMAGE           COMMAND                  CREATED              STATUS              PORTS                                   NAMES
b9d945699957   b045d56761a5    "/bin/sh -c 'httpd -…"   4 seconds ago        Up 3 seconds        0.0.0.0:1239->80/tcp, :::1239->80/tcp   vijweb1
d6652513024e   113982226bcb    "/bin/sh -c 'httpd -…"   9 seconds ago        Up 8 seconds        0.0.0.0:2222->80/tcp, :::2222->80/tcp   anushaweb
13f8282239f9   cc5406c221a7    "/bin/sh -c 'httpd -…"   55 seconds ago       Up 55 seconds       0.0.0.0:1010->80/tcp, :::1010->80/tcp   pavaniweb
0b37bfd9c183   370cb606c170    "/bin/sh -c 'httpd -…"   About a m

```

### Docker networking custom bridges 

<img src="cbr.png">

### creating custom bridge 

```
docker  network  create  ashubr1 
c0356dde05b5a32820f6a956a518a51168d1a762700a1bec94b613b3874f86ef
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  network  ls
NETWORK ID     NAME      DRIVER    SCOPE
c0356dde05b5   ashubr1   bridge    local
d46b64d47296   bridge    bridge    local

```


