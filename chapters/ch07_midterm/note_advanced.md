# 第7章 中間試験対策 - 理解者向け編

この理解者向け編では、初級編・中級編で学んだ内容の理論的背景を深く掘り下げます。留数定理に基づく逆ラプラス変換の導出、畳み込み定理の証明、安定性の数学的解析を中心に、制御理論の数学的基礎を理解することを目指します。

---

## 1. 留数定理と逆ラプラス変換の理論 (Residue Theory and Inverse Laplace Transform)

### 1.1 ラプラス変換の積分表示と収束域

**ラプラス変換 (Laplace Transform)** の定義は、

$$F(s) = \mathcal{L}\{f(t)\} = \int_0^{\infty} f(t)e^{-st}dt$$

ここで、$s = \sigma + j\omega$ は複素変数です。この積分が収束するためには、実部 $\sigma$ が十分大きい必要があります。

**収束域 (Region of Convergence, ROC)** は、

$$\text{ROC} = \{s \in \mathbb{C} : \text{Re}(s) > \sigma_0\}$$

で定義されます。$\sigma_0$ を**収束軸 (Abscissa of Convergence)** と呼びます。

**逆ラプラス変換 (Inverse Laplace Transform)** の正確な定義は、Bromwich積分として知られる複素積分です：

$$f(t) = \mathcal{L}^{-1}\{F(s)\} = \frac{1}{2\pi j}\int_{\sigma-j\infty}^{\sigma+j\infty} F(s)e^{st}ds$$

ここで、積分経路は、$F(s)$ のすべての特異点（極）の右側にある垂直線 $\text{Re}(s) = \sigma$ です（$\sigma$ はROC内）。

### 1.2 留数定理の基礎 (Fundamentals of Residue Theorem)

**留数定理 (Residue Theorem)** は、複素解析における基本定理です。

関数 $F(s)$ が閉曲線 $C$ の内部で、有限個の特異点 $s_1, s_2, \ldots, s_n$ を除いて正則（解析的）であるとき、

$$\oint_C F(s)ds = 2\pi j \sum_{k=1}^{n} \text{Res}[F(s), s_k]$$

ここで、$\text{Res}[F(s), s_k]$ は点 $s_k$ における $F(s)$ の**留数 (Residue)** です。

**留数の計算方法**:

**(1) 単純極 (Simple Pole)** $s = p$ における留数：

$$\text{Res}[F(s), p] = \lim_{s \to p} (s-p)F(s)$$

**(2) $m$ 重極 (Pole of Order $m$)** $s = p$ における留数：

$$\text{Res}[F(s), p] = \frac{1}{(m-1)!}\lim_{s \to p} \frac{d^{m-1}}{ds^{m-1}}\left[(s-p)^m F(s)\right]$$

### 1.3 逆ラプラス変換への応用

逆ラプラス変換の積分路を閉じて、留数定理を適用します。

$t > 0$ のとき、積分路を左半平面の半円で閉じると（Jordan's Lemma により、半円弧上の積分は $\text{Re}(s) \to -\infty$ でゼロになる）、

$$f(t) = \sum_{k=1}^{n} \text{Res}[F(s)e^{st}, s_k]$$

ここで、$s_k$ は $F(s)$ のすべての極です。

**例1（理論的導出）**: $F(s) = \frac{1}{s+a}$ の逆ラプラス変換を留数定理から導出せよ。

**解**:
$F(s)$ は $s = -a$ に単純極を持ちます。

$$f(t) = \text{Res}\left[\frac{e^{st}}{s+a}, -a\right] = \lim_{s \to -a} (s+a) \cdot \frac{e^{st}}{s+a} = e^{-at}$$

したがって、

$$\mathcal{L}^{-1}\left\{\frac{1}{s+a}\right\} = e^{-at}, \quad t > 0$$

**例2（複素共役極の理論的扱い）**: $F(s) = \frac{\omega}{s^2+\omega^2}$ の逆ラプラス変換を留数定理から導出せよ。

**解**:
$F(s)$ は $s = \pm j\omega$ に単純極を持ちます。

$$f(t) = \text{Res}\left[\frac{\omega e^{st}}{s^2+\omega^2}, j\omega\right] + \text{Res}\left[\frac{\omega e^{st}}{s^2+\omega^2}, -j\omega\right]$$

