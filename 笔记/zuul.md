# Zuul

#### 测试项目搭建

1. 创建一个普通的下游服务client-a
    核心代码

```java
@RestController
@RequestMapping("/user")
public class UserController {

    @RequestMapping(value = "/add",method = RequestMethod.GET)
    public String add(Integer id) {
        return "user:" + id;
    }
}
```

配置文件

```yaml
spring:
  application:
    name: client-a
server:
  port: 8080
eureka:
  client:
    service-url:
      defaultZone: http://eureka.springcloud.cn/eureka/
```

2.创建zuul-server服务
 配置文件

```yaml
spring:
  application:
    name: zuul-server
server:
  port: 8888
eureka:
  client:
    service-url:
      defaultZone: http://eureka.springcloud.cn/eureka/
zuul:
  routes:
    client-a:
      path: /client/**
      serviceId: client-a
```

3.接口测试
 启动两个服务，用postman分别测试接口。

 经过zuul网关调取接口

 由结果可知我们在调取[http://localhost:8888/client/user/add?id=1](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A8888%2Fclient%2Fuser%2Fadd%3Fid%3D1)接口的时候，实际上是调用的[http://localhost:8080/user/add?id=1](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A8080%2Fuser%2Fadd%3Fid%3D1)接口。这是因为我们在zuul配置文件中指定了路由规则，当想zuul server发起请求的时候，他就会去Eureka注册中心拉取服务列表，如果发现有指定的路由映射规则，就会按照规则路由到相应的接口上去。

#### Spring Cloud Zuul典型配置

**1.单实例serviceId映射**

```yaml
zuul:
  routes:
    client-a:
      path: /client/**
      serviceId: client-a
```

上例中的配置，是一个/client/** 到client-a服务的映射规则，我们可以把它简化为一个较简单的配置

```yaml
zuul:
  routes:
    client-a: /client/**
```

另外还有一种更加简单的映射规则，映射规则与serivceId都不用写

```yaml
zuul:
  routes:
    client-a:
```

在这种情况下，Zuul会给client-a添加一个默认的映射规则/client-a/**,相当于：

```yaml
zuul:
  routes:
    client-a:
      path: /client-a/**
      serviceId: client-a
```

这时候通过[http://localhost:8888/client-a/user/add?id=1](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A8888%2Fclient-a%2Fuser%2Fadd%3Fid%3D1)来调用。
 **2.单实例url映射**
 除了路由到服务外，还能路由到物理地址，将serviceId替换成url即可

```yaml
zuul:
  routes:
    client-a:
      path: /client/**
      url: http://localhost:8080 #client-a的地址
```

**3.多实例路由**
 在默认情况下，zuul会使用Eureka中集成的基本负载均衡功能，如果想使用Ribbon的负载均衡功能，就需要指定一个serviceId，此操作需要禁止Ribbon使用Eureka，在E版之后新增了负载均衡的配置。



```yaml
zuul:
  routes:
    client-a:
      path: /client/**
      serviceId: client-a
ribbon:
  eureka:
    enabled: false #禁止Ribbon使用Eureka
client-a:
  ribbon:
    NIWSServerListClassName: com.netflix.loadbalancer.ConfigurationBasedServerList
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
    listOfServers: localhost:8080,localhost:8081
```

**4.forward本地跳转**
 有时候我们在zuul中会做一些逻辑处理，在网关(zuul server)中写好一个接口，如下所示

```java
@RestController
public class TestController {

    @RequestMapping(value = "/testMethod",method = RequestMethod.GET)
    public String testMethod(Integer a){
        return "本地跳转:"+a;
    }
}
```

配置以下信息

```yaml
zuul:
  routes:
    client-a:
      path: /test/**
      url: forward:/testMethod
```

调用[http://localhost:8888/test?a=1](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A8888%2Ftest%3Fa%3D1)接口返回如下

**1.功能前缀**
 配置路由规则的时候，我们可以配置一个统一的代理前缀。

```yaml
zuul:
  routes:
    client-a:
      path: /client/**
      serviceId: client-a
  prefix: /pre
```

访问的时候就要加上这个前缀了，如：[http://localhost:8888/pre/client/user/add?id=1](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A8888%2Fpre%2Fclient%2Fuser%2Fadd%3Fid%3D1)。

 **2.服务屏蔽与路径屏蔽**
 有时候为了避免有些服务或者路径入侵，可以将它们屏蔽掉

```yaml
zuul:
  ignored-services: client-b  #忽略服务，防止服务入侵
  ignored-patterns: /**/div/** #忽略接口，屏蔽接口
  prefix: /pre
  routes:
    client-a: /client/**
```

**3.敏感头信息**

在构建系统的时候，使用HTTP的header传值是十分方便的，协议的一些认证信息默认也在header，比如cookie，或者习惯把基本认证通过base64加密后放在Authorization里面。在我们系统内部系统没有什么问题，但是如果系统要和外部系统打交道，就可能会出现这些信息 的泄露。在zuul的配置里可以指定敏感头，切断它和下层服务的交互。



```yaml
zuul:
  routes:
    client-a:
      path: /client/**
      sensitiveHeaders: Cookie,Set-Cookie,Authorization
      serviceId: client-a
```

**4.重定向问题**

重定向后返回之前的host

```yaml
zuul:
  routes:
    client-a: /client/**
  add-host-header: true
```

**5.重试机制**

生产环境中，由于各种原因，可能会使一次请求偶然失败，考虑到某些业务的体验，不能通过有感知的操作来触发，这时候就会用到重试机制了，Zuul可以配合Ribbon（默认个集成）来做重试。

```yaml
zuul:
  retryable: true #开启重试
  ribbon:
    MaxAutoRetries: 1 #同一个服务重试的次数(除去首次)
    MaxAutoRetriesNextServer: 1 #切换相同服务数量
```

当然，此功能要慎用，有一些接口要保证幂等性，一定要做好相关工作。

作者：卑微幻想家
链接：https://www.jianshu.com/p/adec9003bfd2
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

