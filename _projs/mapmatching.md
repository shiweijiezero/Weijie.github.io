---
layout: archive
title: "proj_mapmatching"
permalink: /projs/mapmatching
author_profile: true
---

Step1:
I used python to reproduce the SOTA HMM-based map-matching algorithm for cellular trajectories.
The SOTA paper refers to [ST-Matching](https://www.microsoft.com/en-us/research/publication/map-matching-for-low-sampling-rate-gps-trajectories/), [SnapNet](https://ieeexplore.ieee.org/abstract/document/7539689/) 

Step2:
I transformed these code into spark+scala big-data architecture. 
这一部分的任务为配置Spark环境，构造路网图，Dijsktra最短路径算法，构造Rtree Index用于最近k个道路的查询，各种空间距离的计算实现。
基本的HMM模型实现：候选选择、发射概率计算、转移概率计算、维特比路径搜索算法

Step3:
贡献：针对基站轨迹的高误差，高噪声，乒乓效应等特性，进行轨迹过滤与平滑。
包括在Scala上实现轨迹点的速度过滤、角度过滤、重复点过滤、下采样过滤、均值平滑。

Step4：
贡献：针对性地设计路网匹配评估指标
传统的评估指标：精度与召回
存在的问题：precision and recall cannot simultaneously account for missing and redundant mismatched road segments, we design an error metric: Route Mismatch Fraction (RMF), which is defined by the following: RMF = length of mismatched road segments length of the ground-truth path
