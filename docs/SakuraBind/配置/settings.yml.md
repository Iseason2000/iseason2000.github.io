---
sidebar_position: 4
---

# settings.yml

> 该配置通过配置匹配器来为某一类物品应用不同的物品设置，可以绑定特定物品

`settings.yml` 是最常改的配置文件。你可以把它理解为：

1. 用 `match` 找出“哪些物品属于这一类”。
2. 用 `settings` 定义“这一类物品绑定后如何限制、如何自动绑定、如何送回”。

## 阅读方式

| 目标 | 看哪里 |
| --- | --- |
| 想快速写一个规则 | 先看“最小示例”和“常用配置节点” |
| 不知道该用哪个匹配项 | 看“匹配节点”表格 |
| 规则没生效 | 看“常见错误”，再用 `/sakurabind test tryMatch <匹配器ID>` |
| 想对照默认文件 | 展开页面底部“完整默认示例配置” |

## 最小示例

```yaml
matchers:
  my_sword:
    match:
      material: '.*_SWORD'
    settings:
      auto-bind:
        enable: true
        onClick: true
      item-deny:
        drop@: true
```

这个配置表示：任意剑被点击时自动绑定，非物主不能丢弃，物主可以丢弃。

:::warning[默认 example]

首次生成的 `settings.yml` 会包含 `matchers.example` 演示匹配器。正式开服前请删除、注释或改写它，避免演示规则影响真实物品。

:::



:::tip[注意]

匹配器将按配置顺序逐一检测匹配，过多的匹配器会影响性能，复杂的匹配器应放在最后

:::



配置匹配器的结构如下：

~~~ yaml
matchers:
  匹配器ID: # 不可重复，除了 global-setting 之外的任何名字
    match: # 匹配节点
      匹配项1: ……
      匹配项2: ……
    settings: # 配置节点 global-setting.yml中的配置项
       ……
    
~~~

每个匹配器分为 `匹配器ID`、`匹配节点`、`配置节点` 三部分，缺一不可

如果只想测试匹配，不想真的绑定，可以使用：

```text
/sakurabind test tryMatch <匹配器ID>
```

该命令会逐项输出匹配结果，适合排查正则、材质名、NBT 路径是否写错。

## 匹配器ID

匹配器ID**不能重复**，`global-setting` 是 `global-setting.yml`的匹配器名称(匹配所有物品)，所以你自定义的匹配器不能为这个名字



---

## 匹配节点

:::tip[注]

多个节点之间是 **and(与)** 的关系，所有键**都匹配**时这个匹配器才算匹配到物品

:::



你可以在这个网站测试正则表达式: https://www.bejson.com/othertools/regex/



匹配器节点有以下类型

| 键名                 | 数据类型              | 说明                                                         |
| -------------------- | --------------------- | ------------------------------------------------------------ |
| name                 | String                | 匹配自定义物品名，原版翻译名无法匹配，**正则表达式**，颜色符号为`§` |
| name-without-color   | String                | 匹配去除颜色代码之后的自定义物品名，原版翻译名无法匹配，**正则表达式** |
| material             | String                | 匹配物品材质，**正则表达式**                                 |
| materials            | StringList            | 匹配多个物品材质                                             |
| ids                  | StringList            | 匹配多个物品，格式为 `物品数字ID:子ID`                       |
| materialIds          | StringList            | 匹配多个物品，格式为 `物品ID:子ID` 或者 `物品ID`             |
| lore                 | StringList            | 匹配一行或连续的多行lore，**正则表达式**，颜色符号为`§`      |
| lore!                | StringList            | 同lore，会在绑定时**删除匹配的lore**                         |
| lore-without-color   | StringList            | 匹配一行或连续的多行lore(去除颜色代码)，**正则表达式**       |
| lore-without-color!  | StringList            | 同lore-without-color，会在绑定时**删除匹配的lore**           |
| nbt                  | yaml键值对,值为String | 匹配特定NBT, 值为正则表达式                                  |
| mmoitems             | String 或 StringList  | 匹配MMOItems物品 格式为 `Type:ID` 特殊，填`all`匹配所有物品  |
| mmoitems-type        | String 或 StringList  | 匹配MMOItems物品Type 格式为 `Type` 特殊，填`all`匹配所有物品 |
| itemsadder           | String 或 StringList  | 匹配ItemsAdder物品，格式为 `namespace:id` 特殊，填`all`匹配所有物品 |
| itemsadder-namespace | String 或 StringList  | 匹配ItemsAdder命名空间，格式为 `namespace` 特殊，填`all`匹配所有物品 |
| neigeitems           | String 或 StringList  | 匹配NeigeItems物品ID；填`all`匹配所有NeigeItems物品 |
| mythicmobs           | String 或 StringList  | 匹配MythicMobs物品ID；填`all`匹配所有MythicMobs物品 |
| oraxen               | String 或 StringList  | 匹配oraxen物品，格式为 `物品ID` 。特殊，填`all`匹配所有物品  |

**以下匹配器键互斥，不能同时生效**

* name、name-without-color
* material、materials、ids、materialIds
* lore、lore-without-color、lore!、lore-without-color!
* mmoitems、mmoitems-type
* itemsadder、itemsadder-namespace

---

## 配置节点

配置节点可以覆盖 `global-setting.yml` 中的同名配置，为该匹配器配置特殊的配置，比如自动绑定。

同 `global-setting.yml` 以 `@` 结尾的布尔类型的项，其物主将使用与他人相反的设置。

常用配置节点：

| 节点 | 用途 |
| --- | --- |
| `item.lore` | 绑定后显示的 lore |
| `item-deny.*` | 禁止丢弃、拿取、合成、铁砧、放入容器等行为 |
| `auto-bind.*` | 自动绑定触发点 |
| `auto-unbind.*` | 自动解绑触发点 |
| `block-deny.*` | 方块绑定后的破坏、放置、互动限制 |
| `entity.*` / `entity-deny.*` | 实体绑定显示名、AI、伤害、交互、掉落规则 |
| `module.unique-item` | 唯一物品防刷数量 |

未写的节点会继承 `global-setting.yml`。

## 第三方物品示例

按插件物品 ID 匹配时，对应软依赖必须已经安装并成功加载。ID 列表是精确匹配，`all` 则匹配该插件识别出的所有物品。

```yaml
matchers:
  neige_rewards:
    match:
      neigeitems:
      - example_sword
      - example_pack_item
    settings:
      auto-bind:
        enable: true
        onNeigeItemsGive: true

  mmo_drops:
    match:
      mmoitems:
      - SWORD:FLAME_SWORD
    settings:
      auto-bind:
        enable: true
        onMMOItemsDrop: true

  mythic_drops:
    match:
      mythicmobs:
      - SkeletonKingSword
    settings:
      auto-bind:
        enable: true
        onMythicMobDeath: true
```

`onNeigeItemsGive` 同时覆盖单个物品与物品包给予；`onMMOItemsDrop` 只处理 MMOItems 掉落表事件；`onMythicMobDeath` 处理有玩家击杀者的 MythicMobs 死亡事件及其实际掉落列表，也用于 NeigeItems 生成的 MythicMobs 普通掉落和钓鱼掉落。

## 匹配缓存

插件会把已绑定物品命中的匹配器键写入 NBT：

```yaml
nbt-cache-path: sakura_bind_setting_cache
```

优点是读取设置更快；缺点是不同配置缓存键不同，会影响物品堆叠。修改匹配器名称后，旧物品可能仍带着旧缓存，插件发现配置不存在时会重新匹配并更新缓存。

## 常见错误

| 问题 | 处理 |
| --- | --- |
| 物品没自动绑定 | 确认 `auto-bind.enable: true`，并至少开启一个触发项，如 `onClick` 或 `onScanner` |
| 正则匹配不到 | 用 `/sakurabind test tryMatch <匹配器ID>` 看实际值 |
| 自定义名匹配不到原版物品名 | `name` 只匹配自定义名，不匹配创造栏里的原版翻译名 |
| `materials` 没生效 | 材质名必须是 Bukkit `Material` 全名，例如 `DIAMOND_SWORD` |
| `materialIds` 写成 `materialId` | 当前匹配器识别的是 `materialIds` |
| 多个匹配器都能匹配 | 文件中靠前的匹配器先命中 |

## 示例

### 示例一：按 lore 自动绑定并替换 lore

匹配lore中含有 `灵魂绑定`的物品，自动在点击物品时、扫描器扫描时绑定物品，并在绑定时删除这个lore，指定绑定lore为`&a灵魂绑定2: &6玩家名`。

~~~ yaml
matchers:
  示例一:
    match:
      lore!:
        - '灵魂绑定'
    settings:
      item:
        lore: # 支持 placeholder
        - '&a灵魂绑定2: &6%player%' # %player% 会替换为物主的名字，不是placeholder
      auto-bind:
        enable: true
        onClick: true
        onScanner: true
      
~~~

### 示例二：按名称和材质绑定武器

匹配名为`大宝剑`的所有 `剑` ，自动在点击物品时、扫描器扫描时绑定物品。

~~~ yaml
matchers:
  示例二:
    match:
      name: '大宝剑'
      materials: 
        - 'NETHERITE_SWORD'
        - 'DIAMOND_SWORD'
        - 'GOLDEN_SWORD'
        - 'IRON_SWORD'
    settings:
      auto-bind:
        enable: true
        onClick: true
        onScanner: true
        
  示例二另一个方式:
    match:
      name: '大宝剑'
      material: '_SWORD' # material是正则表达式，但是性能没有 materials 的高
    settings:
      auto-bind:
        enable: true
        onClick: true
        onScanner: true
      
~~~

### 示例三：按 NBT 绑定物品

匹配 NBT`tag`下`custom1`下 `csutom2`值为 `1` 的  `钻石剑` 。



注：查看nbt结构可以用命令 `/sakurabind nbt` 输出手上物品的NBT到控制台

