`JD Chain`集成`Application Performance Management`(`APM`)

## SkyWalking

### PEER

修改`peer-startup.sh`：

```sh
SW_PLUGINS=/home/imuge/Desktop/sky-walking/sky-peer/plugins/:/home/imuge/Desktop/sky-walking/sky-peer/activations/:/home/imuge/Desktop/sky-walking/sky-peer/bootstrap-plugins/:/home/imuge/Desktop/sky-walking/sky-peer/skywalking-agent.jar
KW_AGENT=/home/imuge/Desktop/sky-walking/sky-peer/skywalking-agent.jar
export SW_AGENT_NAME=peer
export SW_AGENT_COLLECTOR_BACKEND_SERVICES=127.0.0.1:11800

#定义程序启动的参数
JAVA_OPTS="-jar -server -Xms2048m -Xmx2048m -Xbootclasspath/a:$SW_PLUGINS -javaagent:$KW_AGENT -Djdchain.log=$APP_HOME/logs -Dlog4j.configurationFile=file:$APP_HOME/config/log4j2-peer.xml"
```

### 网关

修改`startup.sh`：

```sh
SW_PLUGINS=/home/imuge/Desktop/sky-walking/sky-gw/plugins
KW_AGENT=/home/imuge/Desktop/sky-walking/sky-gw/skywalking-agent.jar
export SW_AGENT_NAME=gw
export SW_AGENT_COLLECTOR_BACKEND_SERVICES=127.0.0.1:11800

#定义程序启动的参数
JAVA_OPTS="-jar -server -Xms1024m -Xmx1024m -javaagent:$KW_AGENT -Djdchain.log=$APP_HOME/logs -Dlog4j.configurationFile=file:$APP_HOME/config/log4j2-gw.xml"
```

### @Trace

添加依赖：
```xml
<dependency>
    <groupId>org.apache.skywalking</groupId>
    <artifactId>apm-toolkit-trace</artifactId>
</dependency>
```

在需要跟踪的方法上加上`@Trace`注解

## Prometheus

1. 添加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

2. 修改配置

修改`application-*.properties`：

```properties
# prometheus
management.endpoints.web.exposure.include=prometheus
management.metrics.tags.application=peer
```
> 以上仅展示了非常简单的配置，更多配置请参照`Spring`官方文档

默认数据接口：`/actuator/prometheus`

3. 自定义指标

参照`Prometheus`官方文档自定义实现