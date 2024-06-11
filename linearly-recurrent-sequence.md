---
marp: true
math: mathjax
---

## 扱うこと

$K+1$項間の線型漸化式を持つ数列の$N$項目は行列累乗で求めたら$O(K^3 \log N)$かかるが、このスライドの手法でやると、もうちょっと速くなる。

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

と書いてあります。つまり、$\varphi_A(A) = 0$。ここで $x^N$を $\varphi_A(x)$ で割ったあまりを$r(x)$　と定義する。つまり、 
$x^N = q(x) \varphi_A(x) + r(x)$。ただし$\deg(r(x)) < \deg(\varphi_A(x))$ 

すると、$A^N = r(A^N)$となる。 $r(x) = r_0 + r_1 x + \dots + r_{K-1} x^{K-1}$　とおくと、$A^N = r(A^N) = r_0 E + r_1 A + \dots + r_{K-1} A^{K-1}$とわかる。

---

### 驚きの事実
求めたい$a_N$は、$A^{N}$の一番下の行と、$[a_{K-1}  a_{K-2}   \dots a_0 ]$の内積。
だから、 $E, A^1, A^2, \dots A^{K-1}$の一番下の行のみわかればよい。

ここで、驚きの事実(証明はあとで書く)として、$A^i$ ( $0 \leq i < K$) の一番下の行は、$K-i$列目だけ$1$で、他は$0$となっている。

なので、
$$r_i A^i \begin{bmatrix} a_{K-1} \\ a_{K-2} \\ \vdots \\ a_1 \\ a_0 \end{bmatrix} =
\begin{bmatrix}
 * & * &  * & * & *  \\
 * & * &  * & * & *  \\
\vdots & \vdots & \vdots & \vdots & \vdots & \\
 * & * &  * & * & *  \\
 0 & \dots & r_i  & \dots & 0
 \end{bmatrix}$$
のようになる。($i$があるのは$K-i$列目)



---

よって、
$$ 
r_0 E + r_1 A + \dots + r_{K-1} A^{K-1} = 
\begin{bmatrix}
 * & * &  * & * & *  \\
 * & * &  * & * & *  \\
\vdots & \vdots & \vdots & \vdots & \vdots & \\
 * & * &  * & * & *  \\
 r_{K-1} & r_{K-2} & \dots  & r_{1} & r_0
 \end{bmatrix}
$$
となることがわかるから、$a_N = r_{K-1} a_{K-1} + r_{K-2} a_{K-2} + \dots r_1 a_1 + r_0 a_0$である。

これまでのことをまとめると、
1. 線型漸化式の遷移を表す行列の固有多項式$\varphi_A(x)$を求める。
2. $x^N$を$\varphi_A(x)$で割った余りを求めて、それを$r_0 + r_1 x + \dots + r_{K-1} x^{K-1}$ とする。
3. $a_N = r_{K-1} a_{K-1} + \dots + r_0 a_0$である。

という3ステップで$a_N$が計算できることがわかった。

---

## ステップ1: $\varphi_A(x)$を求める

実は、固有多項式は線型漸化式によって決められるので、入力で線型漸化式を受け取ってから、掃き出し法をして ... みたいな計算は必要なく、($c_1, c_2, \dots c_K$を使った式で)事前に求められる。。(あたりまえかも)

$$

\varphi_A(x) = |xE - A | =  
\begin{vmatrix}
x - c_1  & -c_2 & -c_3 & -c_4 & \dots & -c_K \\
-1 & x & 0 & 0 & \dots & 0 \\
0 & -1 & x & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & -1 & x
\end{vmatrix}
$$

(結論を先に書くと、$=-x^K + c_1 x^{K-1} + \dots + c_{K-1} x^1 + c_{K}$ )

---

## ステップ2: $x^N$を $\varphi_A(x)$で割った余りを求める

多項式除算なので ~~ナイーブにやると$O(K^2)$でできる。行列積よりも速い!~~ →嘘っぽい。

mod $\varphi_A(x)$ 上で$x^N$を計算すればよい。


---

## ステップ3 $a_N = r_K a_K + r_{K-1} a{K-1} + \dots + r_0 a_0$を計算する

はい。やるだけ。

---

## 参考文献
https://blog.miz-ar.info/2019/02/typical-dp-contest-t/ : 
https://qiita.com/ryuhe1/items/da5acbcce4ac1911f47a : Bostan-MoriのMoriさんです