~~~ yaml
matchers:
  示例三:
    match:
      materials: 
        - 'DIAMOND_SWORD'
      nbt:
        tag:
          custom1: 
            custom2: '1'
    settings:
      auto-bind:
        enable: true
        onClick: true
        onScanner: true
      
~~~

<details>
<summary>完整默认示例配置</summary>

~~~ yaml title="settings.yml"
# <--------------------matcher教程-------------------->
# matcher可以匹配某类特殊的物品以应用不同的设置
# 请勿声明名为 'global-setting '的matcher,否则会与公共设置冲突
# match 项为需要匹配的物品特征，采用正则表达式 https://www.bejson.com/othertools/regex/
# 所有 match 项都不是必须的，你可以自由组合, 但至少需要有一个子项, 只有匹配所有子项才算最终匹配到
# name 为 物品名字, 必须为非原版翻译名(也就是从创造物品栏拿出来的'圆石'的name为空)
# name-without-color 为 除去颜色代码的物品名字, 必须为非原版翻译名(也就是从创造物品栏拿出来的'圆石'的name为空)
# material 为 物品材质,使用正则匹配
# materials 为 物品材质,使用全名匹配 https://bukkit.windit.net/javadoc/org/bukkit/Material.html
# ids 为 物品id:子id 匹配方式 如 6578 或 6578:2
# materialIds 为 物品材质:子ID 匹配方式 如 STONE 或 STONE:2 ; 如果只需要匹配材质请使用效率更高的 materials 方式
# lore 为 物品lore正则匹配 如有多行则需全匹配, lore! 表示删除匹配到的lore
# lore-without-color 为 物品lore除去颜色代码正则匹配 如有多行则需全匹配 与 lore 互斥, lore-without-color! 表示删除匹配到的lore
# nbt 为 物品NBT，注意：常规的nbt储存在tag下
# 注：以上的 name 和 name-without-color 互斥，material、materials、ids、materialIds 互斥，
# 注：lore、lore-without-color 及其!后缀互斥。 互斥就是只能同时存在其中一个

# <<--------------------matcher插件兼容-------------------->>
# 以下的项只有在插件存在时有效
# MMOItems 有2个项：mmoitems 和 mmoitems-type, 前者匹配 type:id 的物品id，后者只匹配 type , 请使用 List类型
# MMOItems 有2个特殊的值 mmoitems: all 或 mmoitems-type: all 表示匹配所有MMOItems物品
# ItemsAdder 有2个项：itemsadder 和 itemsadder-namespace, 前者匹配 namespace:id 的物品id，后者只匹配 namespace , 请使用 List类型
# ItemsAdder 有2个特殊的值 itemsadder: all 或 itemsadder-namespace: all 表示匹配所有ItemsAdder物品
# NeigeItems 有1个项：neigeitems, 匹配 NeigeItems 物品ID, 请使用 List类型
# NeigeItems 有1个特殊的值 neigeitems: all 表示匹配所有 NeigeItems 物品
# MythicMobs 有1个项：mythicmobs, 匹配 MythicMobs 物品ID, 请使用 List类型
# MythicMobs 有1个特殊的值 mythicmobs: all 表示匹配所有 MythicMobs 物品
# Oraxen 有1个项：oraxen, 匹配 Oraxen 物品ID, 请使用 List类型
# Oraxen 有1个特殊的值 oraxen: all 表示匹配所有 Oraxen 物品
# 注：以上的 mmoitems 和 mmoitems-type 互斥，itemsadder 和 itemsadder-namespace 互斥。互斥就是只能同时存在其中一个

# <--------------------settings教程-------------------->
# settings 项为此matcher独立的设置，完全兼容global-setting中的选项
# settings 项中以 '@' 结尾的布尔类型的项，其物主将使用与他人相反的设置
#          如 block-deny 下的 break: true 表示所有人都不能破坏此方块物品
#          但如果是 break@: true 表示所有人都不能破坏,但物主可以破坏此方块物品
# settings 中不存在的项将继承 global-setting.yml 中的同名项
readme: ''

# 为提高性能，匹配过一次的物品在绑定之后将会把匹配到的设置键存入物品NBT，此为NBT的路径
# 注意，缓存会导致不同配置的nbt不一致，以至于不同配置的相同物品无法堆叠
# 留空会使用默认的缓存键
nbt-cache-path: sakura_bind_setting_cache

# 在此配置匹配器，example 是一个默认的演示匹配器，不需要请删除
matchers:
  example:
    match:
      name: ^这是.*的物品$
      material: BOW|BOOKSHELF
      materials:
      - DIAMOND_SWORD
      materialIds:
      - SPECIAL:2
      ids:
      - '6578'
      - '2233:2'
      lore:
      - 绑定物品
      - 属于
      mmoitems:
      - LONG_SWORD:KNIFE
      - LONG_SWORD:SHARP_SWORD
      nbt:
        tag:
          testnbt: .*
    settings:
      item:
        lore:
        - '&a灵魂绑定2: &6%player%'
      item-deny:
        interact: false
      block-deny:
        interact@: false


~~~

</details>

