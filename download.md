> **管理工具(`manager-startup.sh`)不再推荐使用，1.6.3版本已完全移除管理工具相关代码，组网和节点管理请参照最新官方文档，使用命令行方式更安全更便捷！**

> **管理工具设计初衷仅做网络初始化使用，可执行节点启停操作，本身不涉及鉴权逻辑，使用旧版本管理工具的用户请停止管理工具服务进程或做好严格的外部访问控制。**

> **不要对外暴露管理工具访问地址！！！**

安装包下载，仅提供最近版本下载：

|  文件   | 校验（SHA-256）  | 更新时间 | 文件大小 |
|  ----  | ----  | ----  | ----  |
| [jdchain-peer-1.6.3.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.3.RELEASE-0411.zip)  | 4a15415b881ef595dcd9ef2ac9c92d5e9527b96f03c502ad757371f17b4a27c9 | 2022/03/18  | 107M  |
| [jdchain-gateway-1.6.3.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.3.RELEASE-0411.zip)  | e086b6b9b7dc0eba7fb312d04da60adcbb7d78e0f33c18be4d38f1c4380cbe4a | 2022/03/18  | 106M  |
| [jdchain-peer-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.2.RELEASE.zip)  | f9e2ef01aae1e2da5507790e3557dbc195f4fbb4734008af3d576d8d5b6b570b | 2022/01/12  | 43.57M  |
| [jdchain-gateway-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.2.RELEASE.zip)  | 6ce5470cb1d4f426afa18490b73585287ebaa8e54fff1e81a01cd683b3b4f40c | 2022/01/12  | 63.30M  |

更多历史版本请编译源码获取

**关于版本升级**

正常情况下版本向下兼容，升级只支持所有节点同时升级的方式。
替换`peer`和`gw`中`bin`、`lib`、`libs`、`system` jar包，移除`runtime/~credentials/`、`runtime/~<ledger-hash>/~credentials/`目录内数据即可。

> 升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
> 数据无价，升级前做好备份，操作请谨慎！
