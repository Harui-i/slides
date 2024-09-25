---
marp: true
math: mathjax
---

## 扱うこと

$K+1$項間の線型漸化式を持つ数列の$N$項目は行列累乗で求めたら$O(K^3 \log N)$かかるが、このスライドの手法でやると、もうちょっと速くなる。

---

## 考える問題

$$a_{n+K}= c_1 a_{n+K-1} + c_2 a_{n+k-2} + \dots + c_{K-1} a_{n+1} + c_K a_n$$
という$K+1$項間線形漸化式があるとする。

$c_1, c_2, \dots , c_K$と、$a_1, a_1, \dots , a_K$ が与えられる。 $a_{N+1}$を求めよ。

制約
- $1 \leq N \leq 10^{18}$
- $K \leq N$
- $K \leq 10^{5}$(だいたい)
- だいたい2秒程度で答えたい

---


## 行列累乗で解く

いわゆる行列累乗の要領で

$$
\begin{bmatrix}
a_{n+K} \\
a_{n+K-1} \\
a_{n+K-2} \\  
\vdots \\
a_{n+2} \\
a_{n+1}
\end {bmatrix}

= 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
0 & 0 & 1 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}

\begin {bmatrix}
a_{n+K-1} \\ 
a_{n+K-2} \\
a_{n+K-3} \\ 
\vdots \\
a_{n+1} \\
a_{n}
\end {bmatrix}
$$

が成立することがわかる。

---
すると、先ほどの式を繰り返し適用すると、以下のような関係式が成り立つから、

$$
\begin{bmatrix}
a_{N+K} \\
a_{N+K-1} \\
a_{N+K-2} \\ 
\vdots \\
a_{N+2} \\
a_{N+1}
\end{bmatrix}

= 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
0 & 0 & 1 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}^{N}

\begin {bmatrix}
a_K \\
a_{K-1} \\
a_{K-2} \\
\vdots \\
a_{2} \\ 
a_1  
\end {bmatrix}
$$
となる。
数列の$N$項目は行列をだいたい$N$回掛けることで求められる。行列の積は1回あたり$O(K^3)$かかるので、$A^{N}$を求めるのに繰り返し二乗法を使うと、$a_{N+1}$は全体で$O(K^3 \log N)$ の計算量で求められる。

---

さっきの制約では$K$が$10^5$になることもあるので、$K^3 \log N$ はだいたい $10^{15} * 60 = 6 \times 10^{16}$となって計算に1000万秒くらいかかり、だいぶキツい。

行列累乗で考えているとどうしても$K^3$が重い。(Strassenのアルゴリズムなど$O(K^3)$より速い行列積アルゴリズムも存在はするが、競プロ文脈だと速くないらしい)


---

## もっと速くできます！！！！

---

### ケーリーハミルトンの定理

緑の線形代数の教科書(線形代数講義と演習 改訂版 小林正典, 寺尾宏明 ) P98 定理17.1を見ると、

>定理17.1 (ケイリーハミルトンの定理)
>$n$次正方行列$A$の固有多項式$\varphi_A(x)$に$A$を代入したものは零行列に等しい

つまり、$\varphi_A(A) = \boldsymbol{0}$。

---

#### ちなみに(1): 多項式に行列を代入するとは

ここで、多項式に行列を代入したものは、

$$ \left. x^3 + 2x^2 + 5x + 3 \right|_{x=A} = A^3 + 2A^2 + 5A + 3E $$

のように、行列の累乗とその和で定義されている。(定数項に注意！)

---

#### ちなみに(2): 固有多項式とは

固有多項式というのは、正方行列$A$に対して、
$$ \det(x - AE) $$ 

で定義される。

だから、

$$\varphi_A(x) = \det
\begin{bmatrix}
x - c_1   & -c_2 & -c_3 & -c_4 & \dots & -c_K \\
-1 & x & 0 & 0 & \dots & 0 \\
0 & -1 & x & 0 & \dots & 0 \\
0 & 0 & -1 & x & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & -1 & x
\end{bmatrix} 
$$
---

#### ちなみに終わり

>定理17.1 (ケイリーハミルトンの定理)(再掲)
>$n$次正方行列$A$の固有多項式$\varphi_A(x)$に$A$を代入したものは零行列に等しい

ここで $x^N$を $\varphi_A(x)$ で割ったあまりを$r(x)$、商を$q(x)$とする。つまり、 
$x^N = q(x) \varphi_A(x) + r(x)$。ただし$\deg(r(x)) < \deg(\varphi_A(x))$ 

---


すると、$A^N = q(A)\varphi_A(A) + r(A) = \boldsymbol{0} + r(A) = r(A)$となる。

 $r(x) = r_0 + r_1 x + \dots + r_{K-1} x^{K-1}$　とおくと(行列$A$は$K\times K$行列だから、$\varphi_A(x)$ は$K$次多項式で、そのあまりである$r(x)$はたかだか$K-1$次)  $A^N = r(A) = r_0 E + r_1 A + \dots + r_{K-1} A^{K-1}$とわかる。

---

### 驚きの事実
求めたい$a_{N+1}$は、$A^{N}$の一番下の行と、$[a_{K}, a_{K-1},   \dots , a_1 ]$の内積だった。
だから、$E, A^1, A^2, \dots A^{K-1}$の一番下の行のみわかればよい。

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

mod $\varphi_A(x)$ 上で$x^N$を計算すればよい。FFTなどをすると割り算に$O(K\log K)$かかりそれを$O(\log N)$回やるので $O(K \log K \log N)$ で求められる。


---

## ステップ3 $a_N = r_K a_K + r_{K-1} a{K-1} + \dots + r_0 a_0$を計算する

はい。やるだけ。

---

## 参考文献
https://blog.miz-ar.info/2019/02/typical-dp-contest-t/ : 
https://qiita.com/ryuhe1/items/da5acbcce4ac1911f47a : Bostan-MoriのMoriさんです