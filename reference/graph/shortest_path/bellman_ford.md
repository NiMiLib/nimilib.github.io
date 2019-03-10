---
layout: reference
title: bellman_ford
breadcrumb: bellman_ford
elem: nimi::bellman_ford
header: lib/graph/shortest_path/bellman_ford.hpp
---

```cpp
namespace nimi{
  template<class T>
  std::vector<std::pair<bool, T>> bellman_ford(const nimi::directed_graph<T>& g, std::size_t s);
}
```

### 引数

|:--|:--|
|`const nimi::graph<T>& g`|最短経路を求めるグラフ|
|`std::size_t s`|最短経路の始点|


### 戻り値

|:--|:--|
|`std::vector<std::pair<bool, T>>`|first...経路上に負閉路が存在するならfalse, second...最短経路の長さ|

## 概要

重み付きのグラフで最短経路を $O(EV)$ で求めるアルゴリズムである.

## 要件

- `std::numeric_limits<T>::max()` が定義されていて, $+\infty$ を表す.
- `T()` が定義されていて, $0$ を表す.
- `T`に`bool operator<()`が定義されている.
- `T`に`T operator+()`が定義されている.

## 例

```cpp
#include <lib/graph/graph.hpp>
#include <lib/graph/shortest_path/bellman_ford.hpp>
#include <cassert>
#include <limits>
#include <vector>

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
    auto dist = std::move(nimi::bellman_ford(g,1));
    assert(dist[0].second == 3);
    assert(dist[1].second == 0);
    assert(dist[2].second == 2);
    assert(dist[3].second == std::numeric_limits<int>::max());
    return 0;
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
    };
    for(const auto& e : es) {
        g[e.from].push_back(e);
    }
    auto dist = std::move(nimi::bellman_ford(g,1));
    assert(dist[0].second == 0);
    assert(dist[1].second == 2);
    assert(dist[2].second == -3);
    assert(dist[3].second == -1);
    return 0;
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
    auto dist = std::move(nimi::bellman_ford(g,1));
    bool res = false;
    for(auto const& d: dist) {
      if(!d.first) res = true;
    }
    assert(res);
    return 0;
  }
}
```

## 参考

- 単一始点最短経路1(ベルマンフォード法) - [蟻本 第2版](https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E3%82%B3%E3%83%B3%E3%83%86%E3%82%B9%E3%83%88%E3%83%81%E3%83%A3%E3%83%AC%E3%83%B3%E3%82%B8%E3%83%96%E3%83%83%E3%82%AF-%E7%AC%AC2%E7%89%88-%EF%BD%9E%E5%95%8F%E9%A1%8C%E8%A7%A3%E6%B1%BA%E3%81%AE%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0%E6%B4%BB%E7%94%A8%E5%8A%9B%E3%81%A8%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%86%E3%82%AF%E3%83%8B%E3%83%83%E3%82%AF%E3%82%92%E9%8D%9B%E3%81%88%E3%82%8B%EF%BD%9E-%E7%A7%8B%E8%91%89%E6%8B%93%E5%93%89/dp/4839941068) P.97
