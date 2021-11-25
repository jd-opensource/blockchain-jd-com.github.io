高级检索提供JD Chain区块链基础数据索引、自定义键值索引服务

源码已开源，请参照[jdchain-indexer](https://github.com/blockchain-jd-com/jdchain-indexer)首页说明安装使用。

修改JD Chain网关配置文件`gateway.conf`中：
- `data.retrieval.url` 对应`Argus`中区块链基础数据检索服务
- `schema.retrieval.url` 对应`Argus`中`Schema`服务

重启网关后可在区块连浏览器中使用搜索功能