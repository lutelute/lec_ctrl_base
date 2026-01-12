# 第3章 LTIシステムの表現 - 理解者向け編

## 1. 因果性とLTIシステム (Causality and LTI Systems)

### 1.1 因果性の定義

**因果性 (causality)** は、物理的に実現可能なシステムの基本的な要件です。因果的システムとは、**現在の出力が過去と現在の入力のみに依存し、未来の入力には依存しない**システムを指します。

数学的には、システムが因果的であるための条件は：

$$y(t_0) \text{ は } x(t), \, t \leq t_0 \text{ のみに依存する}$$

直感的には、システムが「未来を見ることができない」ことを意味します。これは、現実の物理システムが必ず満たすべき性質です。

### 1.2 インパルス応答と因果性

LTIシステムの因果性は、**インパルス応答 $h(t)$ の性質**として表現できます。

**定理**: LTIシステムが因果的であるための必要十分条件は、

$$h(t) = 0 \quad \text{for } t < 0$$

**証明の概略**:

畳み込み積分を考えます：

$$y(t) = \int_{-\infty}^{\infty} h(\tau)x(t - \tau)d\tau$$

変数変換 $\sigma = t - \tau$ を行うと：

$$y(t) = \int_{-\infty}^{\infty} h(t - \sigma)x(\sigma)d\sigma$$

因果性の条件「$y(t)$ は $x(\sigma), \, \sigma \leq t$ のみに依存」を満たすには、$\sigma > t$ の範囲で被積分関数が0でなければなりません。つまり：

$$h(t - \sigma) = 0 \quad \text{for } \sigma > t$$

これは $h(t) = 0 \, (t < 0)$ と等価です。■

### 1.3 因果性の例

**因果的システムの例**:

1. **RC回路**: $h(t) = \frac{1}{\tau}e^{-t/\tau}u(t)$
   - $t < 0$ で $h(t) = 0$ なので因果的

2. **理想的な遅延**: $h(t) = \delta(t - T), \, T > 0$
   - 入力を時間 $T$ だけ遅らせる（因果的）

**非因果的システムの例**:

1. **理想的な進み**: $h(t) = \delta(t + T), \, T > 0$
   - 入力を時間 $T$ だけ進める（物理的に実現不可能）

2. **両側指数関数**: $h(t) = e^{-|t|}$
   - $t < 0$ でも値を持つため非因果的

### 1.4 伝達関数と因果性

周波数領域（$s$領域）では、因果性は伝達関数 $H(s)$ の**解析性 (analyticity)** と関連します。

**パレーの定理 (Paley-Wiener Theorem)** により、因果的なシステムの伝達関数は、実軸上および右半平面で解析的でなければなりません。これは、**極がすべて左半平面または虚軸上に存在する**ことを意味します。

また、因果性を持つ実数値インパルス応答に対して、伝達関数の実部と虚部の間には**ヒルベルト変換 (Hilbert transform)** の関係（**クラマース・クローニッヒの関係 (Kramers-Kronig relations)**）が成り立ちます。

---

## 2. BIBO安定性 (Bounded-Input Bounded-Output Stability)

### 2.1 BIBO安定性の定義

**BIBO安定性 (Bounded-Input Bounded-Output Stability)** は、システムの実用的な安定性の概念です。

**定義**: システムがBIBO安定であるとは、**すべての有界な入力に対して出力も有界となる**ことです。

数学的には：

$$\text{If } |x(t)| \leq M_x < \infty \text{ for all } t, \quad \text{then } |y(t)| \leq M_y < \infty \text{ for all } t$$

ここで、$M_x$ と $M_y$ は有限の定数です。

### 2.2 BIBO安定性の判定条件

LTIシステムがBIBO安定であるための必要十分条件は、**インパルス応答が絶対可積分である**ことです：

$$\int_{-\infty}^{\infty} |h(t)|dt < \infty$$

**証明**:

畳み込み積分の絶対値を取ると：

$$|y(t)| = \left|\int_{-\infty}^{\infty} h(\tau)x(t - \tau)d\tau\right| \leq \int_{-\infty}^{\infty} |h(\tau)||x(t - \tau)|d\tau$$

入力が有界 $|x(t)| \leq M_x$ であれば：

$$|y(t)| \leq M_x \int_{-\infty}^{\infty} |h(\tau)|d\tau$$

したがって、$\int_{-\infty}^{\infty} |h(t)|dt < \infty$ であれば、出力も有界です。■

### 2.3 伝達関数による安定性判定

周波数領域では、BIBO安定性は伝達関数 $H(s)$ の**極の位置**によって判定できます。

**定理**: 因果的なLTIシステムがBIBO安定であるための必要十分条件は、**すべての極が複素平面の左半平面 (Left Half Plane) に存在する**ことです。

