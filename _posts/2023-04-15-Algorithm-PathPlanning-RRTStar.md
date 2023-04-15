---
layout: post
title:  "RRT*"
categories: Algorithm
tags:  Algorithm PathPlanning 路径规划 基于采样
author: SHENYY
---

* content
{:toc}

## 1.算法思想
针对RRT算法所生成的路径不够优的问题，通过为**当前节点x_new重新选择父节点，以及对在范围内的节点重新连接（rewire）的方式**，改进所生成的路径。
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-测试结果.gif" width="400" title="测试结果"></center>




1. 第一步：为当前节点x_new找到更好的父节点
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-为当前节点找父节点.png" width="400" title="为当前节点找父节点"></center>
2. 第二步：将以当前节点x_new为中心的范围内（搜索到越后面，半径越小）的所有节点进行重连，以找到更好的路径。
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-范围内节点重连.png" width="400" title="范围内节点重连"></center>

## 2.伪代码
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-伪代码.png" width="400" title="伪代码"></center>

## 3.RRT与RRT*对比
对比如下：
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-对比.png" width="400" title="对比"></center>

## 4..算法Java代码实现
[路径规划算法演示项目（Java）](https://github.com/SHENYY1993/PathPlanning_SpringBoot)

## 5.测试
测试结果如下：
<center><img src="https://shenyy1993.github.io/blog/assets/2023/04/20230415-Algorithm-PathPlanning-RRTStar-测试结果.gif" width="400" title="测试结果"></center>



