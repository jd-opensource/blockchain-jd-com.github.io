安装包下载，仅提供最近版本下载：

|  文件   | 校验（SHA-256）  | 更新时间 | 文件大小 |
|  ----  | ----  | ----  | ----  |
| [jdchain-peer-1.6.3.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.3.RELEASE.zip)  | 4860ffbbc959edb5a354fe2000527010193e09c942791d709860e7e733810eeb | 2021/03/18  | 107M  |
| [jdchain-gateway-1.6.3.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.3.RELEASE.zip)  | a42ec6241eda16c4ca500d94f30bcd2dabd11788ed2290d4fc02e933b440b4ae | 2021/03/18  | 106M  |
| [jdchain-peer-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.2.RELEASE.zip)  | f9e2ef01aae1e2da5507790e3557dbc195f4fbb4734008af3d576d8d5b6b570b | 2021/01/12  | 43.57M  |
| [jdchain-gateway-1.6.2.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.2.RELEASE.zip)  | 6ce5470cb1d4f426afa18490b73585287ebaa8e54fff1e81a01cd683b3b4f40c | 2021/01/12  | 63.30M  |

更多历史版本请编译源码获取

**关于版本升级**

正常情况下版本向下兼容，升级只支持所有节点同时升级的方式。
替换`peer`和`gw`中`bin`、`lib`、`libs`、`system` jar包，移除`runtime/~credentials/`、`runtime/~<ledger-hash>/~credentials/`目录内数据即可。

> 升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
> 数据无价，升级前做好备份，操作请谨慎！
