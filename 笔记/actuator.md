# actuator

> Spring Boot 程序监控
>
> 这是springboot程序的监控系统，可以实现健康检查，info信息等。在使用之前需要引入spring-boot-starter-actuator，并做简单的配置即可。

## 使用

启动程序后通过如下url访问即可获得状态数据

http://localhost:19090/test-service/actuator/health