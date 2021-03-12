# 服务的开始和停止

### 停止服务

#### 查看端口号运行的应用

```sh
 netstat  -nlp|grep 8081
```

#### 强制停止

```sh
kill -9 pid
```

**稍作等待,或执行下一步前,再次执行一遍查看端口应用,查看是否停止**



### 开始服务

#### 运行jar

```sh
nohup java -jar fop.jar &
//nohup 表示永久运行,& 表示后台运行
```

#### 运行war

```sh
运行tomcat
tomcat/bin下
./startup.sh
或
nohup ./startup.sh &
```

#### 查看日志

logs下

```sh
tail -f catalina.out
//可跟综日志
或者
view catalina.out 
//仅查看
```

