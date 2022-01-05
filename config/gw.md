网关相关配置

## `gateway.conf`

```properties
#网关的HTTP服务地址；
http.host=0.0.0.0
#网关的HTTP服务端口；
http.port=8080
#网关服务是否启用安全证书；
http.secure=false
#网关服务SSL客户端认证模式
https.client-auth=none
#网关的HTTP服务上下文路径，可选；
#http.context-path=

#共识节点的管理服务地址（与该网关节点连接的Peer节点的IP地址）；
peer.host=127.0.0.1
#共识节点的管理服务端口（与该网关节点连接的Peer节点的端口，即在Peer节点的peer-startup.sh中定义的端口）；
peer.port=7080
#共识节点的管理服务是否启用安全证书；
peer.secure=false

#共识节点的共识服务是否启用安全证书；
peer.consensus.secure=false

#账本节点拓扑信息落盘，默认false
topology.store=false

#是否开启共识节点自动感知，默认true
topology.aware=true

#共识节点自动感知间隔（毫秒），0及负值表示仅感知一次。对于不存在节点变更的场景感知一次即可
topology.aware.interval=0

#节点连接心跳（毫秒），及时感知连接有效性，0及负值表示关闭
peer.connection.pin=3000

#节点连接认证（毫秒），及时感知连接合法性，0及负值表示关闭。对于不存在公私钥权限变更的场景可关闭
peer.connection.auth=0

#共识节点的服务提供解析器，此配置暂时无用
#BftSmart共识Provider：com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider
#简单消息共识Provider：com.jd.blockchain.consensus.mq.MsgQueueConsensusProvider
peer.providers=com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider

#数据检索服务对应URL，格式：http://{ip}:{port}，例如：http://127.0.0.1:10001
#若该值不配置或配置不正确，则浏览器模糊查询部分无法正常显示。未配置高级检索，此两选项请填空
data.retrieval.url=
schema.retrieval.url=

#默认公钥的内容（Base58编码数据），非CA模式下必填；
keys.default.pubkey=
#默认网关证书路径（X509,PEM），CA模式下必填；
keys.default.ca-path=
#默认私钥的路径；在 pk-path 和 pk 之间必须设置其一；
keys.default.privkey-path=
#默认私钥的内容；在 pk-path 和 pk 之间必须设置其一；
keys.default.privkey=
#默认私钥的解码密码；
keys.default.privkey-password=

```

## `application-gw.properties`

```properties
# gzip
server.compression.enabled=true
server.compression.mime-types=application/json,application/xml,text/html,text/xml,text/plain

# SSL
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

> `SSL`配置将用于以下三处（此三处任意一处需要安全连接，`SSL`便需要正确配置！）：
> - 网关服务端
> - 网关连接`PEER`管理端口
> - 网关连接`PEER`共识服务

## `startup.sh`

主要注意：
```bash
#定义程序启动的参数
JAVA_OPTS="-jar -server -Xms1024m -Xmx1024m  -Djdchain.log=$APP_HOME/logs -Dlog4j.configurationFile=file:$APP_HOME/config/log4j2-gw.xml"
```
`JVM`内存相关配置