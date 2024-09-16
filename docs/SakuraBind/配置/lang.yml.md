---
sidebar_position: 6
---

# lang.yml

> 语言文件，支持丰富的消息格式



* 消息留空将不会显示，使用 '\n' 或换行符 可以换行

* 支持 & 颜色符号，1.17以上支持16进制颜色代码，如 #66ccff

* {0}、{1}、{2}、{3} 等格式为该消息独有的变量占位符

* 所有消息支持PlaceHolderAPI

  

**以下是一些特殊消息, 大小写不敏感，可以通过 \n 自由组合**

* 以 `[Broadcast]` 开头将以广播的形式发送，支持BungeeCord
* 以 `[Actionbar]` 开头将发送ActionBar消息
* 以 `[Title]` 开头将发送标题消息,格式为 大标题\n小标题
* 以 `[Command]` 开头将以消息接收者的身份运行命令
* 以 `[Console]` 开头将以控制台的身份运行命令
* 以 `[OP-Command]`开头将赋予消息接收者临时op运行命令 (慎用)



## 颜色代码

https://minecraft.fandom.com/zh/wiki/%E6%A0%BC%E5%BC%8F%E5%8C%96%E4%BB%A3%E7%A0%81



支持 将 `&` 转化为 `§` 以方便配置

在 Minecraft 1.16 之后，支持 16进制颜色代码，如 `#66ccff` 或 `#fff`，会自动转化为 `§x§6§6§c§c§f§f` 或 `§x§f§f§f§f§f§f`

## MiniMessage

>  将 `system.mini-message` 打开可以使用 **MiniMessage**
>
>  使用MiniMessage 将不支持&符号，第一次开启将会自动下载依赖


MiniMessage教程: https://docs.advntr.dev/minimessage/format.html
网页可视化: https://webui.advntr.dev/

## 例子

### 例子一

发送消息`回到主城` 给玩家的同时以玩家的身份运行命令 `/spawn`, 并在 actionbar 显示 `已回到主城`

~~~ yaml
message: |-
  [Command] spawn
  [Actionbar] 已回到主城
  回到主城
~~~

### 例子二

给玩家发送多行消息:

* 你好 %player%，这是第一行消息
* 这是第二行消息

~~~ yaml
message: |- 
  你好 %player%，这是第一行消息
  这是第二行消息
~~~



---

~~~ yaml title="lang.yml"

# 消息留空将不会显示，使用 '\n' 或换行符 可以换行
# 支持 & 颜色符号，1.17以上支持16进制颜色代码，如 #66ccff
# {0}、{1}、{2}、{3} 等格式为该消息独有的变量占位符
# 所有消息支持PlaceHolderAPI
# 以下是一些特殊消息, 大小写不敏感，可以通过 \n 自由组合
# 以 [Broadcast] 开头将以广播的形式发送，支持BungeeCord
# 以 [Actionbar] 开头将发送ActionBar消息
# 以 [Title] 开头将发送标题消息,格式为 大标题\n小标题
# 以 [Command] 开头将以消息接收者的身份运行命令
# 以 [Console] 开头将以控制台的身份运行命令
# 以 [OP-Command] 开头将赋予消息接收者临时op运行命令 (慎用)
readme: ''

# 系统消息设置
system:
  
  # 消息前缀
  msg-prefix: '&a[&6SakuraBind&a] &f'
  
  # 控制台消息前缀
  log-prefix: '&a[&6SakuraBind&a] &f'
  
  # 是否使用 MiniMessage 模式, 同时不支持&符号, 第一次开启将会自动下载依赖
  # 格式: https://docs.advntr.dev/minimessage/format.html
  # 网页可视化: https://webui.advntr.dev/
  mini-message: false

# 物品送回消息, 占位符 {0} 是送回的物品数量
send-back:
  player-all: '&7你的{0}个遗失物品已全部放入你的&a背包'
  player-half: '&7你的{0}个遗失物品放入你的&a背包'
  ender-chest-all: '&7你的{0}个遗失物品已全部放入你的&a末影箱'
  ender-chest-half: '&7你的{0}个遗失物品已放入你的&a末影箱'
  database-all: '&7你的{0}个遗失物品已全部放入你的&a暂存箱'
  global-market-plus: '&7你的{0}个遗失物品已全部放入你的&a全球市场邮箱'
