#### HAVING

- [Mysql中HAVING的相关使用方法](https://www.cnblogs.com/mr-wuxiansheng/p/11188733.html)

  > having字句可以让我们**筛选分组之后**的各种数据，where字句在聚合前先筛选记录，也就是说作用在group by和having字句前。
  >
  > 而having子句在聚合后对组记录进行筛选。我的理解就是**真实表中没有此数据**，这些数据是通过一些函数产生的。

- [MySQL查询中having语句的使用场景和用法](https://zhuanlan.zhihu.com/p/107495600?utm_source=wechat_timeline)

  > having语句是**分组后**过滤的条件，在group by之后使用，也就是如果要用having语句，必须要先有group by语句。

- 这篇文章解释得很好，为什么group by和having要一起用：[MySQL查询中group by语句的使用场景和用法](https://zhuanlan.zhihu.com/p/106767752) 精髓是文章里面的图

  一句话总结HAVING和GROUP BY：先分组（GROUP BY），后聚合（使用聚合函数），根据我的经验，若单独使用GROUP BY，那么只会返回分为同一组的**第一条**记录，其他的直接不要:laughing:

