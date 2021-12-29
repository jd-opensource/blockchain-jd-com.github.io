## 启用TLS支持

### 生成TLS测试证书

使用`jdchain-cli.sh ca test` 命令生成测试网测试证书，生成的TLS证书在`confg/certs/tls`目录下。 生成文件说明如下:

* gw1.crt: 网关证书
* gw1.keystore: 网关keystore文件
* peer[0-3].crt: peer测试节点peer0,peer1,peer2,peer3证书
* peer[0-3].keystore: peer测试节点peer0,peer1,peer2,peer3 keystore文件
* trust.jks: truststore文件，已存放所有测试节点的公钥信息
* user[1-10].crt: user测试用户证书
* user[1-10].keystore: user测试用户keystore文件


命令说明:

```
./jdchain-cli.sh ca test --org=<organization> --country=<country> --locality=<locality> --province=<province> --email=<email>

Create certificates for a testnet.
Usage: jdchain-cli ca test [-hV] [--pretty] [-a=<algorithm>]
                           --country=<country> --email=<email> [--gws=<gws>]
                           [--home=<path>] --locality=<locality>
                           [--nodes=<nodes>] --org=<organization>
                           [-p=<password>] --province=<province>
                           [--users=<users>]
  -a, --algorithm=<algorithm>
                             Crypto algorithm
      --country=<country>    Country
      --email=<email>        Email address
      --gws=<gws>            Gateway size
  -h, --help                 Show this help message and exit.
      --home=<path>          Set the home directory.
      --locality=<locality>  Locality
      --nodes=<nodes>        Node size
      --org=<organization>   Organization name
  -p, --password=<password>  Password of the key
      --pretty               Pretty json print
      --province=<province>  Province
      --users=<users>        Available user size
  -V, --version              Print version information and exit.


```


示例:

```

./jdchain-cli.sh ca test --org=jd --country=cn --locality=bj --province=bj --email=test@jd.com

input password for all private keys:
> 12345678

create test certificates in [../../jdchain-peer-1.6.1/config/certs] success


```



### 网关启用TLS支持

网关启用TLS支持涉及到的配置文件有: `application-gw.properties`, `gateway.conf`

a. 网关服务开启TLS, 修改`gateway.conf`：

```
http.secure=true

#是否开启双向认证， 默认为NONE.
#NONE: 单向认证， NEED：严格双向认证， WANT： 非严格双向认证
http.client-auth=NONE
```


b. TLS配置，修改 `application-gw.properties`:

```
# 网关keystore文件地址
server.ssl.key-store=

# 网关keystore类型， 默认PKCS12
server.ssl.key-store-type=PKCS12

# 网关keystore 中存储私钥别名
server.ssl.key-alias=

# 网关keystore密码
server.ssl.key-store-password=

# TLS协议， 默认: TLS1.2
server.ssl.protocol=

# TLS支持协议， 默认为空
server.ssl.enabled-protocols=

# TLS支持加密套件， 默认为空
server.ssl.ciphers=

# 网关开启双向认证时，配置信任证书的keystore文件地址
server.ssl.trust-store=
# 网关开启双向认证时，配置信任证书的keystore文件密码
server.ssl.trust-store-password=
# 网关开启双向认证时，配置信任证书的keystore文件类型， 默认为JKS
server.ssl.trust-store-type=JKS
```

示例如下:

```
server.ssl.key-store=/home/test/config/certs/gw1.keystore
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=12345678
server.ssl.protocol=TLSv1.2
server.ssl.enabled-protocols=TLSv1.2
server.ssl.ciphers=TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
server.ssl.trust-store=/home/test/config/certs/trust.jks
server.ssl.trust-store-password=12345678
server.ssl.trust-store-type=JKS
```

### JDCHAIN CLI命令行

JDCHAIN CLI命令连接启用TLS协议的网关服务时，需增加以下选项进行连接:

```
--gw-secure
--ssl.key-alias=<keyAlias> Set ssl.key-alias for TLS.
--ssl.key-store=<keyStore> Set ssl.key-store for TLS.
--ssl.key-store-password=<keyStorePassword> Set ssl.key-store-password for TLS.
--ssl.key-store-type=<keyStoreType> Set ssl.key-store-type for TLS.
--ssl.trust-store=<trustStore> Set ssl.trust-store for TLS.
--ssl.trust-store-password=<trustStorePassword> Set trust-store-password for TLS.
--ssl.trust-store-type=<trustStoreType> Set ssl.trust-store-type for TLS.
--ssl.protocol=<protocol>
--ssl.enabled-protocols=<enableProtocols>
--ssl.ciphers=<ebableCiphers>
```

选项说明:

* --ssl.key-store: 客户端私钥信息keystore文件地址
* --ssl.key-store-password: 客户端私钥信息keystore文件密码
* 
* --ssl.trust-store: 客户端认证证书truststore文件地址
* --ssl.protocol: 连接服务端使用的协议，默认为TLS1.2
* --ssl.enabled-protocols: 与服务端握手时启用的协议， 默认支持TLS1.2协议
* --ssl.ciphers: 与服务端握手时启用的加密套件， 默认支持TLS1.2协议



示例:

```
./jdchain-cli.sh tx --gw-secure --gw-host=127.0.0.1 --gw-port=8080 --ssl.trust-store=/home/test/config/certs/trust.jks --ssl.trust-store-password=12345678 data-account-register
```


### Peer节点管理服务开启TLS

Peer节点管理服务配置文件为: `application-peer.properties`, 其配置说明与网关配置文件`application-gw.properties`相同。

Peer节点管理服务开启TLS后， 需同步修改网关配置文件`gateway.conf`:

```
#共识节点的管理服务地址（与该网关节点连接的Peer节点的IP地址）；
peer.host=127.0.0.1
#共识节点的管理服务端口（与该网关节点连接的Peer节点的端口，即在Peer节点的peer-startup.sh中定义的端口）；
peer.port=7080
#共识节点的管理服务是否启用安全证书；
peer.secure=true
#共识节点管理服务SSL客户端认证模式
peer.client-auth=none

```

### Peer共识节点开启TLS

Peer共识节点开启TLS涉及到的配置文件有: `bftsmart.conf`, `local.conf`文件:

a. bftsmart.conf

修改共识节点启用TLS:

```
system.server.X.network.secure=true
```

b. local.conf

配置共识节点TLS相关配置:

```
local.parti.ssl.key-store=
local.parti.ssl.key-store-type=
local.parti.ssl.key-alias=
local.parti.ssl.key-store-password=
local.parti.ssl.trust-store=
local.parti.ssl.trust-store-password=
local.parti.ssl.trust-store-type=
local.parti.ssl.protocol=
local.parti.ssl.enabled-protocols=
local.parti.ssl.ciphers=

```

相关参数说明与上述网关等配置一致


示例如下:

```
local.parti.ssl.key-store=/home/test/config/certs/peer0.keystore
local.parti.ssl.key-store-type=PKCS12
local.parti.ssl.key-alias=
local.parti.ssl.key-store-password=12345678
local.parti.ssl.trust-store=/home/test/config/certs/trust.jks
local.parti.ssl.trust-store-password=12345678
local.parti.ssl.trust-store-type=JKS
local.parti.ssl.protocol=TLSv1.2
local.parti.ssl.enabled-protocols=TLSv1.2
local.parti.ssl.ciphers=TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

```


共识节点启用TLS后， 需同步修改网关配置文件`gateway.conf`:

```
peer.consensus.secure=true
```



































