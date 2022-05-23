## 技术栈

**SpringCloud**

**Consul + Spring Security Oauth2 + Zuul + OpenFeign** 

**MybatisPlus + MapStruct + Swagger** **+ Hibernate-Validator** **+ Lunz分页组件**

### 1. Spring Cloud

> Spring Cloud是一系列框架的[有序集合](https://baike.baidu.com/item/有序集合/994839)。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如**服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控**等，都可以用Spring Boot的开发风格做到一键启动和部署。

https://spring.io/projects/spring-cloud

### 2. Consul

> Consul 是 HashiCorp 公司推出的开源产品，用于实现分布式系统的服务发现、服务隔离、服务配置，这些功能中的每一个都可以根据需要单独使用，也可以同时使用所有功能。

本框架中使用了服务发现的功能，来实现各个服务之间的通信

### 3. Spring Security Oauth2

结合认证中心，完成token的认证

```java

@Value("${spring.security.oauth2.resourceserver.jwt.issuer-uri}")
private String jwkIssuerUri;

@Value("${spring.security.oauth2.resourceserver.jwt.jwk-set-uri}")
private String jwkSetUri;

@Value("${spring.cloud.consul.discovery.healthCheckPath}")
private String healthCheckUrl;

@Value("${config.oauth2.resource.id}")
private String resourceId;

/**
 * 认证配置：放过不需要认证的路径
 */
@Override
protected void configure(HttpSecurity http) throws Exception {
  http.addFilterBefore(authorizationResponseFilter(), ChannelProcessingFilter.class)
    .authorizeRequests()
    .antMatchers(CommonConstants.CSRF_PATH, CommonConstants.ROOT_PATH, healthCheckUrl).permitAll()
    .antMatchers(HttpMethod.OPTIONS, "**").permitAll()
    .anyRequest().hasAuthority(CommonConstants.SCOPE_PREFIX + resourceId)
    .and().logout().permitAll()
    .and().cors().and()
    .csrf().disable().oauth2ResourceServer().jwt();
}

@Override
public void configure(WebSecurity web) {
  web.ignoring().antMatchers("/api/v1/**/docs");
  web.ignoring().antMatchers("/swagger-ui.html**");
  web.ignoring().antMatchers("/doc.html**");
  web.ignoring().antMatchers("/webjars/**");
  web.ignoring().antMatchers("/swagger-resources/**");
  web.ignoring().antMatchers(HttpMethod.OPTIONS, "**");

}

@Bean
public JwtDecoder jwtDecoder() {

  // 设置公钥开放端点，本地解析token
  NimbusJwtDecoder jwtDecoder = NimbusJwtDecoder.withJwkSetUri(jwkSetUri).build();
  // 设置需要验证的颁发者
  jwtDecoder.setJwtValidator(new JwtIssuerValidator(jwkIssuerUri));
  
  return jwtDecoder;
}

/**
 * 处理校验filter
 */
private Filter authorizationResponseFilter() {
  return (request, response, chain) -> {
    chain.doFilter(request, response);

    // 解决流提前关闭，继续操作response打印流问题
    if (!response.isCommitted()) {
      HttpServletResponse httpServletResponse = (HttpServletResponse) response;
      HttpStatus httpStatus = HttpStatus.valueOf(httpServletResponse.getStatus());
      response.setContentType(MediaType.APPLICATION_JSON_VALUE);
      response.setCharacterEncoding(StandardCharsets.UTF_8.name());
      if (httpStatus == HttpStatus.UNAUTHORIZED) {
        Map<String, Object> baseException = ExceptionResultUtil.getBaseException(new InvalidTokenException());
        response.getWriter().append(JSON.toJSONString(baseException));

      } else if (httpStatus == HttpStatus.FORBIDDEN) {
        Map<String, Object> baseException = ExceptionResultUtil.getBaseException(new PermissionDeniedException());
        response.getWriter().append(JSON.toJSONString(baseException));
      }
    }

  };
}
```

application.yaml 中进行如下配置：

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://identity-fat.lunz.cn
          jwk-set-uri: http://identity-fat.lunz.cn/.well-known/openid-configuration/jwks
```

### 4. Zuul 

> Zuul是Netflix开源的微服务网关，他可以和Eureka,Ribbon,Hystrix等组件配合使用。
> Zuul组件的核心是一系列的过滤器，这些过滤器可以完成以下功能:
>
> 1. 身份认证和安全：识别每一个资源的验证要求，并拒绝那些不符合的请求
> 2. 审查与监控
> 3. 动态路由：动态将请求路由到不同后端集群
> 4. 压力测试：逐渐增加指向集群的流量，以了解性能
> 5. 负载分配：为每一种负载类型分配对应容量，并弃用超出限定值的请求
> 6. 静态响应处理：边缘位置进行响应，避免转发到内部集群
> 7. 多区域弹性：跨域AWS Region 进行请求路由，旨在实现 ELB(ElasticLoadBalancing)使用多样化

默认和Ribbon结合实现了负载均衡；

本框架中仅使用了**路由和安全过滤**功能

+ **路由**

  + **单实例serviceId映射**

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

  + **功能前缀**

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

  + **服务屏蔽与路径屏蔽**

  有时候为了避免有些服务或者路径入侵，可以将它们屏蔽掉

  ```yaml
  zuul:
    ignored-services: client-b  #忽略服务，防止服务入侵
    ignored-patterns: /**/div/** #忽略接口，屏蔽接口
    prefix: /pre
    routes:
      client-a: /client/**
  ```

  + **Url映射**

   除了路由到服务外，还能路由到物理地址，将serviceId替换成url即可

  ```yaml
  zuul:
    routes:
      client-a:
        path: /client/**
        url: http://localhost:8080 #client-a的地址
  ```


+ **过滤**

  如下代码实现了如下功能：

  1. 对于部分接口认证的放行，可以跳过Spring Security 的token验证
  2. 检查请求中是否携带token，并将token解析放到请求的Header中，传给下游的微服务

  ```java
  @Value("${spring.cloud.consul.discovery.healthCheckPath}")
  private String healthCheckUrl;
  
  @Override
  public boolean shouldFilter() {
  
    try {
      // 放行不走认证的地址
      RequestContext ctx = RequestContext.getCurrentContext();
      HttpServletRequest request = ctx.getRequest();
      if (request.getRequestURI().contains(CommonConstants.SWAGGER_PREFIX) || request.getRequestURI().contains(CommonConstants.WEBJAR_KEY)
          || request.getRequestURI().contains(CommonConstants.CSRF_KEY) || request.getRequestURI().contains(healthCheckUrl)
          || (HttpMethod.OPTIONS.matches(request.getMethod()))
          || request.getRequestURI().contains(CommonConstants.SWAGGER_DOC_PATH)) {
        return false;
      }
    } catch (Exception e) {
      e.printStackTrace();
      log.error("zuul should filter has error:" + e.getMessage());
    }
    return true;
  }
  
  /**
   * 转发前，对需要转发的请求进行判断，判断是否有Authorization参数，没有则返回token无效，有的话获取token
   * 的信息并传递给下游微服务
   */
  @SneakyThrows
  @Override
  public Object run() throws ZuulException {
  
    RequestContext requestContext = RequestContext.getCurrentContext();
    Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
    String authorizationHeaderValue = requestContext.getRequest().getHeader(HttpHeaders.AUTHORIZATION);
  
    if (StringUtils.isEmpty(authorizationHeaderValue) || !authorizationHeaderValue.startsWith(CommonConstants.BEARER_TOKEN_PREFIX)) {
      requestContext.setSendZuulResponse(false);
      requestContext.setResponseStatusCode(HttpStatus.UNAUTHORIZED.value());
      Map<String, Object> baseException = ExceptionResultUtil.getBaseException(new InvalidTokenException());
      requestContext.setResponseBody(JSON.toJSONString(baseException));
      requestContext.getResponse().setContentType(MediaType.APPLICATION_JSON_VALUE);
      requestContext.getResponse().setCharacterEncoding(StandardCharsets.UTF_8.name());
    }
  
  
    // 打印请求参数
    HttpServletRequest request = requestContext.getRequest();
    try (ServletInputStream inputStream = request.getInputStream()) {
  
      // 请求参数
      String requestUrl = String.join("", request.getRequestURL(), request.getRequestURI());
      String body = IOUtils.toString(inputStream, StandardCharsets.UTF_8);
      String queryString = request.getQueryString();
  
      // 认证参数
      Map<String, Object> claims = ((Jwt) authentication.getPrincipal()).getClaims();
      Object iss = claims.get("iss");
      Object name = claims.get("name");
      Object username = claims.get("username");
      Object clientId = claims.get("client_id");
      Object scope = claims.get("scope");
  
      log.info("\n\n请求地址:{}\n请求body体:{}\n请求url地址参数:{}\n认证地址:{}\n用户:{}\n登陆账号:{}\n客户端Id:{}\n资源:{}\n", requestUrl, body, queryString, iss, name, username, clientId, scope);
  
    } catch (Exception e) {
      log.error("请求参数获取失败!{}", e.getMessage());
    }
  
  
    try {
      Map<String, Object> claims = ((Jwt) authentication.getPrincipal()).getClaims();
      requestContext.addZuulRequestHeader(CommonConstants.AUTH_USER_DETAIL, URLEncoder.encode(JSON.toJSONString(claims), "UTF-8"));
    } catch (UnsupportedEncodingException e) {
      log.error("claim:{0}", e);
      throw new ZuulException(e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR.value(), e.getMessage());
    }
    return null;
  }
  
  @Override
  public String filterType() {
    return FilterConstants.PRE_TYPE;
  }
  
  @Override
  public int filterOrder() {
    return 6;
  }
  ```


### 5. OpenFeign

> OpenFeign是一种声明式、模板化的HTTP客户端。在Spring Cloud中使用OpenFeign，可以做到使用HTTP请求访问远程服务，就像调用本地方法一样的，开发者完全感知不到这是在调用远程方法，更感知不到在访问HTTP请求。

可以通过OpenFeign来实现微服务之间的接口调用；

通过如下代码即可调用**同一注册中心下**微服务的接口；

示例中调用的是**test-service**服务对应url的接口；

```java
@FeignClient(name = "test-service")
@RequestMapping("/rpc/test")
public interface ITestFeignClient {

    @GetMapping("/{id}")
    DemoVO getUserInfoById(@PathVariable("id") @NotBlank(message = "id不能为空") String id);

    @GetMapping("/list")
    List<DemoVO> getAll();
}
```

### 6. MapStruct

可以快速完成对象与对象之间的转换；

在mapper接口中声明转换方法后，可以使用注解快速的配置对象之间转换的配置；

默认是会对相同名称的属性进行复制；

```java
/**
 * 实体映射工具
 * @author haha
 */
@Mapper(collectionMappingStrategy = CollectionMappingStrategy.ADDER_PREFERRED,
        nullValueCheckStrategy = NullValueCheckStrategy.ALWAYS)
public interface DemoConvertMappers {

    DemoConvertMappers MAPPER = Mappers.getMapper(DemoConvertMappers.class);

    Demo convertBean(DemoDTO demoDTO);

    List<DemoVO> convertList(List<Demo> demoList);
		
    @Mapping(source = "sourceName", target = "targetName") //指定sourceName 赋值给 targetName
    DemoVO convertVO(Demo demo);

    MpAll<Demo> convertAll(MpAll<DemoDTO> mpAll);

    MpPager<Demo> convertPage(MpPager<DemoDTO> mpPager);

    Page<DemoVO> convertPageList(Page<Demo> page);
  	
  	//自定义实现方法
  	default InnerTarget innerTarget2InnerSource(InnerSource innerSource) {
      if (innerSource == null) {
        return null;
      }
      InnerTarget innerTarget = new InnerTarget();
      innerTarget.setIsDeleted(innerSource.getDeleted() == 1);
      innerTarget.setName(innerSource.getName());
      return innerTarget;
  }
}
```



### 7. Swagger

``` java
@Api(value = "测试API", tags = {"测试API"}) //写在Controller类上，用来说明分组名称，默认使用类名称

@ApiOperation("修改用户") //写在接口方法上，用来说明接口

@ApiModelProperty(value = "id", name = "用户id") //写在接口入参实体类的属性上，用来说明属性

@ApiModel //写在出入参数实体类上；可在swagger中 models模块中看到所有标记的参数实体
```

### 8 .  Hibernate-Validator

+ **空与非空检查**

| 注解      | 支持Java类型                         | 说明                                |
| --------- | ------------------------------------ | ----------------------------------- |
| @Null     | Object                               | 为null                              |
| @NotNull  | Object                               | 不为null                            |
| @NotBlank | CharSequence                         | 不为null，且必须有一个非空格字符    |
| @NotEmpty | CharSequence、Collection、Map、Array | 不为null，且不为空（length/size>0） |

+ **Boolean值检查**

| 注解         | 支持Java类型     | 说明    | 备注       |
| ------------ | ---------------- | ------- | ---------- |
| @AssertTrue  | boolean、Boolean | 为true  | 为null有效 |
| @AssertFalse | boolean、Boolean | 为false | 为null有效 |

+ **日期检查**

| 注解             | 支持Java类型                                                 | 说明                     | 备注       |
| ---------------- | ------------------------------------------------------------ | ------------------------ | ---------- |
| @Future          | Date、Calendar、Instant、LocalDate、LocalDateTime、LocalTime、MonthDay、OffsetDateTime、OffsetTime、Year、YearMonth、ZonedDateTime、HijrahDate、JapaneseDate、MinguoDate、ThaiBuddhistDate | 验证日期为当前时间之后   | 为null有效 |
| @FutureOrPresent | Date、Calendar、Instant、LocalDate、LocalDateTime、LocalTime、MonthDay、OffsetDateTime、OffsetTime、Year、YearMonth、ZonedDateTime、HijrahDate、JapaneseDate、MinguoDate、ThaiBuddhistDate | 验证日期为当前时间或之后 | 为null有效 |
| @Past            | Date、Calendar、Instant、LocalDate、LocalDateTime、LocalTime、MonthDay、OffsetDateTime、OffsetTime、Year、YearMonth、ZonedDateTime、HijrahDate、JapaneseDate、MinguoDate、ThaiBuddhistDate | 验证日期为当前时间之前   | 为null有效 |
| @PastOrPresent   | Date、Calendar、Instant、LocalDate、LocalDateTime、LocalTime、MonthDay、OffsetDateTime、OffsetTime、Year、YearMonth、ZonedDateTime、HijrahDate、JapaneseDate、MinguoDate、ThaiBuddhistDate | 验证日期为当前时间或之前 | 为null有效 |

+ **数值检查**

| 注解                               | 支持Java类型                                                 | 说明                   | 备注              |
| ---------------------------------- | ------------------------------------------------------------ | ---------------------- | ----------------- |
| @Max                               | BigDecimal、BigInteger，byte、short、int、long以及包装类     | 小于或等于             | 为null有效        |
| @Min                               | BigDecimal、BigInteger，byte、short、int、long以及包装类     | 大于或等于             | 为null有效        |
| @DecimalMax                        | BigDecimal、BigInteger、CharSequence，byte、short、int、long以及包装类 | 小于或等于             | 为null有效        |
| @DecimalMin                        | BigDecimal、BigInteger、CharSequence，byte、short、int、long以及包装类 | 大于或等于             | 为null有效        |
| @Negative                          | BigDecimal、BigInteger，byte、short、int、long、float、double以及包装类 | 负数                   | 为null有效，0无效 |
| @NegativeOrZero                    | BigDecimal、BigInteger，byte、short、int、long、float、double以及包装类 | 负数或零               | 为null有效        |
| @Positive                          | BigDecimal、BigInteger，byte、short、int、long、float、double以及包装类 | 正数                   | 为null有效，0无效 |
| @PositiveOrZero                    | BigDecimal、BigInteger，byte、short、int、long、float、double以及包装类 | 正数或零               | 为null有效        |
| @Digits(integer = 3, fraction = 2) | BigDecimal、BigInteger、CharSequence，byte、short、int、long以及包装类 | 整数位数和小数位数上限 | 为null有效        |

+ **其他**

| 注解     | 支持Java类型                         | 说明                      | 备注                        |
| -------- | ------------------------------------ | ------------------------- | --------------------------- |
| @Pattern | CharSequence                         | 匹配指定的正则表达式      | 为null有效                  |
| @Email   | CharSequence                         | 邮箱地址                  | 为null有效，默认正则 `'.*'` |
| @Size    | CharSequence、Collection、Map、Array | 大小范围（length/size>0） | 为null有效                  |

+ **hibernate-validator扩展约束（部分）**

| 注解    | 支持Java类型     | 说明           |
| ------- | ---------------- | -------------- |
| @Length | String           | 字符串长度范围 |
| @Range  | 数值类型和String | 指定范围       |
| @URL    |                  | URL地址验证    |

+ 自定义注解

  也可以自己定义

+ 分组校验

  以上所有注解都可以指定一个类，来区分哪个接口进行哪些校验

  ```java
  @Data
  @ApiModel
  public class DemoDTO {
  
      /**
       * 注：有些字段是新增不需要，但是修改必填，通过分组实现，分组之后按照不同的组实现不同的验证逻辑
       */
      @ApiModelProperty(value = "id", name = "用户id")
      @TableId(type = IdType.AUTO)
      @Null(groups = {AddGroup.class},message = "id必须是空！")
      @NotNull(groups = {UpdateGroup.class},message = "id不能为空！")
      private Integer id;
  
      @ApiModelProperty(value = "name", name = "用户名称", required = true)
      @NotBlank(message = "用户名不能为空！")
      private String name;
  
      @ApiModelProperty(value = "loginName", name = "用户登录名", required = true)
      @NotBlank(message = "登录名不能为空！")
      @Pattern(regexp = "^.{1,50}$",message = "登录账号长度必须在1-50范围内!")
      private String loginName;
  
  }
  ```

  更新接口使用

  ```java
  @ApiOperation("修改用户")
  @PutMapping
  public void updateUser(@RequestBody @Validated({UpdateGroup.class}) @Valid DemoDTO demoDTO) {
    Demo demo = DemoConvertMappers.MAPPER.convertBean(demoDTO);
    demoService.modifyDemo(demo);
  }
  ```

  

### 9. Lunz分页组件

```java
@ApiOperation("获取分页数据")
@PostMapping("/page")
public IPage<DemoVO> page(@RequestBody MpPager<DemoDTO> page) {
  MpPager<Demo> newPage = DemoConvertMappers.MAPPER.convertPage(page);
  MyQueryWrapper<Demo> queryWrapper = MpUtil.convertObjectToMP(newPage);
  IPage<Demo> demoPage = demoService.page(queryWrapper.getPage(), queryWrapper.getQueryWrapper());
  return DemoConvertMappers.MAPPER.convertPageList((Page<Demo>) demoPage);
}

@ApiOperation("查看所有")
@PostMapping("/list")
public List<DemoVO> list(@RequestBody MpAll<DemoDTO> page) {
  MpAll<Demo> newAll = DemoConvertMappers.MAPPER.convertAll(page);
  MyQueryWrapper<Demo> queryWrapper = MpUtil.convertObjectToMP(newAll);
  List<Demo> demoList = demoService.list(queryWrapper.getQueryWrapper());
  return DemoConvertMappers.MAPPER.convertList(demoList);
}
```

与前端查询组件**ngx-query**组合使用

## undertow替代tomcat

网关和微服务中移除了 tomcat 增加了undertow依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

undertow 吞吐量更高一点

> 参考资料：
>
> https://www.jianshu.com/p/ab78515265f4
>
> https://www.cnblogs.com/duanxz/p/9337022.html



## 调用关系

![image-1632450740423](/Users/zhihaoyan/Desktop/笔记/图片/image-1632450740423.png)



## 模板结构介绍

- 公共模块（common）:统一枚举、常量、自定义注解、工具类包、token解析
- 应用通信层（rpc-client）：定义服务本身调用其他外部服务
- 服务接口层（service）：定义业务功能列表和数据库实体
- 服务接口实现层（service-impl）: 定义业务逻辑实现层、数据库持久层（mapper接口和.xml文件）、依赖rpc-service
- 数据传输层（model）：定义Api接口的入参（DTO）和出参（VO）
- 微服务之间API接口层（rpc-stub）：定义controller业务接口供其他微服务使用，方便服务之间的业务通信
- 微服务内部API实现层（rpc-api）：定义微服务之间controller业务接口供其他微服务使用
- 请求API接口层（api）: 暴漏给外部使用，需经过网关
- web层：定义web配置，如拦截器、切面、全局异常处理器、默认字段填充处理器

## 功能介绍

- 统一项目依赖版本控制，使用springboot版本2.3.2.RELEASE，springCloud版本Hoxton.SR8
- 可插拔服务注册与发现，模板默认使用consul做服务注册与发现，业务方可根据自身业务需要，替换为其他注册中心
- 统一异常规范，使用用户中心错误码规范 【[错误码规范](http://wiki.lunztech.cn/uploads/images/gallery/2020-08/netcore-errorcodebase.png)】
- 统一restful接口规范，使用用户中心restful规范做了示例demo（参考《WebApi规范》）【[Lunz-API规范](http://wiki.lunztech.cn/books/lunz-架构规范/page/restful-api设计)】
- 统一日志输出，使用logback规范的日志输出格式，很好的迎合阿里云日志服务（需运维同事在日志服务配置日志输出格式）
- 统一监控指标，集成prometheus可配置指标显示在grafana（需运维同事支持完成）
- 集成分页组件，内部集成用户中心分页组件，配合lunz+前端组件
- 统一异常处理，内部通过自定义全局异常处理器，规范异常日志输出
- 规范入参、出参，请求API层入参DTO,出参VO
- 统一数据库表公共字段抽离（BaseModel）,具有相同公共字段的表可继承该实体，并在操作ORM层时根据操作类型自动填充字段默认值
- 网关层统一鉴权，微服务层只做token解析，使用UserUtils中的静态方法可以获取到解析到的token中的用户信息
- api-stub层使用了feign接口规范，共用同一网关下的微服务之间的通信，可以引入api-stub，直接进行服务通信;不同网关下的微服务使用httpClient进行通信
- 集成了sonar插件通过mvn命令进行代码检查

```
mvn clean install && mvn sonar:sonar -D sonar.projectKey=java -D sonar.projectName=项目名 -D sonar.host.url=https://sonar.lunz.cn -D sonar.login=sonar服务器项目令牌
```