第1項：

$$\text{Res}\left[\frac{\omega e^{st}}{(s-j\omega)(s+j\omega)}, j\omega\right] = \lim_{s \to j\omega} (s-j\omega) \cdot \frac{\omega e^{st}}{(s-j\omega)(s+j\omega)}$$

$$= \frac{\omega e^{j\omega t}}{2j\omega} = \frac{e^{j\omega t}}{2j}$$

第2項：

$$\text{Res}\left[\frac{\omega e^{st}}{(s-j\omega)(s+j\omega)}, -j\omega\right] = \frac{\omega e^{-j\omega t}}{-2j\omega} = -\frac{e^{-j\omega t}}{2j}$$

合計：

$$f(t) = \frac{e^{j\omega t}}{2j} - \frac{e^{-j\omega t}}{2j} = \frac{e^{j\omega t} - e^{-j\omega t}}{2j} = \sin(\omega t)$$

（Eulerの公式 $\sin(\theta) = \frac{e^{j\theta} - e^{-j\theta}}{2j}$ を使用）

### 1.4 Laurent級数展開と留数

より一般的なアプローチとして、**Laurent級数展開 (Laurent Series Expansion)** を用いた留数の計算があります。

点 $s_0$ の周りで $F(s)$ をLaurent級数展開すると、

$$F(s) = \sum_{n=-\infty}^{\infty} a_n(s-s_0)^n$$

留数は、$-1$ 次の項の係数 $a_{-1}$ です：

$$\text{Res}[F(s), s_0] = a_{-1}$$

**例3（高次の留数計算）**: $F(s) = \frac{e^s}{s^3}$ の $s=0$ における留数を求めよ。

**解**:
$e^s$ をMaclaurin級数展開：

$$e^s = 1 + s + \frac{s^2}{2!} + \frac{s^3}{3!} + \cdots$$

したがって、

$$F(s) = \frac{1}{s^3}\left(1 + s + \frac{s^2}{2} + \frac{s^3}{6} + \cdots\right)$$

$$= \frac{1}{s^3} + \frac{1}{s^2} + \frac{1}{2s} + \frac{1}{6} + \cdots$$

留数は $s^{-1}$ の係数なので、

$$\text{Res}\left[\frac{e^s}{s^3}, 0\right] = \frac{1}{2}$$

---

## 2. 畳み込み定理の証明と応用 (Convolution Theorem: Proof and Applications)

### 2.1 畳み込みの定義

2つの関数 $f(t)$ と $g(t)$ の**畳み込み (Convolution)** は、以下のように定義されます：

$$(f * g)(t) = \int_0^t f(\tau)g(t-\tau)d\tau$$

畳み込みは交換可能（可換）です：

$$f * g = g * f$$

**証明**:
$\tau' = t - \tau$ と変数変換すると、$d\tau' = -d\tau$ であり、

