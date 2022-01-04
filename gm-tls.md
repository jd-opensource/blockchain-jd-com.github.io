## JDChain增加国密TLS支持配置


### 网关支持国密TLS

#### 网关增加国密TLS支持，配置文件修改如下

a. application-gw.properties

```
#GM TLS配置
#GM TLS启用条件: server.ssl.enabled=true && server.ssl.protocol=GMSSLv1.1
server.ssl.key-store=国密双证书地址
server.ssl.key-store-password=国密双证书密码
server.ssl.key-store-type=PKCS12
server.ssl.protocol=GMSSLv1.1
server.ssl.hostNameVerifier=NO-OP
server.ssl.enabled-protocols=TLSv1.2,GMSSLv1.1
server.ssl.ciphers=TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,ECC_SM4_CBC_SM3,ECDHE_SM4_GCM_SM3,ECDHE_SM4_CBC_SM3

```

b. gateway.conf

```
#网关的HTTP服务地址；
http.host=0.0.0.0
#网关的HTTP服务端口；
http.port=8080
#网关服务是否启用安全证书；
http.secure=true

```

#### JDChain CLI使用说明

JDChain CLI命令行连接支持国密TLS的网关服务时，使用以下配置进行命令行操作:

```

--gw-secure
--gw-port=网关端口
--gw-host=网关IP
--ssl.protocol=GMSSLv1.1
--ssl.enabled-protocols=GMSSLv1.1
--ssl.ciphers=ECC_SM4_CBC_SM3

```


> 示例: 

```
# 网关地址: https://127.0.0.1:8080
# 创建数据账户
./jdchain-cli.sh tx --gw-secure --gw-port=8080 --gw-host=127.0.0.1  --ssl.protocol=GMSSLv1.1 --ssl.enabled-protocols=GMSSLv1.1 --ssl.ciphers=ECC_SM4_CBC_SM3 data-account-register

```

#### 区块链浏览器

使用360安全浏览器或其他支持国密TLS的浏览器打开网址，360安全浏览器开启国密TLS支持: `设置` -- `安全设置` -- `国密通信协议`: 勾选`启用国密SSL协议支持 `


### Peer节点支持国密TLS

涉及修改的配置文件: `bftsmart.confg`, `local.conf`,`application-peer.properties` 及网关配置文件: `gateway.conf`

a. bftsmart.confg

修改Peer节点共识端口启用TLS支持:

```
system.server.X.network.secure=true
```

b. local.conf

增加Peer节点间共识服务国密TLS支持配置:

```

local.parti.ssl.key-store=国密双证书地址
local.parti.ssl.key-store-type=PKCS12
local.parti.ssl.key-store-password=国密双证书密码
local.parti.ssl.protocol=GMSSLv1.1
local.parti.ssl.enabled-protocols=GMSSLv1.1
local.parti.ssl.ciphers=ECC_SM4_CBC_SM3

```

c. application-peer.properties

Peer节点管理服务启用国密TLS支持:

```

#GM TLS配置
#GM TLS启用条件: server.ssl.enabled=true && server.ssl.protocol=GMSSLv1.1
server.ssl.enabled=true
server.ssl.key-store=国密双证书地址
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=国密双证书密码
server.ssl.protocol=GMSSLv1.1
server.ssl.enabled-protocols=TLSv1.2,GMSSLv1.1
server.ssl.ciphers=TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,ECC_SM4_CBC_SM3,ECDHE_SM4_GCM_SM3,ECDHE_SM4_CBC_SM3
server.ssl.hostNameVerifier=NO-OP
```

d. gateway.conf

修改网关连接Peer节点配置:

```
#共识节点的管理服务地址（与该网关节点连接的Peer节点的IP地址）；
peer.host=127.0.0.1
#共识节点的管理服务端口（与该网关节点连接的Peer节点的端口，即在Peer节点的peer-startup.sh中定义的端口）；
peer.port=7080
#共识节点的管理服务是否启用安全证书；
peer.secure=true
#共识节点管理服务SSL客户端认证模式
peer.client-auth=none

#共识节点的共识服务是否启用安全证书；
peer.consensus.secure=true

```

