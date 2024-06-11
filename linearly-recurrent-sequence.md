---
marp: true
math: mathjax
---

## 線形漸化式を持つ数列の遷移を表す行列の固有多項式

$$a_{n+K}= c_1 a_{n+K-1} + c_2 a_{n+k-2} + \dots + c_{K-1} a_{n+1} + c_K a_n$$
という$K$項間線形漸化式があるとする。

すると、いわゆる行列累乗の要領で

$$
\begin{bmatrix}
a_{n+K} \\
a_{n+K-1} \\
\vdots \\
a_{n+2} \\
a_{n+1}
\end {bmatrix}

= 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}

\begin {bmatrix}
a_{n+K-1} \\ 
a_{n+K-2} \\
\vdots \\
a_{n+1} \\
a_{n}
\end {bmatrix}
$$

であるとわかる。

---
すると、先ほどの式を繰り返し適用すると、以下のような関係式が成り立つから、

$$
\begin{bmatrix}
a_N \\
a_{N-1} \\
\vdots \\
a_{N-K+2} \\
a_{N-K+1}
\end{bmatrix}

= 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}^{N-1}

\begin {bmatrix}
a_1 \\
0 \\
0 \\
\vdots \\
0  
\end {bmatrix}
$$

数列の$N$項目は行列をだいたい$N$回掛けることで求められる。行列の積は1回あたり$O(K^3)$かかるので、$A^{N-1}$を求めるのに繰り返し二乗法を使うと、$a_N$は全体で$O(K^3 \log N)$ の計算量で求められる。


---

## もっと速くできます！！！！


---
### 行列の$A$の固有多項式を求める
$$
\begin{bmatrix}
c_1 - x  & -c_2 & -c_3 & -c_4 & \dots & -c_K \\
-1 & x & 0 & 0 & \dots & 0 \\
0 & -1 & x & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & -1 & x
\end{bmatrix}
$$




---

## 参考文献
https://blog.miz-ar.info/2019/02/typical-dp-contest-t/ : 
https://qiita.com/ryuhe1/items/da5acbcce4ac1911f47a : Bostan-MoriのMoriさんです