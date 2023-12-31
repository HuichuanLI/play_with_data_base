# SQL语法解析器
业内如果是C/C++实现的话，会套用bison, flex 这些库来完成语语法解析和词法解析过程。
由于编译器的前端语法解析和词法解析部分，涉及比较复杂的算法和数据结构，因此我们套用Python上的库:
- sly
- ply

关于sly的代码案例（计算器）：
> https://github.com/dabeaz/sly

# SQL引擎
## 逻辑算子
- ScanOperator: 用于扫描数据，是数据来源的最基本获取点
- GroupOperator: 用于做聚合
- HashOperator: 用户做连接操作
- SortOperator: 用于表明该SQL语句涉及排序操作
...

## 逻辑算子向物理算子进行映射
可以采用对应的规则，我们可以将逻辑执行计划，通过遍历树的形式，产生对应的一个（或多个）物理执行计划，然后选择这些物理执行计划中的执行代价最小的那个。
在这个过程中，我们可以有两种优化机制：
1. CBO: 生成一堆备选的物理执行计划，然后分别计算它们的cost 值，选择最小的那个就行了；
2. RBO: 通过对应的规则，生成执行计划，在该生成过程中，套用一些固有的规则(rule)

由于我们的代码比较简单，所以，我们直接采用RBO就可以了，但是，真正的数据库中，往往是两者都采用。

