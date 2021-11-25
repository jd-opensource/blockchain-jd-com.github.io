### PEER节点日志

`JD Chain`使用`log4j2`输出日志信息。

默认情况下，网关日志会使用`peer*/config/log4j2-peer.xml`配置，默认输出到`peer*/logs/peer.log`中，`peer*/bin/peer.out`中也会有少许启动日志。

可通过修改`log4j2-peer.xml`文件重置网关日志配置。