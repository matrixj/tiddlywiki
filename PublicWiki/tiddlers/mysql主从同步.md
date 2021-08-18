# 主库Dump线程流程

```
#查看主从连接状态
select * from information_schema.processlist;
```

POSITION MODE:使用master log name 和master log position 在主库的binnary log 定位
GTID AUTO_POSITION MODE: 使用GTID SET在主库binary log中定位。但GTID SET与relay_log_recovery 的设置有关，为ON时，Retrieved_Gtid_Set不会被使用

AUTO_POSITION MODE发送到master的数据相关代码：
```c
gtid_executed.add_gtid_set(xxx);//Retrieved_Gtid_Set
gtid_executed.add_gtid_set(xxx);//Executed_Gtid_Set
```
如果使用GTID MODE,但不设置AUTO_POSITION, 将使用POSITION MODE的定位方法

