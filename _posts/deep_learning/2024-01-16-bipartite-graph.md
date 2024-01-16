---
layout: post
title: Bipartite Graph
category: Deep Learning
tag: [bipartite graph]
---

## Bipartite Graph

이분 그래프는 인접한 꼭지점을 서로 다른 색으로 칠할 때, 두 가지 색으로만 칠할 수 있는 그래프를 의미한다. 

즉, 꼭지점들의 집합인 $V$와 꼭지점들을 잇는 변들의 집합인 $E$로 이루어진 그래프 $G = (V, E)$가 있다면, $V$는 $A$와 $B$의 두 집합으로 이루어져 있고, $E$는 $A$와 $B$로만 이루어져 있다. 


예를 들어, 다음과 같은 꼭지점들이 있다. 

$$ V = {1, 2, 3, 4, 5, 6}$$

<img src='/assets/deep_learning/bipartite_graph/graph_v.png'>

이 때, $$ E = {{1, 2}, {1, 4}, {2, 3}, {2, 5}, {3, 6}, {4, 5}, {5, 6}} $$으로 되어 있다면, 이분 그래프에 해당한다. 

<img src='/assets/deep_learning/bipartite_graph/bipartite_graph.png'>

## References:

- [쾨니그의 정리 (Kőnig's Theorem)](https://gazelle-and-cs.tistory.com/12)

- [Distance-IoU Loss: Faster and Better Learning for Bounding Box Regression](https://arxiv.org/abs/1911.08287)