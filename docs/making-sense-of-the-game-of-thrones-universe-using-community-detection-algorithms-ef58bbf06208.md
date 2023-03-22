# 使用社区检测算法理解权力的游戏世界

> 原文：<https://towardsdatascience.com/making-sense-of-the-game-of-thrones-universe-using-community-detection-algorithms-ef58bbf06208?source=collection_archive---------26----------------------->

## 社区检测算法非常准确，而且非常容易使用

我越来越多地研究图表。当您分析连接和社区时，图表特别有用，并且是存储数据的一种非常有效的方式。有时我会使用图形数据库来获取数据，因为它比传统的关系数据库更快。

在这篇文章中，我想把重点放在社区检测算法上，它有很多实际用途。它们可以帮助识别一组交互中的结构，这可以应用于组织设计，但也可以用于数字通信和犯罪调查等其他领域——例如，著名的 *Panama Papers* 调查主要使用了图表和社区检测。

# 什么是社区检测？

如果社区存在于一个网络中，你会认为两个联系更紧密的节点(比如人)更有可能是同一个社区的成员。根据这一原理，存在各种算法，这些算法迭代网络中的节点和节点组，并试图形成节点的最佳分组，使得组内的连接是密集的，而组间的连接是稀疏的。

当然，大型网络是复杂的，所以通常很难找到完全独立的社区。社区可以大量重叠，算法主要关注优化。从视觉上看，在这些情况下，最终结果看起来有点混乱是很常见的，但是视觉化不是重点。通过执行社区检测，我们可以获得有价值的信息，这些信息可以帮助我们采取行动来提高效率。例如，我们可以建立一个正式的组织结构，与真实的工作流程更加一致，或者我们可以重新配置服务器集群以加快信息流动。

# 权力的游戏网络

为了说明社区检测算法的威力，我将采用《权力的游戏》第 1-8 季中所有角色互动的网络，看看我能找到哪些社区。我从来没有看过这个节目，所以不知道结果会有多直观——也许一些读者在看到结果后可以给出一个观点。

在这个网络中，任何两个角色如果在同一个场景中出现过，就是连接的，而连接的强度是由他们一起出现的场景数量决定的。

