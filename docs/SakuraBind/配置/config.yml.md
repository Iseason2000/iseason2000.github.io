---
sidebar_position: 2
---

# config.yml

`config.yml` 是插件基础配置，控制监听器、扫描器、送回队列、暂存箱、命令行为和缓存。

物品绑定默认可用。方块绑定与实体绑定需要分别开启 `block-listener`、`entity-listener`，这两个开关会增加监听和缓存开销，修改后需要重启。

## 推荐先检查的项目

| 配置 | 建议 |
| --- | --- |
| `block-listener` | 不绑定方块时保持 `false`，需要方块绑定时再开启并重启 |
| `entity-listener` | 不绑定实体时保持 `false`，需要刷怪蛋/实体绑定时再开启并重启 |
| `scanner-period` | 需要自动绑定、扫描送回时保持大于 `0`；玩家多时不要设太低 |
| `send-back-database` | 建议保持 `true`，作为送回失败的兜底 |
| `send-back-queue` | 推荐至少包含 `database` |
| `enable-setting-permission-check` | 不需要权限覆盖配置时保持 `false`，减少权限系统依赖 |
| `setting-cache-size` | 建议约等于“在线玩家数 × 背包槽位数”，大型服可增大 |

遗失物品会按 `send-back-queue` 顺序送回。某一种途径放不下时，剩余物品会继续尝试下一个途径。

| 途径 | 说明 |
| --- | --- |
| `player` | 玩家背包，仅玩家在线且存活时有效 |
| `ender-chest` | 玩家末影箱，仅玩家在线时有效 |
| `database` | 插件暂存箱，需要 `send-back-database: true` |
| `GlobalMarketPlus` | GlobalMarketPlus 邮箱 |
| `SweetMail` | SweetMail 邮件 |

## 当前默认配置

```yaml title="config.yml"
# 识别绑定玩家的NBT路径
nbt-path-uuid: sakura_bind_uuid

# 识别绑定Lore的NBT路径,数据是玩家旧的lore
nbt-path-lore: sakura_bind_lore

# 登入时如果暂存箱有物品则提醒，此为延迟，单位tick, 设置小于0以关闭提示
login-message-delay: 100

# 方块物品检测开关，需要重启生效。打开才能支持方块物品，同时性能损耗也会增加
block-listener: false

# 实体检测开关，需要重启生效。打开才能支持实体绑定，同时性能损耗也会增加
entity-listener: false

# 定时扫描所有玩家背包, 此为扫描周期,单位tick，0表示关闭
# 此项关闭将影响 scanner开头的设置
scanner-period: 60

# 玩家禁用消息的冷却时间, 单位毫秒
message-coolDown: 1000

# 是否开启暂存箱功能, 使用插件自带数据库功能进行物品存储，关闭时会禁用数据库功能, 重启生效
send-back-database: true

# 丢失物品返还顺序, 满了才下一个顺序
# player: 玩家背包 仅玩家活着有效
# ender-chest: 末影箱 仅玩家在线有效
# database: 插件自带暂存箱
# GlobalMarketPlus: GlobalMarketPlus插件的邮箱
# SweetMail: SweetMail 邮件
send-back-queue:
- player
- ender-chest
- database

# 物品送回 GlobalMarketPlus 的设置
global-market-plus:
  # 邮件发送者名称
  name: 绑定系统
  # 邮件有效期, 单位 秒, -1 表示不过期
  expire: -1

# 物品送回 SweetMail 的设置
sweet-mail:
  # 邮件发送者名称
  sender: 绑定系统
  # 邮件图标, SweetMail的格式
  icon: BOOK
  # 邮件标题
  title: 绑定系统
  # 邮件内容,一行一页\n换行
  content:
  - |-
    以下是你丢失的绑定物品
    请查收
  # 邮件有效时间(秒) -1 无限
  expire: -1

# 启用配置权限检查, 在获取绑定设置前优先从权限中读取。
# 绑定全局设置权限 `sakurabind.settings.{键名}.true|false` 不支持`键名@`的形式
# 绑定设置权限 `sakurabind.setting.{设置名}.{键名}.true|false` 设置名是 `settings.yml` 匹配键,覆盖全局权限 不支持`键名@`
enable-setting-permission-check: false

# 识别到此NBT就自动绑定物主
auto-bind-nbt: sakura_auto_bind

# 选择命令的最大超时时间,单位毫秒
command-select-timeout: 30000

# 允许打开空的暂存箱
command-openLost-open-empty: false

# 暂存箱标题，支持placeholder
temp-chest-title: '&a{0} 的暂存箱'

# 当玩家退出登陆时优化暂存箱数据库(整理分散的物品数据)
# 全服5分钟内只能优化一次 每玩家2小时冷却
temp-chest-purge-on-quit: false

# 当主线程卡住时可能会导致放下的方块丢失绑定, 重启生效
thread-dump-protection:
  # 是否启用主线程卡顿监控
  enable: false
  # 主线程卡顿判断时间，单位秒
  timeout: 50
  # 插件加载之后多少秒开始检查主线程卡顿
  delay: 30
  # 当主线程卡顿时, 注销插件
  disable-plugin: false
  # 当主线程卡顿时, 关闭服务器
  stop-server: false

# 物品读取设置的缓存个数,建议值是 玩家背包格子数量*玩家数量
setting-cache-size: 2000

# 物品读取设置的缓存时间(毫秒),建议值大于 扫描器时间
setting-cache-time: 3500

# 大部分服务端核心在取消物品丢弃事件时没有对背包已满进行容错，会导致物品没有返回的空间而丢失
# 本插件提供在这种情况将物品找回的功能（按照send-back-queue的顺序）
# 有以下几种模式:
# none：关闭功能  bind-item：仅绑定物品  all：全部物品(有些插件取消事件的优先级较高可能会失效)
# 默认 none，可降低与服务端核心或其他插件冲突时造成复制问题的风险
replace-cancel-drop-event: none
```

:::tip[旧配置不会被覆盖]

如果旧版本已经生成过 `config.yml`，插件会补全缺失键，但不会覆盖已有值。

:::

## 常用配置片段

只启用物品绑定，并保留暂存箱兜底：

```yaml
block-listener: false
entity-listener: false
send-back-database: true
send-back-queue:
- player
- ender-chest
- database
```

开启方块和实体绑定：

```yaml
block-listener: true
entity-listener: true
```

修改后重启服务端。

使用 SweetMail 作为送回途径：

```yaml
send-back-queue:
- player
- SweetMail
- database
sweet-mail:
  sender: 绑定系统
  icon: BOOK
  title: 绑定物品返还
  content:
  - |-
    以下是你丢失的绑定物品
    请查收
  expire: -1
```

开启权限覆盖配置：

```yaml
enable-setting-permission-check: true
```

开启后可以用类似 `sakurabind.settings.item-deny.drop.false` 的权限覆盖布尔配置。