item-bind-hand: '&7你手上的物品已绑定'
block-bind: '&7你前面的方块已绑定'
entity-bind: '&7你前面的实体已绑定'
item-bind-all: '&7你的背包物品已绑定'
item-unbind-hand: '&7你手上的物品已解绑'
block-unbind: '&7你前面的方块已解绑'
entity-unbind: '&7你手上的物品已解绑'
item-unbind-all: '&7你的背包物品已解绑'
entity-bind-on-spawner-egg: '&6你的生物已绑定!'
item:
  deny-command: '&6你不能拿着此物品输入该命令!'
  deny-drop: '&6该物品不能丢出!'
  deny-pickup: '&6该物品属于 &a{0},&6已归还'
  deny-itemFrame: '&6该物品禁止放入展示框!'
  deny-interact-left: '&6该物品禁止交互!'
  deny-interact-right: '&6该物品禁止交互!'
  deny-left-click-at-bind-block: '&6该物品禁止右键交互绑定方块!'
  deny-right-click-at-bind-block: '&6该物品禁止右键交互绑定方块!'
  deny-armor-stand-set: '&6该物品禁止放入盔甲架!'
  deny-armor-stand-get: '&6该物品禁止从盔甲架取出!'
  deny-entity-interact: '&6该物品禁止与实体交互!'
  deny-click: '&6你不能拿走这个物品!'
  deny-inventory: '&6此物品禁止放入这个容器!'
  deny-throw: '&6该物品禁止投掷!'
  deny-consume: '&6该物品禁止消耗!'
  deny-craft: '&6该物品禁止用于合成!'
  deny-anvil: '&6该物品禁止用于铁砧!'
  deny-container-break: '&6该容器含有绑定物品，禁止破坏!'
auto-bind:
  onClick: '&a此物品已绑定你的灵魂!'
  onPickup: '&a你捡到了与你相性最好的物品!'
  onDrop: '&a你刚刚丢弃的物品将永远属于你!'
  onScanner: '&a你背包有适合你的物品，已绑定你的灵魂!'
  onUse: '&a此物品已绑定你的灵魂!'
  onLeft: '&a此物品已绑定你的灵魂!'
  onRight: '&a此物品已绑定你的灵魂!'
  onEquiped: '&a你刚刚装备的物品已绑定'
auto-unbind:
  onClick: '&6此物品已解除绑定!'
  onPickup: '&6此物品已解除绑定!'
  onDrop: '&6此物品已解除绑定!'
  onScanner: '&6你背包的部分物品已解除绑定!'
  onUse: '&6此物品已解除绑定!'
  onLeft: '&6此物品已解除绑定!'
  onRight: '&6此物品已解除绑定!'
  onEquiped: '&6你刚刚装备的物品已解绑'
block:
  deny-break: '&6你不能破坏这个方块,这个方块属于 &b{0}'
  deny-place: '&6你不能放置这个方块，此物物品已绑定'
  deny-interact: '&6你没有这个方块的使用权限,这个方块属于 &b{0}'
entity:
  deny-damage: '&6你不能伤害这个实体，这个实体属于 &b{0}'
  deny-interact: '&6你没有这个实体的交互,这个实体属于 &b{0}'
scanner-item-send-back: '&6检测到你的背包存在别人的物品，已归还物主!'

