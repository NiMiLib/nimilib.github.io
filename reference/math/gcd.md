---
layout: reference
title: math-gcd
breadcrumb: gcd
elem: nimi::gcd
header: lib/math/gcd.hpp
---

```cpp
namespace nimi{
    std::size_t gcd(std::size_t a, std::size_t b);
}
```

## 概要

最大公約数をユークリッドの互除法で求める.


## 要件

`a`, `b`が正の整数であること.

## 例

```cpp
#include <lib/math/gcd.hpp>
#include <cassert>

int main() {
    assert(nimi::gcd(91, 14) == 7);   
}
```

## 参考

- [ユークリッドの互除法 - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%A6%E3%83%BC%E3%82%AF%E3%83%AA%E3%83%83%E3%83%89%E3%81%AE%E4%BA%92%E9%99%A4%E6%B3%95)