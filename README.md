# このリポジトリは何？
サークルの勉強会などで使ったスライドをおいています。

# 1. 線形漸化式を高速に解く！

`https://github.com/Harui-i/slides/blob/main/linearly-recurrent-sequence.pdf`は、2024年の冬にサークルの勉強会で使用したスライドです。

$a_{n+K} = c_0 * a_{n+K-1} + c_1 * a_{n+K-2} \dots + c_{K-1} * a_{n} $ という形で表される数列 $a_n$ の$N$項目を $O(K \log K \log N)$ で求めるKaratsuba法を紹介しています。これは、行列累乗で計算した場合よりも真に速く、Bostan-Mori法と体数倍の差を除けば同じ計算量です。


# 2. 形式的冪級数(FPS: Formal Power Series)入門

`https://github.com/Harui-i/slides/blob/main/formal-power-series.pdf`は、2025年の秋にサークルの勉強会で使用したスライドです。

- 形式的冪級数(FPS: Formal Power Series)の定義/性質
- 通常型母関数(OGF: Ordinary Generating Function)の定義/性質
- 指数型母関数(EGF: Exponential Generating Function)の定義/性質
- FPSを使った問題の解き方の例
    - ABC 178 - D - Redistribution
    - ABC 231 - G - Balls in Boxes(橙Diff)

などを紹介しました。