数学的には：

$$\text{Re}(p_i) < 0 \quad \text{for all poles } p_i$$

**注意**: 虚軸上の極（$\text{Re}(p) = 0$）は、BIBO安定性を満たしません。ただし、**限界安定 (marginally stable)** と呼ばれます。

### 2.4 安定性の例

**安定なシステム**:

1. **1次遅れ系**: $H(s) = \frac{1}{s + a}, \, a > 0$
   - 極: $s = -a$ （負の実数）
   - インパルス応答: $h(t) = e^{-at}u(t)$
   - $\int_0^\infty e^{-at}dt = \frac{1}{a} < \infty$ ✓

2. **不足減衰2次系**: $H(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_ns + \omega_n^2}, \, 0 < \zeta < 1$
   - 極: $s = -\zeta\omega_n \pm j\omega_n\sqrt{1 - \zeta^2}$ （左半平面の複素共役対）
   - 安定 ✓

**不安定なシステム**:

1. **積分器**: $H(s) = \frac{1}{s}$
   - 極: $s = 0$ （虚軸上）
   - 限界安定（BIBO安定ではない）
   - ステップ入力に対してランプ出力（無限に増加）

2. **右半平面の極**: $H(s) = \frac{1}{s - a}, \, a > 0$
   - 極: $s = a$ （正の実数）
   - インパルス応答: $h(t) = e^{at}u(t)$ （指数的に発散）
   - 不安定 ✗

### 2.5 ラウスの安定判別法

**ラウスの安定判別法 (Routh-Hurwitz Criterion)** は、特性多項式の係数から、極を直接求めることなく安定性を判定する方法です。

特性方程式：

$$a_ns^n + a_{n-1}s^{n-1} + \cdots + a_1s + a_0 = 0$$

に対して、**ラウス表 (Routh array)** を作成し、第1列の符号変化の回数を数えます。符号変化の回数が、右半平面にある極の個数と等しくなります。

**例**: $s^3 + 3s^2 + 3s + 1 = 0$

ラウス表：
$$
\begin{array}{c|cc}
s^3 & 1 & 3 \\
s^2 & 3 & 1 \\
s^1 & \frac{8}{3} & 0 \\
s^0 & 1 & \\
\end{array}
$$

第1列（$1, 3, \frac{8}{3}, 1$）はすべて正なので、符号変化なし → すべての極が左半平面 → 安定 ✓

---

## 3. 周波数応答との関係 (Relationship with Frequency Response)

### 3.1 周波数応答の定義

**周波数応答 (frequency response)** $H(j\omega)$ は、伝達関数 $H(s)$ を虚軸上 $s = j\omega$ で評価したものです：

$$H(j\omega) = H(s)\big|_{s = j\omega}$$

これは、システムに正弦波入力 $x(t) = A\sin(\omega t)$ を加えたときの定常応答を表します。

### 3.2 正弦波入力に対する応答

LTIシステムに正弦波 $x(t) = A\sin(\omega t)$ を入力すると、定常状態では出力も同じ周波数の正弦波になります：

$$y_{ss}(t) = A|H(j\omega)|\sin(\omega t + \angle H(j\omega))$$

ここで：
- $|H(j\omega)|$ は**ゲイン (gain)** または**振幅比**
- $\angle H(j\omega)$ は**位相 (phase)**

つまり、周波数応答は**複素数**で、その大きさが振幅比、偏角が位相を表します。

### 3.3 ボード線図

**ボード線図 (Bode diagram)** は、周波数応答を視覚化する標準的な方法です。

**ゲイン線図 (magnitude plot)**:
$$\text{Gain [dB]} = 20\log_{10}|H(j\omega)|$$

**位相線図 (phase plot)**:
$$\text{Phase [deg]} = \angle H(j\omega) \times \frac{180}{\pi}$$

これらを対数周波数軸 $\omega$ に対してプロットします。

### 3.4 ボード線図の例：1次遅れ系

1次遅れ系 $H(s) = \frac{K}{1 + \tau s}$ の周波数応答は：

$$H(j\omega) = \frac{K}{1 + j\omega\tau}$$

**ゲイン**:
$$|H(j\omega)| = \frac{K}{\sqrt{1 + (\omega\tau)^2}}$$

**位相**:
$$\angle H(j\omega) = -\tan^{-1}(\omega\tau)$$

**特徴**:
- 低周波域（$\omega \ll 1/\tau$）: ゲイン ≈ $K$、位相 ≈ 0°
- カットオフ周波数（$\omega = 1/\tau$）: ゲイン = $K/\sqrt{2}$ (-3dB)、位相 = -45°
- 高周波域（$\omega \gg 1/\tau$）: ゲイン ∝ $1/\omega$ (-20dB/decade)、位相 ≈ -90°

