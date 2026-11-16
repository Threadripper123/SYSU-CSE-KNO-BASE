# Lecture7-机器学习5-非监督聚类

> 主题库：人工智能　｜　源文件：Lecture7-机器学习5-非监督聚类.pdf　｜　27,991 字符　｜　159 个片段　｜　更新于 2026-11-11


人工智能
机器学习—非监督算法（聚类）

机器学习类型

                   3 
What is Cluster Analysis?
lFinding groups of objects such that the objects in a group 
will be similar (or related) to one another and different 
from (or unrelated to) the objects in other groups
Inter-cluster 
distances are 
maximized
Intra-cluster 
distances are 
minimized

                   4 
Clustering in Social Network

                   5 
Clustering in Image Segmentation

                   6 
What is not Cluster Analysis?
lSupervised classification
–Have class label information
lSimple segmentation
–Dividing students into different registration groups 
alphabetically, by last name

                   7 
Notion of a Cluster can be Ambiguous
How many clusters?
Four Clusters Two Clusters 
Six Clusters 

                   8 
Notion of a Cluster can be Ambiguous

                   9 
Types of Clusterings
lA clustering is a set of clusters
lImportant distinction between hierarchical and 
partitional sets of clusters 
lPartitional Clustering (分割式聚类)
–A division data objects into non-overlapping subsets (clusters) 
such that each data object is in exactly one subset
–# clusters is needed, kmeans....
lHierarchical clustering (阶层式聚类)
–A set of nested clusters organized as a hierarchical tree 
–# clusters is not needed

                   10 
Partitional Clustering
Original PointsA Partitional  Clustering

                   11 
Hierarchical Clustering
p4
p1
p3
p2
 
p4 
p1 
p3 
p2 
p4
p1p2
p3
p4
p1p2
p3
Traditional Hierarchical Clustering
Non-traditional Hierarchical ClusteringNon-traditional Dendrogram
Traditional Dendrogram

                   12 
Types of Clusters
l Well-separated clusters 
l Center-based clusters
l Contiguous clusters
l Density-based clusters
l Property or Conceptual
l Described by an Objective Function

                   13 
Types of Clusters: Well-Separated
lWell-Separated Clusters: 
–A cluster is a set of points such that any point in a cluster is 
closer (or more similar) to every other point in the cluster than 
to any point not in the cluster. 
3 well-separated clusters

                   14 
Types of Clusters: Center-Based
lCenter-based
– A cluster is a set of objects such that an object in a cluster is 
closer (more similar) to the “center” of a cluster, than to the 
center of any other cluster  
–The center of a cluster is often a centroid, the average of all 
the points in the cluster, or a medoid, the most “representative” 
point of a cluster 
4 center-based clusters

                   15 
Types of Clusters: Contiguity(邻近)-Based
lContiguous Cluster (Nearest neighbor or 
Transitive)
–A cluster is a set of points such that a point in a cluster is 
closer (or more similar) to one or more other points in the 
cluster than to any point not in the cluster.
8 contiguous clusters

                   16 
Types of Clusters: Density-Based
lDensity-based
–A cluster is a dense region of points, which is separated by 
low-density regions, from other regions of high density. 
–Used when the clusters are irregular or intertwined(不规则or纠
缠), and when noise and outliers are present. 
6 density-based clusters

                   17 
Types of Clusters: Conceptual(概念) Clusters
lShared Property or Conceptual Clusters
–Finds clusters that share some common property or represent 
a particular concept. 
 
2 Overlapping Circles

                   18 
