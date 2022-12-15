docker version
ctr ns ls

ctr i pull docker.io/library/redis:alpine
命名空间 default
docker pull redis:apline
命名空间 moby
ctr -n default i ls
ctr -n moby i ls

docker tag redis:alpine registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine
docker login registry.cn-hangzhou.aliyuncs.com

docker push registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine
需要将阿里云镜像仓库改为public或授权下载
ctr i pull registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine

[root@test containerd]# ctr i ls
REF                                                   TYPE                                                      DIGEST                                                                  SIZE     PLATFORMS                                                                                LABELS 
docker.io/library/redis:alpine                        application/vnd.docker.distribution.manifest.list.v2+json sha256:5bafd88139e6063c1e8114b06f1474e93db2c40bd3709ade8efbb5046f7aa5dc 11.8 MiB linux/386,linux/amd64,linux/arm/v6,linux/arm/v7,linux/arm64/v8,linux/ppc64le,linux/s390x -      
registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine application/vnd.docker.distribution.manifest.v2+json      sha256:d937dcf39e204d8c1d148544d94cb542a60bac0dfddb83b62939f7ec1ae9a865 11.8 MiB linux/amd64                                                                              -    

[root@test containerd]# ctr run -t -d registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine redis
[root@test containerd]# ctr c ls
CONTAINER    IMAGE                                                    RUNTIME                  
redis        registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine    io.containerd.runc.v2    
[root@test containerd]# ctr t ls
TASK     PID      STATUS    
redis    16683    RUNNING
[root@test containerd]# ctr t kill redis
[root@test containerd]# ctr t ls
TASK     PID      STATUS    
redis    16683    STOPPED
[root@test containerd]# ctr t rm redis
[root@test containerd]# ctr t ls
TASK    PID    STATUS    
[root@test containerd]# ctr c ls
CONTAINER    IMAGE                                                    RUNTIME                  
redis        registry.cn-hangzhou.aliyuncs.com/fw0037/redis:alpine    io.containerd.runc.v2    
[root@test containerd]# ctr c rm redis
[root@test containerd]# ctr c ls
CONTAINER    IMAGE    RUNTIME    

[root@test containerd]# docker run -idt redis:alpine
ccf3a644dc53b1baf91e7a5ae209251a24eb87bf2610839235888cf4ac263e39
[root@test containerd]# 
[root@test containerd]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS      NAMES
ccf3a644dc53   redis:alpine   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   6379/tcp   vibrant_visvesvaraya
[root@test containerd]# ctr -n moby t ls
TASK                                                                PID      STATUS    
ccf3a644dc53b1baf91e7a5ae209251a24eb87bf2610839235888cf4ac263e39    16830    RUNNING
[root@test containerd]# 