$$(f * g)(t) = \int_0^t f(\tau)g(t-\tau)d\tau = \int_t^0 f(t-\tau')g(\tau')(-d\tau')$$

$$= \int_0^t g(\tau')f(t-\tau')d\tau' = (g * f)(t)$$

### 2.2 畳み込み定理の主張と証明

**畳み込み定理 (Convolution Theorem)**:

$$\mathcal{L}\{f(t) * g(t)\} = F(s) \cdot G(s)$$

ここで、$F(s) = \mathcal{L}\{f(t)\}$、$G(s) = \mathcal{L}\{g(t)\}$ です。

**証明**:

$$\mathcal{L}\{f * g\} = \int_0^{\infty} \left[\int_0^t f(\tau)g(t-\tau)d\tau\right] e^{-st}dt$$

積分順序を交換します（Fubiniの定理）：

$$= \int_0^{\infty} f(\tau) \left[\int_{\tau}^{\infty} g(t-\tau)e^{-st}dt\right] d\tau$$

内側の積分で $u = t - \tau$ と置換すると、$t = u + \tau$、$dt = du$ であり、$t$ が $\tau$ から $\infty$ のとき、$u$ は $0$ から $\infty$ になります：

$$= \int_0^{\infty} f(\tau) \left[\int_0^{\infty} g(u)e^{-s(u+\tau)}du\right] d\tau$$

$$= \int_0^{\infty} f(\tau)e^{-s\tau} \left[\int_0^{\infty} g(u)e^{-su}du\right] d\tau$$

$$= \left[\int_0^{\infty} f(\tau)e^{-s\tau}d\tau\right] \left[\int_0^{\infty} g(u)e^{-su}du\right]$$

$$= F(s) \cdot G(s)$$

### 2.3 畳み込み定理の制御工学への応用

制御系の出力は、入力 $u(t)$ とインパルス応答 $g(t)$ の畳み込みで表されます：

$$y(t) = (g * u)(t) = \int_0^t g(\tau)u(t-\tau)d\tau$$

ラプラス領域では、

$$Y(s) = G(s) \cdot U(s)$$

したがって、伝達関数 $G(s) = Y(s)/U(s)$ が得られます。

**例1（畳み込みによる応答計算）**: インパルス応答が $g(t) = e^{-t}$ のシステムに、入力 $u(t) = t$ を加えたときの出力を畳み込みで求めよ。

**解**:
畳み込み定理より、

$$Y(s) = G(s) \cdot U(s) = \frac{1}{s+1} \cdot \frac{1}{s^2} = \frac{1}{s^2(s+1)}$$

部分分数展開：

$$Y(s) = \frac{A}{s} + \frac{B}{s^2} + \frac{C}{s+1}$$

$$A = \left[\frac{d}{ds}\left(\frac{s^2}{s^2(s+1)}\right)\right]_{s=0} = \left[\frac{d}{ds}\left(\frac{1}{s+1}\right)\right]_{s=0} = \left[-\frac{1}{(s+1)^2}\right]_{s=0} = -1$$

$$B = \left[\frac{1}{s+1}\right]_{s=0} = 1$$

$$C = \left[\frac{1}{s^2}\right]_{s=-1} = 1$$

$$Y(s) = -\frac{1}{s} + \frac{1}{s^2} + \frac{1}{s+1}$$

$$y(t) = -1 + t + e^{-t} = t + e^{-t} - 1, \quad t \geq 0$$

**検証（直接計算）**:

$$y(t) = \int_0^t e^{-\tau} \cdot (t-\tau)d\tau = \int_0^t (t-\tau)e^{-\tau}d\tau$$

部分積分により計算すると、同じ結果が得られます。✓

### 2.4 逆畳み込み定理

畳み込み定理の逆も成り立ちます：

$$\mathcal{L}\{f(t) \cdot g(t)\} = \frac{1}{2\pi j}\int_{\sigma-j\infty}^{\sigma+j\infty} F(s-\lambda)G(\lambda)d\lambda$$

これは周波数領域での畳み込みを意味します。実用上は、時間領域の積よりも、畳み込み定理を使ってラプラス領域で計算する方が簡単です。

---

## 3. 安定性の理論的解析 (Theoretical Analysis of Stability)

### 3.1 安定性の定義

線形時不変（LTI）システムの安定性には、いくつかの定義があります：

**（1）BIBO安定性 (Bounded-Input Bounded-Output Stability)**

有界な入力に対して、出力が常に有界であるとき、システムはBIBO安定です。

数学的には、

$$|u(t)| < M_u < \infty \quad \forall t \geq 0 \implies |y(t)| < M_y < \infty \quad \forall t \geq 0$$

**定理**: LTIシステムがBIBO安定であるための必要十分条件は、インパルス応答が絶対可積分であることです：

$$\int_0^{\infty} |g(t)|dt < \infty$$

**（2）漸近安定性 (Asymptotic Stability)**

初期条件に関わらず、システムの状態が時間とともにゼロに収束するとき、システムは漸近安定です：

$$\lim_{t \to \infty} x(t) = 0$$

**（3）Lyapunov安定性 (Lyapunov Stability)**

任意の $\epsilon > 0$ に対して、$\delta > 0$ が存在し、初期状態が $\|x(0)\| < \delta$ ならば、すべての $t \geq 0$ で $\|x(t)\| < \epsilon$ となるとき、システムはLyapunov安定です。

### 3.2 伝達関数の極と安定性

LTIシステムの安定性は、伝達関数の極の位置によって完全に決定されます。

**定理（極配置と安定性）**:

$$G(s) = \frac{N(s)}{D(s)}$$

の極を $p_1, p_2, \ldots, p_n$ とするとき、

- **安定 (Stable)**: すべての極が左半平面にある（$\text{Re}(p_i) < 0$ for all $i$）
- **限界安定 (Marginally Stable)**: 極が虚軸上にあり（$\text{Re}(p_i) = 0$）、かつ単純極である
- **不安定 (Unstable)**: 少なくとも1つの極が右半平面にある（$\text{Re}(p_i) > 0$）、または虚軸上に重根がある

**証明の概要**:
部分分数展開により、インパルス応答は以下の形になります：

$$g(t) = \sum_{i=1}^{n} A_i e^{p_i t}$$

（重根や複素共役極の場合は $t^k e^{p_i t}$ の形も含む）

- $\text{Re}(p_i) < 0$ ならば、$e^{p_i t} \to 0$ as $t \to \infty$ （安定）
- $\text{Re}(p_i) > 0$ ならば、$e^{p_i t} \to \infty$ as $t \to \infty$ （不安定）
- $\text{Re}(p_i) = 0$ ならば、$e^{p_i t} = e^{j\omega t}$ は振動（限界安定）

### 3.3 Routh-Hurwitzの安定判別法

極を直接計算せずに安定性を判定する方法として、**Routh-Hurwitzの安定判別法 (Routh-Hurwitz Criterion)** があります。

特性方程式が、

$$D(s) = a_n s^n + a_{n-1}s^{n-1} + \cdots + a_1 s + a_0 = 0$$

のとき、**Routh配列 (Routh Array)** を以下のように構成します：

$$\begin{array}{c|cccc}
s^n & a_n & a_{n-2} & a_{n-4} & \cdots \\
s^{n-1} & a_{n-1} & a_{n-3} & a_{n-5} & \cdots \\
s^{n-2} & b_1 & b_2 & b_3 & \cdots \\
s^{n-3} & c_1 & c_2 & c_3 & \cdots \\
\vdots & \vdots & \vdots & \vdots & \ddots \\
s^0 & * & & &
\end{array}$$

ここで、

$$b_1 = \frac{a_{n-1} a_{n-2} - a_n a_{n-3}}{a_{n-1}}, \quad b_2 = \frac{a_{n-1} a_{n-4} - a_n a_{n-5}}{a_{n-1}}, \quad \ldots$$

$$c_1 = \frac{b_1 a_{n-3} - a_{n-1} b_2}{b_1}, \quad c_2 = \frac{b_1 a_{n-5} - a_{n-1} b_3}{b_1}, \quad \ldots$$

**定理（Routh-Hurwitzの安定判別法）**:
システムが安定であるための必要十分条件は、Routh配列の第1列の要素がすべて正であることです。

符号変化の回数は、右半平面にある極の個数を表します。

**例1**: 特性方程式 $D(s) = s^3 + 6s^2 + 11s + 6 = 0$ の安定性を判定せよ。

**解**:
Routh配列：

$$\begin{array}{c|cc}
s^3 & 1 & 11 \\
s^2 & 6 & 6 \\
s^1 & \frac{6 \cdot 11 - 1 \cdot 6}{6} = \frac{60}{6} = 10 & 0 \\
s^0 & \frac{10 \cdot 6 - 6 \cdot 0}{10} = 6 &
\end{array}$$

第1列：$1, 6, 10, 6$ すべて正 → **安定**

（検証：実際に因数分解すると $D(s) = (s+1)(s+2)(s+3)$、すべての極が負の実数 ✓）

**例2**: 特性方程式 $D(s) = s^3 + 2s^2 + 2s + 4 = 0$ の安定性を判定せよ。

**解**:
Routh配列：

$$\begin{array}{c|cc}
s^3 & 1 & 2 \\
s^2 & 2 & 4 \\
s^1 & \frac{2 \cdot 2 - 1 \cdot 4}{2} = 0 & 0 \\
s^0 & \cdots &
\end{array}$$

第1列に0が現れた（$s^1$ 行）。これは虚軸上に極があることを示します。

特別な処理：$s^2$ 行の多項式 $2s^2 + 4$ を微分して $4s$ を $s^1$ 行に使用します。

$$\begin{array}{c|cc}
s^3 & 1 & 2 \\
s^2 & 2 & 4 \\
s^1 & 4 & 0 \\
s^0 & 4 &
\end{array}$$

第1列：$1, 2, 4, 4$ すべて正ですが、0が現れたことから虚軸上に極があります → **限界安定または不安定**

（実際の極を計算すると、$s = -4, j\sqrt{2}, -j\sqrt{2}$ となり、虚軸上に極があることが確認できます）

### 3.4 Nyquist安定判別法の予告

より一般的で強力な安定判別法として、**Nyquist安定判別法 (Nyquist Stability Criterion)** があります。これは、開ループ伝達関数 $G(s)H(s)$ の周波数応答（Nyquist線図）から、閉ループ系の安定性を判定する方法です。

**Nyquistの安定判別法**:
開ループ伝達関数 $G(s)H(s)$ が右半平面に $P$ 個の極を持つとき、Nyquist線図が点 $-1+j0$ を $N$ 回（反時計回りを正）囲むならば、閉ループ系は右半平面に $Z = N + P$ 個の極を持ちます。

安定であるためには、$Z = 0$ つまり $N = -P$ である必要があります。

通常、開ループが安定（$P=0$）ならば、Nyquist線図が $-1$ 点を囲まなければ（$N=0$）、閉ループも安定です。

この詳細は、次章（周波数応答）で学びます。

### 3.5 2次系の過渡応答特性と安定性

標準的な2次系の伝達関数：

$$G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

ここで、$\omega_n$ は固有角周波数、$\zeta$ は減衰係数です。

特性方程式の極：

$$s = -\zeta\omega_n \pm \omega_n\sqrt{\zeta^2 - 1}$$

安定性と過渡応答の関係：

- **$\zeta > 1$ (過減衰, Overdamped)**: 2つの負の実数極、振動なし、安定
- **$\zeta = 1$ (臨界減衰, Critically Damped)**: 重根 $s = -\omega_n$、振動なし、安定（最速収束）
- **$0 < \zeta < 1$ (減衰振動, Underdamped)**: 複素共役極 $s = -\zeta\omega_n \pm j\omega_n\sqrt{1-\zeta^2}$、振動あり、安定
- **$\zeta = 0$ (非減衰振動, Undamped)**: 純虚数極 $s = \pm j\omega_n$、持続振動、限界安定
- **$\zeta < 0$ (発散, Unstable)**: 正の実部を持つ極、不安定

減衰振動系（$0 < \zeta < 1$）の重要なパラメータ：

**オーバーシュート (Overshoot)**:

$$M_p = e^{-\pi\zeta/\sqrt{1-\zeta^2}} \times 100\%$$

**整定時間 (Settling Time, 2%基準)**:

$$t_s = \frac{4}{\zeta\omega_n}$$

**ピーク時間 (Peak Time)**:

$$t_p = \frac{\pi}{\omega_n\sqrt{1-\zeta^2}}$$

これらのパラメータは、制御系設計において重要な性能指標となります。

---

## まとめ

この理解者向け編では、中間試験の理論的背景を深く学びました：

### 留数定理と逆ラプラス変換
- **Bromwich積分**: 逆ラプラス変換の正確な定義は複素積分
- **留数定理**: 単純極と重極の留数計算、Laurent級数展開
- **理論的導出**: 基本的な変換ペアを留数定理から厳密に導出

### 畳み込み定理
- **畳み込みの定義**: $(f * g)(t) = \int_0^t f(\tau)g(t-\tau)d\tau$
- **定理の証明**: Fubiniの定理を用いた積分順序の交換による証明
- **制御系への応用**: 出力 = インパルス応答 ⊗ 入力、$Y(s) = G(s)U(s)$

### 安定性の理論
- **安定性の定義**: BIBO安定性、漸近安定性、Lyapunov安定性
- **極配置と安定性**: 左半平面の極 → 安定、右半平面の極 → 不安定
- **Routh-Hurwitz判別法**: 特性方程式の係数から安定性を判定
- **2次系の特性**: 減衰係数 $\zeta$ による過渡応答の分類

これらの理論的知識は、制御工学の高度な解析と設計の基礎となります。中間試験では、これらの理論を理解した上で、実際の計算問題に適用できることが求められます。

次の段階では、周波数応答解析、Nyquist安定判別法、Bode線図、状態空間表現など、さらに高度なトピックへと進みます。

---

**作成日**: 2024年
**対象**: 制御工学の中間試験準備
**レベル**: 理解者向け（理論的背景と数学的導出）
