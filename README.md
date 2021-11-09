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



