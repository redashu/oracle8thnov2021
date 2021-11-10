## Docker & container summary 

<img src="sum.png">

### Docker engine configuration on LInux 

### case 1

<img src="case1.png">

### case 2 

<img src="case2.png">

### Restart policy in docker engine for container 

<img src="restart.png">

### restart policy for containers 

<img src="policy.png">

### checking restart policy of any container 

```
docker  inspect  ashuc1  --format='{{.HostConfig.RestartPolicy.Name}}'
no

```

### setting restart policy during container creation time 

```
docker  run -itd --name ashuc2  --restart on-failure  c5f49a140b13  
ee0dee145bf83eed8ed891f70fcbee2e118d53ced780db97d157e127813cdf6e
```

###  Cgroups in Container 

<img src="cg.png">

### checking resources inside contaienr 

```
 362  docker  stats ashuc1
  363  docker  exec -itd  ashuc1  ping fb.com 
  364  docker  stats ashuc1
  365  docker  exec -itd  ashuc1  sleep 100 
  366  docker  stats ashuc1
  367  history 
[ashu@ip-172-31-17-159 oracledockerimages]$ docker  top  ashuc1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                9545                9503                0                   04:35               pts/0               00:00:00            python3 /mycode/ashu.py
root                14480               9503                0                   04:48               pts/1               00:00:00            ping fb.com
root                14883               9503                0                   04:48               pts/2               00:00:00            sleep 100

```

### putting hardware limit to containers 

```
372  docker  run -itd --name ashuc3  --restart always --memory 100m   c5f49a140b13
  373  docker  run -itd --name ashuc4  --restart always --memory 100m  --cpu-shares=20  c5f49a140b13
  
```

## remove all data of docker engine 

```
 378  docker  rm $(docker ps -aq) -f
  379  docker rmi $(docker  images -q) -f
  380  history 
  381  docker  images 
  382  docker  ps -a
  383  docker network prune 
  384  docker volume rm $(docker  volume ls -q)
  
```



