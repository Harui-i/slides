---
marp: true
math: mathjax
---

## 線形漸化式を持つ数列の遷移を表す行列

$$a_{n+K}= c_1 a_{n+K-1} + c_2 a_{n+k-2} + \dots + c_{K-1} a_{n+1} + c_K a_n$$
という$K+1$項間線形漸化式があるとする。

すると、いわゆる行列累乗の要領で

$$
\begin{bmatrix}
a_{n+1} \\
a_{n} \\
\vdots \\
a_{n-K+3} \\
a_{n-K+2}
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
a_{n} \\ 
a_{n-1} \\
\vdots \\
a_{n-K+2} \\
a_{n-K+1}
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
\end{bmatrix}^{N-K}

\begin {bmatrix}
a_K \\
a_{K-1} \\
a_{K_2} \\
\vdots \\
a_1  
\end {bmatrix}
$$
となる。
数列の$N$項目は行列をだいたい$N$回掛けることで求められる。行列の積は1回あたり$O(K^3)$かかるので、$A^{N-1}$を求めるのに繰り返し二乗法を使うと、$a_N$は全体で$O(K^3 \log N)$ の計算量で求められる。


---

## もっと速くできます！！！！

さっきの式:
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
\end{bmatrix}^{N-K}

\begin {bmatrix}
a_K \\
a_{K-1} \\
a_{K_2} \\
\vdots \\
a_1  
\end {bmatrix}
$$


---

これをちょっと変えて、
$$
\begin{bmatrix}
a_{N+K-1} \\
a_{N+K-2} \\
\vdots \\
a_{N+1} \\
a_{N}
\end{bmatrix}

= 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}^{N}

\begin {bmatrix}
a_{K-1} \\
a_{K-2} \\
\vdots \\
a_1 \\ 
a_0  
\end {bmatrix}
$$

のようにしてみる。

---

### ケーリーハミルトンの定理

緑の線形代数の教科書(線形代数講義と演習 改訂版 小林正典, 寺尾宏明 ) P98 定理17.1を見ると、

>定理17.1 (ケイリーハミルトンの定理)
>$n$次正方行列$A$の固有多項式$\varphi_A(x)$に$A$を代入したものは零行列に等しい

と書いてあります。つまり、$\varphi_A(A) = 0$。ここで $x^N$を $\varphi_A(x)$ で割ったあまりを　と定義する。つまり、 
$x^N = q(x) \varphi_A(x) + r(x)$。ただし$\deg(r(x)) < \deg(\varphi_A(x))$ 

すると、$A^N = r(A^N)$となる。 $r(x) = r_0 + r_1 x + \dots + r_{K-1} x^{K-1}$　とおくと、$A^N = r(A^N) = r_0 E + r_1 A + \dots + r_{K-1} A^{K-1}$で、 実際にほしい$a_N$は、


### 実際に必要な部分の計算

---
### 行列の$A$の固有多項式を求める
$$
\begin{vmatrix}
c_1 - x  & -c_2 & -c_3 & -c_4 & \dots & -c_K \\
-1 & x & 0 & 0 & \dots & 0 \\
0 & -1 & x & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & -1 & x
\end{vmatrix}
$$

(結論を先に書くと、$=-x^K + c_1 x^{K-1} + \dots + c_{K-1} x^1 + c_{K}$ )



---

## 参考文献
https://blog.miz-ar.info/2019/02/typical-dp-contest-t/ : 
https://qiita.com/ryuhe1/items/da5acbcce4ac1911f47a : Bostan-MoriのMoriさんです