
提供组网相关工具

```bash
:bin$ ./jdchain-cli.sh testnet -h
Usage: jdchain-cli testnet [-hV] [--pretty] [--home=<path>] [COMMAND]
Tools for testnet
  -h, --help          Show this help message and exit.
      --home=<path>   Set the home directory.
                        Default: ../config
      --pretty        Pretty json print
  -V, --version       Print version information and exit.
Commands:
  config  Generate testnet init configs.
  help    Displays help information about the specified command
```

- `config` 初始化配置

## 初始化配置

```bash
:bin$ ./jdchain-cli.sh testnet config -h
Generate testnet init configs.
Usage: jdchain-cli testnet config [-hV] [--pretty] [-a=<algorithm>]
                                  [-c=<consensus>] [--gw-port=<gwPort>]
                                  --gw-zip=<gwZip> [--home=<path>]
                                  --ledger-name=<ledgerName> --output=<output>
                                  --password=<password>
                                  [--peer-size=<peerSize>] --peer-zip=<peerZip>
                                  [--rabbit=<rabbit>] [--init-hosts=<initHosts>
                                  [,<initHosts>...]]...
                                  [--init-ports=<initPorts>[,
                                  <initPorts>...]]...
                                  [--peer-consensus-ports=<peerConsensusPorts>[,
                                  <peerConsensusPorts>...]]...
                                  [--peer-hosts=<peerHosts>[,
                                  <peerHosts>...]]...
                                  [--peer-manage-ports=<peerManagePorts>[,
                                  <peerManagePorts>...]]...
  -a, --algorithm=<algorithm>
                             Crypto algorithm
  -c, --consensus=<consensus>
                             Consensus, options: BFTSMART, RAFT, MQ
      --gw-port=<gwPort>     Port for gateway server
      --gw-zip=<gwZip>       Zip file of jdchain-gateway
  -h, --help                 Show this help message and exit.
      --home=<path>          Set the home directory.
      --init-hosts=<initHosts>[,<initHosts>...]
                             Hosts for initialization, input one (all the peers
                               use the same host) or peer-size(comma division)
      --init-ports=<initPorts>[,<initPorts>...]
                             Ports for initialization, input one (all the peers
                               use the same manage port) or peer-size(comma
                               division)
      --ledger-name=<ledgerName>
                             Ledger name
      --output=<output>      Output directory for initialized files
      --password=<password>  Raw password for keypair
      --peer-consensus-ports=<peerConsensusPorts>[,<peerConsensusPorts>...]
                             Ports for node consensus server, input one (all
                               the peers use the same consensus port) or
                               peer-size(comma division)
      --peer-hosts=<peerHosts>[,<peerHosts>...]
                             Hosts for nodes, input one (all the peers use the
                               same host) or peer-size(comma division)
      --peer-manage-ports=<peerManagePorts>[,<peerManagePorts>...]
                             Ports for node manage server, input one (all the
                               peers use the same manage port) or peer-size
                               (comma division)
      --peer-size=<peerSize> Size of peers
      --peer-zip=<peerZip>   Zip file of jdchain-peer
      --pretty               Pretty json print
      --rabbit=<rabbit>      RabbitMQ Server address for MQ consensus
  -V, --version              Print version information and exit.
```

- `ledger-name` 账本名称
- `consensus` 共识协议，`1.6.3`版本已支持：`BFTSMART`，`RAFT`，`MQ`
- `peer-size` 参与方节点个数，`BFTSMART`最少`4`个节点，`RAFT`、`MQ`均支持单节点模式
- `algorithm` 公私钥/签名算法，支持`ED25519`/`ECDSA`/`RSA`/`SM2`，默认`ECDSA`
- `init-hosts` 初始化时各节点使用的地址，默认`127.0.0.1`，可只填一个（所有节点使用相同`IP`地址），也可以填写与节点个数相等的地址列表（半角逗号相隔）
- `output` 配置文件输出目录，将在该目录下创建`peer-size`个`peer`目录和一个`gw`（网关）。
- `password` 私钥本地存储加密密码，初始化完成后可在`peer*/config/keys`目录下查看公私钥信息。
- `peer-hosts` 共识节点`IP`地址，默认`127.0.0.1`，可只填一个（所有节点使用相同`IP`地址），也可以填写与节点个数相等的地址列表（半角逗号相隔）
- `peer-consensus-ports` 共识节点共识端口，默认`10080`，可只填一个（所有节点使用相同端口），也可以填写与节点个数相等的端口列表（半角逗号相隔）
- `peer-manage-ports` 共识节点管理端口，默认`7080`，可只填一个（所有节点使用相同端口），也可以填写与节点个数相等的端口列表（半角逗号相隔）
- `gw-port`网关运行端口
- `peer-zip` `jdchain-peer-*.RELEASE.zip`文件路径
- `gw-zip` `jdchain-gateway-*.RELEASE.zip`文件路径
- `rabbit` 使用`MQ`共识方式时指定`RabbitMQ`地址端口

