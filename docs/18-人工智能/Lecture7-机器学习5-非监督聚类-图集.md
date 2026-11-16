# Lecture7-机器学习5-非监督聚类-图集

> 主题库：人工智能　｜　源文件：Lecture7-机器学习5-非监督聚类-图集.pdf　｜　6,042 字符　｜　46 个片段　｜　更新于 2026-11-11
中大               SUNYATSENUNIVERSITY

回归 k近邻 SVM 决策树监督学习朴素贝叶斯有标签：对于输入数据X能预测Y 逻辑回归 XGBoost Lightgbm 机器学习聚类降维算法无标签： 对于输入数据X能发现什么 [监 M算法 … 马尔科策方法强化学习序列决策问题

大

Data Data

Cluster0 cluster1 Cluster2 cluster3 Clustee4

Opac 6 Mask0% HESD4019 Sky Building Road Sidewalk Fence Vegetation Pole Car Sign Pedestrian Cyclist

What isa natural grouping among these objects? Simpson''s Family School Employees Females Males

point x 3 y p1 p2 p3 p4 2 p1 p1 0 2 pI 0.000 2.828 3.162 5.099 p3 p4 p2 2 0 p2 2.828 0.000 1.414 3.162 1 p2 p3 3 1 p3 3.162 1.414 0.000 2.000  p4 5 1 p4 5.099 3.162 2.000 0.000 01 1 2 3 4 5 6 Figure Fourpointsandtheircorrespondingdataandproximity(distance matrices.

TOOEASY memogonorator.not

Identify the two clusters that Merge the two most similar aaelesesttogether lusters

Dendrogram A A B C D E F

1: SelectK pointsastheinitialcentroids. 3: Form K clusters by assigning all points to the closest centroid. 4:  Recomputethecentroidofeach cluster.

Optimal number of clusters 600— 200 3 4 5 6 7 8 9 10 Numberofclustersk

6.8  13 18 6.5 9 10 15 !16 18.5 7.75 12.5 17.25 6.5 9 10 15 16 18.5

1: Initialize the list ofclusters to contain the cluster containing all points. 2:repeat 3: Select a cluster from the listof clusters 4: fori = 1to number_of_iterations do 5: Bisect the selected cluster using basic K-means 6: endfor 7: Add the two clusters from the bisection with the lowest SSE to the list of clusters. 8: until Until the list ofclusters contains K clusters

Tteration 1 8F 6 4- 2   >0 -21 -4- -6 0 5 10 15 20 X

lteration 2 8F 6 4- 2 >0 -21 -4- -6 0 5 10 15 20 X

lteration 3 8F 6 4- 2   >0 -21 -4- -6 0 5 10 15 20 X

Yaerataon 4 6 4 2   >0 -2 -4- -6 0 5 10 15 20

lteration 5 8F 6 4- 2   >0 -21 -4- -6 0 5 10 15 20 X

Iteration 6 8F 6 4- 2  0 -21 -4- -6 0 5 10 15 20 X

Iteration 7 8F 6 4- 2  >0 -21 -4- -6 0 5 10 15 20 X

Iteration 8 8F 6 4- 2  0 -21 -4- -6 0 5 10 15 20 X

Iteration 9 87 6 4 2 4 口 -4 -6 1 20 0 5 10 15

Iteration 10 87 6 4 2    >0 - -4 -6 0 5 10 15 20

-2  -3 -4 -3 -2 -1 0 1 2 3 4 X

10             5        -51 0 0 0 1 1 1 1 -15 -10 -5 5 10

15 10 5 -5 9 1 上上 -15 -10 -5 5 10 15

15 年旧 10 5 0 -5 10 15 -15 -10 -5 5 X

D(x_)2 ∑ x ∈x D(x): 2

Oversa pling factor =3

Ovetsampliong factor =3

Oversaompling factor =3

Oversampling factor =3

Oversamplingfactor=3 Cluster the iotenoediate ceoters

R =k:Sinoulating K-means++ (=1) 》 Strong guarantee Small R: K-mneans || > Can it possibly givc avoy guaurauntecs? T R=O:Iloyd ≥ Noguarantees

。  Eps Density= points Figure 8.20. Center-based densi Density = number of points within a specified radius (Eps)

bareerpoint cerepoint 0 noise point A MinPts=4 C Eps EpS Figure 8.21. Core, border, and noise points. A pointis acore pointif ithas morethan aspecified number ofpoints (MinPts) within Eps. Theseare points that areattheinteriorofacluster A border point has fewer than MinPts within Eps, butis in the neighborhood of a core point Anoisepointisany pointthatisnota corepointora borderpoint.

1.5 Noise Pdint 1 0.5F OF Core Pojnt -0.5 Border Point -1上 -1.5 MinPts = 4 1 1 -2  -1 0 1 2

Algorithm 8.4 DBSCANalgorithm. 1: abelallpointsascore,border,ornoisepoints. 2: Eliminate noise points. 3: Put an edge between all core points that are within Eps of each other. tMakeeacgroupofconnectedcorepoinsinoaseparatecluser 5: Assign each border point to one of the clusters ofits associated core points.

4 5 5 5 0 500 1000 1500 2000 2500 3000 Points Sorted According to Distance of 4th Nearest Neighbor

The basic time complexity of the DBSCAN algorithm is O(m × time to find points in the Eps-neighborhood), where m is the number of points. In the worst case, this complexity is O(m²). However, in low-dimensional spaces, there are data structures, such as kd-trees, that allow efficient retrieval of all

points within a given distance of a specified point, and the time complexity can be as low as O(m log m). The space requirement of DBSCAN, even for high-dimensional data,is O(m) because itis only necessary to keep a small amount of data for each point, i.e., the cluster label and the identification of eachpointasacore, order,ornoisepoint

[ Aug182014/ 2014SlGKDDTestofTimeAward: TheSlGKDDTestofTime awardrecognizesoutstanding papers frompast KDDConferences beyond the lastdecade thathave f had an important impact on the data mining research community. The 2014 Testof Time award recognizes the following influential contributions to SlGKDD that have withstood the test of time: A Density-Based Algorithm for Discovering Clusters in Large Spatial Databases with Noise [KDD 1996]

[PDF] A density-basedalgorithm for discovering clusters in large spatial databases withnoise. M Ester. HP Kriegel.JSander XXu-Kdd, 1996 -aaai.org Abstract Clustering algorithms are attractive for the task of class identification in spatial databases. However the application to large spatial databases rises the following requirements for clustering algorithms: minimal requirements of domain knowledge to determine the input parameters, discovery of clusters with arbitrary shape and good efficiency on large databases. The well-known clustering algorithms offer no solution to... 被引用次数：10010 相关文章所有75个版本引用保存更多

Recipients of the award are the following: 2015 DBSCANRevisited: Mis-Claim, Un-Fixability, and Approximation. YufeiTao, Junhao Gan 20l4 Materialization Optimizations for Feature Selection Workloads. Ce Zhang, ArunKumar, Christopher Ré 2013 Massive Graph Triangulation. XiaochengHu, Yufei Tao, Chin-Wan Chung RSS 2012 High-Pe 2013 SIGMOD BestPaper AwardProcessing over XML Streams. Barzan Mozafari, Kai Zeng, Carlo Zaniolo Rss SIGMOD News SIGMOD/PODS 2015 2011 Entangled Queries: Enabling Declarative Data-Driven Coordination. Nitin Gupta, Lucja Kot, Sudip Roy, Gabriel Bender, Johannes Gehrke, Christoph Koch SIGMOD Record Website FAST: Fast Architecture Sensitive TreeSearch on Modern CPUs and GPUs. Changkyu Kim, Jatin Chhugani, SlGMOD 2010 Facebook Nadathur Satish, EricSedlar, AnthonyNguyen, Tim Kaldewey, VictorLee, Scott Brandt, and Pradeep RECORD
