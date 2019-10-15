# GTCE多方块数据整理

说明

```
BA 		JEI中的基础消耗数量（mb）
BT 		JEI中的基础配方时间（tick）
BV		JEI中写的基础配方电压
BBT 	固体物品燃烧时间，比如煤是1600
cycle 一次配方处理
```

# Diesel Engine

## 燃料消耗

```java
Oxygen: 		2mb / per cycle
Lubricant:	2mb / 20 cycle
Fuel:				有氧每次 BA*64/(BT*20) 有氧每次 BA*64/(BT*20)*2 mb/t
Time:				BA
备注
以Cetane-Boosted Diesel为例，BA=1440，BT=45
其中Fuel单次配方的消耗，如果要求mb/t，可以用Fuel/Time
```

## 能量产出

```
Oxygen Boosted:	6144 eu/t
Normal:					2048 eu/t
有氧产出提高三倍，燃料消耗提高两倍，所以提高了50%的效率
```

# MultiFurnace

多方块熔炉的配方其实只有一个，无论烧啥东西，BT都是512，但是配方的电压等级会变 power = (4 * LEVEL / DISCOUNT)

每次烧 16 * LEVEL个物品

多方块熔炉的超频机制和单方块一样，简单来讲

* 对于小于等于16eu/t的配方，每超频一次，能耗4倍，时间除以2，且最多超频电压等级差次。即8eu/t的配方，放mv机器，也只会超频一次，实际上消耗32eu/t（而不是想象中的超频两次，128eu/t），可以自行做下实验，我有提供一个可以在TOP里看到机器超频功率的包。
* 对于大于32eu/t的配方，每超频一次，能耗4倍，时间除以2.8，只要电压够

假定电压等级是EV（4096），

* 对于聚变线圈来说，基础配方是 4 * (16 / 4) = 16 eu/t，持续512 tick，16 * 16 = 256个物品

  超频3次，能耗128 eu/t，持续 512/2/2/2 = 64 tick

* 对于钨钢线圈来说，基础配方是 4 * (8 / 1) = 32 eu/t，持续512 tick，16 * 16 = 256个物品

  超频3次，能耗2048 eu/t，持续 512/2/2/2 = 64 tick

  

| TYPE           | LEVEL | DISCOUNT |
| -------------- | ----- | -------- |
| CUPRONICKEL    | 1     | 1        |
| KANTHAL        | 2     | 1        |
| NICHROME       | 4     | 1        |
| TUNGSTENSTEEL  | 8     | 1        |
| HSS_G          | 8     | 2        |
| NAQUADAH       | 16    | 1        |
| NAQUADAH_ALLOY | 16    | 2        |
| SUPERCONDUCTOR | 16    | 4        |
| FUSION_COIL    | 16    | 8        |

## Large Boiler

每秒温度上升一度，吸收1mb水或蒸馏水，温度>100开始产出蒸汽（科学），超过100度后，缺水会爆炸



## 燃料消耗

```
液体燃料
input 		= BA * 100 * fuelConsumptionMultiplier mb/t
duration 	= BT * 50 tick
半流质燃料
input			= BA * 100 * fuelConsumptionMultiplier mb/t
duration 	= BT * 200 tick
固体燃料
input 		= 1
duration 	= BBT / (50 * fuelConsumptionMultiplier) tick，上取整
```

对于青铜锅炉，一块煤能烧1.6s，钨钢锅炉只能烧7tick

## 蒸汽产出

```java
outputMultiplier = currentTemperature / (maxTemperature * 1.0)
outputMultiplier对于温度到顶的锅炉，值就是1
output = baseSteamOutput * outputMultiplier mb/t
```

也即，对于满温度锅炉，青铜900mb/t，1600mb/t，钛3700mb/t，钨钢7800mb/t

大锅炉的液体燃料和柴油发电一样，凡是柴油发电机可以烧的，大锅炉都可以烧



| Type          | BaseSteamOutput | fuelConsumptionMultiplier | MaxTemperature |
| ------------- | --------------- | ------------------------- | -------------- |
| BRONZE        | 900             | 1.0                       | 500            |
| STEEL         | 1600            | 1.6                       | 800            |
| TITANIUM      | 3700            | 3.0                       | 2000           |
| TUNGSTENSTEEL | 7800            | 5.0                       | 4000           |

# Large Turbine

## 燃料消耗

```
(2048 + steamTurbineBonusOutput * rotorEfficiency) / BV * BA
```

基本还是2mb => 1eu，但是随着转子仓等级的提高（注意，和转子效率没关系），会比1eu略多，最多有1.56eu

## 耐久损耗

转速达到最大时，12点 / 230 cycle

现在这个版本比起两三个月前的版本，这个耐久机制靠谱多了，之前最高等级的转子才能转一天多

可以看得出来，cycle越长，转子寿命越长，因此对于等离子配方（几百万tick）来说，任何转子寿命都可以轻松上一个月甚至几年

算了下，即使是渣渣的钢转子，且是蒸汽这种cycle特别短（只有1tick）的配方，也可以轻松转12小时

## 能量产出

三种涡轮的产出不一样，涉及到三个配置项，三个值默认都是6144（整合包作者可以改，而且Omnifacotyr的作者确实把这个值改大了，不然产能成迷）

| Type           | Power (eu/t)                                                 |
| -------------- | ------------------------------------------------------------ |
| Stream Turbine | 2048 + steamTurbineBonusOutput * rotorEfficiency * multiplier |
| Gas Turbine    | 2048 + gasTurbineBonusOutput * rotorEfficiency * multiplier  |
| Plasma Turbine | 2048 + plasmaTurbineBonusOutput * rotorEfficiency * multiplier |

其中`multiplier = (当前转速 / 6000)^2`，对于max转子仓，如果达到满速，multiplier = (7500 - 6000) * (7500 - 600) = 1.56，这就是上面那个1.56的来历了