### 3.5 ナイキスト線図

**ナイキスト線図 (Nyquist diagram)** は、$H(j\omega)$ を複素平面上に極座標プロットしたものです。周波数 $\omega$ を $0$ から $\infty$ まで変化させると、軌跡が描かれます。

ナイキスト線図は、**ナイキストの安定判別法 (Nyquist stability criterion)** によってフィードバックシステムの安定性を解析するのに使われます。

### 3.6 周波数応答からインパルス応答への変換

周波数応答 $H(j\omega)$ とインパルス応答 $h(t)$ は、**フーリエ変換**によって関連付けられます：

$$H(j\omega) = \int_{-\infty}^{\infty} h(t)e^{-j\omega t}dt$$

逆変換：

$$h(t) = \frac{1}{2\pi}\int_{-\infty}^{\infty} H(j\omega)e^{j\omega t}d\omega$$

これにより、周波数領域と時間領域の情報を相互に変換できます。

---

## 4. 状態空間表現への発展 (Development to State-Space Representation)

### 4.1 入出力表現の限界

これまで学んだ**入出力表現 (input-output representation)** は、外部から観測可能な入力と出力の関係のみを記述します：

- 微分方程式: $a_n\frac{d^ny}{dt^n} + \cdots = b_m\frac{d^mx}{dt^m} + \cdots$
- 伝達関数: $H(s) = \frac{Y(s)}{X(s)}$
- 畳み込み積分: $y(t) = h(t) * x(t)$

しかし、システム内部の**状態 (state)** を直接扱うことができません。

### 4.2 状態と状態変数

**状態 (state)** とは、システムの現在の「内部的な記憶」を表す最小限の情報の集合です。

**状態変数 (state variables)** $x_1(t), x_2(t), \ldots, x_n(t)$ は、状態を表現する変数で、以下の性質を持ちます：

1. 時刻 $t_0$ での状態 $\mathbf{x}(t_0)$ と、$t \geq t_0$ での入力 $u(t)$ が分かれば、$t \geq t_0$ での出力 $y(t)$ を一意に決定できる

2. 状態変数の個数 $n$ は、システムを記述する微分方程式の次数と等しい

### 4.3 状態空間表現

**状態空間表現 (state-space representation)** は、システムを1階の連立微分方程式として記述します：

**状態方程式 (state equation)**:
$$\frac{d\mathbf{x}(t)}{dt} = \mathbf{A}\mathbf{x}(t) + \mathbf{B}u(t)$$

**出力方程式 (output equation)**:
$$y(t) = \mathbf{C}\mathbf{x}(t) + \mathbf{D}u(t)$$

ここで：
- $\mathbf{x}(t) \in \mathbb{R}^n$ は**状態ベクトル**
- $u(t)$ は**入力**（スカラーまたはベクトル）
- $y(t)$ は**出力**（スカラーまたはベクトル）
- $\mathbf{A} \in \mathbb{R}^{n \times n}$ は**システム行列**
- $\mathbf{B} \in \mathbb{R}^{n \times 1}$ は**入力行列**
- $\mathbf{C} \in \mathbb{R}^{1 \times n}$ は**出力行列**
- $\mathbf{D} \in \mathbb{R}$ は**直達項**（多くの場合0）

### 4.4 例：RC回路の状態空間表現

RC回路の微分方程式：

$$\tau\frac{dv_C}{dt} + v_C = v_{in}$$

状態変数を $x = v_C$ とすると：

$$\frac{dx}{dt} = -\frac{1}{\tau}x + \frac{1}{\tau}u$$

出力を $y = v_C = x$ とすると：

$$y = x$$

行列形式：

$$\frac{dx}{dt} = -\frac{1}{\tau}x + \frac{1}{\tau}u, \quad y = x$$

つまり、$A = -\frac{1}{\tau}$, $B = \frac{1}{\tau}$, $C = 1$, $D = 0$

### 4.5 例：質量-ばね-ダンパ系の状態空間表現

質量-ばね-ダンパ系の微分方程式：

$$m\frac{d^2x}{dt^2} + c\frac{dx}{dt} + kx = f$$

状態変数を $x_1 = x$ （変位）、$x_2 = \frac{dx}{dt}$ （速度）とすると：

$$\frac{dx_1}{dt} = x_2$$

$$\frac{dx_2}{dt} = -\frac{k}{m}x_1 - \frac{c}{m}x_2 + \frac{1}{m}f$$

行列形式：

$$\frac{d}{dt}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ -\frac{k}{m} & -\frac{c}{m} \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix} + \begin{bmatrix} 0 \\ \frac{1}{m} \end{bmatrix}f$$

$$y = \begin{bmatrix} 1 & 0 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$$