> `init-hosts`与`init-ports` 决定账本初始化`ledger-init`执行的真实地址，只在初始化过程中占用相应地址端口，初始化完成后释放

> `peer-hosts`、`peer-consensus-ports`、`peer-manage-ports` 配置决定了`peer`节点运行时地址端口，多机器部署时请使用真实可用的地址信息

> 该指令只初始化了一个网关，运行时端口由`gw-port`指定，需要部署多个网关的情况，可以复制网关（除runtime目录）的所有内容，修改`gateway.conf`里的运行地址端口信息即可

## 示例

### BFT-SMaRt

1. 初始化使用`BFT-SMaRt`共识协议，单机四节点单网关网络

```bash
:bin$ ./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --consensus BFTSMART --peer-size 4 --init-hosts 127.0.0.1 --peer-hosts 127.0.0.1 --peer-consensus-ports 10080,10082,10084,10086 --peer-manage-ports 7080,7081,7082,7083 --init-ports 8800,8810,8820,8830 --gw-port 8080 --output /home/imuge/jd/nodes/
```


2. 初始化使用`BFT-SMaRt`共识协议，单机初始化、多机运行四节点单网关网络：

```bash
:bin$ ./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --consensus BFTSMART --peer-size 4 --init-hosts 127.0.0.1 --peer-hosts 192.168.101.10,192.168.101.11,192.168.101.12,192.168.101.13 --peer-consensus-ports 10080,10082,10084,10086 --peer-manage-ports 7080,7081,7082,7083 --init-ports 8800,8810,8820,8830 --gw-port 8080 --output /home/imuge/jd/nodes/
```

> 使用本机执行`ledger-init.sh`，使用`不同地址端口`运行`peer-startup.sh`初始化启动网络，在执行`peer-startup.sh`前将对应的`peer*`文件复制到对应机器相同目录下，使用不同目录时请修改`ledger-binding.conf`中涉及到目录的配置。

3. 初始化使用`BFT-SMaRt`共识协议，多机初始化、多机运行四节点单网关网络：

```bash
:bin$ ./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --consensus BFTSMART --peer-size 4 --init-hosts 127.0.0.1 --peer-hosts 192.168.101.10,192.168.101.11,192.168.101.12,192.168.101.13 --peer-consensus-ports 10080,10082,10084,10086 --peer-manage-ports 7080,7081,7082,7083 --init-ports 8800,8810,8820,8830 --gw-port 8080 --output /home/imuge/jd/nodes/
```

> 使用多机执行`ledger-init.sh`，在执行`ledger-init.sh`前将`peer*`文件复制到对应机器相同目录下，使用不同目录时请修改`init`下`bftsmart.config`、`ledger.init`、`local.conf`中涉及到目录的配置。
> 使用`不同地址端口`运行`peer-startup.sh`初始化启动网络

### Raft

初始化使用`Raft`共识协议，单机单节点单网关网络
```bash
:bin$ ./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --consensus RAFT --peer-size 1 --init-hosts 127.0.0.1 --peer-consensus-ports 10080 --peer-manage-ports 7080 --init-ports 8800 --gw-port 8080 --output /home/imuge/jd/nodes/
```

### MQ

初始化使用`RabbitMQ`共识协议，单机单节点单网关网络
```bash
:bin$ ./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --rabbit 127.0.0.1:5672 --consensus MQ --peer-size 1 --init-hosts 127.0.0.1 --peer-consensus-ports 10080 --peer-manage-ports 7080 --init-ports 8800 --gw-port 8080 --output /home/imuge/jd/nodes/
```

> 执行`peer-startup.sh`前，请确保`rabbit`配置的`RabbitMQ`服务可用