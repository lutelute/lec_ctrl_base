# 第15章 期末試験対策 - 中級編

## 1. 状態空間表現の応用 (Applications of State Space Representation)

### 1.1 伝達関数から状態空間表現への変換

**伝達関数 (Transfer Function)** と **状態空間表現 (State Space Representation)** は相互に変換可能です。

#### 1.1.1 可制御正準形 (Controllable Canonical Form)

伝達関数 $G(s) = \frac{Y(s)}{U(s)}$ が次のように与えられるとします：

$$G(s) = \frac{b_n s^n + b_{n-1} s^{n-1} + \cdots + b_1 s + b_0}{s^n + a_{n-1} s^{n-1} + \cdots + a_1 s + a_0}$$

可制御正準形の状態空間表現は：

$$\mathbf{A} = \begin{bmatrix}
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & 1 \\
-a_0 & -a_1 & -a_2 & \cdots & -a_{n-1}
\end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix}
0 \\ 0 \\ \vdots \\ 0 \\ 1
\end{bmatrix}$$

$$\mathbf{C} = \begin{bmatrix} b_0 & b_1 & \cdots & b_{n-1} \end{bmatrix}, \quad D = b_n$$

**例題**: 次の伝達関数を可制御正準形に変換せよ。

$$G(s) = \frac{5s + 10}{s^2 + 3s + 2}$$

**解答**:
$a_0 = 2$, $a_1 = 3$, $b_0 = 10$, $b_1 = 5$, $b_2 = 0$ より：

$$\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 10 & 5 \end{bmatrix}, \quad
D = 0$$

#### 1.1.2 可観測正準形 (Observable Canonical Form)

可観測正準形は可制御正準形の転置関係にあります：

$$\mathbf{A} = \begin{bmatrix}
0 & 0 & \cdots & 0 & -a_0 \\
1 & 0 & \cdots & 0 & -a_1 \\
0 & 1 & \cdots & 0 & -a_2 \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & \cdots & 1 & -a_{n-1}
\end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix}
b_0 \\ b_1 \\ b_2 \\ \vdots \\ b_{n-1}
\end{bmatrix}$$

$$\mathbf{C} = \begin{bmatrix} 0 & 0 & \cdots & 0 & 1 \end{bmatrix}, \quad D = b_n$$

### 1.2 可制御性と可観測性 (Controllability and Observability)

#### 1.2.1 可制御性の定義と判定

システムが **可制御 (Controllable)** であるとは、適切な入力を加えることで、任意の初期状態から任意の終端状態へ有限時間で移すことができることを意味します。

**可制御性行列 (Controllability Matrix)**:
$$\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{AB} & \mathbf{A}^2\mathbf{B} & \cdots & \mathbf{A}^{n-1}\mathbf{B} \end{bmatrix}$$

**判定条件**: $\text{rank}(\mathcal{C}) = n$ （nは状態変数の数）

**例題**: 次のシステムの可制御性を判定せよ。

$$\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$$

**解答**:
$$\mathbf{AB} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix} \begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 1 \\ -3 \end{bmatrix}$$

$$\mathcal{C} = \begin{bmatrix} 0 & 1 \\ 1 & -3 \end{bmatrix}$$

$\det(\mathcal{C}) = 0 \cdot (-3) - 1 \cdot 1 = -1 \neq 0$ より、$\text{rank}(\mathcal{C}) = 2$

**結論**: このシステムは可制御である。

#### 1.2.2 可観測性の定義と判定

システムが **可観測 (Observable)** であるとは、出力の観測から有限時間で初期状態を一意に決定できることを意味します。

**可観測性行列 (Observability Matrix)**:
$$\mathcal{O} = \begin{bmatrix} \mathbf{C} \\ \mathbf{CA} \\ \mathbf{CA}^2 \\ \vdots \\ \mathbf{CA}^{n-1} \end{bmatrix}$$

**判定条件**: $\text{rank}(\mathcal{O}) = n$

**例題**: 次のシステムの可観測性を判定せよ。

$$\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 1 & 0 \end{bmatrix}$$

