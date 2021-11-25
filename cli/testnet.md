
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
                                  [--gw-port=<gwPort>] --gw-zip=<gwZip>
                                  [--home=<path>] --ledger-name=<ledgerName>
                                  --output=<output> --password=<password>
                                  [--peer-size=<peerSize>] --peer-zip=<peerZip>
                                  [--init-hosts=<initHosts>[,
                                  <initHosts>...]]... [--init-ports=<initPorts>
                                  [,<initPorts>...]]...
                                  [--peer-consensus-ports=<peerConsensusPorts>[,
                                  <peerConsensusPorts>...]]...
                                  [--peer-hosts=<peerHosts>[,
                                  <peerHosts>...]]...
                                  [--peer-manage-ports=<peerManagePorts>[,
                                  <peerManagePorts>...]]...
  -a, --algorithm=<algorithm>
                             Crypto algorithm
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
  -V, --version              Print version information and exit.
```

- `ledger-name` 账本名称
- `peer-size` 参与方节点个数
- `algorithm` 公私钥/签名算法，支持`ED25519`/`ECDSA`/`RSA`/`SM2`，默认`ECDSA`
- `init-hosts` 初始化时各节点使用的地址，默认`127.0.0.1`，可只填一个（所有节点使用相同`IP`地址），也可以填写与节点个数相等的地址列表（半角逗号相隔）
- `init-ports` 初始化时各节点使用的端口，默认`8800,8810,8820,8830`，可只填一个（所有节点使用相同端口），也可以填写与节点个数相等的地址列表（半角逗号相隔）
- `output` 配置文件输出目录，将在该目录下创建`peer-size`个`peer`目录和一个`gw`（网关）。
- `password` 私钥本地存储加密密码，初始化完成后可在`peer*/config/keys`目录下查看公私钥信息。
- `peer-hosts` 共识节点`IP`地址，默认`127.0.0.1`，可只填一个（所有节点使用相同`IP`地址），也可以填写与节点个数相等的地址列表（半角逗号相隔）
- `peer-consensus-ports` 共识节点共识端口，默认`10080,10082,10084,10086`，可只填一个（所有节点使用相同端口），也可以填写与节点个数相等的端口列表（半角逗号相隔）
- `peer-manage-ports` 共识节点管理端口，默认`7080,7081,7082,7083`，可只填一个（所有节点使用相同端口），也可以填写与节点个数相等的端口列表（半角逗号相隔）
- `peer-zip` `jdchain-peer-*.RELEASE.zip`文件完整路径
- `gw-zip` `jdchain-gateway-*.RELEASE.zip`文件完整路径

如创建四节点，单网关基于`BFT-SMaRt`共识、`KEYPAIR`身份认证模式的单机运行网络初始化配置：
```bash
./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip jdchain-peer-1.6.0.RELEASE.zip --gw-zip jdchain-gateway-1.6.0.RELEASE.zip --output /home/nodes
INIT CONFIGS FOR LEDGER INITIALIZATION SUCCESS: 

initializer addresses: 127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830 
peer addresses: 127.0.0.1:7080 127.0.0.1:7081 127.0.0.1:7082 127.0.0.1:7083 
consensus addresses: 127.0.0.1:10080 127.0.0.1:10081 127.0.0.1:10082 127.0.0.1:10083 127.0.0.1:10084 127.0.0.1:10085 127.0.0.1:10086 127.0.0.1:10087 
gateway port: 8080

more details, see /home/nodes
```

> 初始化使用的端口在初始化完成后会释放，其他端口在网络运行时会一直占用
>
> 单机初始化或者运行多个节点请确保使用的端口未被占用

如创建四节点，单网关基于`BFT-SMaRt`共识、`KEYPAIR`身份认证模式的单机初始化，多机运行网络初始化配置：
```bash
./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip jdchain-peer-1.6.0.RELEASE.zip --gw-zip jdchain-gateway-1.6.0.RELEASE.zip --output /home/nodes --peer-hosts 192.168.1.1,192.168.1.2,192.168.1.3,192.168.1.4
INIT CONFIGS FOR LEDGER INITIALIZATION SUCCESS: 

initializer addresses: 127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830 
peer addresses: 192.168.1.1:7080 192.168.1.2:7081 192.168.1.3:7082 192.168.1.4:7083 
consensus addresses: 192.168.1.1:10080 192.168.1.1:10081 192.168.1.2:10082 192.168.1.2:10083 192.168.1.3:10084 192.168.1.3:10085 192.168.1.4:10086 192.168.1.4:10087 
gateway port: 8080

more details, see /home/nodes
```
初始化完成后，共识网络将使用`192.168.1.1,192.168.1.2,192.168.1.3,192.168.1.4`作为各参与方地址，请在实际地址环境启动、运行`peer`节点。