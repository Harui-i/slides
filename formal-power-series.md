---
marp: true
math: mathjax
---


# 形式的冪級数勉強会

### 2025/10/25

Harui (Twitter: @Nutoriaezu88808, GitHub: @Harui-i)

---

# FPSとは？

数列 $(a_i)_{i=0}^{\infty}$ に対して、$\sum_{i=0}^{\infty} a_i x^i$ と書くものを通常型母関数(Ordinary Generating Function, **OGF**)といいます。

$\sum_{i=0}^{\infty} a_i \frac{x^i}{i!}$ と書くものを指数型母関数(Exponential Generating Function, **EGF**)といいます。

このように、数列に対応する冪級数を考えている、ということを意識することが重要な気がしています。

---
## いろいろな言葉について

- FPS(Formal Power Series) = 形式的冪級数
- OGF(Ordinary Generating Function) = 通常型母関数
- EGF(Exponential Generating Function) = 指数型母関数



<!-- ↓ここを最後のスライドとしたい-->
---
# 参考文献

- https://codeforces.com/blog/entry/77468
  - OGF/EGFの定義からいろいろな性質まで扱っている(英語)
- https://trap.jp/post/1657/
    - traPのFPSの講習会の資料らしい。計算方法や、問題を手計算で解く方法などが詳しく書かれている。