**解答**:
$$\mathbf{CA} = \begin{bmatrix} 1 & 0 \end{bmatrix} \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix} = \begin{bmatrix} 0 & 1 \end{bmatrix}$$

$$\mathcal{O} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$$

$\det(\mathcal{O}) = 1$ より、$\text{rank}(\mathcal{O}) = 2$

**結論**: このシステムは可観測である。

### 1.3 状態空間表現から伝達関数への変換

状態空間表現から伝達関数への変換は次の公式で行います：

$$G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D$$

ここで、$\mathbf{I}$ は単位行列です。

**例題**: 次の状態空間表現を伝達関数に変換せよ。

$$\mathbf{A} = \begin{bmatrix} -1 & 0 \\ 0 & -2 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 1 & 2 \end{bmatrix}, \quad D = 0$$

**解答**:
$$s\mathbf{I} - \mathbf{A} = \begin{bmatrix} s+1 & 0 \\ 0 & s+2 \end{bmatrix}$$

$$(s\mathbf{I} - \mathbf{A})^{-1} = \begin{bmatrix} \frac{1}{s+1} & 0 \\ 0 & \frac{1}{s+2} \end{bmatrix}$$

$$(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} = \begin{bmatrix} \frac{1}{s+1} \\ \frac{1}{s+2} \end{bmatrix}$$

$$G(s) = \begin{bmatrix} 1 & 2 \end{bmatrix} \begin{bmatrix} \frac{1}{s+1} \\ \frac{1}{s+2} \end{bmatrix} = \frac{1}{s+1} + \frac{2}{s+2}$$

$$G(s) = \frac{(s+2) + 2(s+1)}{(s+1)(s+2)} = \frac{3s + 4}{s^2 + 3s + 2}$$

---

## 2. 安定性解析の手法 (Methods of Stability Analysis)

### 2.1 ラウス・フルビッツの判別法 (Routh-Hurwitz Criterion)

**ラウス・フルビッツの判別法**は、特性方程式の根を直接求めずに安定性を判定できる強力な手法です。

#### 2.1.1 ラウス表の作成手順

特性方程式が次のように与えられたとします：
$$a_n s^n + a_{n-1} s^{n-1} + \cdots + a_1 s + a_0 = 0$$

**ラウス表 (Routh Array)** を次のように作成します：

| 行 | $s^n$ | $s^{n-1}$ | $s^{n-2}$ | ... |
|---|-------|-----------|-----------|-----|
| $s^n$ | $a_n$ | $a_{n-2}$ | $a_{n-4}$ | ... |
| $s^{n-1}$ | $a_{n-1}$ | $a_{n-3}$ | $a_{n-5}$ | ... |
| $s^{n-2}$ | $b_1$ | $b_2$ | $b_3$ | ... |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ | ... |

ここで、
$$b_1 = \frac{a_{n-1}a_{n-2} - a_n a_{n-3}}{a_{n-1}}, \quad
b_2 = \frac{a_{n-1}a_{n-4} - a_n a_{n-5}}{a_{n-1}}$$

**安定条件**: 第1列のすべての要素が正であれば安定

#### 2.1.2 例題：3次系の安定性判定

**例題**: 次の特性方程式を持つシステムの安定性を判定せよ。
$$s^3 + 6s^2 + 11s + 6 = 0$$

**解答**:
ラウス表を作成します：

$$b_1 = \frac{6 \cdot 11 - 1 \cdot 6}{6} = \frac{66 - 6}{6} = 10$$

$$c_1 = \frac{10 \cdot 6 - 6 \cdot 0}{10} = 6$$

| 行 | 列1 | 列2 |
|---|-----|-----|
| $s^3$ | 1 | 11 |
| $s^2$ | 6 | 6 |
| $s^1$ | 10 | 0 |
| $s^0$ | 6 | - |

第1列の要素：1, 6, 10, 6 → すべて正

**結論**: システムは安定である。

#### 2.1.3 特殊ケース：第1列に0が現れる場合

**ケース1**: 第1列に0が現れた場合
- 小さな正の数 $\epsilon$ で置き換えて計算を続ける

**ケース2**: ある行がすべて0になる場合
- 補助方程式 (auxiliary equation) を作成して解析

