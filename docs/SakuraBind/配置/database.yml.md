---
sidebar_position: 5
---

# database.yml

数据库用于暂存箱、绑定日志、物品送回日志和唯一物品日志。插件会在连接成功后自动创建所需数据表；数据库本身需要提前存在，或使用支持本地文件创建的 H2/SQLite。

支持的 `database-type`：

- `H2`
- `SQLite`
- `MySQL` / `MySQL5`
- `MySQL8`
- `MariaDB`
- `Oracle`
- `PostgreSQL`
- `SQLServer`

`MySQL` 等同于 `MySQL5`。如果 MySQL 连接经常失败，可以尝试改用 `MariaDB` 驱动连接 MySQL 服务端。

默认配置使用本地 H2，数据文件位于 `plugins/SakuraBind/database/database_sakurabind.mv.db`。小型单服可以直接使用 H2；多服共享暂存箱或需要跨服日志时，建议切换到 MySQL、MariaDB、PostgreSQL 等外部数据库。

```yaml title="database.yml"
# 修改完配置保存时是否自动重连数据库
autoReload: true

# 数据库驱动类型: 支持 MySQL、MySQL5、MySQL8、MariaDB、H2、SQLite、Oracle、PostgreSQL、SQLServer
# 默认 MySQL = MySQL5，如果你的 MySQL 总是连不上请将驱动类型改为 MariaDB，它支持连接到mysql
database-type: H2

# 数据库地址
address: plugins/SakuraBind/database

# 数据库名
database-name: database_sakurabind

# jdbcUrl 最后面的参数, 紧跟在database-name后面,请注意添加分隔符
# 如果你的mysql提示 ssl 有关的信息请添加: '?useSSL=false'
params: ''

# 完整的jdbcUrl由 address、database-name、params 根据数据库类型拼接而来
# 如果您发现拼接的url有误可以自定义url，留空则关闭
custom-jdbcUrl: ''

# 数据库用户名，如果有的话
user: user

# 数据库密码，如果有的话
password: password

# 数据表名前缀，如果与其他插件共用数据库可以修改表名前缀避免冲突, 重载生效
# 请注意，这意味着将创建新的数据表，旧的数据不会删除，但不会将数据转移到新的表中
table-prefix: ''

# 连接池设置，不懂不要乱调, 配置解释: https://github.com/brettwooldridge/HikariCP
data-source:
  autoCommit: true
  connectionTimeout: 30000
  idleTimeout: 600000
  keepaliveTime: 30000
  maxLifetime: 1800000
  connectionTestQuery: SELECT 1
  minimumIdle: 1
  maximumPoolSize: 5
  initializationFailTimeout: 1
  isolateInternalQueries: false
  allowPoolSuspension: false
  readOnly: false
  registerMbeans: false
  connectionInitSql: ''
  transactionIsolation: ''
  validationTimeout: 5000
  leakDetectionThreshold: 0
```

`custom-jdbcUrl` 非空时会覆盖插件按数据库类型拼接出的 JDBC 地址。除非你明确知道驱动需要什么 URL，否则优先使用 `address`、`database-name`、`params`。

## 常见数据库配置

本地 H2 默认配置：

```yaml
database-type: H2
address: plugins/SakuraBind/database
database-name: database_sakurabind
params: ''
```

MySQL 8 示例：

```yaml
database-type: MySQL8
address: 127.0.0.1:3306
database-name: sakurabind
params: '?useSSL=false&characterEncoding=utf8'
user: sakurabind
password: '你的密码'
```

MariaDB 示例：

```yaml
database-type: MariaDB
address: 127.0.0.1:3306
database-name: sakurabind
params: '?useSSL=false'
user: sakurabind
password: '你的密码'
```

## 运维注意

- 切换 `database-type` 后会按需下载对应驱动，离线服务器需要提前准备 `libraries`。
- 修改 `table-prefix` 会创建一套新表，不会自动迁移旧表数据。
- 数据库连接失败时，暂存箱、`getLost`、占位符中的遗失物品统计和数据库日志都会受影响。
- SQLite 在部分低版本 Spigot 上会与服务端内置旧驱动冲突；遇到加载异常时优先使用 H2 或外部数据库。
- 当前源码的 Oracle 默认 JDBC 地址拼接可能不适用于你的环境；如果使用 Oracle，建议显式填写 `custom-jdbcUrl`。
