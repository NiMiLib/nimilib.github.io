---
layout: reference
title: spfa
breadcrumb: spfa
elem: nimi::spfa
header: lib/graph/shortest_path/spfa.hpp
---

```cpp
namespace nimi{
  template<class T>
  std::vector<T> spfa(const nimi::directed_graph<T>& g, std::size_t s) {
}
```

### 引数

|:--|:--|
|`const nimi::graph<T>& g`|最短経路を求めるグラフ|
|`std::size_t s`|最短経路の始点|


### 戻り値

|:--|:--|
|`std::vector<T>`|負閉路が存在するならから空, そうでないなら最短経路の長さ|

## 概要

重み付きのグラフで最短経路を $O(EV)$ で求めるアルゴリズムである. 
だが, Bellman Ford より実用上は早い

## 要件

- `std::numeric_limits<T>::max()` が定義されていて, $+\infty$ を表す.
- `T()` が定義されていて, $0$ を表す.
- `T`に`bool operator<()`が定義されている.
- `T`に`T operator+()`が定義されている.

## 例

```cpp
#include <lib/graph/graph.hpp>
#include <lib/graph/shortest_path/spfa.hpp>
#include <cassert>
#include <limits>
#include <vector>
#include <iostream>

int main() {
  {
    using E = nimi::edge<int>;
    nimi::directed_graph<int> g(4);
    std::vector<E> es = {
        E(0,0,1),
        E(0,2,4),
        E(2,0,1),
        E(1,2,2),
        E(3,1,1),
        E(3,2,5)
    };
    for(const auto& e : es) {
        g[e.from].push_back(e);
    }
    auto dist = std::move(nimi::spfa(g,1));
    assert(dist[0] == 3);
    assert(dist[1] == 0);
    assert(dist[2] == 2);
    assert(dist[3] == std::numeric_limits<int>::max());
  }
  {
    using E = nimi::edge<int>;
    nimi::directed_graph<int> g(4);
    std::vector<E> es = {
        E(0,1,2),
        E(0,2,3),
        E(1,2,-5),
        E(1,3,1),
        E(2,3,2),
        E(3,1,0)
    };
    for(const auto& e : es) {
        g[e.from].push_back(e);
    }
    auto dist = std::move(nimi::spfa(g,1));
    assert(dist.empty());
  }
    return 0;
}
```

## 参考

- [Wikipedia - Shortest Path Faster Algorithm](https://en.wikipedia.org/wiki/Shortest_Path_Faster_Algorithm)