### 4.6 伝達関数との関係

状態空間表現から伝達関数を導出できます。状態方程式と出力方程式をラプラス変換すると：

$$s\mathbf{X}(s) = \mathbf{A}\mathbf{X}(s) + \mathbf{B}U(s)$$

$$Y(s) = \mathbf{C}\mathbf{X}(s) + \mathbf{D}U(s)$$

これらから $\mathbf{X}(s)$ を消去すると：

$$\mathbf{X}(s) = (s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B}U(s)$$

$$Y(s) = [\mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}]U(s)$$

したがって、伝達関数は：

$$H(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}$$

この公式により、状態空間表現と伝達関数が統一的に理解できます。

### 4.7 状態空間表現の利点

状態空間表現は、入出力表現に比べて以下の利点があります：

1. **多入力多出力 (MIMO) システム**への自然な拡張が可能
2. **時変システム**や**非線形システム**も同じ枠組みで扱える
3. システムの**可制御性 (controllability)** と**可観測性 (observability)** を解析できる
4. **最適制御**や**状態フィードバック**の設計が可能
5. 数値計算（シミュレーション）が容易

現代制御理論では、状態空間表現が標準的な表現方法となっています。

### 4.8 第6章への橋渡し

状態空間表現は、第6章「状態空間法による制御系設計」で詳しく学びます。そこでは：

- 状態フィードバックによる極配置
- 最適レギュレータ（LQR: Linear Quadratic Regulator）
- カルマンフィルタ（状態推定）
- オブザーバー設計

などの高度な制御手法を学習します。本章で学んだLTIシステムの基礎知識が、これらの発展的内容の土台となります。

---

## 5. 実システムへの応用例 (Application to Real Systems)

### 5.1 自動車のサスペンションシステム

自動車のサスペンションは、質量-ばね-ダンパ系としてモデル化できます。

- **入力**: 路面の凹凸 $r(t)$
- **出力**: 車体の変位 $z(t)$
- **目標**: 乗り心地の向上（振動の抑制）

設計パラメータ（ばね定数 $k$、減衰係数 $c$）を調整することで、応答特性を最適化します。

### 5.2 電力系統の周波数制御

発電機の回転速度（周波数）制御は、LTIシステムとしてモデル化できます。

- **入力**: 負荷変動 $\Delta P_L(t)$
- **出力**: 周波数偏差 $\Delta f(t)$
- **目標**: 周波数を定格値（50Hzまたは60Hz）に維持

伝達関数モデルに基づいて、ガバナー制御やAGC（Automatic Generation Control）を設計します。

### 5.3 航空機の姿勢制御

航空機のピッチ角制御は、2次系としてモデル化できます。

- **入力**: エレベーター角 $\delta_e(t)$
- **出力**: ピッチ角 $\theta(t)$
- **目標**: 安定性と応答性の両立

状態空間表現を用いて、ピッチ角だけでなく、ピッチ角速度やその他の状態変数も考慮した制御系を設計します。

### 5.4 プロセス制御：温度制御

化学プラントの温度制御は、典型的な1次遅れ系です。

- **入力**: 加熱量 $q(t)$
- **出力**: 温度 $T(t)$
- **時定数**: 熱容量と熱伝達係数に依存

PID制御器を設計する際、システムの伝達関数モデルが基礎となります。

---

## まとめ

この理解者向け編では、以下の発展的内容を学びました：

- **因果性**：
  - 物理的に実現可能なシステムの必須条件
  - LTIシステムでは $h(t) = 0$ for $t < 0$ と等価

- **BIBO安定性**：
  - 有界入力に対して出力も有界
  - 判定条件：$\int_{-\infty}^{\infty}|h(t)|dt < \infty$
  - 伝達関数の極がすべて左半平面にあれば安定

- **周波数応答**：
  - $H(j\omega) = H(s)|_{s=j\omega}$
  - ボード線図によるゲインと位相の可視化
  - ナイキスト線図による安定性解析

- **状態空間表現**：
  - $\frac{d\mathbf{x}}{dt} = \mathbf{A}\mathbf{x} + \mathbf{B}u$, $y = \mathbf{C}\mathbf{x} + \mathbf{D}u$
  - システムの内部状態を直接扱える
  - 伝達関数との関係：$H(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}$
  - 現代制御理論の基礎

- **実システムへの応用**：
  - サスペンション、電力系統、航空機、プロセス制御など
  - LTIモデルが実際の制御系設計の基盤

本章で学んだLTIシステムの理論は、制御工学全体の基礎となります。次章以降では、フィードバック制御系の設計、周波数領域での解析、そして状態空間法による現代制御へと発展していきます。第2章のラプラス変換、第3章のLTIシステム表現の知識を統合して、実用的な制御系を設計できるようになります。