### 2.2 根軌跡法の基礎 (Fundamentals of Root Locus Method)

#### 2.2.1 根軌跡とは

**根軌跡 (Root Locus)** は、フィードバックゲインを変化させたときの閉ループ極の軌跡を複素平面上に描いたものです。

閉ループ特性方程式:
$$1 + KG(s)H(s) = 0$$

または
$$1 + K\frac{N(s)}{D(s)} = 0$$

ここで、$K \geq 0$ はフィードバックゲイン、$N(s)/D(s)$ は開ループ伝達関数です。

#### 2.2.2 根軌跡の作図規則

**基本規則**:

1. **分岐数**: $n = \max(\text{deg}(D), \text{deg}(N))$ 本の軌跡
2. **始点**: $K=0$ での開ループ極（$D(s)=0$ の根）
3. **終点**: $K=\infty$ での開ループ零点（$N(s)=0$ の根）、または無限遠点
4. **実軸上の軌跡**: 実軸上で右側の極と零点の合計数が奇数の部分
5. **漸近線**: 極の数が零点の数より多い場合、$n-m$ 本の漸近線が無限遠点へ

**漸近線の角度**:
$$\theta_k = \frac{(2k+1)\pi}{n-m}, \quad k = 0, 1, 2, \ldots, n-m-1$$

**漸近線の交点**:
$$\sigma_a = \frac{\sum \text{極} - \sum \text{零点}}{n - m}$$

#### 2.2.3 例題：2次系の根軌跡

**例題**: 次の開ループ伝達関数の根軌跡を描け。
$$G(s) = \frac{K}{s(s+2)}$$

**解答**:
- 極: $s = 0, -2$（2個）
- 零点: なし（0個）
- 分岐数: 2本
- 始点: $s = 0, -2$
- 終点: 無限遠点（2本の漸近線）

漸近線:
$$\theta_0 = \frac{\pi}{2} = 90°, \quad \theta_1 = \frac{3\pi}{2} = 270°$$

$$\sigma_a = \frac{(0 + (-2)) - 0}{2 - 0} = -1$$

実軸上の軌跡: $-2 \leq s \leq 0$（右側の極の数が奇数）

**分離点 (Breakaway Point)** の計算:
$$\frac{d}{ds}\left(\frac{s(s+2)}{1}\right) = 2s + 2 = 0 \Rightarrow s = -1$$

根軌跡は $s=-1$ で実軸を離れ、虚軸方向へ向かいます。

#### 2.2.4 安定余裕の評価

根軌跡から、システムが安定である $K$ の範囲を求めることができます：

**安定条件**: すべての閉ループ極が左半平面にある

虚軸との交点を求めることで、**臨界ゲイン (Critical Gain)** $K_{cr}$ を決定できます。

### 2.3 状態空間表現での安定性

