---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

Scholarly Contributions
======
- [ICDE 2023](https://icde2023.ics.uci.edu/) - The 39th IEEE International Conference on Data Engineering
  
  "LHMM: A Learning Enhanced HMM Model for Cellular Trajectory Map Matching"
  **Weijie Shi**, Jiajie Xu, Junhua Fang, Pingfu Chao, An Liu, and Xiaofang Zhou
  [Download PDF](https://shiweijiezero.github.io/weijie.github.io/files/LHMM.pdf)
  [Access Source Code](https://github.com/shiweijiezero/LHMM)

The brief description of this paper is as follows:
As shown in figure 1, **map-matching** is a problem to align recorded location data to a digital map. It has been well studied to map GPS data collected from vehicles to paths in a road network. The problem of **Cellular Trajectory Map-Matching (CTMM)** is a new problem that deals with trajectories of cellular-based positioning data. It has a wide range of applications, for example, for telecommunication companies to understand and predict traffic information based on telecom tokens obtained from vehicles. CTMM is a significantly more challenging task that faces much **lower data precision and higher positioning errors**.
![]({{site.url}}/images/figure1.png)


While **Hidden Markov Model (HMM)** based methods can achieve satisfactory results for GPS-based map matching, we show that they cannot be directly applied to the CTMM problem. In this paper, we aim at **reducing the impact of positioning errors by incorporating knowledge obtained by neural networks into learned probabilities**.

As depicted in figure 2, a multi-relational graph learning method is developed to generate meaningful embedding, with multi-relational useful information fully preserved in a shared space. An attentive neural network is then designed as the learner for observation probability, incorporating the knowledge of the dynamic correlation between roads and cell towers under varying trajectory contexts. A transition probability learner is used to capture implicit deep features for enhanced transition probability modeling. Finally, the learned observation and transition probabilities are seamlessly integrated into HMM to guide more accurate path-finding. Extensive experiments on two large-scale cellular datasets reveal that our approach achieves high accuracy and robustness on CTMM.
![]({{site.url}}/images/figure2.png)

- 专利、软著
  
  一种面向信令数据的定位与路网匹配方法及系统 专利号：2023103350720
  
  基于seq2seq模型的智能聊天机器人系统 登记号：2020SR1702072