---
sidebar_position: 2
---

# config.yml

此配置是插件的基础设置



插件默认开启物品绑定。为了节省不必要的性能开销，方块绑定 `block-listener` 和 实体绑定 `entity-listener` 是选择性开启的。



插件通过**UUID**来识别物品的**拥有者**，将该信息储存在NBT中，`nbt-path-uuid` 决定了该nbt的储存路径。

为了能够在解绑之后删除**绑定lore**，也会在NBT中储存绑定lore，`nbt-path-lore` 决定了该nbt的储存路径。

请确保以上路径不与其他插件冲突。



插件具有**保护绑定物品**的功能，可以通过**以下方式**通过`send-back-queue` 配置的方式按顺序将物品发放到指定的途径中，如果该方式达到上限则会将多余的物品发往下一个途径，如果最终还是有无法发放的物品则物品会**消失**。

* 掉落物掉入岩浆
* 掉落物掉入虚空
* 掉落物被点燃
* 掉落物超时消失
* 命令 `/sakurabind callback` 支持的方式找回
* 命令 `/sakurabind superCallback` 支持的方式找回
* `global-setting.yml` 或 `settings.yml`中配置的方式找回



配置预览

~~~ yaml title="config.yml"
# 识别绑定玩家的NBT路径
nbt-path-uuid: sakura_bind_uuid

# 识别绑定Lore的NBT路径,数据是玩家旧的lore
nbt-path-lore: sakura_bind_lore

# 遗失物品使用 SakuraMail 发送而不是暂存箱
sakuraMail-hook: false

# 如果要发送丢失物品邮件
# 填入SakuraMail的邮件id，丢失物品将会替换邮件的物品
# 按顺序替换，不够的将会删除, 多余的将会在另外的邮件里
mailId: ''

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
# player: 玩家背包 仅玩家在线有效
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
enable-setting-permission-check: true

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

# 物品读取设置的缓存时间(秒),建议值大于 扫描器时间
setting-cache-time: 3500

# 数据迁移设置, 从其他lore类型的绑定插件中读取数据重新绑定
# 启用将会定时扫描玩家背包和打开的容器
data-migration:
  
  # 是否启用
  enable: false
  
  # 扫描的周期, 单位tick
  period: 10
  
  # 从物品中读取lore，将其转换为SakuraBind的物品, 正则表达式
  # 默认为: '\[绑定]: (.*)' 可以匹配lore: '[绑定]: Iseason', 如果lore中有符号请在前面加上'\' 请将代表玩家名的地方使用()包围
  # 在这测试你的正则表达式: https://www.bejson.com/othertools/regex/
  lore:
  - '\[绑定]: (.*)'
  
  # lore中不是玩家名，而是玩家的uuid
  is-uuid: false
  
  # 如果检测不到这个玩家的数据, 强行绑定物品
  # 如果安装了AuthMe将尝试通过AuthMe获取UUID
  # 如果没有则使用该名字对应的离线UUID
  force-bind: false
  
  # 删除匹配到的lore
  remove-lore: true
  
  # 不绑定，如果打开了，且上面也是打开的，那么就是删除旧的绑定lore的效果
  dont-bind: false
  
  # 匹配到的设置,留空或者填写错误都会使用常规匹配
  setting: ''


~~~

