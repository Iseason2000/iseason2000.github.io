---
sidebar_position: 7
---

# logger.yml

> 日志系统，记录绑定日志



每个绑定行为都有自己的枚举，在 `bind-type-description-v2` 中



支持将绑定信息输出到`控制台`、`独立文件`、`数据库`中。



其中数据库需要你自己使用第三方工具查看，位于`BindLogs` 表中



输出到`控制台`的绑定日志不支持 `lang.yml` 中的格式，仅输出格式化后的信息，但是支持相同的**颜色代码**



---

~~~ yaml title="lang.yml"

# 日志系统，用于记录绑定信息
readme: ''

# 总开关
enable: false

# 将绑定信息输出到控制台
console: true

# 将绑定信息输出到数据库中，可跨服
database: false

# 将绑定信息输出到独立的文件中
file: false

# 独立的文件的位置,修改需重启生效
file-path: plugins\SakuraBind\log\bind-log-%g-%u.log

# 独立的文件的最大数量，每个1M,修改需重启生效
file-max-count: 10

# 日志忽略特定绑定类型, 从下面选
filter: 
- ITEM_TO_BLOCK_BIND
- ITEM_TO_BLOCK_UNBIND
- BLOCK_TO_ENTITY_BIND
- BLOCK_TO_ENTITY_UNBIND
- BLOCK_MOVE_BIND
- BLOCK_MOVE_UNBIND
- ENTITY_TO_ITEM_BIND
- ENTITY_TO_ITEM_UNBIND
- ITEM_TO_ENTITY_BIND
- ITEM_TO_ENTITY_UNBIND
- BLOCK_TO_ITEM_BIND
- BLOCK_TO_ITEM_UNBIND
- ENTITY_TO_BLOCK_BIND
- ENTITY_TO_BLOCK_UNBIND

# 绑定类型翻译
bind-type-description-v2:
  UNKNOWN_BIND_ITEM: 未知的方式绑定物品
  UNKNOWN_UNBIND_ITEM: 未知的方式解绑物品
  UNKNOWN_BIND_BLOCK: 未知的方式绑定方块
  UNKNOWN_UNBIND_BLOCK: 未知的方式解绑方块
  UNKNOWN_BIND_ENTITY: 未知的方式绑定实体
  UNKNOWN_UNBIND_ENTITY: 未知的方式解绑实体
  API_BIND_ITEM: API绑定物品
  API_UNBIND_ITEM: API解绑物品
  API_BIND_BLOCK: API绑定方块
  API_UNBIND_BLOCK: API解绑方块
  API_BIND_ENTITY: API绑定实体
  API_UNBIND_ENTITY: API解绑实体
  COMMAND_BIND_ITEM: 命令绑定物品
  COMMAND_UNBIND_ITEM: 命令解绑物品
  COMMAND_BIND_BLOCK: 命令绑定方块
  COMMAND_UNBIND_BLOCK: 命令解绑方块
  COMMAND_BIND_ENTITY: 命令绑定实体
  COMMAND_UNBIND_ENTITY: 命令解绑实体
  SCANNER_BIND_ITEM: 扫描器绑定物品
  SCANNER_UNBIND_ITEM: 扫描器解绑物品
  CLICK_BIND_ITEM: 点击物品绑定物品
  CLICK_UNBIND_ITEM: 点击物品解绑物品
  PICKUP_BIND_ITEM: 捡起物品绑定物品
  PICKUP_UNBIND_ITEM: 捡起物品解绑物品
  DROP_BIND_ITEM: 丢弃物品绑定物品
  DROP_UNBIND_ITEM: 丢弃物品解绑物品
  ITEM_TO_BLOCK_BIND: 物品转为方块绑定
  ITEM_TO_BLOCK_UNBIND: 物品转为方块解绑
  BLOCK_TO_ENTITY_BIND: 方块转为实体绑定
  BLOCK_TO_ENTITY_UNBIND: 方块转为实体解绑
  BLOCK_MOVE_BIND: 方块移动绑定
  BLOCK_MOVE_UNBIND: 方块移动解绑
  ENTITY_TO_ITEM_BIND: 实体转为物品绑定
  ENTITY_TO_ITEM_UNBIND: 实体转为物品解绑
  ITEM_TO_ENTITY_BIND: 物品转为实体绑定
  ITEM_TO_ENTITY_UNBIND: 物品转为实体解绑
  BLOCK_TO_ITEM_BIND: 方块转为物品绑定
  BLOCK_TO_ITEM_UNBIND: 方块转为物品解绑
  ENTITY_TO_BLOCK_BIND: 实体转为方块绑定
  ENTITY_TO_BLOCK_UNBIND: 实体转为方块解绑
  USE_BIND_ITEM: 消耗耐久物品绑定物品
  USE_UNBIND_ITEM: 消耗耐久解绑物品
  LEFT_BIND_ITEM: 拿着物品左键绑定物品
  LEFT_UNBIND_ITEM: 拿着物品左键解绑物品
  RIGHT_BIND_ITEM: 拿着物品右键绑定物品
  RIGHT_UNBIND_ITEM: 拿着物品右键解绑物品
  MIGRATION_FROM_LORE_ITEM: 从其他插件lore迁移
  MIGRATION_FROM_NBT_ITEM: 从其他插件NBT迁移
  EQUIP_BIND_ITEM: 装备物品时绑定
  EQUIP_UNBIND_ITEM: 装备物品时解绑
  BIND_ITEM_BIND_ITEM: 绑定物品功能-绑定物品
  BIND_ITEM_UNBIND_ITEM: 绑定物品功能-解绑物品

# 日志显示格式
format: '物主: {0} 行为: {1} 配置: {2} 信息: {3}'

# 物品显示格式, 替换logger.format的 {3}
# 占位符分别为 类型、名字、数量、子ID
format-item: 物品 {0}{3} {1} x {2}

# 方块显示格式, 替换logger.format的 {3}
# 占位符分别为 类型、位置
format-block: '方块: {0} {1}'

# 实体显示格式, 替换logger.format的 {3}
# 占位符分别为 类型、名字、UUID、位置
format-entity: '实体: {0} {1} {2} {3} {4}'


~~~
