# GTCE大涡轮详解

影响大涡轮产能和效率的有：转子仓等级，以及转子效率

* 产能

$$
能量产出 = (2048 + 6144 \times 转子效率) \times (相对转子速度 \times 相对转子速度) \\
相对转子速度 = 当前转速 \div 6000
$$

* 消耗
  $$
  当前燃料消耗=(JEI中燃料消耗 \times 64) \times (相对转子速度 \times 相对转子速度)\\
  相对转子速度公式参考上文
  $$

* 参考

各级转子仓转速计算公式
$$
转速 = 6000 \times 0.2 \times (转子仓等级 + 1)\\
注：ULV等级为0
$$

* 案例

  个人以前用12台钨钢锅炉，每台2400mb/t蒸汽，用IV转子仓，锇转子

  ```java
  转速 = 6000 * 0.2 * (5 + 1) = 7200rpm。匹配，没毛病
  ```

  产能

  ```
  产能 = (2048 + 6144 * 0.66) * (7200 / 6000) * (7200 / 6000) = 8788。匹配，没毛病
  ```

  消耗

  蒸汽在JEI中是64mb

  ```java
  消耗 = 64 * 64 * 1.2 * 1.2 = 5898.24mb/t
  ```

  用12台钨钢锅炉，总共28800mb/t

  大约能供`28800 / 5898.24 = 4.88台`，我刚好供了五台IV蒸汽涡轮，其中有一台会跳蒸汽，很科学