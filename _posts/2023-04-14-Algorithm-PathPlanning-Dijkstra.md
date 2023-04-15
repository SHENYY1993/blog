---
layout: post
title:  "Dijkstra"
categories: Algorithm
tags:  Algorithm PathPlanning 路径规划 基于搜索
author: SHENYY
---

* content
{:toc}

## 1.算法思想
从起点开始逐步扩展，每一步为一个节点找到最短路径。Q
While True:
    1.从未访问的节点选择距离最小的节点收录（贪心思想）
    2.收录节点后遍历该节点的邻接节点，更新距离

<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-测试结果.gif" width="400" title="测试结果"></center>




## 2.伪代码
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-伪代码.png" width="400" title="伪代码"></center>

## 3.示例
找到一条从节点1到节点6的最短路径。
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-示例.png" width="400" title="示例"></center>

具体操作如下：
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-示例步骤.png" width="600" title="示例步骤"></center>

## 4.时间复杂度分析
哈希表、列表实现：O(n^2)
优先级队列：O(n·logn)

## 5.算法Java代码实现
[路径规划算法演示项目（Java）](https://github.com/SHENYY1993/PathPlanning_SpringBoot)

## 6.测试
测试结果如下：
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-测试结果.gif" width="400" title="测试结果"></center>



