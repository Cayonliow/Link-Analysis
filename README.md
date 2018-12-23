# Data Mining Homework3 - Link Analysis
###### tags: `datamining`

## development environment
*    Python 3.5
*    Jupyter Notebook
*    Numpy 
*    Pandas
*    Json

## System Structure
```
Main Prorgam
    -main.ipynb

Dataset
    -dataset
        -hw3dataset
            -graph_*.txt
    -ibm
        -converted
            -bidirected_ibm.txt
            -directed_ibm.txt    
        -dataset.txt
Result
    -result
        -graph_*_au_hub_rank.csv
        -graph_*_add_link.csv
        -graph_*_simrank.txt
        -bidirected_ibm_au_hub_rank.csv
        -directed_ibm_au_hub_rank.csv
```
`*` - stands for number of graph naming (1, 2, 3 ,4 ,5 ,6 )

*    `main.ipynb` - where the main program to b executed
*    `dataset` - ther eare 2 types of datasets
        *    `hw3dataset` - containing 6 graphs 
                *    `graph_1.txt` - 6 nodes, 5 edges
                *    `graph_2.txt` - 5 nodes, 5 edges (a circle)
                *    `graph_3.txt` - 4 nodes, 6 edges
                *    `graph_4.txt` - 7 nodes, 18 edges (the example in Lecture3, p29)
                *    `graph_5.txt` - 469 nodes, 1102 edges
                *    `graph_6.txt` - 1228 nodes, 5220 edges
        *    `ibm` - (dataset of hw1) same nodes with single directed and bi-directed
                *    `dataset.txt` - the raw dataset of hw1
                *    `converted/bidirected_ibm.txt` - bi-directed graph 
                *    `converted/directed_ibm.txt` - directed graph
*    `result` - the output result is dumped here and dfferent format containing
        *    `graph_*_au_hub_rank.csv` , `bidirected_ibm_au_hub_rank.csv`, `directed_ibm_au_hub_rank.csv`
                *    result of Authority, Hubness ad PageRank of all the graphs
        *    `graph_*_add_link.csv` - Find a way (add some links) to increase hub, authority, and PageRank of Node 1 in first 3 graphs respectively
                *    add link as the way of increasing hub, authority, and PageRank of Node 1
        *    `graph_*_simrank.txt` -  the result of first 5 graphs of project3dataset
                *    SimRankis used to calculate pair-wise similarity of nodes 
                *    parameter C is 0.9 


## Implementaion Detail
HITS, PageRank and SimRank are implemented as classes
```python=
class HITS
class PAGERANK
class SIMRANK
```
### Class HITS
*    Parent and Children of each node is recorded
*    Iteration is followed as given
*    Authority ans Hubness of each node is updated during iteration
*    Normalization during every iteration
*    Early stoped if the difference between current hubness and the previous one is smaller the a epsilon value 
*    For increasing Hubness 
        *    increase children
        *    effective if the children getting largest authority value
        *    add a link that the node 1 pointing to the another node
*    For increasing Authority
        *    increase parent
        *    effective if the parent getting largest hubness value
        *    add a link that the node pointing to the node 1

### Class PAGERANK
*    Parent and Children of each node is recorded
*    Iteration is followed as given
*    PageRank of each node is initializad as(1 - damping_factor/num_of_nodes) 
*    PageRank of each node is updated during iteration
*    For increasing PageRank
        *    increase parent
        *    effective if the parent getting largest hubness value
        *    add a link that the node pointing to the node 1

### Class SIMRANK
*    Identity Matrix is initialized with the size of number of nodes
*    Iteration is followed as given
*    Parent and Children of each node is recorded
*    Maxtrix is updated with average of sum of the every 2 items of ther cartesian product of the nodes

### Preprocessing 
*    Get all the file from the dataset folder 
*    Transform the raw into the format of nodes and graph
#### IBM data
*    raw data have to be transform and converted into the form of nodes and graph

### Way of increasing Hubness, Authority, and PageRank of Node 1
*    there is a way of adding links to and from Node 1
*    increasing Hubness 
        *    a link of the node 1 pointing to the another node is added
*    increasing Authority, PageRank
        *    a link of the node pointing to the node 1 is added

### Post-processing
*    all the results are stored as files
*    the format of `.csv` and `.txt` are used

## Result analysis and discussion
all the results are stored as files 
```
-result
    -graph_*_au_hub_rank.csv
    -graph_*_add_link.csv
    -graph_*_simrank.txt
    -bidirected_ibm_au_hub_rank.csv
    -directed_ibm_au_hub_rank.csv
```
Graph1 and Graph2 are shown below for explanation 

### For graph 1
```
Authority BEFORE 0.0
Authority  AFTER 0.49975597852611037
Hubness   BEFORE 0.2
Hubness    AFTER 0.6178646451730752
PageRank  BEFORE 0.024999999999999998
PageRank   AFTER 0.16666666618633766
```
node 1 has no parent, i.e. no links pointing to node1
a link is added to node 1 to increase authority and pagerank
a link is added from node 1 to increase hubndess

