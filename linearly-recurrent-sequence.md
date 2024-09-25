---
marp: true
math: mathjax
---

## 扱うこと

$K+1$項間の線型漸化式を持つ数列の$N$項目は行列累乗で求めたら$O(K^3 \log N)$かかるが、このスライドの手法でやると、もうちょっと速くなる。

---

## 対象とする人

- 行列の積を知っている

- 行列式の定義を知っている

- 余因子展開(ラプラス展開)を知っていると嬉しいかも

- 競技プログラミングの知識を別に問うわけではないけど、実装を理解するにはPythonの知識が必要かも

---

## 考える問題

$$a_{n+K}= c_1 a_{n+K-1} + c_2 a_{n+k-2} + \dots + c_{K-1} a_{n+1} + c_K a_n$$
という$K+1$項間線形漸化式があるとする。

$c_1, c_2, \dots , c_K$と、$a_0, a_1, \dots , a_{K-1}$ が与えられる。 $a_{N}$を求めよ。

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
a_{N+K-1} \\
a_{N+K-2} \\
a_{N+K-3} \\ 
\vdots \\
a_{N+1} \\
a_{N}
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
a_{K-1} \\
a_{K-2} \\
a_{K-3} \\
\vdots \\
a_{1} \\ 
a_0  
\end {bmatrix}
$$
となる。
数列の$N$項目は$A^N$を計算することで求められる。行列の積は1回あたり$O(K^3)$かかるので、$A^{N}$を求めるのに繰り返し二乗法を使うと、$a_{N}$は全体で$O(K^3 \log N)$ の計算量で求められる。

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
求めたい$a_{N+1}$は、$A^{N}$の一番下の行と、$[a_{K-1}, a_{K-2},   \dots , a_0 ]$の内積だった。
だから、$E, A^1, A^2, \dots A^{K-1}$の一番下の行のみわかればよい。

ここで、驚きの事実として、$A^i$ ( $0 \leq i < K$) の一番下の行は、$K-i$列目だけ$1$で、他は$0$となっている。

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
### 驚きの事実の説明

横ベクトル $\boldsymbol{p_1}, \boldsymbol{p_2}, \dots \boldsymbol{p_K}$を縦に並べた行列に左からAをかけることを考える。

$$ 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
0 & 0 & 1 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}


\begin{bmatrix}
\boldsymbol{p_1} \\
\boldsymbol{p_2} \\
\boldsymbol{p_3} \\
\boldsymbol{p_4} \\
\vdots \\
\boldsymbol{p_{K}} \\
\end{bmatrix}

= 

\begin{bmatrix}
c_1\boldsymbol{p_1} + c_2 \boldsymbol{p_2} + \dots + c_K \boldsymbol{p_K} \\
\boldsymbol{p_1} \\
\boldsymbol{p_2} \\
\boldsymbol{p_3} \\
\vdots \\
\boldsymbol{p_{K-1}} \\
\end{bmatrix}

$$

このように、行列に対して$A$を左からかけると、行が一行下にずれて、一番上の行は行の線形結合になる。

--- 

ここで、

> 驚きの事実 
> $A^i$ ( $0 \leq i < K$) の一番下の行は、$K-i$列目だけ$1$で、他は$0$となっている。

を拡張した、

> 驚きの事実2
> $A^i$ ($0 \leq i < K$)の$j$行目 ($i+1 \leq j \leq K$)は、$j-i$列目だけ$1$で、他は$0$となっている。

を考える。 $A^0$や$A^1$について成立するのは明らか。

$$ 
A = 
\begin{bmatrix}
c_1   & c_2 & c_3 & c_4 & \dots & c_K \\
1 & 0 & 0 & 0 & \dots & 0 \\
0 & 1 & 0 & 0 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \\
0 & \dots & 0 & 0  & 1 & 0
\end{bmatrix}
$$

---

> 驚きの事実2
> $A^i$ ($0 \leq i < K$)の$j$行目 ($i+1 \leq j \leq K$)は、$j-i$列目だけ$1$で、他は$0$となっている。

帰納法で証明する。$i=0,1$で成立するのは明らか。$0$以上$K$未満のある整数$i$について驚きの事実2が成立すると仮定する。

仮定から、$A^i$の $j$行目 ($i+1 \leq j \leq K$)は、$j-i$列目だけ$1$で、他は$0$。

$A A^i = A^{i+1}$だから、$A^{i+1}$の$j$行目 ($2 \leq j \leq K$)は $A^i$の$j-1$行目に一致する。

つまり、$(i+1) + 1 \leq j \leq K$ なる$j$について、$A^{i+1}$の$j$行目は$A^i$の$j-1$行目と同じく$j-1-i = j - (i+1)$列目だけ$1$で、他は$0$であるとわかる。

これは、

驚きの事実2が$i+1$でも成立していることに他ならない。

帰納的に、$0 \leq i \lt K$についても驚きの事実2が成立する。(説明終わり)

---

驚きの事実などを使うと、
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

(結論を先に書くと、$=x^K - c_1 x^{K-1} - \dots - c_{K-1} x^1 - c_{K}$ )

---

## ステップ2: $x^N$を $\varphi_A(x)$で割った余りを求める

mod $\varphi_A(x)$ 上で$x^N$を計算すればよい。FFTなどをすると割り算に$O(K\log K)$かかり、繰り返し二乗法で掛け算を$O(\log N)$回やるので $O(K \log K \log N)$ で求められる。


---

## ステップ3 $a_N = r_K a_K + r_{K-1} a{K-1} + \dots + r_0 a_0$を計算する

はい。やるだけ。

---

## 参考文献

https://blog.miz-ar.info/2019/02/typical-dp-contest-t/ : 
https://qiita.com/ryuhe1/items/da5acbcce4ac1911f47a : Bostan-MoriのMoriさんです