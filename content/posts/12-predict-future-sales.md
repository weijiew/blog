---
title: 'Predict Future Sales'
date: 2020-12-12
published: false
slug: predict-future-sales
tags: ['数据挖掘']
cover_image: "./images/p12.jpg"
canonical_url: false
description: '数据挖掘大作业，我选择了一个销量预测的比赛，作为我的结课作业。'
---

# 1.0 背景

对于商家而言，估计销售额是一件很重要的事情。如果对销售额有一个很好的预测，根据销售额可以来备货可以提高周转率，实现良性的库存。

如果发现异常销售并加以改进，制定相应的成本预算，审时度势因地制宜制定有效营销策略等等。这些操作都可大大的提高资金利用率，提高企业的抗风险能力。

本文以 [1C Company](https://1c.ru/eng/title.htm) 公司所提供的数据集为例，该公司为俄罗斯最大的软件销售公司。

采用 XGBoost 模型，以该数据集来训练 XGBoost 模型，根据训练成熟的模型来预测未来的销售额。

改数据集提供了商店，商品，价格，日销量等连续的 34 个月内的数据，目标是预测第 35 个月的各商店商品的销量。

# 2.0 数据清洗

首先要对数据集的大致情况有一个清晰的认识。

数据集共有 6 个文件，其中每个文件代表的含义如下：

* sales_train.csv 训练集，从 2013-1 到 2015-10 之间的数据。
  * 该文件数据共有六列分别是日期，月份，商店，商品，价格和日销量。

* items.csv	商品和产品的补充信息 
  * item_name item_id item_category_id
* shops.csv	商店的补充信息
  * shop_name	shop_id
* item_categories.csv	项目类别的补充信息
  * item_category_name	item_category_id
* test.csv	测试集，要预测的销售额
  * ID	shop_id	item_id
* sample_submission.csv	最终测试集的形式
  * ID	shop_id	item_id

## 2.1 清除噪点

经过大致分析后发现商品的 price 和 sales 数据分布存在问题。

下图是 sales 属性的分布图，其中绝大部分值都小于 1000 。将大于 1000 的数据当作噪点，进行删除处理。

![sales](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/download.68cx3uhfa9g0.png)

下图是 price 属性的分布情况，同样问题依旧存在。将大于 price > 100000 的数据当作噪点，进行删除处理。

![price](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/aaa.123pud9e23o0.png)

## 2.2 清除异常值

item_price 属性也就是价格，该属性存在小于 0 的值。根据常识，这是不可能的。

![item_price](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.6ganu6fto7k0.png)

对于此情况采取的措施是用均值来填充。

## 3.0 特征工程

### 3.1 相关性

通过查看相关系数矩阵可知 item_id 和 item_price 对 item_cnt_day 有更强的相关性

![下载](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/下载.7kl8f44xxe80.png)

该数据集中，价值最大的数据只有 item_price 和 item_cnt_day 这两个特征，

测试集是 34 个月， 42 家商店 5100 件商品的数据。



1. 商店商品价格：按照商店+商品进行分组，统计每个组内的价格的最大值，最小值，平均值，中位数，标准差。
2. 商品价格：和1相似，不同的是仅按照商品分组，不考虑商店的区分。
3. 上个月的销售量，价格，销售额及其滞后值：按照商店+商品+月份分组，统计前一个月的销售量，价格和销售额。对这三者分别计算其往前三个月的值（滞后值），并计算当月+前两个月的均值。
4. 月销售量：按照商品+商店分组，统计商品无任何销售量的月份数量，统计商品月销售量的最小值，最大值，均值，中位数和方差。
5. 商品数量：按照商店分组，统计这个商店内所有商品的数量。
6. 同一类商品中，最大月销售量：按照商店+商品类别分组，统计同一类别商品中的最大销售量
7. 价格走势：统计商品前一个月价格和历史最大，均值，中位数价格的差值。
8. 销量走势：统计商品前一个月销量和历史最大，均值，中位数销量的差值。
9. 销售额走势：统计商品前一个月销售额和历史最大，均值，中位数销售额的差值。
10. 销售时间区间：统计商品的销售月份长度。
11. 无销售量时间区间：统计商品无销售量月份长度。
12. 是否是当月新品：统计商品是否是当月新品，及销售时间区间长度为0
13. 月份：将月份编号转为1-12个具体月份。
14. 商店ID：数据中有，直接取。
15. 商品ID：数据中有，直接取。
16. 商品分类ID：数据中有，直接取。
17. 月份编号：数据中有，直接取。