### For graph 2
```
Authority BEFORE 0.2
Authority  AFTER 0.6177600472813239
Hubness   BEFORE 0.2
Hubness    AFTER 0.6178646451730752
PageRank  BEFORE 0.2
PageRank   AFTER 0.2786751675697936
result dumped into  result/graph_2_add_link.csv 
```

the graph is a circle
node 1 has 1 parent and 1 child(1 pointing in and 1 pointing out)
a link is added to node 1 to increase authority and pagerank
a link is added from node 1 to increase hubndess

### For graph 3
```
Authority BEFORE 0.19090909090909092
Authority  AFTER 0.3383098192765027
Hubness   BEFORE 0.19101123595505617
Hubness    AFTER 0.33830424122300395
PageRank  BEFORE 0.17545159217677955
PageRank   AFTER 0.2616610572242342
```

bi-directed graph
a link is added to node 1 to increase authority and pagerank
a link is added from node 1 to increase hubndess
be careful of the double direction of links (the member of parent and children of a node have to be double checked)


## Computation Performance Analysis
###Type of Analysis 
since Graph 5 is a large graph here and has large number of nodes 
```
469 nodes, 1102 edges
```
it is used as the source for the analysis

`computation time ` is analysed
|Execution|Time taken(s)|
|---|---|
|HITS|0.03474|
|PageRank|0.03678|
|SimRank|28.58939|

from the table above, we know that SimRank takes the longest time to execute
because there is a matrix of size `n*n` is computed with iteration 

### Size of graph
|Graph|Size|HITS|PageRank|SimRank|
|---|---|---|---|---|
|1|6 nodes, 5 edges|0.00015|0.0002|0.00375|
|2|5 nodes, 5 edges|5e-05|9e-05|0.00331|
|3|4 nodes, 6 edges|6e-05|9e-05|0.00296|
|4|7 nodes, 18 edges|0.00014|0.00019|0.00995 |
|5|469 nodes, 1102 edges|0.03474|0.03678|28.58939|
|6|1228 nodes, 5220 edges|0.29376|0.30441|x|

from the table above, we know that the increment of edge or node causes longer time executation

### Direction
|Direction|HITS|PageRank|
|---|---|---|
|single-directed|0.23836|0.25369|
|bi-directed|0.47094|0.57211|

from the table above, we know that the execution of bi-directed graph takes longer times
Because the edges of the bi-directed graph is double to the directed graph

more edges causes much computation, and further takes longer times

## Discussion
*    Various size(large and small) and type(single and bi-directed) of dataset provided are tried
*    Cautious is desperately needed to make it clear
*    Learned about how to increase authority and hubnesss of a (web)
*    Make understanding of the relationship between the value of authority and hubnesss, and the number  of parent and children

## Questions & Discussion
### More limitations about link analysis algorithms
*    Direction(single or bi-) of the graph can be limited
*    The maximum number of parent and children of the node can be limited
*    Loop is prohibited to reduce computation

### Can link analysis algorithms really find the "important" pages from Web? 
*    I think no
*    Because the value of authority and hubnesss can be easily increased
*    The meaningless and unimportant pages and the really important pages are both considered as nodes 
*    no weights causes the only importance of all these pages is the number of links

### What are practical issues when implement these algorithms in a real Web? 
*    There are huge number of pages in a real Web
*    There is hysteria of always updating of the values of these algorithm，since the number of new pages is also increasing
*    As what mentioned above, the meaningless and unimportant pages and the really important pages are both considered as nodes 
*    no weights causes the only importance of all these pages is the number of links

### Any new idea about the link analysis algorithm?
*    Weight and confidence of visits are suggested to tackle the issue of the meaningless and unimportant pages and the really important pages are both considered the same 

### What is the effect of “C” parameter in SimRank?
*    the parameter C is the decay factor in the algorithm
*    Converging is needed in the recursive computation of the algorithm
*    the parameter C helps in converging
*    smaller the parameter C, faster the converging 

## Reference
*    [HITS演算法與PageRank演算法比較](https://sls.weco.net/node/10937)
*    [理解HITS算法](https://computational-communication.com/algorithm/understanding-hits/)
*    [PageRank Examples ](http://www.dcs.bbk.ac.uk/~dell/teaching/ir/examples/pr_example.pdf)
*    [How Google Finds Your Needle in the Web's Haystack](http://www.ams.org/publicoutreach/feature-column/fcarc-pagerank)
*    [Google PageRank 演算法實作(Python版)](https://www.cnblogs.com/wentingtu/archive/2013/01/22/2872199.html)
*    [Google PageRank algorithm in Python](https://www.peterbe.com/plog/blogitem-040321-1)
*    [PageRank算法与python实现](https://blog.csdn.net/John_xyz/article/details/78915097)
*    [HITS, PageRank 與 SimRank 的實現與比較](https://github.com/chihsuan/Link-analysis)
*    [SimRank协同过滤推荐算法](https://cloud.tencent.com/developer/article/1184560)





