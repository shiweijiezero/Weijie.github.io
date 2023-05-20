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
此外，我们还实现了路网数据的重组，将大量可以合并的道路合并为一个，提升Rtree查询效率

Step3:
贡献：针对基站轨迹的高误差，高噪声，乒乓效应等特性，进行轨迹分段、过滤与平滑。
轨迹分段：
针对GPS轨迹，核心思想是利用车辆载客状态标签识别出租车的每次载客（视为一段），并对未载客的轨迹段利用停留时间阈值进一步分段。
针对信令轨迹，考虑信令轨迹采样频率不均衡问题，不能采用固定时间间隔阈值的切分方式。对此，我们设计一个时间间隔阈值自适应的切分方式。算法的核心思想是：对于信令轨迹点pi和pi+1，我们将分别考虑其前后滑窗(窗口大小为k个轨迹点)之间的平均时间间隔，来作为判断是否在该轨迹点处进行切分的自适应时间间隔阈值。若相邻采样点的时间间隔同时大于前面窗口与后面窗口内时间间隔的均值的\gama倍，则将信令轨迹从该采样点处进行切分。

提出轨迹点的速度过滤、角度过滤、重复点过滤、下采样过滤、加权均值平滑。
![RMF-CMF]({{site.url}}/images/LHMM-filter.png)


Step4：
传统的评估指标：精度与召回
存在的问题：
(1) precision and recall cannot simultaneously account for missing and redundant mismatched road segments;
(2) the matching path often locates on parallel side roads of the ground-truth path (e.g. urban viaduct and its underlying roads), it can be regarded as a successful matching for some low-demanding applications.
(3) 缺少针对HMM的候选准备阶段的质量评估
贡献：针对性地设计路网匹配评估指标：
(1) RMF: we design an error metric: Route Mismatch Fraction (RMF), which focuses on the ratio of the length of missing and redundant mismatched road segments to the ground-truth path length.
(2) CMF: we propose coarser-grained Corridor Mismatch Fraction (CMF), which uses a x-meter-wide corridor to wrap the matching path and to measure matching accuracy.
(3) HR: to evaluate HMM-based methods’ quality of candidate preparation, we propose Hitting Ratio (HR), which focuses on the proportion of candidate road sets covering the traveled path.
![RMF-CMF]({{site.url}}/images/LHMM-RMF-CMF.png)

step5:
实现一个交互式可视化路网匹配效果的一个网站，编写前后端代码。利用前端可视化，进行HMM超参调整。
网站的链接为(暂时只部署在局域网中)
网站开源的代码放在仓库中。
前端使用Vue实现。
后端使用Flask+pyechart+BMap实现。
数据库使用spark+MongoDB实现。

step6:
通过可视化交互，我们发现HMM中存在大量绕路匹配现象。
贡献：我们设计了绕路识别指标，来判断HMM匹配结果中是否存在绕路。并通过忽略/跳过绕路对应的噪声轨迹点，来避免绕路匹配。
进一步探索：我们发现绕路现象本质是由于HMM为每个轨迹点寻找它对应的道路。而如果在候选准备过程中，候选道路集合中缺少真实行驶道路，那么这样会不可避免产生绕路。
进一步的改进策略：改善Viterbi算法的篱笆图，添加一系列shortcuts，并设计无缝衔接到HMM寻路过程的算法。
![]({{site.url}}/images/LHMM-detour.png)
![]({{site.url}}/images/LHMM-detour-2.png)

step7:
进一步学术的思考：
传统方法计算发射概率与转移概率的缺陷：仅考虑了显式物理特征，缺少对统计特征与隐式特征的挖掘。
贡献：设计了一套使用深度学习增强HMM的路网匹配算法，基于深度学习进一步增强HMM算法中的observation probability与transition probability
参照 [PPT Link]({{site.url}}/files/ICDE-camera.pdf)