Types of Clusters: Objective Function
lClusters Defined by an Objective Function
–Finds clusters that minimize or maximize an objective 
function. 
–穷举法：Enumerate all possible ways of dividing the 
points into clusters and evaluate the `goodness' of each 
potential set of clusters by using the given objective 
function.  (NP Hard)
–Can have global or local objectives.
u Hierarchical clustering algorithms typically have local objectives
u Partitional algorithms typically have global objectives

                   19 
Types of Clusters: Objective Function ...
lMap the clustering problem to a different domain and solve a related 
problem in that domain
–Proximity（亲近度） matrix defines a weighted graph, where the nodes 
are the data points, and the weighted edges represent the proximities 
(similarity or dissimilarity) between data points
–Similarity? Dissimilarity?           
Euclidean Distance / Manhattan Distance / Chebyshev Distance/ 
Cosine Similarity /.....
Similarity=1-Dissimilarity

                   20 
Bonus: Some Distance Metrics
l퐀
퐀
 
퐀
1
,퐀
2
 
=  
퐀=1
퐀
 퐀
퐀
 
퐀
 
−퐀
퐀
 
퐀
 
 
퐀
  
1
퐀
l퐀
1
 
퐀
1
,퐀
2
 
= 
퐀=1
퐀
 퐀
퐀
 
퐀
 
−퐀
퐀
 
퐀
 
             Manhattan Distance
l퐀
2
 
퐀
1
,퐀
2
 
= 
퐀=1
퐀
 
퐀
퐀
 
퐀
 
−퐀
퐀
 
퐀
 
 
2
       Euclidean Distance 
l퐀
∞
 
퐀
1
,퐀
2
 
=
∞
 
퐀=1
퐀
 퐀
퐀
 
퐀
 
−퐀
퐀
 
퐀
 
 
∞
=퐀栀퐀  퐀
퐀
−퐀
퐀
      
    Chebyshev Distance
l퐀
−∞
 
퐀
1
,퐀
2
 
=
−∞
 
퐀=1
퐀
 퐀
퐀
 
퐀
 
−퐀
퐀
 
퐀
 
 
−∞
=퐀栀퐀  퐀
퐀
−퐀
퐀
  
lCosine Distance: 퐀
 
퐀
1
,퐀
2
 
=1−cos
 
퐀
1
,퐀
2
 
=1−
퐀
1
∙퐀
2
 
 
퐀
1
 
 
2
 
 
퐀
2
 
 
2

                   21 
Types of Clusters: Objective Function ...
–Clustering is equivalent to breaking the graph into connected 
components, one for each cluster. 
–Want to minimize the edge weight between clusters and maximize the 
edge weight within clusters 
C
i,
:      
   
a community
|C
i
|:     # nodes in C
i
vol(C
i
): sum of degrees in C
i

                   22 
Clustering Algorithms
lHierarchical clustering (Omit, because...)
lK-means and its variants
lDensity-based clustering

                   23 
Clustering Algorithms
lHierarchical clustering
Hierarchical clustering starts by treating each observation 
as a separate cluster. 
Then, it repeatedly executes the following two steps:
(1)identify the two clusters that are closest together, 
(2)merge the two most similar clusters. 
This iterative process continues until all the clusters are 
merged together. 

                   24 
K-means Clustering
lPartitional clustering approach 
lEach cluster is associated with a centroid (center point) 
lEach point is assigned to the cluster with the closest 
centroid
lNumber of clusters, K, must be specified
lThe basic algorithm is very simple
https://www.naftaliharris.com/blog/visualizing-k-means-clustering/

                   25 
K-means Clustering – Details
lInitial centroids are often chosen randomly.
–Clusters produced vary from one run to another.
lThe centroid is (typically) the mean of the points in the 
cluster.
l‘Closeness’ is measured by Euclidean distance, cosine 
similarity, correlation, etc.
lK-means will converge for common similarity measures 
mentioned above.
lMost of the convergence happens in the first few 
iterations.
–Often the stopping condition is changed to ‘Until relatively few 
points change clusters’
lComplexity is O( n * K * I * d )
–n = number of points, K = number of clusters, 
I = number of iterations, d = number of attributes

                   26 
Two different K-means Clusterings
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Sub-optimal Clustering
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Optimal Clustering
Original Points

                   27 
Importance of Choosing Initial Centroids
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 1
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 2
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 3
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 4
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 5
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 6

                   28 
Importance of Choosing Initial Centroids
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 1
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 2
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 3
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 4
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 5
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 6

                   29 
Evaluating K-means Clusters
lMost common measure is Sum of Squared Error (SSE)
–For each point, the error is the distance to the nearest cluster
–To get SSE, we square these errors and sum them.
–x is a data point in cluster C
i 
and m
i
 is the representative point for 
cluster C
i
 
u can show that m
i 
corresponds to the center (mean) of the cluster
–Given two clusters, we can choose the one with the smallest 
error
–One easy way to reduce SSE is to increase K, the number of 
clusters
u A good clustering with smaller K can have a lower SSE than a poor 
clustering with higher K
퐀퐀퐀= 
栀=1
퐀
 
퐀∈퐀
퐀
퐀栀퐀퐀
2
 퐀
퐀
,퐀 

                   30 
Bonus: how to choose a lovely “K”
plot a line chart of the SSE for 
each value of k. If the line chart looks like an arm, 
then the "elbow" on the arm is the value of K that 
is the best.
l手臂曲线的肘关节
https://bl.ocks.org/rpgove/0060ff3b656618e9136b

                   31 
Importance of Choosing Initial Centroids ...
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 1
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 2
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 3
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 4
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 5

                   32 
Importance of Choosing Initial Centroids ...
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 1
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 2
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 3
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 4
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 5

                   33 
Importance of Choosing Initial Centroids
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 1
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 2
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 3
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 4
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 5
-2-1.5-1-0.500.511.52
0
0.5
1
1.5
2
2.5
3
x
y
Iteration 6

                   34 
Problems with Selecting Initial Points
lIf there are K ‘real’ clusters then the chance of selecting 
one centroid from each cluster is small. 
–Chance is relatively small when K is large
–If clusters are the same size, n, then
–For example, if K = 10, then probability = 10!/10
10
 = 0.00036
–Sometimes the initial centroids will readjust themselves in 
‘right’ way, and sometimes they don’t
–Consider an example of five pairs of clusters

                   35 
10 Clusters Example
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 1
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 2
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 3
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 4
Starting with two initial centroids in one cluster of each pair of clusters

                   36 
10 Clusters Example
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 1
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 2
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 3
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 4
Starting with two initial centroids in one cluster of each pair of clusters

                   37 
10 Clusters Example
Starting with some pairs of clusters having three initial centroids, while other have only one.
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 1
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 2
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 3
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 4

                   38 
10 Clusters Example
Starting with some pairs of clusters having three initial centroids, while other have only one.
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 1
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 2
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 3
05101520
-6
-4
-2
0
2
4
6
8
x
y
Iteration 4

                   39 
Solutions to Initial Centroids Problem
lMultiple runs
–Helps, but probability is not on your side
lSample and use hierarchical clustering to 
determine initial centroids
lSelect more than k initial centroids and then 
select among these initial centroids
–Select most widely separated
lBisecting K-means
–Not as susceptible to initialization issues

                   40 
Handling Empty Clusters
lBasic K-means algorithm can yield empty clusters
lChoose a replacement centroid, several strategies
–Choose the point that contributes most to SSE
–Choose a point from the cluster with the highest SSE
–If there are several empty clusters, the above can be 
repeated several times.

                   41 
Pre-processing and Post-processing
lPre-processing
–Normalize the data
–Eliminate outliers
lPost-processing
–Eliminate small clusters that may represent outliers
–Split ‘loose’ clusters, i.e., clusters with relatively high 
SSE
–Merge clusters that are ‘close’ and that have relatively 
low SSE

                   42 
Bisecting K-means
lBisecting K-means algorithm
–Variant of K-means that can produce a partitional or a 
hierarchical clustering

                   43 
Bisecting K-means Example

                   44 
lK-means has problems when clusters are of 
differing 
–Sizes
–Densities
–Non-globular shapes
lK-means also has problems when the data 
contains outliers.
Limitations

                   45 
Limitations of K-means: Differing Sizes
Original Points
K-means (3 Clusters)

                   46 
Limitations of K-means: Differing Density
Original Points
K-means (3 Clusters)

                   47 
Limitations of K-means: Non-globular Shapes
Original Points
K-means (2 Clusters)

                   48 
Overcoming K-means Limitations
Original PointsK-means Clusters
One solution is to use many clusters.
Find parts of clusters, but need to put together.

                   49 
Original PointsK-means Clusters
Overcoming K-means Limitations

                   50 
Overcoming K-means Limitations
Original PointsK-means Clusters

                   51 
K-means++ [Arthur et al. ’07]
lSpreads out the centers
lChoose first center, x1, uniformly at random from 
the data set 
lRepeat for 2 ≤ i ≤ k: 
–Choose to be equal to a data point sampled 
from the distribution: 
: the shortest distance from a data point  to 
the closest center we have already chosen

                   52 
K-means++ [Arthur et al. ’07]

                   53 
K-means++ [Arthur et al. ’07]

                   54 
K-means++ [Arthur et al. ’07]

                   55 
K-means++ [Arthur et al. ’07]

                   56 
K-means++ [Arthur et al. ’07]

                   57 
K-means++ [Arthur et al. ’07]
lWhat’s wrong with K-means++?
–Needs K passes over the data
–In large data applications, not only the data is 
massive, but also K is typically large (e.g., easily 
1000). 
–Does not scale!

                   58 
Scalable K-means: K-means||
lIntuition of K-means||
–K-means++ samples one point per iteration and 
updates its distribution 
–What if we oversample by sampling each point 
independently with a larger probability? 
–Intuitively equivalent to updating the distribution much 
less frequently
–Coarser sampling
–Turns out to be sufficient: K-means|| 

                   59 
Scalable K-means
l主要思路在于改变每次遍历时的取样策略，并非按照 k-
means++ 那样每次遍历只取样一个样本，而是每次遍历
取样 O(k) 个样本，重复该取样过程大约 O(logn) 次，共
得到 O(klogn) 个样本点组成的集合
l然后再聚类这 O(klogn) 个点成 k 个点，最后将这 k 个点
作为初始聚类中心
l实际实验证明 O(logn) 次重复取样是不需要的，一般5次
重复取样就可以得到一个较好的聚类初始中心

                   60 
Scalable K-means
lK-means|| Initialization

                   61 
Scalable K-means
lK-means|| Initialization

                   62 
Scalable K-means
lK-means|| Initialization

                   63 
Scalable K-means
lK-means|| Initialization

                   64 
Scalable K-means
lK-means|| Initialization

                   65 
Scalable K-means
lK-means|| [Bahmani et al. ’12]
–Choose l>1 [Think l=Θ(k)] 
–Initialize C to an arbitrary set of points
–For R iterations do: 
–Sample l points, each point x in X independently with 
probability 
–Add all the sampled points to C 
–Cluster the (weighted) points in C to find the final k 
centers 

                   66 
Scalable K-means
lK-means, K-means++, and K-means||

                   67 
Scalable K-means
lTheorem
–Optimal clustering： the sum of the squared distances 
between each point and its closest center is minimal
uSolving this problem exactly is NP-hard
–Let ψ= cost of initial clustering, ψ’=cost of clustering after 
convergence, OPT=cost of the optimal clustering
–Using K-means++ for clustering the intermediate centers, 
the overall approximation factor = O(log k)
uIt means ψ’ = O(log k)*OPT 
u也就是说最终确定的质心能够使cost达到最优聚类的cost的O（log k）倍
–K-means|| produces a constant-factor approximation to 
OPT, using only O(log (ψ/OPT)) iterations 

                   68 
Scalable K-means
lExperimental Results: Quality
Clustering Cost Right 
After Initialization 
Clustering Cost After 
Convergence
RandomNA22,000
K-means++ 43065
K-means|| 1614

                   69 
Scalable K-means
lExperimental Results: Convergence
–K-means|| reduces number of iterations even more 
than K-means++
Iterations 
After Initialization 
Random167
K-means++ 42
K-means|| 28
SPAM: 4,601 points in 58 dimensions 
K=50

                   70 
lDBSCAN is a density-based algorithm.
–Density = number of points within a specified radius (Eps)
–A point is a core point if it has more than a specified number 
of points (MinPts) within Eps 
uThese are points that are at the interior of a cluster
–A border point has fewer than MinPts within Eps, but is in 
the neighborhood of a core point
–A noise point is any point that is not a core point or a border 
point. 
DBSCAN

                   71 
DBSCAN-Density

                   72 
Core, border, noise points

                   73 
DBSCAN: Core, Border, and Noise Points

                   74 
lCenter-based approach
DBSCAN Algorithm

                   75 
DBSCAN: Core, Border and Noise Points
Original Points
Point types: core, 
border and noise
Eps = 10, MinPts = 4

                   76 
When DBSCAN Works Well
Original Points
Clusters
• Resistant to Noise
• Can handle clusters of different shapes and sizes

                   77 
When DBSCAN Does NOT Work Well
Original Points
(MinPts=4, Eps=9.75). 
 (MinPts=4, Eps=9.92)
• Varying densities
• High-dimensional data

                   78 
DBSCAN: Determining EPS and MinPts
lIdea is that for points in a cluster, their k
th
 nearest 
neighbors are at roughly the same distance
lNoise points have the k
th
 nearest neighbor at farther 
distance
lSo, plot sorted distance of every point to its k
th
 
nearest neighbor

                   79 
Time and Space Complexity

                   80 
lA Density-Based Algorithm for Discovering 
Clusters in Large Spatial Databases with 
Noise [KDD 1996]
2014 KDD TEST OF TIME AWARD

                   81 
2014 KDD TEST OF TIME AWARD
lA Density-Based Algorithm for Discovering 
Clusters in Large Spatial Databases with 
Noise [KDD 1996]
–Proposed the now well-known clustering algorithm 
DBSCAN
–10000+ citations

                   82 
2014 KDD TEST OF TIME AWARD
lA Density-Based Algorithm for Discovering 
Clusters in Large Spatial Databases with 
Noise [KDD 1996]
–Claimed that the average run time complexity of 
DBSCAN is O(n*logn)
–Follow papers mis-claimed that the complexity of 
DBSCAN is O(n*logn)

                   83 
2015 SIGMOD Best Paper Award 
lDBSCAN Revisited: Mis-Claim, Un-Fixability, and 
Approximation.

                   84 
2015 SIGMOD Best Paper Award 
l

                   85 
2015 SIGMOD Best Paper Award 

                   86 
DBSCAN Revisited
l怀疑的精神
l善于思考，善于发现问题
l科学研究的发展是在曲折中不断前进的！

                   87 
Reference
lEster, Martin, et al. "A density-based algorithm for discovering clusters 
in large spatial databases with noise." Kdd. Vol. 96. No. 34. 1996.
lGan, Junhao, and Yufei Tao. "DBSCAN Revisited: Mis-Claim, Un-
Fixability, and Approximation." Proceedings of the 2015 ACM SIGMOD 
International Conference on Management of Data. ACM, 2015.
l如何评价 SIGMOD 2015 最佳论文《DBSCAN Revisited》？
http://www.zhihu.com/question/31845624

                   88 
Comparing K-means and DBSCAN
lBoth are partitional clustering that assign each 
object to a single cluster
–K-means: all object
–DBSCAN: discard noise 
lType
–K-means: prototype-based notion
–DBSCAN: density-based notion
lBoth perform poorly with widely differing densities
–K-means: has difficult with non-globular cluster and 
clusters with different size
–DBSCAN: can handle clusters of different size and 
shapes, not affected by nosie 

                   89 
Comparing K-means and DBSCAN
lDefinition requirement
–K-means: need well-defined centroid
–DBSCAN: need definition of density
lSparse, high-dimensional data
–K-means: work well, such as document data
–DBSCAN: performs poorly for Euclidean definition of 
density
lDistribution of data
–K-means: spherical Gaussian distributions 
–DBSCAN: no assumption

                   90 
Comparing K-means and DBSCAN
lBoth using all attributes
lOverlap
–K-means: can find clusters
–DBSCAN: merge clusters that overlap
lDetermination
–K-means: no
–DBSCAN: yes

                   91 
Comparing K-means and DBSCAN
lNumber of clusters
–K-means: user define
–DBSCAN: automatically, 
    Two parameters: Eps and Minpts

                   92 
Cluster Validity 
lFor supervised classification we have a variety of 
measures to evaluate how good our model is
–Accuracy, precision, recall
lFor cluster analysis, the analogous question is how to 
evaluate the “goodness” of the resulting clusters?
lBut “clusters are in the eye of the beholder”! 
lThen why do we want to evaluate them?
–To avoid finding patterns in noise
–To compare clustering algorithms
–To compare two sets of clusters
–To compare two clusters

                   93 
1.Determining the clustering tendency聚类趋势 of a set of data, i.e., 
distinguishing whether non-random structure actually exists in the 
data  聚类的前提需要数据是非均匀分布的
2.Comparing the results of a cluster analysis to externally known 
results, e.g., to externally given class labels.
3.Evaluating how well the results of a cluster analysis fit the data 
without reference to external information. 
- Use only the data
4.Comparing the results of two different sets of cluster analyses to 
determine which is better.
5.Determining the ‘correct’ number of clusters.
For 2, 3, and 4, we can further distinguish whether we want to 
evaluate the entire clustering or just individual clusters. 
Different Aspects of Cluster Validation 簇评估

                   94 
lNumerical measures that are applied to judge various aspects 
of cluster validity, are classified into the following three types.
–External Index: Used to measure the extent to which cluster labels 
match externally supplied class labels.
uEntropy 熵
–Internal Index:  Used to measure the goodness of a clustering 
structure without respect to external information. 
uSum of Squared Error (SSE) 
–Relative Index: Used to compare two different clusterings or 
clusters. 实际上不是一个单独的簇评估类型，而是度量的具体使用
uOften an external or internal index is used for this function, e.g., SSE or 
entropy
lSometimes these are referred to as criteria instead of indices
–However, sometimes criterion is the general strategy and index is the 
numerical measure that implements the criterion.
Measures of Cluster Validity

                   95 
lTwo matrices 
–Proximity Matrix (Similarity/Dissimilarity Matrix)
–“Incidence” Matrix
uOne row and one column for each data point
uAn entry is 1 if the associated pair of points belong to the same cluster
uAn entry is 0 if the associated pair of points belongs to different clusters
lCompute the correlation between the two matrices
–Since the matrices are symmetric, only the correlation between 
n(n-1) / 2 entries needs to be calculated.
lHigh correlation indicates that points that belong to the 
same cluster are close to each other. 
lNot a good measure for some density or contiguity based 
clusters.
Measuring Cluster Validity Via Correlation

                   96 
Measuring Cluster Validity Via Correlation
lCorrelation of incidence and proximity matrices 
for the K-means clusterings of the following two 
data sets. 
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y
Corr = 0.9235Corr = 0.5810

                   97 
lOrder the similarity matrix with respect to cluster 
labels and inspect visually. 
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y
Points
P
o
i
n
t
s
20406080100
10
20
30
40
50
60
70
80
90
100
Similarity
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
Using Similarity Matrix for Cluster Validation

                   98 
Using Similarity Matrix for Cluster Validation
lClusters in random data are not so crisp
Points
P
o
i
n
t
s
20406080100
10
20
30
40
50
60
70
80
90
100
Similarity
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
DBSCAN
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y

                   99 
Points
P
o
i
n
t
s
20406080100
10
20
30
40
50
60
70
80
90
100
Similarity
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
Using Similarity Matrix for Cluster Validation
lClusters in random data are not so crisp
K-means
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y

                   
100 
Using Similarity Matrix for Cluster Validation
lClusters in random data are not so crisp
00.20.40.60.81
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
x
y
Points
P
o
i
n
t
s
20406080100
10
20
30
40
50
60
70
80
90
100
Similarity
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
Complete linkage hierarchical clustering

                   
101 
Using Similarity Matrix for Cluster Validation
1 
2
3
5
6
4
7
DBSCAN
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
50010001500200025003000
500
1000
1500
2000
2500
3000

                   
102 
lClusters in more complicated figures aren’t well separated
lInternal Index:  Used to measure the goodness of a clustering 
structure without respect to external information
–SSE
lSSE is good for comparing two clusterings or two clusters 
(average SSE).
lCan also be used to estimate the number of clusters
Internal Measures: SSE
2 5 1015202530
0
1
2
3
4
5
6
7
8
9
10
K
S
S
E
51015
-6
-4
-2
0
2
4
6

                   
103 
Internal Measures: SSE
lSSE curve for a more complicated data set
1 
2
3
5
6
4
7
SSE of clusters found using K-means

                   
104 
lCluster Cohesion 凝聚度: Measures how closely 
related are objects in a cluster
–Example: SSE
lCluster Separation 分离度: Measure how distinct 
or well-separated a cluster is from other clusters
lExample: Squared Error
–Cohesion is measured by the within cluster sum of squares (SSE)
–Separation is measured by the between cluster sum of squares
–Where |C
i
| is the size of cluster i 
Internal Measures: Cohesion and Separation
2
()
i
i
ixC
SSExm



2
()
ii
i
SSBCmm


                   
105 
Internal Measures: Cohesion and Separation
lExample: 
SSE+SSB=TSS constant
12345

m
1
m
2
m
2222
22
(11.5)(21.5)(44.5)(54.5)1
2(31.5)2(4.53)9
1910
SSE
SSB
Total



K=2 clusters:
2222
2
(13)(23)(43)(53)10
4(33)0
10010
SSE
SSB
Total



K=1 cluster:
2
()
x
TSSxm


                   
106 
lA proximity graph based approach can also be used for 
cohesion and separation.
–Cluster cohesion is the sum of the weight of all links within a cluster.
–Cluster separation is the sum of the weights between nodes in the cluster 
and nodes outside the cluster.
Internal Measures: Cohesion and Separation
cohesionseparation

                   
107 
   “The validation of clustering structures is the most 
difficult and frustrating part of cluster analysis. 
   Without a strong effort in this direction, cluster 
analysis will remain a black art accessible only to 
those true believers who have experience and 
great courage.”
Algorithms for Clustering Data, Jain and Dubes
Final Comment on Cluster Validity

                   
108 
Q：假如灭霸一个响指就能灭掉一半生命，如何挑选消灭
对象？
Discussion
p4
p1p2
p3
Method 1：Hierarchical Clustering
将全宇宙的生命按照基因相似度构造Dendrogram，确定聚类
数目后，在每两个相邻的聚类簇中随机挑选一个进行消灭，以
保证宇宙生物多样性。

                   
109 
Method 2：K-means Clustering
宇宙包含“九大国度”，为防止某个国度灭
绝，在每个国度选取一个聚类中心，通过K-
means得到聚类簇，再在每个聚类中随机选
取一半生物进行消灭，宇宙生物多样性得到
进一步保证。
Discussion
