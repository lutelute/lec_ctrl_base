# 第2章 複素数とラプラス変換 - 理解者向け技術ノート

## 1. 複素数の理論的側面 (Theoretical Aspects of Complex Numbers)

### 1.1 複素関数と解析性 (Complex Functions and Analyticity)

複素数 $s = \sigma + j\omega$ の関数 $F(s)$ を**複素関数 (complex function)** と呼びます。制御工学では、伝達関数がこれに該当します。

複素関数が**解析的 (analytic)** であるとは、その点の近傍で微分可能であることを意味します。解析性は、**コーシー・リーマンの条件 (Cauchy-Riemann conditions)** によって判定されます。

### 1.2 複素平面での極と零点 (Poles and Zeros in Complex Plane)

有理関数 $F(s) = \frac{N(s)}{D(s)}$ において：

- **零点 (zero)**: $N(s) = 0$ となる $s$ の値
- **極 (pole)**: $D(s) = 0$ となる $s$ の値

極と零点の複素平面上の位置は、システムの安定性と動特性を決定します：
- **左半平面の極**: 安定なシステム（実部 $\sigma < 0$）
- **右半平面の極**: 不安定なシステム（実部 $\sigma > 0$）
- **虚軸上の極**: 臨界安定（実部 $\sigma = 0$）

## 2. ラプラス変換の理論的性質 (Theoretical Properties of Laplace Transform)

### 2.1 収束条件と収束領域 (Convergence Conditions and ROC)

ラプラス変換の積分 $\int_0^\infty f(t)e^{-st}dt$ が収束するための条件を考えます。

**収束領域 (Region of Convergence, ROC)** は、ラプラス変換が存在する $s$ 平面上の領域です：

$$\text{Re}(s) > \sigma_0$$

ここで $\sigma_0$ は収束座標です。ROCは、極の位置によって決定されます。

**例**:
- $f(t) = e^{-at}u(t)$ のとき、$F(s) = \frac{1}{s+a}$、ROC: $\text{Re}(s) > -a$

### 2.2 最終値定理と初期値定理 (Final and Initial Value Theorems)

**最終値定理 (Final Value Theorem)**:
$$\lim_{t \to \infty} f(t) = \lim_{s \to 0} sF(s)$$

ただし、$f(t)$ が有限値に収束し、$sF(s)$ の極が左半平面にある場合に適用可能です。

**初期値定理 (Initial Value Theorem)**:
$$\lim_{t \to 0^+} f(t) = \lim_{s \to \infty} sF(s)$$

これらの定理は、時間応答の最終値・初期値を $s$ 領域から直接計算できるため、非常に有用です。

### 2.3 畳み込み定理 (Convolution Theorem)

時間領域の畳み込み (convolution) は、$s$ 領域では単純な乗算になります：

$$\mathcal{L}\left\{\int_0^t f(\tau)g(t-\tau)d\tau\right\} = F(s)G(s)$$

**制御工学での意義**: システムの出力 $y(t)$ は、入力 $u(t)$ とインパルス応答 $h(t)$ の畳み込みで表されます。ラプラス領域では $Y(s) = H(s)U(s)$ となり、計算が大幅に簡略化されます。

## 3. 部分分数展開と逆ラプラス変換 (Partial Fraction Expansion and Inverse Laplace Transform)

### 3.1 部分分数展開の手法 (Method of Partial Fraction Expansion)

有理関数 $F(s) = \frac{N(s)}{D(s)}$ を逆ラプラス変換する際、まず部分分数に展開します。

**前提条件**: $\deg(N) < \deg(D)$ （分子の次数 < 分母の次数）

分母を因数分解し、部分分数に展開します：

$$F(s) = \frac{A_1}{s - p_1} + \frac{A_2}{s - p_2} + \cdots + \frac{A_n}{s - p_n}$$

### 3.2 単純極の場合 (Simple Poles)

極 $p_i$ が単純極（重複度1）の場合、留数 $A_i$ は以下で求められます：

$$A_i = \lim_{s \to p_i} (s - p_i)F(s)$$

**例題**: $F(s) = \frac{5}{(s+1)(s+2)}$ を部分分数展開せよ。

