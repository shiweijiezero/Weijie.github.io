---
layout: archive
collection: projects
permalink: /projects/mapmatching
author_profile: true
date: 2023-05-19
---

Available Resources: [Project Proposal]({{site.url}}/files/基于信令轨迹的路网匹配项目方案.pdf) ,[PPT Link]({{site.url}}/files/ICDE-camera.pdf)

# Step1:

I used python to reproduce the SOTA HMM-based map-matching algorithm for cellular trajectories.

The SOTA paper refers to [ST-Matching](https://www.microsoft.com/en-us/research/publication/map-matching-for-low-sampling-rate-gps-trajectories/), [SnapNet](https://ieeexplore.ieee.org/abstract/document/7539689/) 

# Step2:

I transformed this Python code into a Spark+Scala architecture suitable for big data.
This task involved: setting up the Spark environment; constructing a road network graph; implementing the Dijkstra shortest path algorithm; and building an R-tree Index for the nearest k road queries.

Additionally, the core implementation of the HMM model: candidate selection, observation probability calculation, transition probability calculation, and the Viterbi path-search algorithm. 

Moreover, we reorganized the road network data by merging a large number of roads into one, enhancing the efficiency of the R-tree query.

# Step3:
**Contributions**: I applied **trajectory segmentation, filtering, and smoothing methods** for the high error, high noise, and ping-pong effects found in cellualr trajectories. 

For GPS trajectories, the main approach was to use the vehicle's passenger status label to identify each taxi ride (considered as a segment) and further divide non-passenger-carrying trajectories based on dwell time thresholds. 

For cellular trajectories, an adaptive segmentation method was designed to account for inconsistent sampling rates. 

Meanwhile, **velocity filtering, angle filtering, duplicate point filtering, downsampling, and weighted mean smoothing** were proposed to improve the quality of cellular trajectory.

![RMF-CMF]({{site.url}}/images/LHMM-filter.png)


# Step4：
Traditional evaluation metrics: precision and recall, were found to be insufficient for three main reasons:
1. precision and recall cannot simultaneously account for missing and redundant mismatched road segments;
2. the matching path often locates on parallel side roads of the ground-truth path (e.g. urban viaduct and its underlying roads), it can be regarded as a successful matching for some low-demanding applications.
3. there is a lack of quality assessment for the candidate preparation stage of the HMM. 

**Contributions:** by specifically designing map-matching evaluation metrics to address these issues:

(1) RMF: we design an error metric: Route Mismatch Fraction (RMF), which focuses on the ratio of the length of missing and redundant mismatched road segments to the ground-truth path length.

(2) CMF: we propose coarser-grained Corridor Mismatch Fraction (CMF), which uses a x-meter-wide corridor to wrap the matching path and to measure matching accuracy.

(3) HR: to evaluate HMM-based methods’ quality of candidate preparation, we propose Hitting Ratio (HR), which focuses on the proportion of candidate road sets covering the traveled path.

![RMF-CMF]({{site.url}}/images/LHMM-RMF-CMF.png)

# step5:
I developed an **interactive website for visualizing** the map-matching results. The frontend visualization was used to fine-tune HMM hyperparameters.
The site was built using Vue for frontend, and Flask, Pyecharts, and BMap for backend. The database was set up using Spark and MongoDB. 

The code for this website is open source and available in the [repository](https://github.com/shiweijiezero/mapmatching-visualization). 

The web currently deployed on a [local link](http://192.168.134.122:8000/#/)

# step6:
Through interactive visualization, we discovered a significant amount of detour matching in the HMM. 

To address this, we proposed a detour identification metric to detect detours in the HMM matching results. 

We also proposed to ignore or skip the noisy trajectory points corresponding to detours to prevent such matching.

Upon further exploration, we discovered that this phenomenon fundamentally arises from the HMM locating the corresponding roads for each trajectory point. We suggested an improved strategy to enhance the Viterbi algorithm's lattice graph by adding a series of shortcuts, which are designed to seamlessly integrate into the HMM path-finding process.

![]({{site.url}}/images/LHMM-detour.png)

![]({{site.url}}/images/LHMM-detour-2.png)

# step7:
A further academic consideration revealed that traditional methods of calculating observation and transition probabilities only considered explicit physical features and lacked the exploration of statistical and implicit features. 

To resolve this, we proposed a deep learning-enhanced HMM map-matching algorithm to enhance observation and transition probabilities in the HMM algorithm.

Detailed information could be accessed in [PPT Link]({{site.url}}/files/ICDE-camera.pdf)