首先，让我们建立网络。非常感谢 Github 用户 mathbeveridge，他在这里为网络创建了每一季的 edge lists:[https://Github . com/mathbeveridge/gameoftrones/tree/master/data](https://github.com/mathbeveridge/gameofthrones/tree/master/data)。我将直接从 github 中获取这些边列表，并将它们构建到一个编译好的边列表中，使用 r 中的`igraph`包创建网络。

```
library(tidyverse) 
library(readr) 
library(igraph) # get s1 edgelist edgefile_url "https://raw.githubusercontent.com/mathbeveridge/gameofthrones/master/data/got-s1-edges.csv" edgelist <- readr::read_csv(edgefile_url) # append edglistists for s2-8 
for (i in 2:8) { 

  edgefile_url <-      paste0("https://raw.githubusercontent.com/mathbeveridge/gameofthrones/master/data//got-s", i, "-edges.csv")   edges <- readr::read_csv(edgefile_url) edgelist <- edgelist %>% 
    dplyr::bind_rows(edges) 
} seasons <- 1:8 # <- adjust if you want to focus on specific seasons edgelist <- edgelist %>% 
  dplyr::filter(Season %in% seasons) # create igraph network with weighted edges edgelist_matrix <- as.matrix(edgelist[ ,1:2]) got_graph <- igraph::graph_from_edgelist(edgelist_matrix, directed = FALSE) %>% 
  igraph::set.edge.attribute("weight", value = edgelist$Weight)
```

好了，我们已经建立了我们的关系网。就这么简单。让我们快速浏览一下。

```
l <- igraph::layout_with_mds(got_graph) 
plot(got_graph, vertex.label = NA, vertex.size = 5, rescale = F, layout = l*0.02)
```

![](img/5ca5bd785c485d47c9a9bc2c27ef7781.png)

这看起来像一个有很多连接的非常繁忙的图表，很难看出任何有意义或有帮助的结构。但我们不会花太多时间来解决这个问题。

# 使用卢万社区检测算法

Louvain 社区检测算法是一种在复杂网络中创建最佳社区结构的公认算法。它不是唯一可用的算法(一种叫做 Leiden 算法的相当新的算法被认为性能稍好)，但是在`igraph`包中有一个 Louvain 算法的简单实现，因此我们可以用一个快速的单行命令运行社区检测。让我们这样做，将所有节点分配到各自的社区，看看我们的社区有多大

```
# run louvain with edge weights louvain_partition <- igraph::cluster_louvain(got_graph, weights = E(got_graph)$weight) got_graph$community <- louvain_partition$membership sizes(louvain_partition) %>% knitr::kable()
```

![](img/66959f7b5b16e924582db9214ce9c300.png)

我们这里有几个非常小的社区——让我们看看谁在其中。

```
membership(louvain_partition)[which(membership(louvain_partition) %in% c(6,7))]## BALON_DWARF ROBB_DWARF RENLY_DWARF STANNIS_DWARF JOFFREY_DWARF BLACK_JACK KEGS MULLY 
## 6 6 6 6 6 7 7 7
```

因此，快速的在线研究表明，这些是一次性的社区，出现在非常具体的《GoT》剧集中，几乎完全相互互动——五个小矮人在第四季中扮演五个金，黑杰克、小桶和穆利的三人组也出现在同一季中。虽然这些外围社区被识别出来是令人欣慰的，但在我们这里讨论的主要问题中考虑它们可能没有帮助，所以在我们继续讨论时，我将把它们放在一边。

# 主要社区的特征

我们希望找到一种方法来描述我们的主要社区。一种方法是确定每个社区中的重要节点——在该社区的整体连通性中，角色是如何重要的。

首先，我们可以找到每个社区中与其他节点连接最多的节点。让我们这样做，通过创建每个社区的子图，并迭代一个向量，该向量包含每个社区中度数最高的节点的名称。

```
high_degree_nodes <- c() for (i in 1:8) { 
  subgraph <- induced_subgraph(got_graph, v =  which(got_graph$community == i))   degree <- igraph::degree(subgraph) high_degree_nodes[i] <- names(which(degree == max(degree))) } high_degree_nodes[c(1:5, 8)]## [1] "DAENERYS" "SANSA" "CERSEI" "JON" "THEON" "ARYA"
```

因此，我们可以看到，不出所料，六个联系最紧密的字符是主要字符，它们帮助我们描述我们检测到的社区的特征。我们还可以尝试通过节点来描述每个社区，这个节点是其他节点的最佳连接者，这是一种称为介数中心性的度量。

```
high_btwn_nodes <- c() for (i in 1:8) {   subgraph <- induced_subgraph(got_graph, v = which(got_graph$community == i))   btwn <- igraph::betweenness(subgraph) high_btwn_nodes[i] <- names(which(btwn == max(btwn))) } high_btwn_nodes[c(1:5, 8)]## [1] "JORAH" "SANSA" "TYRION" "SAM" "ROBB" "ARYA"
```

我们可以看到，对于一对夫妇的社区，中心特征是相同的，但对于其他人来说是不同的——这可能有助于进一步描述社区的特征。

# 可视化社区

尽管如此大规模的网络可能非常复杂，但查看网络通常是有帮助的。通过对社区中的节点和边进行颜色编码，我们可以更好地区分它们，然后我们还可以标记最中心字符的节点。另一种选择是根据节点在整个网络中的重要性来调整节点的大小。

首先，我们可以生成一个漂亮的球形网络。

```
# give our nodes some properties, incl scaling them by degree and coloring them by community V(got_graph)$size <- degree(got_graph)/10 
V(got_graph)$frame.color <- "white" 
V(got_graph)$color <- got_graph$community 
V(got_graph)$label <- V(got_graph)$name # also color edges according to their starting node edge.start <- ends(got_graph, es = E(got_graph), names = F)[,1] E(got_graph)$color <- V(got_graph)$color[edge.start] E(got_graph)$arrow.mode <- 0 # only label central characters v_labels <- which(V(got_graph)$name %in% high_degree_nodes[c(1:5, 8)]) for (i in 1:length(V(got_graph))) { 
  if (!(i %in% v_labels)) { 
    V(got_graph)$label[i] <- "" 
  } 
} # plot network l1 <- layout_on_sphere(got_graph) 
plot(got_graph, rescale = F, layout = l1)
```

![](img/3c5262b9158418cb7dd7085883f1b2cb.png)

这很漂亮，但是对可视化独立的社区没有帮助。为此，最好使用多维标度，以确保距离较远的节点连接较少。

```
l2 <- layout_with_mds(got_graph) 
plot(got_graph, rescale = F, layout = l2*0.02)
```

![](img/171a52ff357a06c886de8118b9252dfa.png)

不错！当然，在这里我们可以做更多的事情来分析和描述社区，但是我想让你看到，用几行代码实现这种类型的分析是非常容易的，而且这些算法检测真实社区的能力是相当了不起的！我鼓励你考虑在工作或学习中使用这种技术。如果你能想到这个的好用途，请留言。

最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在 [*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/) *或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。也可以看看我在*[*drkeithmcnulty.com*](http://drkeithmcnulty.com)*的博客。*