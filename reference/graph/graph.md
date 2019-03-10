---
layout: reference
title: graph
breadcrumb: graph
elem: nimi::graph
header: lib/graph/graph.hpp
---

```cpp
namespace nimi {
  struct base_edge {
    int from;
    int to;
    base_edge(int from, int to);
  };

  template<class T>
    struct edge : public base_edge {
      T val;
      edge(int from, int to, T v);
    };

  template<>
    struct edge<void> : public base_edge {
      edge(int from, int to);
    };
  template<class C>
  struct maxflow_edge : public base_edge {
    C cap;
    std::size_t rev;
    maxflow_edge(int from, int to, C cap, std::size_t rev);
  };

  template<class T>
  struct directed_graph : public std::vector<std::vector<edge<T>>> {
    directed_graph(std::size_t n);
    void add_edge(const edge<T>& e);
  };

  template<class T>
  struct undirected_graph : public std::vector<std::vector<edge<T>>> {
    undirected_graph(std::size_t n);
    void add_edge(const edge<T>& e);
  };

  template<class C>
  struct maxflow_graph : public std::vector<std::vector<maxflow_edge<C>>> {
    maxflow_graph(std::size_t n);
    void add_edge(int from, int to, C cap, std::size_t rev_cap = 0);
  };
}
```

## 概要

`base_edge`は`edge`クラスの基底となるクラスである.

`edge`は

- `T`が`void`以外 -> `T`型の重み`val`を持つ辺
- `T`が`void` -> 重みを持たない辺

を示すクラスである.

- `directed_graph`...有向グラフ
- `undirected_graph`は無向グラフ
- `maxflow_graph`は最大フローを求める際に使う残余グラフ

## 例

 
1. 4頂点の有向グラフ
```
0 --> 1 --> 2
         |
         v
         3
```

2. 3頂点の整数の重み付き有向グラフ
```
0 ----> 1 <---- 3
      -1       5
```

3. 3頂点の無向グラフ

```
0 -- 1 -- 2
```

はそれぞれ, 以下のように実装できる.

```cpp
#include <lib/graph/graph.hpp>

int main() {
  { // 1.
    nimi::directed_graph<void> g(4);
    using E = nimi::edge<void>;
    g.add_edge(E(0, 1));
    g.add_edge(E(1, 2));
    g.add_edge(E(1, 3));
  }
  { // 2.
    nimi::directed_graph<int> g(3);
    using E = nimi::edge<int>;
    g.add_edge(E(0, 1, -1));
    g.add_edge(E(2, 1, 5));
  }
  { // 3.
    nimi::undireced_graph<void> g(3);
    using E = nimi::edge<void>;
    g.add_edge(E(0, 1));
    g.add_edge(E(1, 2));
  }
}
```

## 参考

- [隣接リスト - Wikipedeia](https://ja.wikipedia.org/wiki/%E9%9A%A3%E6%8E%A5%E3%83%AA%E3%82%B9%E3%83%88)

- 隣接リスト - [蟻本 第2版](https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E3%82%B3%E3%83%B3%E3%83%86%E3%82%B9%E3%83%88%E3%83%81%E3%83%A3%E3%83%AC%E3%83%B3%E3%82%B8%E3%83%96%E3%83%83%E3%82%AF-%E7%AC%AC2%E7%89%88-%EF%BD%9E%E5%95%8F%E9%A1%8C%E8%A7%A3%E6%B1%BA%E3%81%AE%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0%E6%B4%BB%E7%94%A8%E5%8A%9B%E3%81%A8%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%86%E3%82%AF%E3%83%8B%E3%83%83%E3%82%AF%E3%82%92%E9%8D%9B%E3%81%88%E3%82%8B%EF%BD%9E-%E7%A7%8B%E8%91%89%E6%8B%93%E5%93%89/dp/4839941068) P.91
