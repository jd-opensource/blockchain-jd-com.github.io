PEER节点相关配置

## `application-peer.properties`

```properties
# gzip
server.compression.enabled=true
server.compression.mime-types=application/json,application/xml,text/html,text/xml,text/plain

# 管理服务TLS配置
server.ssl.enabled=false
server.ssl.client-auth=none
server.ssl.protocol=
server.ssl.enabled-protocols=
server.ssl.ciphers=
server.ssl.key-store=
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=
server.ssl.key-store-password=
server.ssl.trust-store=
server.ssl.trust-store-password=
server.ssl.trust-store-type=JKS
```

## `peer-startup.sh`

主要注意：

```bash
#Peer节点Web端口
#请运维根据实际环境进行调整或通过-p参数传入
WEB_PORT=7080

#定义程序启动的参数
JAVA_OPTS="-jar -server -Xms1024m -Xmx1024m  -Djdchain.log=$APP_HOME/logs -Dlog4j.configurationFile=file:$APP_HOME/config/log4j2-gw.xml"
```
启动端口和`JVM`内存相关配置