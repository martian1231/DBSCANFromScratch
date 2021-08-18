<p align= "center">
<a href="https://colab.research.google.com/drive/1GkEbe_BKiyAAi3XGN0GhFcDq3Ux8XEyi?usp=sharing"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Colab"></a>
</p>

# Density based clustering

Density clustering algorithms cluster the data based on the density of the region separated by a less dense region. The dense region can be defined using hyper parameters like a minimum number of points required within a given radius R.

<img src="https://miro.medium.com/max/1612/1*eOemT697_oOr-bqQ0B1adg.png" alt="How does DBSCAN clustering algorithm work? - Shritam Kumar Mund ..." style="zoom:50%;" />

### DBSCAN(**D**ensity-**B**ased **S**patial **C**lustering of **A**pplications with Noise)

DBSCAN is a clustering algorithm that partitions the data into dense regions separated by non-dense regions.

We call a region dense region if **The region contains the minimum number of points within a given radius epsilon**

<img src="https://i.imgur.com/JutH8xv.png" alt="img" style="zoom:50%;" />

[ClusteringAnalysis](https://cs.wmich.edu/alfuqaha/summer14/cs6530/lectures/ClusteringAnalysis.pdf)

### Key terms:

* **Density of point:** Number of points within epsilon radius from point P.
* **Dense region:** Region which has a minimum number of points given by MinPts within the epsilon radius.
* **Epsilon:** a value which denotes radius, the maximum distance between two points.
* **min point:** minimum number of points within the epsilon radius from a point P.
* **Core point:**  A point P which has _M_ (minimum number of points) points within a given radius (epsilon). M points should be in the neighborhood of point P to be a core point.
* **Border point:** A point which is not a core point (has points less than the minimum number of points to declare as a core point) but is a part of a core point (lies in the neighborhood of core point).
* **Noise point:** A point that is neither a core point nor a border point is a noise point. A point that is not a core point also which does not fall within the epsilon radius of any other point.
* **Density edge:** is the edge that connects 2 points P and Q such that P and Q are core points and their distance is less than the epsilon value.
* **Density connected points:** is the set of core points and there exists a path between core point P and Q via other core points which are connected by density edge.

<img src="https://www.researchgate.net/profile/Amineh_Amini/publication/258442676/figure/fig1/AS:613961674272771@1523391278299/DBSCAN-core-border-and-noise-points.png" alt="DBSCAN: core, border, and noise points. | Download Scientific Diagram" style="zoom:50%;" />

![How does the DBSCAN clustering algorithm work? - Shritam Kumar Mund ...](https://miro.medium.com/max/2642/1*07gsw-YZTndhH0Ftox7W5w.png)

### DBSCAN algorithm steps

<img src="https://i.imgur.com/9OkH1kT.png" alt="img" style="zoom:50%;" />

[ClusteringAnalysis](https://cs.wmich.edu/alfuqaha/summer14/cs6530/lectures/ClusteringAnalysis.pdf)

### Parameters in DBSCAN algorithm

- **eps**: Two points are considered neighbors if the distance between the two points is below the threshold epsilon.
- **min_samples**: The minimum number of neighbours a given point should have to be classified as a core point. **It’s important to note that the point itself is included in the minimum number of samples.**
- **metric**: The metric to use when calculating distance between instances in a feature array (Euclidean distance).

* Choosing right epsilon using elbow method

<img src="https://i.imgur.com/p1297bX.png" alt="img" style="zoom:50%;" />

To choose the right epsilon, for every point in the dataset, get the Nth nearest neighbour (let us say 4th NN), sort the distance in ascending order and plot it on the graph. Choose epsilon value at elbow point (or inflection point) which shows, following distances(distances greater than chosen elbow point or inflection point) are noisy points.

### Choosing hyperparameters value (value of min points and epsilon radius)

* Rule of thumb is, min points is typically equal to 2 * D where D is the dimensionality of data
* Typically we choose the large value of min points when the dataset is noisy
* Domain expect could help us choose this value

### Advantages of DBSCAN

- Is great at separating clusters of high density versus clusters of low density within a given dataset.
- Is great with handling outliers and noise within the dataset.
- DBSCAN does not require one to specify the number of clusters in the data a priori, as opposed to [k-means](https://en.wikipedia.org/wiki/K-means_algorithm).
- DBSCAN can find arbitrarily shaped clusters. It can even find a cluster surrounded by (but not connected to) a different cluster. Due to the MinPts parameter, the so-called single-link effect (different clusters being connected by a thin line of points) is reduced.
- DBSCAN has a notion of noise and is robust to [outliers](https://en.wikipedia.org/wiki/Anomaly_detection).
- DBSCAN requires just two parameters and is mostly insensitive to the ordering of the points in the database. (However, points sitting on the edge of two different clusters might swap cluster membership if the ordering of the points is changed, and the cluster assignment is unique only up to isomorphism.)
- DBSCAN is designed for use with databases that can accelerate region queries, e.g. using an [R* tree](https://en.wikipedia.org/wiki/R*_tree).
- The parameters MinPts and ε can be set by a domain expert if the data is well understood.

[DBSCAN#Advantages](https://en.wikipedia.org/wiki/DBSCAN#Advantages)

### Disadvantages of DBSCAN

- Does not work well when dealing with clusters of varying densities. While DBSCAN is great at separating *high* density clusters from *low* density clusters, DBSCAN struggles with clusters of similar density*.*
- Struggles with high dimensionality data.
- DBSCAN is not entirely deterministic: border points that are reachable from more than one cluster can be part of either cluster, depending on the order in which the data are processed. For most data sets and domains, this situation does not arise often and has little impact on the clustering result:[[5\]](https://en.wikipedia.org/wiki/DBSCAN#cite_note-tods-5) both on core points and noise points, DBSCAN is deterministic. DBSCAN*[[7\]](https://en.wikipedia.org/wiki/DBSCAN#cite_note-hdbscan1-8) is a variation that treats border points as noise, and this way achieves a fully deterministic result as well as a more consistent statistical interpretation of density-connected components.
- The quality of DBSCAN depends on the [distance measure](https://en.wikipedia.org/wiki/Metric_(mathematics)) used in the function regionQuery(P,ε). The most common distance metric used is [Euclidean distance](https://en.wikipedia.org/wiki/Euclidean_distance). Especially for [high-dimensional data](https://en.wikipedia.org/wiki/Clustering_high-dimensional_data), this metric can be rendered almost useless due to the so-called "[Curse of dimensionality](https://en.wikipedia.org/wiki/Curse_of_dimensionality#Distance_functions)", making it difficult to find an appropriate value for ε. This effect, however, is also present in any other algorithm based on Euclidean distance.
- DBSCAN cannot cluster data sets well with large differences in densities, since the minPts-ε combination cannot then be chosen appropriately for all clusters.[[8\]](https://en.wikipedia.org/wiki/DBSCAN#cite_note-WIREs-9)
- If the data and scale are not well understood, choosing a meaningful distance threshold ε can be difficult.

**Varying densities, when you have clusters with varying densities setting hyperparameters could be challenging**

<img src="https://i.imgur.com/TKQHe8u.png" alt="img" style="zoom:50%;" />

**Highly sensitive to hyperparameters settings, a slight change in hyperparameters values would change the result completely**

<img src="https://i.imgur.com/zEUaiKE.png" alt="img" style="zoom:50%;" />

### Time and space complexity

**Space complexity** is O(n * d) because we need to store all the points in the case of non-matrix-based implementation. In the case of matrix/kernel/distance-based implementation, It takes O(n<sup>2</sup>) as kernel matrix will be of shape n x n.

**Time complexity**  for each query point search time is O(log(n)), that makes O(N * log(n)) for N queries.

### DBSCAN vs K means

Unlike k-means, DBSCAN will figure out the number of clusters. DBSCAN works by determining whether the minimum number of points is close enough to one another to be considered part of a single cluster. DBSCAN is very sensitive to scale since epsilon is a fixed value for the maximum distance between two points.

# References

[AppliedAICourse](appliedaicourse.com)

 [ClusteringAnalysis by alfuqaha](https://cs.wmich.edu/alfuqaha/summer14/cs6530/lectures/ClusteringAnalysis.pdf)