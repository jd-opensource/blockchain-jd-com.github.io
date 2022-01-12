安装包下载，仅提供最近版本下载：

|  文件   | 校验（SHA-256）  | 更新时间 | 文件大小 |
|  ----  | ----  | ----  | ----  |
| [jdchain-peer-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.2.RELEASE.zip)  | f9e2ef01aae1e2da5507790e3557dbc195f4fbb4734008af3d576d8d5b6b570b | 2021/01/12  | 43.57M  |
| [jdchain-gateway-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.2.RELEASE.zip)  | 6ce5470cb1d4f426afa18490b73585287ebaa8e54fff1e81a01cd683b3b4f40c | 2021/01/12  | 63.30M  |

历史版本请编译源码获取

**关于版本升级**

- `1.6.1` > `1.6.2`移除`peer`中`libs`、`manager`、`system`，`gateway`中`lib`旧依赖，换成新版本依赖，重启所有节点即可。
- `1.6.0` > `1.6.1`移除`peer`中`libs`、`manager`、`system`，`gateway`中`lib`旧依赖，换成新版本依赖，替换`peer-startup.sh`脚本，替换`log4j2-peer.xml`配置，重启所有节点即可。
- `1.5.0` > `1.6.0`移除`peer`中`libs`、`manager`、`system`，`gateway`中`lib`旧依赖，换成新版本依赖，重启所有节点即可。

> 升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
> 数据无价，升级前做好备份，操作请谨慎！
