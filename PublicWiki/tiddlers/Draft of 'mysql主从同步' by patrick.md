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

# 从库流程
## 一、mysql slave查询状态语句
```
SELECT @@GLOBAL.SERVER_ID #查询主库server_id,用于避免冲突
SELECT @@GLOBAL.GTID_MODE #查询master 的GTID_MODE，避免主库没有开GTID_MODE
SELECT @@GLOBAL.SERVER_UUID #查询master的server_uuid#避免server_uuid冲突
```
## 二、注册slave流程
```
mysql源码：register_slave #注册从库
request_dump # 发送slave的配置数据给master dump线程
com_binlog_dump_gtid #master调用此函数来处理
```
随后就开始等待master发送Event



