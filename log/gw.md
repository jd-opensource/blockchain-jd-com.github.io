### 网关日志

`JD Chain`使用`log4j2`输出日志信息。

默认情况下，网关日志会使用`gw/config/log4j2-gw.xml`配置，默认输出到`gw/logs/gw.log`中，`gw/bin/gw.out`中也会有少许启动日志。

可通过修改`log4j2-gw.xml`文件重置网关日志配置。