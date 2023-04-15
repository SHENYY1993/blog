---
layout: post
title:  "A*"
categories: Algorithm
tags:  Algorithm PathPlanning 路径规划 基于搜索 启发式
author: SHENYY
---

* content
{:toc}

## 1.算法思想
是对Dijkstra算法的改进，引入启发式函数(Heuristics)，减少Dijkstra算法过程中收录的栅格数量，增加搜索速度。

<center>
<img src="https://shenyy1993.github.io/blog/assets/2023/04/20230414-Algorithm-PathPlanning-Dijkstra-测试结果.gif" width="400" title="Dijkstra测试结果">
<img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-AStar-测试结果.gif" width="400" title="A*测试结果">
<div>Dijkstra V.S. A*</div>
</center>





## 2.伪代码
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-AStar-伪代码.png" width="400" title="伪代码"></center>

## 3.最优性要求
引入启发式函数后，保证最优性要求：h(n) <= *h(n)
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-AStar-最优性要求.png" width="400" title="最优性要求"></center>

## 4.时间复杂度分析
哈希表、列表实现：O(n^2)
优先级队列：O(n·logn)

## 5.算法Java代码实现
[路径规划算法演示项目（Java）](https://github.com/SHENYY1993/PathPlanning_SpringBoot)

## 6.测试
测试结果如下：
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-AStar-测试结果.gif" width="400" title="测试结果"></center>



