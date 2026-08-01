---
sidebar_position: 3
---

# global-setting.yml

`global-setting.yml` 是绑定行为的全局默认配置，优先级最低。`settings.yml` 中匹配到的独立设置会覆盖同名项；开启 `config.yml` 的 `enable-setting-permission-check` 后，权限覆盖优先级最高。

布尔键可以在键名后加 `@`，表示物主读取相反值。例如：

```yaml
item-deny:
  click@: true
```

含义是非物主不能从容器拿走物品，物主可以拿走。

:::warning[注意]

不要在全局直接开启 `auto-bind.enable` 后再开启大量触发项，否则普通物品也可能被批量绑定。自动绑定通常应写在 `settings.yml` 的具体匹配器下。

:::

## 节点速查

| 节点 | 作用 |
| --- | --- |
| `item` | 绑定 lore、丢失送回、扫描送回 |
| `item-unbind` | 解绑后是否添加 lore |
| `item-deny` | 禁止绑定物品被丢弃、拿走、捡起、合成、铁砧、消耗、放入容器等 |
| `block-deny` | 禁止绑定方块被破坏、放置、互动、爆炸、活塞、流体破坏等 |
| `entity` | 绑定实体名称、刷怪蛋检测、死亡掉落绑定、守护逻辑 |
| `entity-deny` | 禁止绑定实体被攻击、交互、掉落等 |
| `auto-bind` | 自动绑定触发点 |
| `auto-unbind` | 自动解绑触发点 |
| `module` | 附加模块相关开关，例如 `unique-item` 数量 |
| `on-bind` | 绑定成功后给物主发送额外消息 |
| `addons` | 第三方插件兼容限制，例如 mcMMO |

## 常用限制组合

默认配置中 `item-deny.craft` 是 `true`，绑定物品默认不能参与合成。如果你的玩法允许绑定物品合成，需要改成：

```yaml
item-deny:
  craft: false
```

只允许物主操作绑定物品：

```yaml
item-deny:
  drop@: true
  click@: true
  pickup@: true
```

禁止绑定物品进入指定容器：

```yaml
item-deny:
  inventory: true
  inventory-pattern:
  - '^垃圾桶$'
```

开启扫描器发现他人物品时送回：

```yaml
item:
  send-back-scanner: true
```

禁止死亡掉落绑定物品：

```yaml
item-deny:
  drop-on-death: true
```

该项只在服务器没有开启死亡不掉落规则时参与处理。

## 第三方插件自动绑定

以下触发项都要求目标物品匹配到的设置中同时存在 `auto-bind.enable: true`。建议把它们写进 `settings.yml` 的具体匹配器，不要全局开启：

| 配置键 | 触发时机 | 绑定对象 |
| --- | --- | --- |
| `auto-bind.onNeigeItemsGive` | NeigeItems 物品或物品包给予事件 | 接收物品的玩家 |
| `auto-bind.onMMOItemsDrop` | MMOItems 掉落表生成物品 | 挖掘者或击杀者（事件来源必须是玩家） |
| `auto-bind.onMythicMobDeath` | MythicMobs 生物死亡，或 NeigeItems 生成 MythicMobs 普通/钓鱼掉落 | 玩家击杀者或 NeigeItems 掉落接收者 |

拥有 `sakurabind.bypass.all` 的玩家会跳过这些自动绑定。带有 `auto-bind-nbt` 的物品也会在这些事件中尝试绑定，即使对应触发键为 `false`，但仍要求 `auto-bind.enable: true`。

## 当前默认配置

<details>
<summary>展开查看完整默认配置</summary>

