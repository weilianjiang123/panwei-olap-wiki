# psql 用 `\d` 查看表结构很慢

**现场已恢复**

第一次维护对象注释目录后，查看表结构恢复。后来相同症状再现，只处理注释目录没有解决。对后续列出的系统目录都执行 COMPACT FULL 后，现场确认恢复。

`\d` 会展开为多条系统目录查询。一次检查中，读取列定义以及反复查询类型、列注释占去了大部分时间，业务表自身的数据量不能解释这段耗时。

| 这次涉及的目录 | 用途 |
| --- | --- |
| `pg_description` | 表和列的注释。首次处理的对象。 |
| `pg_attribute`、`pg_type` | 列定义和类型。后续计划中观察到较重的目录访问。 |
| `pg_attrdef`、`pg_collation`、`pg_attribute_encoding` | 列默认值、排序规则和列编码。 |
| `pg_class`、`pg_policy` | 关系基本信息和行级策略。 |

下次出现时，可在 psql 会话开启 `\set ECHO_HIDDEN on`，重新执行原来的 `\d` 或 `\d+`，按耗时找到受影响的目录查询。对照维护前后同一条命令的耗时与显示内容。[psql 的 ECHO_HIDDEN 说明](https://www.postgresql.org/docs/current/app-psql.html)

此次恢复是在多张目录表完成维护后确认的，无法把效果归到某一张。系统目录上的 FULL 维护涉及锁与并发影响，应按版本和维护窗口选择范围。

[返回问题索引](../README.md)
