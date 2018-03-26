## Linux 下 XPE 操作命令


#### 在xpe中手动建一个git项目

```
1. login as root
2. su xpe
3. cd /home/xpe/git
4. mkdir study
5. cd study
6. git init --bare
7. cd /home/xpe/projects
8. git clone /home/xpe/git/study
9. cd study
10. cp -rf ../demo/* .
11. git add .
12. git commit -a -m "init"
13. git push origin master
```

#### 克隆项目

```
1. /home/xpe/git/demo
2. /home/xpe/git/weekly
3. https://github.com/softtouchit/xpe_demo
```

#### 如何clone项目到本地

```
URI:xpe@114.115.142.202:git/study
Host:114.115.142.202
Repository:git/study
Protocol:ssh
User:xpe
Store in Secure Store: true
```

#### 编译和部署

```
1. 编译：production build
2. 部署目录： ~/xpe/deploy 
3. 变更监控查询：
    1. printLog xpe.log
    2. printLog +f xpe.log
4. 示例
    1. ls xars
    2. cp xars/xpe_ide_backend.xar ~/xpe/deploy
    3. cd
    4. bin  git  jars  projects  xars  xpe  xpe_all.zip  xpe-js
    5. cp xars/xpe_ide_backend.xar xpe/deploy/
    6. cd xpe
    7. printLog xpe.log
    8. printLog +f xpe.log
```
#### 停止xpe

```
[xpe@ecs-cbb5 ~]$ jps
- 101025 Jps
- 38296 xpe_dist.jar

[xpe@ecs-cbb5 ~]$ kill -INT 38296
```
#### 启动 xpe

```
[xpe@ecs-cbb5 ~]cd xpe
[xpe@ecs-cbb5 ~]$ nohup sh run.sh &
[xpe@ecs-cbb5 ~]cd
[xpe@ecs-cbb5 ~]cd xpe_ms
[xpe@ecs-cbb5 ~]$ nohup sh run.sh &

说明：两个服务
.\xpe -> the main web server
.\xpe_ms -> the messaging server

```

#### 如何使用自己的 java 包

```
#
java -cp lib/xpe_ms_dist.jar:lib/your_lib.jar com.softtouchit.xpe.ms.MSServer $@
change run.sh to the above

java -cp lib/xpe_ms_dist.jar:lib/your_lib.jar com.softtouchit.xpe.ms.MSServer $@
run.sh
java -jar lib/xpe_dist.jar

```
