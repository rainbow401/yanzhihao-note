# Docker

## 下载镜像

docker pull nacos/nacos-server

## docker镜像访问宿主机localhost

```java
host.docker.internal
```

docker访问宿主机时：

``` java
localhost = host.docker.internal
```

## 启动镜像时环境变量赋值和端口映射

```java
docker run --env MODE=standalone --name nacos --restart=always -d -p 8848:8848 docker.io/nacos/nacos-server:1.3.1
```

解析:

```java
-p 8080:8080  //映射到宿主机8080端口
```

``` java
MODE=standalone	//MODE是某个环境变量
```

## docker run 参数

```java
-p 8080:8080 	//将镜像的端口映射到宿主机，宿主机可以直接通过localhost:8080进行访问
```

```java
-MODE=stanalone		//某个环境变量 此处示例为nacos设置为单机模式
```

```java
--name	//生成镜像的名称
```

