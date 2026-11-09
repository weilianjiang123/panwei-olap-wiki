# 磐维 OLAP 现场问题 Wiki

从报错、等待事件或 SQL 操作进入相应条目。状态核对至 2026-11-09。

[系统目录与权限](#系统目录与权限) · [写入与表维护](#写入与表维护) · [查询与执行计划](#查询与执行计划) · [内存与进程异常](#内存与进程异常) · [连接与数据同步](#连接与数据同步) · [节点、存储与备份](#节点存储与备份)

**现场已恢复**表示现场确认操作后恢复。**暂时缓解**表示恢复了使用，仍需观察复发。**已定位**说明已找到问题发生的环节，**测试已验证**的范围见各条目的对照条件。**待验证**保留仍缺的证据。**操作说明**用于没有事故结论的使用问题。

## 系统目录与权限

| 问题或症状 | 当前状态 |
| --- | --- |
| [psql 用 `\d` 查看表结构很慢](catalog/describe-slow.md) | 现场已恢复 |
| [普通角色可见列保护策略的元数据](catalog/policy-visibility.md) | 测试已验证 |
| [删除角色时报仍有对象依赖](catalog/role-dependencies.md) | 已定位 |
| [安装扩展时默认表空间不合适](catalog/extension-tablespace.md) | 测试已验证 |

## 写入与表维护

| 问题或症状 | 当前状态 |
| --- | --- |
| [COPY 小批写入后目录文件数达到上限](storage/copy-directory-limit.md) | 暂时缓解 |
| [开启自动归档和清理后存储 I/O 升高](storage/archive-io.md) | 待验证 |
| [小表查询的大部分时间花在规划阶段](storage/timeline-planning.md) | 待验证 |
| [宽表批量写入吞吐偏低](storage/batch-ingest.md) | 待验证 |
| [Hudi 外表单分区查询报 Duplicate key](storage/hudi-duplicate-partition.md) | 已定位 |
| [TEXT 导入报 end-of-copy marker corrupt](storage/copy-text-marker.md) | 待验证 |
| [写入期间执行 Compact 会不会阻塞业务](storage/compact-concurrency.md) | 操作说明 |

## 查询与执行计划

| 问题或症状 | 当前状态 |
| --- | --- |
| [Hive 外表按月查询慢](query/monthly-external-scan.md) | 已定位 |
| [广播发送等待与慢 Nested Loop](query/broadcast-nested-loop.md) | 测试已验证 |
| [多指标报表因执行内存过高被取消](query/report-distinct-memory.md) | 待验证 |
| [窗口排序前能否先过滤大表](query/window-filter.md) | 待验证 |
| [空间关联广播大表并逐对计算](query/spatial-join.md) | 已定位 |
| [Hash 运算落盘后临时文件数超限](query/workfile-limit.md) | 已定位 |
| [怎样追查已经结束的慢 SQL](query/past-slow-query.md) | 操作说明 |
| [怎样理解执行计划中的两个 actual time](query/explain-time.md) | 操作说明 |

## 内存与进程异常

| 问题或症状 | 当前状态 |
| --- | --- |
| [长 IN 列表使协调节点规划内存持续增长](stability/planner-memory.md) | 测试已验证 |
| [主机 OOM 终止数据库进程](stability/host-oom.md) | 待验证 |
| [查询仍在规划时取消请求延迟生效](stability/planner-cancel.md) | 测试已验证 |
| [TRUNCATE 含高精度 Decimal 列的表时后端退出](stability/truncate-decimal.md) | 待验证 |
| [分布式查询在文本长度计算时后端退出](stability/text-expression-exit.md) | 待验证 |
| [Magma 进程因事务可见性检查失败退出](stability/magma-xmin.md) | 待验证 |
| [高并发读取 Parquet 外表时 JVM 堆接近上限](stability/parquet-jvm.md) | 待验证 |
| [自动记录系统目录查询计划时后端退出](stability/auto-explain-exit.md) | 待验证 |

## 连接与数据同步

| 问题或症状 | 当前状态 |
| --- | --- |
| [JDBC 停止读取后会话持续等待 ClientWrite](connection/jdbc-clientwrite.md) | 测试已验证 |
| [SSL 握手在执行查询前失败](connection/ssl-handshake.md) | 测试已验证 |
| [连接池初始化时报不支持数据库类型](connection/jdbc-dialect.md) | 已定位 |
| [同步作业报告数据库连接意外关闭](connection/unexpected-disconnect.md) | 待验证 |
| [同步在访问 Hive Metastore 时失败](connection/hive-metastore.md) | 已定位 |
| [同步读取源表字段信息时报错](connection/source-schema.md) | 待验证 |

## 节点、存储与备份

| 问题或症状 | 当前状态 |
| --- | --- |
| [数据节点离线前后 SQL 变慢](ops/hdfs-read-latency.md) | 待验证 |
| [主机重启后计算节点仍显示离线](ops/node-after-reboot.md) | 待验证 |
| [数据盘占用高并遗留查询临时目录](ops/temp-directories.md) | 待验证 |
| [系统盘日志改写到数据盘](ops/log-migration.md) | 操作说明 |
| [pg_dump 导出文件为零字节](ops/empty-dump.md) | 待验证 |
| [大库逻辑备份需要多少内存](ops/dump-memory.md) | 操作说明 |
| [计算资源组下线后仍有队列依赖](ops/virtual-cluster-retire.md) | 操作说明 |
| [纠删码 HDFS 冷存储接入](ops/cold-hdfs.md) | 操作说明 |

## 更新条目

同一症状有新的处置或回访结果时，更新原条目并同步索引状态，保留仍适用的版本和验证条件。新问题沿用“现象、判断依据、处置结果、待验证项”的写法。对外分享只使用本 Wiki 内的材料。