**解**:
$$F(s) = \frac{A}{s+1} + \frac{B}{s+2}$$

$$A = \lim_{s \to -1} (s+1)F(s) = \frac{5}{-1+2} = 5$$

$$B = \lim_{s \to -2} (s+2)F(s) = \frac{5}{-2+1} = -5$$

したがって：
$$F(s) = \frac{5}{s+1} - \frac{5}{s+2}$$

逆ラプラス変換：
$$f(t) = 5e^{-t} - 5e^{-2t} = 5(e^{-t} - e^{-2t})$$

### 3.3 重根の場合 (Repeated Poles)

極 $p$ が $m$ 重根の場合、展開は以下の形式になります：

$$F(s) = \frac{A_1}{(s-p)^m} + \frac{A_2}{(s-p)^{m-1}} + \cdots + \frac{A_m}{s-p}$$

留数の計算式：
$$A_k = \frac{1}{(m-k)!} \lim_{s \to p} \frac{d^{m-k}}{ds^{m-k}} \left[(s-p)^m F(s)\right]$$

**例**: $F(s) = \frac{1}{(s+1)^2}$ の場合、$f(t) = te^{-t}$

### 3.4 複素共役極の場合 (Complex Conjugate Poles)

極が複素共役対 $s = -\alpha \pm j\omega$ の場合：

$$F(s) = \frac{As + B}{(s+\alpha)^2 + \omega^2}$$

これは以下の形に変形できます：

$$F(s) = \frac{A(s+\alpha)}{(s+\alpha)^2 + \omega^2} + \frac{B-A\alpha}{(s+\alpha)^2 + \omega^2} \cdot \frac{\omega}{\omega}$$

逆ラプラス変換により：

$$f(t) = Ae^{-\alpha t}\cos(\omega t) + \frac{B-A\alpha}{\omega}e^{-\alpha t}\sin(\omega t)$$

または、より一般的には：

$$f(t) = Ce^{-\alpha t}\sin(\omega t + \phi)$$

ここで、振幅 $C$ と位相 $\phi$ は $A, B$ から計算されます。

### 3.5 計算例: 2次システムの応答

**問題**: 単位ステップ入力に対する2次遅れ系の応答を求めよ。伝達関数は $G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$ とする。

**解**:
出力は $Y(s) = G(s) \cdot \frac{1}{s}$ なので：

$$Y(s) = \frac{\omega_n^2}{s(s^2 + 2\zeta\omega_n s + \omega_n^2)}$$

部分分数展開と逆ラプラス変換により、減衰振動応答が得られます（$\zeta < 1$ の場合）。

## 4. 制御工学への応用 (Applications to Control Engineering)

### 4.1 伝達関数との関係 (Relationship with Transfer Functions)

制御系の**伝達関数 (transfer function)** は、入力と出力のラプラス変換の比として定義されます：

$$G(s) = \frac{Y(s)}{U(s)}$$

初期条件がゼロの場合、システムの動特性は伝達関数によって完全に記述されます。

### 4.2 システム応答の計算手順

1. システムの微分方程式からラプラス変換により伝達関数を導出
2. 入力信号をラプラス変換
3. 出力 $Y(s) = G(s)U(s)$ を計算
4. 部分分数展開
5. 逆ラプラス変換により時間応答 $y(t)$ を求める

### 4.3 次章への橋渡し

第3章では、伝達関数を用いた制御系の詳細な解析を学びます：
- ボード線図による周波数応答解析（$s = j\omega$ の代入）
- ナイキスト線図による安定性判別
- 根軌跡法によるシステム設計

これらの手法は、本章で学んだ複素数とラプラス変換の理論を基礎としています。

---

## まとめ

この理解者向けノートでは、以下の発展的内容を学びました：

- 複素関数の解析性と極・零点の意味
- ラプラス変換の収束領域（ROC）と最終値・初期値定理
- 畳み込み定理とその制御工学での重要性
- 部分分数展開の詳細な手法（単純極、重根、複素共役極）
- 具体的な計算例と実践的な応用
- 伝達関数解析への橋渡し

これらの知識により、制御系の数学的解析と設計の基礎が確立されました。次章では、これらを活用した実践的な制御系解析を学習します。