```yaml title="global-setting.yml"
# 本配置为绑定的全局权限设置，优先级最低
# 在布尔类型的选项之后加上@则表示对于物主采取相反的结果,部分没有提示消息的无效
# 如 item-deny.click@: true 表示仅允许物主拿走容器内的物品
# 如 item-deny.drop: true 表示禁止所有人丢弃绑定物品
# 如 block-deny.place@: true 表示禁止所有人放置方块，但允许物主放置
global-setting: ''

# 物品设置
item:
  # 显示的lore,玩家名称占位符为 %player%
  lore:
  - '&a灵魂绑定: &6%player%'
  # 显示的lore位置, 设置足够大可以放在最后面，-2表示倒数第3行
  lore-index: 0
  # 如果删除匹配的lore则以绑定lore替换之(覆盖lore-index)
  lore-replace-matched: true
  # 当物品丢失时(掉虚空、消失等)归还物主(在线则发背包，否则发邮件)
  send-when-lost: true
  # 当容器被破坏时将绑定物品归还物主(在线则发背包，否则发邮件)
  send-when-container-break: true
  # 当捡到不属于你的绑定物品时是否送回原物主, 前提是 item-deny.pickup 开启
  send-back-on-pickup: true
  # 当物品作为掉落物时延迟多少tick还物主(在线则发背包，否则发邮件), 0表示立马返回，-1关闭
  send-back-delay: -1
  # 当扫描器扫描物品时如果发现物品不是该玩家的就返还物主
  send-back-scanner: true

# 物品解绑设置
item-unbind:
  # 解绑之后添加lore,留空不添加
  lore: []
  # 解绑之后添加lore的位置,设置足够大可以放在最后面，-2表示倒数第3行
  lore-index: 0
  # 解绑之后添加lore替换绑定lore, 此项覆盖item-unbind.lore-index
  lore-replace-matched: false

# 物品禁用设置
item-deny:
  # 手上拿着绑定物品时禁止左键交互
  interact-left: false
  # 手上拿着绑定物品时禁止右键交互
  interact-right: false
  # 手上拿着绑定物品时禁止左键交互绑定方块, 需要开启方块监听器
  left-click-at-bind-block: false
  # 手上拿着绑定物品时禁止右键交互交互绑定方块, 需要开启方块监听器
  right-click-at-bind-block: false
  # 禁止实体交互(攻击或右键)
  interact-entity: false
  # 禁止盔甲架交互
  armor-stand: false
  # 禁止丢弃
  drop: false
  # 禁止含有绑定物品的容器被玩家破坏
  container-break: false
  # 禁止捡起
  pickup: false
  # 禁止拿走绑定物品
  click: false
  # 禁止铁砧(1.9以上)
  anvil: false
  # 禁止合成
  craft: true
  # 禁止发射器射出
  dispense: false
  # 掉落物禁止被漏斗或漏斗矿车吸入
  hopper: false
  # 容器里的绑定物品不被漏斗或漏斗矿车吸走
  container-move: false
  # 禁止放入展示框
  item-frame: false
  # 禁止弹射物射出(药水、箭、雪球等)
  throw: false
  # 禁止消耗(吃)
  consume: false
  # 手上拿着绑定物品时禁止输入一下匹配命令
  command: false
  # 匹配的命令正则表达式: '.*' 表示全部
  command-pattern:
  - .*
  # 禁止绑定物品放入特定的容器里,是以下2个选项的总开关
  # 为了防止各种操作绕过将同时也会禁止点击
  inventory: false
  # 禁止绑定物品放入特定标题的容器里,正则表达式
  inventory-pattern:
  - ^垃圾桶$
  # 禁止绑定物品放入特定类型的容器里
  inventory-types:
  - ANVIL
  - DISPENSER
  - DROPPER
  - FURNACE
  - GRINDSTONE
  - SMITHING
  # 禁止绑定物品死亡掉落(只在死亡不掉落游戏规则未开启时有效)
  drop-on-death: false

# 方块禁用设置
# 由于监听方块物品需要较多的资源，如果不绑定方块物品关闭以节省性能
block-deny:
  # 禁止方块物品被破坏
  break@: true
  # 禁止方块物品被放置
  place@: true
  # 禁止方块物品被互动(左右键)
  interact@: true
  # 禁止方块物品被爆炸损坏
  explode: false
  # 禁止方块物品被活塞推动/拉动. 建议禁止, 如果不禁止，被活塞推/拉动后的方块也会绑定(实验性功能)
  piston: false
  # 禁止流水/岩浆破坏
  flow: false
  # 将流水/岩浆破坏的方块直接送回物主不掉落
  flow-send-back: false
  # 禁止绑定的方块转化为实体，比如重力方块变成下落方块，tnt被点燃
  change-to-entity: false
  # 方块类的绑定物品放置时禁止绑定放置的方块
  bind-from-item: false

# 实体设置
entity:
  # 绑定的生物的名字, {0} 为玩家名 {1} 为实体名
  bind-name: '&a{0} &f的 &7{1}'
  # 绑定实体死亡掉落物也绑定
  bind-drops: false
  # 是否对敌对目标
  hostility@: true
  # 是否守护非敌对目标(吸引仇恨)
  defend: false
  # 绑定的实体对友好目标的守护距离
  defend-distance: 10.0
  # 是否启用刷怪蛋检测, 启用之后绑定的刷怪蛋生成的生物会绑定
  spawn-egg-check: false

# 由绑定物品生成的实体的监听器
# 一般指刷怪蛋
entity-deny:
  # 是否禁止该实体被玩家攻击掉血
  damage-by-player@: true
  # 是否禁止与该实体交互(右键)
  interact@: true

# 是否禁止该实体被除了玩家之外的实体攻击掉血
entity-deny-damage-by-entity: false

# 是否禁止该实体受到任何伤害
entity-deny-damage: false

# 是否禁用实体AI (1.9+)
entity-deny-ai: false

# 是否禁用实体重力 (1.10+)
entity-deny-gravity: false

# 禁止绑定实体死亡掉落物
entity-deny-drops: false

# 自动绑定设置
auto-bind:
  # 是否开启自动绑定,如果全局开启将会绑定所有物品，请在setting.yml中配置开启以绑定特殊物品
  enable: false
  # 点击物品时绑定
  onClick: false
  # 捡起物品时绑定
  onPickup: false
  # 丢弃物品时绑定
  onDrop: false
  # 使用物品消耗耐久时绑定(包括工具、武器、盔甲等有耐久的物品)
  onUse: false
  # 手拿物品左键时绑定
  onLeft: false
  # 手拿物品右键时绑定
  onRight: false
  # 装备物品穿戴时绑定(仅限Paper及其下游服务端核心)
  onEquipWear: false
  # 扫描器扫描时绑定(在config.yml中配置扫描器)
  onScanner: false
  # 通过 NeigeItems 物品或物品包给予指令生成物品时绑定
  onNeigeItemsGive: false
  # 通过 MMOItems 掉落表生成物品时绑定给挖掘者或击杀者
  onMMOItemsDrop: false
  # MythicMobs 生物死亡时将实际掉落物绑定给玩家击杀者
  onMythicMobDeath: false

# 自动解绑设置(前提是已经绑定)
auto-unbind:
  # 是否开启自动解绑,如果全局开启将会解绑所有物品，请在setting.yml中配置开启以解绑特殊物品
  enable: false
  # 点击物品时解绑
  onClick: false
  # 捡起物品时解绑
  onPickup: false
  # 丢弃物品时解绑
  onDrop: false
  # 使用物品消耗耐久时解绑(包括工具、武器、盔甲等有耐久的物品)
  onUse: false
  # 手拿物品左键时解绑
  onLeft: false
  # 手拿物品右键时解绑
  onRight: false
  # 装备物品穿戴时解绑(仅限Paper及其下游服务端核心)
  onEquipWear: false
  # 扫描器扫描时解绑(在config.yml中配置扫描器)
  onScanner: false

# 一些额外的功能
module:
  # 唯一物品, 物品防刷功能。请先在 modules/unique-item.yml 中 打开 enable 才有效
  # 在物品第一次绑定时记录其数量和生成一个唯一的UUID，并检查所有玩家的物品栏、所有世界掉落物、方块中的数量, 超过的将会删除
  # 此处指定相同UUID的最大堆叠数，-1关闭功能，0会在绑定时立刻删除这个物品
  unique-item: -1

# 当物品/方块/实体绑定后的动作
on-bind:
  # 物品绑定时发送消息给物主(支持lang.yml中的所有格式)
  # 占位符: {0}表示物品的材质; {1}表示物品的自定义名称,没有返回空内容; {2}返回物品的数量
  # {3} 材质id; {4} 材质子id
  item-msg: []
  # 方块绑定时发送消息给物主(支持lang.yml中的所有格式)
  # 占位符: {0}表示方块的材质; {1}表示方块的世界名称,没有返回空内容; {2}方块X坐标; {3}方块Y坐标; {4}方块Z坐标
  # {5} 方块id
  block-msg: []

# 插件兼容性控制
addons:
  # 禁止 mcMMO 分解绑定物品
  mcmmo-salvage: false
  # 禁止 mcMMO 修复绑定物品
  mcmmo-repair: false
  # 禁止 绑定物品 被 mcMMO 技能缴械
  mcmmo-disarm: false
```

</details>

`item-deny.pickup` 是捡起时送回物主的前提。只开启 `item.send-back-on-pickup` 不会单独阻止玩家捡起。
