---
sidebar_position: 8
---

# send-back-logger.yml

物品送回日志记录“谁的物品在什么时候通过什么途径送回”。日志可输出到控制台、独立文件和数据库；数据库记录位于 `SendBackLogs` 表。

当前默认配置中日志总开关关闭，需要时手动开启。

```yaml title="send-back-logger.yml"
# 物品送回日志系统
readme: ''

# 总开关
enable: false

# 将物品送回输出到控制台
console: true

# 将物品送回输出到数据库中，可跨服
database: false

# 将物品送回输出到独立的文件中
file: false

# 独立的文件的位置,修改需重启生效
file-path: plugins\SakuraBind\log\send-back-log-%g-%u.log

# 独立的文件的最大数量，每个5M,修改需重启生效
file-max-count: 10

# 送回类型翻译
send-back-type-description:
  API: API
  COMMON_CALLBACK: callback命令
  COMMON_SUPER_CALLBACK: supercallback命令
  BLOCK_FROM_TO: 方块变化替换
  PLAYER_DROP: 玩家丢弃
  CONTAINER_BREAK: 容器破坏
  ITEM_DAMAGE: 物品实体被毁坏
  ITEM_DE_SPAWN: 物品实体自然消失
  PLAYER_DEATH: 玩家死亡
  PLAYER_PICKUP: 玩家捡起物品
  DROP: 掉落物保护
  CONTAINER_DROP: 容器掉落物中的物品保护
  SCANNER: 扫描器

# 日志忽略特定送回类型, 从 send-back-type-description 选择键
filter: []

# 日志显示格式
format: '[物品送回] 物主: {0} 类型: {1} 途径: {2} 物品: {3}'

# 物品显示格式, 替换上面format的 {3}
# 占位符分别为 类型、名字、数量、子ID
format-item: 物品 {0}{3} {1} x {2}
```