状態空間表現 $\dot{\mathbf{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{u}$ の安定性は、**システム行列 $\mathbf{A}$ の固有値**で決まります。

**安定条件**: $\mathbf{A}$ のすべての固有値の実部が負

**固有値の計算**:
$$\det(s\mathbf{I} - \mathbf{A}) = 0$$

**例題**: 次のシステム行列の固有値を求め、安定性を判定せよ。

$$\mathbf{A} = \begin{bmatrix} -1 & 2 \\ -2 & -3 \end{bmatrix}$$

**解答**:
$$\det(s\mathbf{I} - \mathbf{A}) = \det\begin{bmatrix} s+1 & -2 \\ 2 & s+3 \end{bmatrix}$$
$$= (s+1)(s+3) - (-2)(2) = s^2 + 4s + 3 + 4 = s^2 + 4s + 7$$

$$s = \frac{-4 \pm \sqrt{16 - 28}}{2} = \frac{-4 \pm \sqrt{-12}}{2} = -2 \pm j\sqrt{3}$$

固有値: $s_1 = -2 + j\sqrt{3}$, $s_2 = -2 - j\sqrt{3}$

両方の固有値の実部が $-2 < 0$ より、**システムは安定**。

---

## 3. 周波数応答の詳細解析 (Detailed Analysis of Frequency Response)

### 3.1 ゲイン余裕と位相余裕の計算

#### 3.1.1 ゲイン余裕 (Gain Margin)

**ゲイン余裕 $GM$** は、位相が$-180°$となる周波数（位相交差周波数 $\omega_{pc}$）でのゲインの逆数です。

$$GM = \frac{1}{|G(j\omega_{pc})|}, \quad \text{where} \quad \angle G(j\omega_{pc}) = -180°$$

dB表示:
$$GM_{dB} = -20\log_{10}|G(j\omega_{pc})|$$

**例題**: 次の伝達関数のゲイン余裕を求めよ。
$$G(s) = \frac{10}{s(s+1)(s+5)}$$

**解答**:
まず、位相が$-180°$となる周波数を求めます。

$$G(j\omega) = \frac{10}{j\omega(j\omega+1)(j\omega+5)}$$

位相:
$$\angle G(j\omega) = -90° - \tan^{-1}\omega - \tan^{-1}\frac{\omega}{5}$$

$\angle G(j\omega_{pc}) = -180°$ より:
$$-90° - \tan^{-1}\omega_{pc} - \tan^{-1}\frac{\omega_{pc}}{5} = -180°$$
$$\tan^{-1}\omega_{pc} + \tan^{-1}\frac{\omega_{pc}}{5} = 90°$$

これより $\omega_{pc} \approx 2.236$ rad/s

ゲイン:
$$|G(j\omega_{pc})| = \frac{10}{\omega_{pc}\sqrt{1+\omega_{pc}^2}\sqrt{1+(\omega_{pc}/5)^2}} \approx 0.894$$

$$GM_{dB} = -20\log_{10}(0.894) \approx 0.97 \text{ dB}$$

#### 3.1.2 位相余裕 (Phase Margin)

**位相余裕 $PM$** は、ゲインが1（0dB）となる周波数（ゲイン交差周波数 $\omega_{gc}$）での位相の$-180°$からの余裕です。

$$PM = 180° + \angle G(j\omega_{gc}), \quad \text{where} \quad |G(j\omega_{gc})| = 1$$

**例題**: 次の伝達関数の位相余裕を求めよ。
$$G(s) = \frac{5}{s(s+1)}$$

**解答**:
$$G(j\omega) = \frac{5}{j\omega(j\omega+1)} = \frac{5(-j\omega)(1-j\omega)}{\omega^2(1+\omega^2)}$$

$$|G(j\omega)| = \frac{5}{\omega\sqrt{1+\omega^2}}$$

$|G(j\omega_{gc})| = 1$ より:
$$\frac{5}{\omega_{gc}\sqrt{1+\omega_{gc}^2}} = 1$$
$$25 = \omega_{gc}^2(1+\omega_{gc}^2)$$
$$\omega_{gc}^4 + \omega_{gc}^2 - 25 = 0$$

$x = \omega_{gc}^2$ とおくと: $x^2 + x - 25 = 0$
$$x = \frac{-1 + \sqrt{1 + 100}}{2} \approx 4.525$$
$$\omega_{gc} \approx 2.127 \text{ rad/s}$$

位相:
$$\angle G(j\omega_{gc}) = -90° - \tan^{-1}\omega_{gc} \approx -90° - 64.8° = -154.8°$$

$$PM = 180° + (-154.8°) = 25.2°$$

### 3.2 共振特性 (Resonance Characteristics)

#### 3.2.1 共振ピーク値

2次系の標準形:
$$G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

周波数応答:
$$|G(j\omega)| = \frac{\omega_n^2}{\sqrt{(\omega_n^2-\omega^2)^2 + (2\zeta\omega_n\omega)^2}}$$

**共振周波数 (Resonance Frequency)**:
$$\omega_r = \omega_n\sqrt{1 - 2\zeta^2} \quad (0 < \zeta < \frac{1}{\sqrt{2}})$$

**共振ピーク値 (Resonance Peak)**:
$$M_r = \frac{1}{2\zeta\sqrt{1-\zeta^2}} \quad (0 < \zeta < 1)$$

**例題**: $\omega_n = 10$ rad/s, $\zeta = 0.3$ の2次系の共振周波数と共振ピーク値を求めよ。

**解答**:
$$\omega_r = 10\sqrt{1 - 2(0.3)^2} = 10\sqrt{1 - 0.18} = 10\sqrt{0.82} \approx 9.06 \text{ rad/s}$$

$$M_r = \frac{1}{2(0.3)\sqrt{1-0.3^2}} = \frac{1}{0.6\sqrt{0.91}} \approx 1.74$$

dB表示:
$$M_{r,dB} = 20\log_{10}(1.74) \approx 4.81 \text{ dB}$$

#### 3.2.2 帯域幅 (Bandwidth)

**帯域幅 $\omega_B$** は、ゲインが低周波ゲインから$-3$dB減少する周波数です。

2次系の場合:
$$\omega_B = \omega_n\sqrt{(1-2\zeta^2) + \sqrt{4\zeta^4 - 4\zeta^2 + 2}}$$

**意味**: 帯域幅が大きいほど、システムの応答速度が速い。

### 3.3 ボード線図の作図技法

#### 3.3.1 基本要素の組み合わせ

複雑な伝達関数は基本要素の積として表現できます：

$$G(s) = K \cdot \frac{1}{s^p} \cdot \prod_i \frac{1}{T_i s + 1} \cdot \prod_j (T_j s + 1) \cdot \prod_k \frac{1}{\frac{s^2}{\omega_{nk}^2} + \frac{2\zeta_k}{\omega_{nk}}s + 1}$$

**作図手順**:
1. 各要素のボード線図を描く
2. 縦軸方向に加算（積は加算になる）

#### 3.3.2 実例：3次系のボード線図

**例題**: 次の伝達関数のボード線図を描け。
$$G(s) = \frac{100}{s(s+1)(s+10)}$$

**解答**:
まず、標準形に変形：
$$G(s) = \frac{10}{s(s+1)(0.1s+1)}$$

**各要素**:
1. $K = 10$ → $20\log_{10}10 = 20$ dB
2. $\frac{1}{s}$ → -20dB/decade, 位相-90°
3. $\frac{1}{s+1}$ → 折点周波数 $\omega=1$ rad/s
4. $\frac{1}{0.1s+1}$ → 折点周波数 $\omega=10$ rad/s

**低周波漸近線**($\omega \ll 1$):
$$|G(j\omega)| \approx \frac{10}{\omega} \rightarrow 20 - 20\log_{10}\omega \text{ dB}$$

**高周波漸近線**($\omega \gg 10$):
傾き = $-20 - 20 - 20 = -60$ dB/decade

---

## 4. フィードバック系の設計技法 (Feedback System Design Techniques)

### 4.1 PID制御の詳細設計

#### 4.1.1 各要素の伝達関数表現

**比例 (P) 制御**:
$$C(s) = K_p$$

- 効果: 応答速度向上、定常偏差低減
- 問題点: 定常偏差が残る、オーバーシュート増加

**積分 (I) 制御**:
$$C(s) = \frac{K_i}{s}$$

- 効果: 定常偏差を完全に除去
- 問題点: 応答が遅くなる、振動的になる

**微分 (D) 制御**:
$$C(s) = K_d s$$

- 効果: 過渡応答改善、オーバーシュート抑制
- 問題点: ノイズに敏感、実現は近似的

**PID制御**:
$$C(s) = K_p + \frac{K_i}{s} + K_d s = K_p\left(1 + \frac{1}{T_i s} + T_d s\right)$$

ここで、$T_i = K_p/K_i$ は**積分時間**、$T_d = K_d/K_p$ は**微分時間**です。

#### 4.1.2 ジーグラー・ニコルス法 (Ziegler-Nichols Method)

**第1法（ステップ応答法）**:

プラントのステップ応答から次のパラメータを測定：
- $L$: むだ時間 (dead time)
- $T$: 時定数
- $a = K_p T/L$

| 制御器 | $K_p$ | $T_i$ | $T_d$ |
|--------|-------|-------|-------|
| P | $T/L$ | - | - |
| PI | $0.9T/L$ | $L/0.3$ | - |
| PID | $1.2T/L$ | $2L$ | $0.5L$ |

**第2法（限界感度法）**:

P制御のみでゲインを増加させ、持続振動が発生する限界ゲイン $K_u$ と周期 $T_u$ を測定：

| 制御器 | $K_p$ | $T_i$ | $T_d$ |
|--------|-------|-------|-------|
| P | $0.5K_u$ | - | - |
| PI | $0.45K_u$ | $T_u/1.2$ | - |
| PID | $0.6K_u$ | $T_u/2$ | $T_u/8$ |

#### 4.1.3 PID制御の実例

**例題**: 次のプラントに対してPI制御器を設計せよ（限界感度法）。
$$G(s) = \frac{1}{s(s+1)(s+2)}$$

**解答**:
P制御のみの閉ループ特性方程式：
$$1 + K_u G(s) = 0$$
$$s(s+1)(s+2) + K_u = 0$$
$$s^3 + 3s^2 + 2s + K_u = 0$$

ラウス表を作成して、$s^1$ 行が0になる条件を求める：

| 行 | 列1 | 列2 |
|---|-----|-----|
| $s^3$ | 1 | 2 |
| $s^2$ | 3 | $K_u$ |
| $s^1$ | $\frac{6-K_u}{3}$ | 0 |
| $s^0$ | $K_u$ | - |

$s^1$ 行が0になるとき: $6 - K_u = 0$ → $K_u = 6$

補助方程式: $3s^2 + K_u = 3s^2 + 6 = 0$ → $s = \pm j\sqrt{2}$

持続振動の周波数: $\omega_u = \sqrt{2}$ rad/s
周期: $T_u = 2\pi/\omega_u = 2\pi/\sqrt{2} \approx 4.44$ s

ジーグラー・ニコルス第2法より：
$$K_p = 0.45K_u = 0.45 \times 6 = 2.7$$
$$T_i = \frac{T_u}{1.2} = \frac{4.44}{1.2} = 3.7 \text{ s}$$
$$K_i = \frac{K_p}{T_i} = \frac{2.7}{3.7} \approx 0.73$$

**PI制御器**: $C(s) = 2.7 + \frac{0.73}{s}$

### 4.2 ゲイン調整による性能改善

#### 4.2.1 開ループゲインの影響

開ループ伝達関数 $G(s)H(s)$ のゲイン $K$ を変化させたときの影響：

**ゲイン増加の効果**:
- 定常偏差の低減
- 応答速度の向上
- 外乱抑制性能の向上

**ゲイン増加の問題点**:
- オーバーシュートの増加
- 安定余裕の減少
- 振動的になる

#### 4.2.2 最適ゲインの決定

**設計要求の例**:
- オーバーシュート < 20%
- 整定時間 < 5s
- 定常偏差 < 5%
- ゲイン余裕 > 6dB
- 位相余裕 > 30°

これらの要求を満たす $K$ の範囲を求め、その中から最適値を選択します。

**例題**: 次の1型系において、定常偏差 < 10% を満たす最小ゲインを求めよ。
$$G(s) = \frac{K}{s(s+5)}$$

**解答**:
単位ランプ入力 $r(t) = t$ に対する定常偏差：
$$e_{ss} = \frac{1}{K_v}$$

ここで、$K_v$ は**速度定数 (Velocity Constant)**：
$$K_v = \lim_{s \to 0} sG(s) = \lim_{s \to 0} s \cdot \frac{K}{s(s+5)} = \frac{K}{5}$$

$e_{ss} < 0.1$ より：
$$\frac{1}{K_v} < 0.1 \Rightarrow K_v > 10 \Rightarrow \frac{K}{5} > 10 \Rightarrow K > 50$$

**結論**: $K > 50$ が必要。

### 4.3 定常偏差の型別解析

#### 4.3.1 システムの型 (System Type)

開ループ伝達関数を次の形に表します：
$$G(s)H(s) = \frac{K \prod(s-z_i)}{s^N \prod(s-p_j)}$$

$N$ を**システムの型**といいます。

#### 4.3.2 定常偏差の公式

| 入力 | 0型 | 1型 | 2型 |
|------|-----|-----|-----|
| ステップ $u(t)$ | $\frac{1}{1+K_p}$ | 0 | 0 |
| ランプ $tu(t)$ | $\infty$ | $\frac{1}{K_v}$ | 0 |
| 放物線 $\frac{t^2}{2}u(t)$ | $\infty$ | $\infty$ | $\frac{1}{K_a}$ |

ここで：
- $K_p = \lim_{s \to 0} G(s)H(s)$ : 位置定数
- $K_v = \lim_{s \to 0} sG(s)H(s)$ : 速度定数
- $K_a = \lim_{s \to 0} s^2G(s)H(s)$ : 加速度定数

---

## 5. 応用問題の解法 (Problem-Solving for Applied Problems)

### 5.1 総合問題の解き方

#### 5.1.1 問題解決のフレームワーク

**Step 1: 問題の分類**
- 安定性問題、周波数応答問題、設計問題のいずれか
- 時間領域か周波数領域か

**Step 2: 与えられた情報の整理**
- 伝達関数、パラメータ
- 設計仕様、制約条件

**Step 3: 適用手法の選択**
- ラウス・フルビッツ、根軌跡、ボード線図など
- 状態空間法か伝達関数法か

**Step 4: 段階的な計算**
- 中間結果を明記
- 単位・次元の確認

**Step 5: 結果の検証と考察**
- 物理的妥当性の確認
- 設計仕様の充足確認

### 5.2 計算を伴う例題

#### 例題1: 安定性とゲイン調整

**問題**: 次のフィードバック系において、システムが安定となるゲイン $K$ の範囲を求めよ。
$$G(s) = \frac{K}{s^3 + 6s^2 + 11s + 6}$$

**解答**:

閉ループ特性方程式:
$$s^3 + 6s^2 + 11s + 6 + K = 0$$

ラウス表:

| 行 | 列1 | 列2 |
|---|-----|-----|
| $s^3$ | 1 | 11 |
| $s^2$ | 6 | $6+K$ |
| $s^1$ | $\frac{66-6-K}{6} = \frac{60-K}{6}$ | 0 |
| $s^0$ | $6+K$ | - |

安定条件:
1. $6 + K > 0$ → $K > -6$
2. $\frac{60-K}{6} > 0$ → $K < 60$
3. すべての係数が正

**結論**: $-6 < K < 60$ で安定（実際には $K > 0$ が現実的）

#### 例題2: 位相余裕からのゲイン決定

**問題**: 次の系において、位相余裕が45°となるゲイン $K$ を求めよ。
$$G(s) = \frac{K}{s(s+1)(s+5)}$$

**解答**:

位相余裕 $PM = 45°$ より、ゲイン交差周波数 $\omega_{gc}$ での位相：
$$\angle G(j\omega_{gc}) = -135°$$

$$\angle G(j\omega) = -90° - \tan^{-1}\omega - \tan^{-1}\frac{\omega}{5}$$

$-90° - \tan^{-1}\omega_{gc} - \tan^{-1}\frac{\omega_{gc}}{5} = -135°$ より：
$$\tan^{-1}\omega_{gc} + \tan^{-1}\frac{\omega_{gc}}{5} = 45°$$

数値計算により: $\omega_{gc} \approx 1.382$ rad/s

ゲイン交差周波数で $|G(j\omega_{gc})| = 1$ より：
$$\frac{K}{\omega_{gc}\sqrt{1+\omega_{gc}^2}\sqrt{1+(\omega_{gc}/5)^2}} = 1$$

$$K = \omega_{gc}\sqrt{1+\omega_{gc}^2}\sqrt{1+(\omega_{gc}/5)^2}$$
$$K \approx 1.382 \times \sqrt{1+1.910} \times \sqrt{1+0.076} \approx 1.382 \times 1.706 \times 1.037 \approx 2.45$$

**結論**: $K \approx 2.45$

#### 例題3: 状態フィードバックによる極配置

**問題**: 次のシステムに状態フィードバック $\mathbf{u} = -\mathbf{Kx} + \mathbf{r}$ を施し、閉ループ極を $s = -2 \pm j2$ に配置せよ。

$$\dot{\mathbf{x}} = \begin{bmatrix} 0 & 1 \\ -1 & -2 \end{bmatrix}\mathbf{x} + \begin{bmatrix} 0 \\ 1 \end{bmatrix}\mathbf{u}$$

**解答**:

状態フィードバック後のシステム行列：
$$\mathbf{A}_{cl} = \mathbf{A} - \mathbf{BK} = \begin{bmatrix} 0 & 1 \\ -1 & -2 \end{bmatrix} - \begin{bmatrix} 0 \\ 1 \end{bmatrix}\begin{bmatrix} k_1 & k_2 \end{bmatrix}$$

$$= \begin{bmatrix} 0 & 1 \\ -1-k_1 & -2-k_2 \end{bmatrix}$$

特性方程式:
$$\det(s\mathbf{I} - \mathbf{A}_{cl}) = s^2 + (2+k_2)s + (1+k_1) = 0$$

目標の特性方程式（極 $s = -2 \pm j2$）:
$$(s+2-j2)(s+2+j2) = (s+2)^2 + 4 = s^2 + 4s + 8 = 0$$

係数比較:
$$2 + k_2 = 4 \Rightarrow k_2 = 2$$
$$1 + k_1 = 8 \Rightarrow k_1 = 7$$

**結論**: $\mathbf{K} = \begin{bmatrix} 7 & 2 \end{bmatrix}$

### 5.3 実践的な設計問題

#### 例題: サーボモータの速度制御系設計

**問題**: DCサーボモータの速度制御系を設計する。モータの伝達関数は次の通り：
$$G(s) = \frac{\Omega(s)}{V(s)} = \frac{1}{s(s+10)}$$

要求仕様:
- オーバーシュート < 10%
- 整定時間 < 1s
- 定常偏差 = 0

**解答**:

**Step 1**: オーバーシュート < 10% より、減衰比 $\zeta$ を決定
$$\zeta \geq 0.6$（オーバーシュート10%に対応）

**Step 2**: 整定時間 < 1s より、固有周波数 $\omega_n$ を決定
$$t_s = \frac{4}{\zeta\omega_n} < 1 \Rightarrow \omega_n > \frac{4}{\zeta} \geq \frac{4}{0.6} = 6.67 \text{ rad/s}$$

**Step 3**: 目標の閉ループ伝達関数
$$G_{cl}(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

$\zeta = 0.6$, $\omega_n = 7$ rad/s を選択：
$$G_{cl}(s) = \frac{49}{s^2 + 8.4s + 49}$$

**Step 4**: 必要なコントローラの決定
$$\frac{C(s)G(s)}{1+C(s)G(s)} = G_{cl}(s)$$

$$C(s) = \frac{G_{cl}(s)}{G(s)(1-G_{cl}(s))}$$

計算により:
$$C(s) = K_p + \frac{K_i}{s} = 8.4s + 49 - 10 = 8.4s + 39$$

実用的なPI制御器として:
$$C(s) = 8.4\left(s + \frac{39}{8.4}\right) = 8.4(s + 4.64)$$

**結論**: PI制御器 $C(s) = 8.4 + \frac{39}{s}$ を使用。

---

## まとめ (Summary)

この中級編では、以下の内容を詳しく学習しました：

**状態空間表現**:
- 伝達関数と状態空間表現の相互変換（可制御正準形、可観測正準形）
- 可制御性・可観測性の判定方法
- 状態空間表現の実用的応用

**安定性解析**:
- ラウス・フルビッツの判別法による安定性判定
- 根軌跡法の基礎と作図規則
- 状態空間表現での固有値による安定性判定

**周波数応答**:
- ゲイン余裕・位相余裕の詳細な計算方法
- 共振特性と帯域幅の解析
- ボード線図の実践的作図技法

**フィードバック系設計**:
- PID制御の詳細設計（ジーグラー・ニコルス法）
- ゲイン調整による性能改善
- 定常偏差の型別解析

**応用問題**:
- 総合的な問題解決のフレームワーク
- 計算を伴う実践的な例題
- 実システムの設計問題

次の理解者編では、より発展的な内容（カルマン分解、リアプノフ安定性、最適制御など）と複合的な考察問題に取り組みます。