# 命令消息
command:
  bind-item: '&a已绑定 &b{0} &a手上的物品'
  bind-block: '&a已绑定 &b{0} &a前面的方块'
  bind-entity: '&a已绑定 &b{0} &a前面的实体'
  bindTo-item: '&a已将手上的物品绑定至 &b{0}'
  bindTo-block: '&a已将前方的方块绑定至 &b{0}'
  bindTo-entity: '&a已将前方的实体绑定至 &b{0}'
  unbind-not-bind: '&6该目标并未绑定!'
  unbind-item: '&a已将 &b{0} 手上的物品解绑'
  unbind-block: '&a已将 &b{0} 前方的方块解绑'
  unbind-entity: '&a已将 &b{0} 前方的实体解绑'
  bindAll: '&a已绑定 &b{0} 整个背包的物品'
  unbindAll: '&a已解绑 &b{0} 整个背包的物品'
  getLost-item: '&a你领取了 &b{0} &a个遗失物品,请腾出空间领取剩余物品!'
  getLost-empty: '&6没有遗失物品!'
  getLost-full: '&6背包空间不足!'
  getLost-all: '&a你领取了所有的遗失物品! 共 &b{0} &a个物品'
  getLost-coolDown: '&6你输入得太快了!'
  openLost-empty: '&6该玩家没有遗失的物品!'
  openLost-page-not-exist: '&6没有这么多页,最多{0}页!'
  openLost-open: '&6你打开了玩家 {0} 的暂存箱第 {1} 页'
  openLost-closed: '&6玩家 {0} 暂存箱已更新'
  select-on: "&a已经开启目标选择模式,右键选择 物品|方块|实体 \n&6输入/sakurabind select bind 确认绑定 \n&6再次\
    输入命令取消选择"
  select-off: '&6已经开启目标选择模式已关闭'
  select-select-item: '&6你选择了手上的物品 {0} x {1}'
  select-select-block: '&6你选择了点击的方块 {0} ({1},{2},{3})'
  select-select-entity: '&6你选择了点击的实体 {0} ({1},{2},{3})'
  select-has-bind: '&6该目标已经绑定，无法选择'
  select-no-selected: '&6你没有选择目标!'
  select-bind: '&6已绑定目标!'
  select-timeout: '&6长时间未确认绑定，已退出选择模式!'
  autoBind: '&a已添加 &b{0}'
  test:
    match-start: '&6计划匹配物品 &b{0} &6次, 请等待...'
    match: '&a匹配物品 &b{0} &a次, 设置为: &6{1} &7耗时: {2} 毫秒 / {3} 纳秒'
    try-match-not-found: '&6该设置不存在'
    try-match-header: |-
      &4<--------使用设置 &6{0} &4匹配物品-------->
      &6  - [选项]: {模板} -> {物品内容} = {对比结果}
    try-match-name: '&a  - 名字: &f{0} -> &7{1} = &6{2}'
    try-match-name-strip: '&a  - 名字(去色): &f{0} -> &7{1} = &6{2}'
    try-match-lore: '&a  - Lore[{0}]: &f{1} -> &7{2} = &6{3}'
    try-match-lore-strip: '&a  - 去色Lore[{0}]: &f{1} -> &7{2} = &6{3}'
    try-match-material-pattern: '&a  - 材质正则: &f{0} -> &7{1} = &6{2}'
    try-match-material-set: '&a  - 材质集合: &f{0} -> &7{1} = &6{2}'
    try-match-material-id: '&a  - 材质:子ID: &f{0} -> &7{1} = &6{2}'
    try-match-ids: '&a  - ID: &f{0} -> &7{1} = &6{2}'
    try-match-nbt: '&a  - NBT: &f{0} -> &7{1} = &6{2} &7路径: {3}'
    try-match-mmoitems: '&a  - MMOItems: &f{0} -> &7{1} = &6{2}'
    try-match-itemsadder: '&a  - ItemsAdder: &f{0} -> &7{1} = &6{2}'
    try-match-oraxen: '&a  - Oraxen: &f{0} -> &7{1} = &6{2}'
    try-match-result: '&6匹配结束, 结果: &6{0}'
  callback-on: '&a已开启物品召回模式，绑定您的物品将会陆续返回，再次输入命令关闭'
  callback-off: '&6已关闭物品召回模式!'
  callback: '&6该物品已被物主召回'
  super-callback-start: '&7开始搜索 &6{0} &7的物品...'
  super-callback-backpacks: '&7已搜索 &6{0} &7个玩家背包, 找到目标: &a{1} &7个, 耗时: &b{2} &7ms'
  super-callback-sync: '&7已同步获取掉落物和容器数据, 耗时: &b{0} &7ms'
  super-callback-drops: '&7已搜索 &6{0} &7个掉落物, 找到目标: &a{1} &7个, 耗时: &b{2} &7ms'
  super-callback-containers: '&7已搜索 &6{0} &7个容器, 找到目标: &a{1} &7个, 耗时: &b{2} &7ms'
  super-callback-end: '&a搜索结束, 已送回 &b{0} &a堆 共 &b{1} &a个物品, 耗时 {2} ms'
  super-callback-player: '&a管理员为你找回了所有 &b{0} &a堆 共 &b{1} &a个绑定物品'
  debug: '&aDebug模式: &b{0}'
  debug-player-open: '&a已为开启玩家 {0} 的动作检查'
  debug-player-close: '&a已关闭玩家 {0} 的动作检查'
has-lost-item: '&a你有遗失的物品,请输入 &6''/sakurabind getLost'' &a领取'
lost-item-send-when-online: '&a你有遗失的物品,请输入 &6''/sakurabind getLost'' &a领取'
bind-item:
  not-owner: '&6你不是这个物品的主人, 无法解绑'
  unbind-failure: '&6解绑失败'
  unbind-success: '&a解绑成功'
  bind-failure: '&6绑定失败'
  bind-success: '&a绑定成功'
  no-amount: '&6物品数量不足'
addons:
  mcmmo-salvage: '&6该物品无法分解'
  mcmmo-repair: '&6该物品无法修复'
  mcmmo-disarm: '&6该物品无法缴械'


~~~
