---
sidebar_position: 5
---

# database.yml

> 该配置是数据库配置，由暂存箱和绑定日志使用



插件支持 MySQL、MariaDB、H2、SQLite、Oracle、PostgreSQL、SQLServer 数据库，会在链接时**自动创建不存在的数据库**



~~~ yaml title="database.yml"

# 修改完配置保存时是否自动重连数据库
autoReload: true

# 数据库驱动类型: 支持 MySQL、MariaDB、H2、SQLite、Oracle、PostgreSQL、SQLServer
# 如果你的 MySQL 总是连不上请将驱动类型改为 MariaDB，它支持连接到mysql
database-type: H2

# 数据库地址
address: E:\mc\purpur-1.20\plugins\SakuraBind\database

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



~~~
