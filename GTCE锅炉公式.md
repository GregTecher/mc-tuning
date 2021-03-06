* 小型燃煤锅炉

  * 燃料消耗

  以物品燃烧时间为基数，每隔12Tick，普通锅炉-1，高压锅炉-2。

  即 

  ​     高压锅炉燃烧时间：燃料燃烧时间 * 0.3

  ​     普通锅炉燃烧时间：燃料燃烧时间 * 0.6

  以煤炭为例，1600tick的燃烧时间，1600 x 0.3 = 480 s，8分钟

  其他物品自行即可推导出来。

  * 蒸汽产出

    高压燃煤锅炉每10tick产出150mb

    普通燃煤锅炉每25tick产出150mb

* 大锅炉燃烧时间（单位tick）

  * 燃料消耗

    * 固体燃料

      每次消耗一块固体燃料，

      每块固体燃料燃烧时间：燃料本身燃烧时间 / (60 * 倍率因子)

      倍率因子：

      青铜1.0，钢1.2，钛1.6，钨钢2.4

    * 流体燃料

      * 柴油引擎燃料

        消耗速率：JEI中所需燃料数量 * 100 * 倍率因子

        燃烧时间：JEI所写燃烧时间 * 25
        
        举例来说，高十六烷值柴油，钨钢锅炉消耗速率：2 * 100 * 2.4，燃烧时间: 2.25 * 20 * 25，每Tick消耗 (消耗速率 / 燃烧时间) = 0.4mb/t，超值
        
        用鱼油永动简直不要太舒服，1十六烷桶可以烧100s，四台流体提取机搞鱼油大约就有0.6桶/s的十六烷产量了

      * 半流质燃料

        消耗速率：JEI中所需燃料数量 * 100 * 倍率因子

        燃烧时间：JEI所写燃烧时间 * 200

        倍率因子参考固体燃料

  * 蒸汽产出

    青铜 600

    钢     900

    钛    1400

    钨钢 2400

* 不同锅炉对比

  评价标准：一块煤的蒸汽产出

  | 类型         | 产出速率（mb/tick） | 燃烧时间（tick） | 总产出 |
  | ------------ | ------------------: | ---------------: | -----: |
  | 小型燃煤锅炉 |                  10 |         19200.00 | 192000 |
  | 高压燃煤锅炉 |                  15 |          9600.00 | 144000 |
  | 青铜锅炉     |                 600 |            26.67 |  16002 |
  | 钢锅炉       |                 900 |            22.22 |  19998 |
  | 钛锅炉       |                1400 |            16.67 |  23338 |
  | 钨钢锅炉     |                2400 |            11.22 |  26928 |

综合来看，显然小型燃煤锅炉更具性价比。大锅炉中，即使是燃煤效率最高的钨钢锅炉，也比小型燃煤锅炉相差近7倍。推荐前期使用高压燃煤锅炉阵列，后期大锅炉阵列，毕竟小型燃煤锅炉要想达到大锅炉同样的输出，无论是布线还是TPS都比较不利。
