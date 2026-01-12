# 第15章 期末試験対策 - 理解者向け技術ノート

## 1. 状態空間表現の高度な理論 (Advanced Theory of State Space Representation)

### 1.1 カルマン分解 (Kalman Decomposition)

**カルマン分解 (Kalman Decomposition)** は、システムを可制御性・可観測性の観点から4つの部分空間に分解する理論です。

システム $(\mathbf{A}, \mathbf{B}, \mathbf{C})$ は次のように分解されます：

$$\bar{\mathbf{x}} = \begin{bmatrix} \mathbf{x}_{\bar{c}\bar{o}} \\ \mathbf{x}_{\bar{c}o} \\ \mathbf{x}_{c\bar{o}} \\ \mathbf{x}_{co} \end{bmatrix}$$

ここで：
- $\mathbf{x}_{\bar{c}\bar{o}}$ : 可制御でも可観測でもない部分
- $\mathbf{x}_{\bar{c}o}$ : 可制御でないが可観測な部分
- $\mathbf{x}_{c\bar{o}}$ : 可制御だが可観測でない部分
- $\mathbf{x}_{co}$ : 可制御かつ可観測な部分

**座標変換後のシステム行列**:

$$\bar{\mathbf{A}} = \begin{bmatrix}
\mathbf{A}_{\bar{c}\bar{o}} & 0 & \mathbf{A}_{13} & 0 \\
0 & \mathbf{A}_{\bar{c}o} & 0 & \mathbf{A}_{24} \\
0 & 0 & \mathbf{A}_{c\bar{o}} & 0 \\
0 & 0 & 0 & \mathbf{A}_{co}
\end{bmatrix}$$

$$\bar{\mathbf{B}} = \begin{bmatrix} 0 \\ 0 \\ \mathbf{B}_{c\bar{o}} \\ \mathbf{B}_{co} \end{bmatrix}, \quad
\bar{\mathbf{C}} = \begin{bmatrix} 0 & \mathbf{C}_{\bar{c}o} & 0 & \mathbf{C}_{co} \end{bmatrix}$$

**重要な性質**:
- 伝達関数には可制御かつ可観測な部分 $(\mathbf{A}_{co}, \mathbf{B}_{co}, \mathbf{C}_{co})$ のみが現れる
- 可制御でも可観測でもない部分は、入出力特性に全く影響しない

### 1.2 最小実現 (Minimal Realization)

**最小実現 (Minimal Realization)** は、与えられた伝達関数を実現する状態空間表現のうち、状態変数の次元が最小のものです。

**定理**: 伝達関数 $G(s)$ の最小実現の次数は、$G(s)$ を既約分数（分子と分母が互いに素）で表したときの分母の次数に等しい。

**例題**: 次の伝達関数の最小実現を求めよ。

$$G(s) = \frac{s+3}{s^2 + 5s + 6} = \frac{s+3}{(s+2)(s+3)}$$

**解答**:

まず約分します：
$$G(s) = \frac{1}{s+2}$$

これは1次系であり、最小実現は：
$$\dot{x} = -2x + u, \quad y = x$$

すなわち、$A = -2$, $B = 1$, $C = 1$, $D = 0$

**検証**: 可制御性・可観測性ともに満たされる（スカラ系なので自明）。

### 1.3 状態フィードバックと極配置 (State Feedback and Pole Placement)

#### 1.3.1 状態フィードバック制御

**状態フィードバック (State Feedback)** は、すべての状態変数を測定し、それらの線形結合を制御入力とする手法です。

$$\mathbf{u}(t) = -\mathbf{K}\mathbf{x}(t) + \mathbf{r}(t)$$

ここで、$\mathbf{K}$ は**フィードバックゲイン行列**、$\mathbf{r}(t)$ は参照入力です。

**閉ループ系**:
$$\dot{\mathbf{x}} = (\mathbf{A} - \mathbf{BK})\mathbf{x} + \mathbf{B}\mathbf{r}$$

閉ループシステム行列 $\mathbf{A}_{cl} = \mathbf{A} - \mathbf{BK}$ の固有値が閉ループ極となります。

#### 1.3.2 極配置定理

**定理 (Pole Placement Theorem)**: システムが可制御であれば、状態フィードバックにより閉ループ極を任意の位置に配置できる。

**アッカーマンの公式 (Ackermann's Formula)** (単入力系の場合):

$$\mathbf{K} = \begin{bmatrix} 0 & 0 & \cdots & 0 & 1 \end{bmatrix} \mathcal{C}^{-1} \alpha_c(\mathbf{A})$$

ここで：
- $\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{AB} & \cdots & \mathbf{A}^{n-1}\mathbf{B} \end{bmatrix}$ は可制御性行列
- $\alpha_c(\mathbf{A})$ は目標特性多項式 $\alpha_c(s) = s^n + \alpha_{n-1}s^{n-1} + \cdots + \alpha_1 s + \alpha_0$ に $\mathbf{A}$ を代入したもの

#### 1.3.3 例題: 3次系の極配置

**問題**: 次のシステムに状態フィードバックを施し、閉ループ極を $s = -5, -3 \pm j4$ に配置せよ。

$$\mathbf{A} = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ -6 & -11 & -6 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}$$

**解答**:

可制御性行列を計算：
$$\mathbf{AB} = \begin{bmatrix} 0 \\ 1 \\ -6 \end{bmatrix}, \quad
\mathbf{A}^2\mathbf{B} = \begin{bmatrix} 1 \\ -6 \\ 25 \end{bmatrix}$$

$$\mathcal{C} = \begin{bmatrix} 0 & 0 & 1 \\ 0 & 1 & -6 \\ 1 & -6 & 25 \end{bmatrix}$$

$\det(\mathcal{C}) = 1 \neq 0$ より可制御です。

目標特性方程式（極 $s = -5, -3 \pm j4$）:
$$(s+5)(s+3-j4)(s+3+j4) = (s+5)(s^2+6s+25)$$
$$= s^3 + 11s^2 + 55s + 125 = 0$$

したがって、$\alpha_c(s) = s^3 + 11s^2 + 55s + 125$

元の特性方程式: $\det(s\mathbf{I} - \mathbf{A}) = s^3 + 6s^2 + 11s + 6$

バス変換または直接計算により：
$$\mathbf{K} = \begin{bmatrix} 119 & 44 & 5 \end{bmatrix}$$

### 1.4 オブザーバ設計 (Observer Design)

実際の制御系では、すべての状態変数を直接測定できるとは限りません。**オブザーバ (Observer)** または **推定器 (Estimator)** は、出力の測定値からすべての状態変数を推定します。

#### 1.4.1 全次元オブザーバ

**全次元オブザーバ (Full-Order Observer)** の動特性：

$$\dot{\hat{\mathbf{x}}} = \mathbf{A}\hat{\mathbf{x}} + \mathbf{B}\mathbf{u} + \mathbf{L}(\mathbf{y} - \hat{\mathbf{y}})$$
$$\hat{\mathbf{y}} = \mathbf{C}\hat{\mathbf{x}}$$

ここで、$\hat{\mathbf{x}}$ は推定状態、$\mathbf{L}$ は**オブザーバゲイン行列**です。

**推定誤差**:
$$\mathbf{e} = \mathbf{x} - \hat{\mathbf{x}}$$

推定誤差の動特性：
$$\dot{\mathbf{e}} = (\mathbf{A} - \mathbf{LC})\mathbf{e}$$

**設計指針**: オブザーバゲイン $\mathbf{L}$ を選び、$\mathbf{A} - \mathbf{LC}$ の固有値（オブザーバ極）を適切に配置します。

**双対性 (Duality)**: システムが可観測であれば、$\mathbf{L}$ を選んでオブザーバ極を任意の位置に配置できます。これは極配置問題と双対です：
- 極配置: $(\mathbf{A}, \mathbf{B})$ と $\mathbf{K}$ → $\mathbf{A} - \mathbf{BK}$
- オブザーバ: $(\mathbf{A}^T, \mathbf{C}^T)$ と $\mathbf{L}^T$ → $(\mathbf{A} - \mathbf{LC})^T = \mathbf{A}^T - \mathbf{C}^T\mathbf{L}^T$

#### 1.4.2 分離定理 (Separation Principle)

**分離定理**: 状態フィードバック制御器とオブザーバは独立に設計できます。

制御則 $\mathbf{u} = -\mathbf{K}\hat{\mathbf{x}} + \mathbf{r}$ を用いたときの閉ループ系の極は：
- $\mathbf{A} - \mathbf{BK}$ の固有値（制御極）
- $\mathbf{A} - \mathbf{LC}$ の固有値（オブザーバ極）

の和集合となります。

**設計指針**: オブザーバ極は制御極より3〜5倍速く（実部の絶対値が大きく）配置するのが一般的です。

---

## 2. 安定性解析の高度な手法 (Advanced Methods for Stability Analysis)

### 2.1 リアプノフ安定性理論 (Lyapunov Stability Theory)

**リアプノフの安定性理論**は、線形・非線形を問わず適用できる強力な安定性解析手法です。

#### 2.1.1 リアプノフの意味での安定性

平衡点 $\mathbf{x}_e$ に対する安定性の定義：

**安定 (Stable)**: 任意の $\epsilon > 0$ に対して、ある $\delta > 0$ が存在し、$\|\mathbf{x}(0) - \mathbf{x}_e\| < \delta$ ならば、すべての $t \geq 0$ で $\|\mathbf{x}(t) - \mathbf{x}_e\| < \epsilon$

**漸近安定 (Asymptotically Stable)**: 安定かつ、$\lim_{t \to \infty} \mathbf{x}(t) = \mathbf{x}_e$

#### 2.1.2 リアプノフ関数

**リアプノフ関数 (Lyapunov Function)** $V(\mathbf{x})$ は、エネルギー関数に類似した正定値関数です。

**定理 (リアプノフの直接法)**:

$V(\mathbf{x})$ が次の条件を満たすとき、平衡点 $\mathbf{x}_e = \mathbf{0}$ は漸近安定である：
1. $V(\mathbf{0}) = 0$ かつ $V(\mathbf{x}) > 0$ for $\mathbf{x} \neq \mathbf{0}$ （正定値）
2. $\dot{V}(\mathbf{x}) < 0$ for $\mathbf{x} \neq \mathbf{0}$ （負定値）

ここで、$\dot{V}(\mathbf{x}) = \frac{\partial V}{\partial \mathbf{x}} \cdot \dot{\mathbf{x}}$ は $V$ の時間微分です。

#### 2.1.3 線形系のリアプノフ方程式

線形系 $\dot{\mathbf{x}} = \mathbf{A}\mathbf{x}$ に対して、リアプノフ関数を $V(\mathbf{x}) = \mathbf{x}^T\mathbf{P}\mathbf{x}$ の形に選びます（$\mathbf{P}$ は正定値対称行列）。

$$\dot{V}(\mathbf{x}) = \mathbf{x}^T(\mathbf{A}^T\mathbf{P} + \mathbf{P}\mathbf{A})\mathbf{x}$$

**リアプノフ方程式 (Lyapunov Equation)**:
$$\mathbf{A}^T\mathbf{P} + \mathbf{P}\mathbf{A} = -\mathbf{Q}$$

ここで、$\mathbf{Q}$ は任意の正定値対称行列です。

**定理**: $\mathbf{A}$ が安定（すべての固有値の実部が負）であることと、任意の正定値 $\mathbf{Q}$ に対してリアプノフ方程式の解 $\mathbf{P}$ が存在し、それが正定値となることは同値である。

#### 2.1.4 例題: リアプノフ方程式による安定性判定

**問題**: 次のシステムの安定性をリアプノフ方程式で判定せよ。

$$\mathbf{A} = \begin{bmatrix} -1 & 1 \\ -2 & -3 \end{bmatrix}$$

**解答**:

$\mathbf{Q} = \mathbf{I}$ として、リアプノフ方程式 $\mathbf{A}^T\mathbf{P} + \mathbf{P}\mathbf{A} = -\mathbf{I}$ を解きます。

$\mathbf{P} = \begin{bmatrix} p_{11} & p_{12} \\ p_{12} & p_{22} \end{bmatrix}$ とおくと：

$$\begin{bmatrix} -1 & -2 \\ 1 & -3 \end{bmatrix}\begin{bmatrix} p_{11} & p_{12} \\ p_{12} & p_{22} \end{bmatrix} + \begin{bmatrix} p_{11} & p_{12} \\ p_{12} & p_{22} \end{bmatrix}\begin{bmatrix} -1 & 1 \\ -2 & -3 \end{bmatrix} = -\mathbf{I}$$

展開すると：
$$\begin{bmatrix} -2p_{11} - 4p_{12} & -2p_{12} - 2p_{22} + p_{11} \\ -2p_{12} - 2p_{22} + p_{11} & -6p_{22} + 2p_{12} \end{bmatrix} = \begin{bmatrix} -1 & 0 \\ 0 & -1 \end{bmatrix}$$

連立方程式を解くと：
$$p_{11} = \frac{7}{10}, \quad p_{12} = \frac{1}{10}, \quad p_{22} = \frac{3}{10}$$

$$\mathbf{P} = \begin{bmatrix} 0.7 & 0.1 \\ 0.1 & 0.3 \end{bmatrix}$$

$\det(\mathbf{P}) = 0.7 \times 0.3 - 0.1^2 = 0.2 > 0$ かつ $p_{11} > 0$ より、$\mathbf{P}$ は正定値。

**結論**: システムは漸近安定である。

### 2.2 ナイキスト安定判別法 (Nyquist Stability Criterion)

**ナイキスト安定判別法**は、開ループ周波数応答から閉ループ系の安定性を判別する手法です。

#### 2.2.1 ナイキスト線図

**ナイキスト線図 (Nyquist Diagram)** は、$G(j\omega)$ を複素平面上に $\omega: -\infty \to +\infty$ として描いた軌跡です。

実際には、$\omega: 0 \to +\infty$ の軌跡を描き、実軸に関して対称な軌跡を追加します（$G(-j\omega) = G^*(j\omega)$ より）。

#### 2.2.2 ナイキストの安定判別法

フィードバック系の開ループ伝達関数を $L(s) = C(s)G(s)$ とします。

**ナイキストの安定判別法**:

閉ループ系が安定であるための条件は、ナイキスト線図が点 $-1+j0$ を左側に見て（反時計回りに）$P/2$ 周回することです。

ここで、$P$ は開ループ伝達関数 $L(s)$ の右半平面の極の数です。

**特殊ケース（$P=0$、開ループ安定の場合）**:

閉ループ系が安定 $\Leftrightarrow$ ナイキスト線図が点 $-1+j0$ を囲まない

#### 2.2.3 ゲイン余裕・位相余裕の幾何学的意味

ナイキスト線図上で：

**ゲイン余裕**: ナイキスト線図が負の実軸と交差する点から、点 $-1+j0$ までの距離の逆数

**位相余裕**: 単位円（$|L(j\omega)| = 1$）とナイキスト線図の交点における、負の実軸からの角度

#### 2.2.4 例題: ナイキスト線図による安定性判定

**問題**: 次の開ループ伝達関数を持つフィードバック系の安定性を判定せよ。

$$L(s) = \frac{K}{s(s+1)(s+2)}$$

**解答**:

$L(j\omega) = \frac{K}{j\omega(j\omega+1)(j\omega+2)}$

**$\omega \to 0^+$ での振る舞い**:
$$|L(j\omega)| \approx \frac{K}{2\omega} \to \infty, \quad \angle L(j\omega) \approx -90°$$

ナイキスト線図は無限遠から角度 $-90°$ の方向に入ってきます。

**$\omega \to \infty$ での振る舞い**:
$$|L(j\omega)| \approx \frac{K}{\omega^3} \to 0, \quad \angle L(j\omega) \approx -270°$$

ナイキスト線図は原点に角度 $-270°$ の方向に収束します。

**位相が$-180°$となる周波数**:
$$\angle L(j\omega_{pc}) = -90° - \tan^{-1}\omega_{pc} - \tan^{-1}\frac{\omega_{pc}}{2} = -180°$$
$$\tan^{-1}\omega_{pc} + \tan^{-1}\frac{\omega_{pc}}{2} = 90°$$

これより $\omega_{pc} = \sqrt{2}$ rad/s

このとき：
$$|L(j\sqrt{2})| = \frac{K}{\sqrt{2} \cdot \sqrt{3} \cdot \sqrt{4}} = \frac{K}{2\sqrt{6}}$$

**安定条件**: ナイキスト線図が点 $-1$ を囲まない
$$\frac{K}{2\sqrt{6}} < 1 \Rightarrow K < 2\sqrt{6} \approx 4.9$$

**結論**: $0 < K < 4.9$ で安定。

### 2.3 絶対安定性と円判別法 (Absolute Stability and Circle Criterion)

**絶対安定性 (Absolute Stability)** は、非線形要素を含む系の安定性を扱います。

#### 2.3.1 セクタ非線形性

非線形関数 $f(e)$ が**セクタ $[k_1, k_2]$** に属するとは：

$$k_1 e^2 \leq ef(e) \leq k_2 e^2 \quad \text{for all } e$$

#### 2.3.2 円判別法

線形部分の周波数応答 $G(j\omega)$ と非線形部分のセクタ $[k_1, k_2]$ に対して、システムが安定であるための十分条件は：

ナイキスト線図 $G(j\omega)$ が、2点 $(-1/k_1, 0)$ と $(-1/k_2, 0)$ を直径とする円盤の内部に入らないこと

---

## 3. 周波数応答の高度な解析 (Advanced Analysis of Frequency Response)

### 3.1 ナイキスト線図と閉ループ特性 (Nyquist Diagram and Closed-Loop Characteristics)

#### 3.1.1 M円とN円 (M-circles and N-circles)

開ループ周波数応答 $L(j\omega)$ から閉ループ周波数応答 $T(j\omega) = \frac{L(j\omega)}{1+L(j\omega)}$ を予測する手法として、**M円**と**N円**があります。

**M円**: 閉ループゲイン $|T(j\omega)| = M$ が一定となる開ループ応答の軌跡

M円は次の円の方程式で表されます：
$$\left(X + \frac{M^2}{M^2-1}\right)^2 + Y^2 = \left(\frac{M}{M^2-1}\right)^2$$

ここで、$L(j\omega) = X + jY$ です。

**M円の性質**:
- $M > 1$ の円は点 $-1+j0$ の左側
- $M < 1$ の円は点 $-1+j0$ の右側
- $M = 1$ は $X = -1/2$ の直線

**共振ピーク値 $M_p$**: ナイキスト線図が接する最小のM円の値

#### 3.1.2 ニコルス線図 (Nichols Chart)

**ニコルス線図 (Nichols Chart)** は、開ループ周波数応答を対数ゲイン（縦軸）と位相（横軸）のグラフ上に描いたものです。

ニコルス線図上にM円とN円を重ねることで、閉ループ特性を直接読み取れます。

**利点**:
- 閉ループ周波数応答の直接的な可視化
- ゲイン調整の影響が上下方向の平行移動として現れる

### 3.2 感度関数とロバスト性 (Sensitivity Function and Robustness)

#### 3.2.1 感度関数

**感度関数 (Sensitivity Function)** は、プラントの変動に対する閉ループ系の感度を表します：

$$S(s) = \frac{1}{1 + L(s)}$$

ここで、$L(s) = C(s)G(s)$ は開ループ伝達関数です。

**補感度関数 (Complementary Sensitivity Function)**:
$$T(s) = \frac{L(s)}{1 + L(s)}$$

**重要な関係式**:
$$S(s) + T(s) = 1$$

#### 3.2.2 ロバスト安定性

プラントに**乗法的不確かさ (Multiplicative Uncertainty)** $\Delta(s)$ がある場合：

$$G_p(s) = G(s)(1 + \Delta(s)), \quad |\Delta(j\omega)| \leq W(j\omega)$$

**ロバスト安定条件**:
$$|W(j\omega)T(j\omega)| < 1 \quad \text{for all } \omega$$

すなわち、補感度関数が重み関数の逆数より小さければロバスト安定です。

#### 3.2.3 $H_\infty$ ノルムと性能指標

システムの周波数領域での性能は **$H_\infty$ ノルム**で評価されます：

$$\|G\|_\infty = \sup_\omega |G(j\omega)|$$

**設計仕様の例**:
- 感度低減: $\|S\|_\infty < \alpha$ （低周波での外乱抑制）
- ロバスト安定性: $\|WT\|_\infty < 1$ （不確かさへの頑健性）
- ノイズ抑制: $\|T\|_\infty < \beta$ （高周波でのノイズ抑制）

### 3.3 多変数系の周波数応答 (Frequency Response of Multivariable Systems)

#### 3.3.1 特異値分解

多入力多出力（MIMO）系 $\mathbf{G}(s)$ の周波数応答 $\mathbf{G}(j\omega)$ は行列となります。

**特異値分解 (Singular Value Decomposition, SVD)**:
$$\mathbf{G}(j\omega) = \mathbf{U}(j\omega)\mathbf{\Sigma}(j\omega)\mathbf{V}^*(j\omega)$$

ここで、$\mathbf{\Sigma}$ は対角行列で、対角成分 $\sigma_i$ が**特異値 (Singular Values)** です。

**最大特異値** $\bar{\sigma}(\mathbf{G})$ と**最小特異値** $\underline{\sigma}(\mathbf{G})$ は、MIMO系のゲインの上限・下限を表します。

#### 3.3.2 一般化ナイキスト定理

MIMO系の安定性判別には、**一般化ナイキスト定理**を用います：

閉ループ系が安定 $\Leftrightarrow$ $\det(\mathbf{I} + \mathbf{L}(j\omega))$ のナイキスト線図が原点を $P/2$ 周回する

---

## 4. フィードバック系の高度な設計 (Advanced Feedback System Design)

### 4.1 位相進み・遅れ補償 (Lead-Lag Compensation)

#### 4.1.1 位相進み補償器 (Lead Compensator)

**位相進み補償器**は、中周波領域で位相を進ませ、位相余裕を改善します。

$$C(s) = K_c \frac{Ts + 1}{\alpha Ts + 1}, \quad 0 < \alpha < 1$$

**設計パラメータ**:
- $\alpha$ : 位相進みの量を決定（小さいほど位相進みが大きい）
- $T$ : 最大位相進みの周波数を決定
- $K_c$ : ゲイン調整

**最大位相進み**:
$$\phi_m = \sin^{-1}\frac{1-\alpha}{1+\alpha}$$

これは周波数 $\omega_m = \frac{1}{T\sqrt{\alpha}}$ で生じます。

**設計手順**:
1. 要求される位相余裕から必要な位相進み $\phi_m$ を決定
2. $\alpha$ を上式から計算
3. $\omega_m$ を新しいゲイン交差周波数に設定
4. $T = \frac{1}{\omega_m\sqrt{\alpha}}$ を計算
5. ゲイン $K_c$ を調整

#### 4.1.2 位相遅れ補償器 (Lag Compensator)

**位相遅れ補償器**は、低周波領域でゲインを増加させ、定常偏差を改善します。

$$C(s) = K_c \frac{Ts + 1}{\beta Ts + 1}, \quad \beta > 1$$

**設計手順**:
1. 要求される定常偏差から必要な低周波ゲイン増加 $\beta$ を決定
2. 折点周波数 $1/T$ を現在のゲイン交差周波数の $1/5 \sim 1/10$ に設定
3. 位相余裕が十分確保されているか確認

#### 4.1.3 位相進み遅れ補償器 (Lead-Lag Compensator)

位相進みと遅れを組み合わせた補償器：

$$C(s) = K_c \frac{(T_1 s + 1)(T_2 s + 1)}{(\alpha T_1 s + 1)(\beta T_2 s + 1)}, \quad 0 < \alpha < 1, \beta > 1$$

**利点**: 過渡応答と定常特性の両方を改善できる

#### 4.1.4 例題: 位相進み補償器の設計

**問題**: 次のプラントに対して、位相余裕 $PM = 45°$ を達成する位相進み補償器を設計せよ。

$$G(s) = \frac{4}{s(s+2)}$$

**解答**:

**Step 1**: 補償前のボード線図を描き、位相余裕を確認

ゲイン交差周波数で $|G(j\omega_{gc})| = 1$:
$$\frac{4}{\omega_{gc}\sqrt{4+\omega_{gc}^2}} = 1$$
$$\omega_{gc} \approx 1.24 \text{ rad/s}$$

位相:
$$\angle G(j\omega_{gc}) = -90° - \tan^{-1}\frac{\omega_{gc}}{2} \approx -90° - 32° = -122°$$

現在の位相余裕: $PM = 180° - 122° = 58°$

（この例では既に十分な位相余裕があるが、説明のため続行）

**Step 2**: 要求される位相進みを計算

目標位相余裕を50°とし、安全余裕5°を見込むと：
$$\phi_m = 55° - 58° + 12° = 9°$$（ゲイン増加による位相減少を考慮）

**Step 3**: $\alpha$ を計算
$$\sin\phi_m = \frac{1-\alpha}{1+\alpha} \Rightarrow \alpha = \frac{1-\sin 9°}{1+\sin 9°} \approx 0.71$$

**Step 4**: $\omega_m$ と $T$ を決定

新しいゲイン交差周波数を $\omega_m = 2$ rad/s とすると：
$$T = \frac{1}{\omega_m\sqrt{\alpha}} = \frac{1}{2\sqrt{0.71}} \approx 0.59$$

**Step 5**: ゲイン補正

補償器のゲインを調整して、$\omega_m$ でゲイン交差するようにします。

**結論**:
$$C(s) = K_c \frac{0.59s + 1}{0.42s + 1}$$

ここで、$K_c$ は適切に調整します。

### 4.2 内部モデル原理 (Internal Model Principle)

**内部モデル原理 (Internal Model Principle)** は、外乱や参照信号の定常的な追従・除去の条件を与えます。

**定理**: 外乱や参照信号の生成する微分方程式のモデルをコントローラ内部に含めば、定常偏差をゼロにできる。

**例**:
- ステップ外乱 → 積分器 $1/s$ を含む
- ランプ外乱 → $1/s^2$ を含む
- 正弦波外乱（周波数 $\omega_0$） → $\frac{s}{s^2 + \omega_0^2}$ を含む

**応用**: 反復制御、学習制御など

### 4.3 2自由度制御系 (Two-Degree-of-Freedom Control)

通常の1自由度フィードバック制御では、目標値追従と外乱抑制の両方を一つのコントローラで実現する必要があり、設計の自由度が制限されます。

**2自由度制御系**は、フィードフォワード補償器 $C_r(s)$ とフィードバック補償器 $C_y(s)$ を独立に設計します：

```
       Cr(s)           Cy(s)
r ──→ [ ] ─→(+)───→ [G(s)] ───→ y
              ↑  -            │
              └───────────────┘
```

制御入力: $u = C_r(s)r - C_y(s)y$

**設計方針**:
- $C_y(s)$: 外乱抑制と安定化に注力
- $C_r(s)$: 目標値応答の整形に注力

---

## 5. 複合的な考察問題 (Comprehensive Discussion Problems)

### 5.1 設計問題の体系的アプローチ

制御系設計の体系的な手順：

**Phase 1: 要求仕様の明確化**
- 時間領域仕様（オーバーシュート、整定時間、定常偏差）
- 周波数領域仕様（帯域幅、ゲイン余裕、位相余裕）
- ロバスト性仕様（パラメータ変動、外乱抑制）

**Phase 2: 制御構造の選択**
- 1自由度 vs 2自由度
- 状態フィードバック vs 出力フィードバック
- 古典制御 vs 現代制御

**Phase 3: 設計手法の選択**
- 極配置、LQ制御、$H_\infty$ 制御など

**Phase 4: パラメータ調整**
- 解析的手法、数値最適化、試行錯誤

**Phase 5: 検証とシミュレーション**
- 線形解析、非線形シミュレーション
- ロバスト性検証

### 5.2 総合例題: 倒立振子の安定化

**問題**: 台車上の倒立振子を安定化する制御系を設計せよ。

**システムモデル**（線形化モデル）:

$$\dot{\mathbf{x}} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & 0 & -\frac{mg}{M} & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & \frac{(M+m)g}{Ml} & 0 \end{bmatrix}\mathbf{x} + \begin{bmatrix} 0 \\ \frac{1}{M} \\ 0 \\ -\frac{1}{Ml} \end{bmatrix}u$$

ここで、状態変数は $\mathbf{x} = [x, \dot{x}, \theta, \dot{\theta}]^T$ （台車位置、台車速度、振子角度、振子角速度）。

パラメータ: $M = 1$ kg（台車質量）、$m = 0.1$ kg（振子質量）、$l = 0.5$ m（振子長さ）、$g = 9.8$ m/s²

**解答の方針**:

**Step 1**: 可制御性の確認

可制御性行列を計算し、$\text{rank}(\mathcal{C}) = 4$ を確認。

**Step 2**: 極配置設計

適切な閉ループ極を選択します。例えば：
- $s = -2 \pm 2j, -5 \pm 5j$

これらは十分速い応答を保証し、かつ現実的な制御入力の範囲内です。

**Step 3**: 状態フィードバックゲインの計算

アッカーマンの公式または LQR（線形2次レギュレータ）を用いて、$\mathbf{K}$ を計算。

**Step 4**: オブザーバの設計

実際には角度のみ測定可能とすると、オブザーバが必要です。オブザーバ極を制御極の3倍程度速く配置。

**Step 5**: シミュレーション

初期条件（例: $\theta(0) = 10°$）からの応答を確認。

**Step 6**: ロバスト性検証

パラメータ変動（$m$ や $l$ の不確かさ）に対する安定性を確認。

### 5.3 高度な考察問題

#### 問題1: 時間遅れ系の安定化

**問題**: 次の時間遅れを含むプラントを安定化せよ。

$$G(s) = \frac{e^{-Ls}}{s(s+1)}, \quad L = 0.5 \text{ s}$$

**考察のポイント**:
- 時間遅れは無限次元システムであり、無限個の極を持つ
- スミス補償器（Smith Predictor）の適用
- ロバスト性の問題（遅れ時間の不確かさ）

#### 問題2: 非最小位相系の制御

**問題**: 次の非最小位相系（右半平面零点を持つ系）の制御限界を議論せよ。

$$G(s) = \frac{-s+1}{(s+2)(s+3)}$$

**考察のポイント**:
- 右半平面零点による帯域幅の制約
- 逆応答（undershoot）の発生
- 感度関数の積分制約（Bode の積分定理）

#### 問題3: 適応制御とロバスト制御の比較

**問題**: パラメータが時変する系に対して、適応制御とロバスト制御の設計方針を比較し、それぞれの長所・短所を論じよ。

**考察のポイント**:
- 適応制御: パラメータ推定とコントローラ調整
  - 長所: 大きなパラメータ変動に対応可能
  - 短所: 過渡特性が保証されない、実装が複雑
- ロバスト制御: 不確かさを陽に考慮した設計
  - 長所: 安定性と性能が保証される
  - 短所: 過度に保守的な設計になりうる

---

## 6. 最先端トピックへの橋渡し (Bridge to Advanced Topics)

### 6.1 最適制御理論 (Optimal Control Theory)

**線形2次レギュレータ (Linear Quadratic Regulator, LQR)** は、次の性能指標を最小化します：

$$J = \int_0^\infty (\mathbf{x}^T\mathbf{Q}\mathbf{x} + \mathbf{u}^T\mathbf{R}\mathbf{u})dt$$

**解**: 状態フィードバック $\mathbf{u} = -\mathbf{K}\mathbf{x}$ where $\mathbf{K} = \mathbf{R}^{-1}\mathbf{B}^T\mathbf{P}$

ここで、$\mathbf{P}$ は**代数リカッチ方程式 (Algebraic Riccati Equation, ARE)** の解：

$$\mathbf{A}^T\mathbf{P} + \mathbf{P}\mathbf{A} - \mathbf{P}\mathbf{B}\mathbf{R}^{-1}\mathbf{B}^T\mathbf{P} + \mathbf{Q} = 0$$

**利点**:
- 安定性が保証される
- 重み行列 $\mathbf{Q}, \mathbf{R}$ で性能を調整

### 6.2 ロバスト制御理論 (Robust Control Theory)

**$H_\infty$ 制御問題**: 次の条件を満たすコントローラを設計

$$\left\|\begin{bmatrix} W_1 S \\ W_2 T \end{bmatrix}\right\|_\infty < 1$$

ここで、$W_1, W_2$ は重み関数です。

**解法**: リカッチ方程式の解、または線形行列不等式（LMI）による最適化

### 6.3 非線形制御 (Nonlinear Control)

**フィードバック線形化 (Feedback Linearization)**: 非線形系を状態変換と非線形フィードバックにより線形化

**スライディングモード制御 (Sliding Mode Control)**: 不確かさに対してロバストな不連続制御

**バックステッピング (Backstepping)**: 段階的にリアプノフ関数を構築する設計法

### 6.4 ディジタル制御 (Digital Control)

**離散時間系**:
$$\mathbf{x}_{k+1} = \mathbf{A}_d\mathbf{x}_k + \mathbf{B}_d\mathbf{u}_k$$

**サンプリング定理**: サンプリング周波数は、システムの帯域幅の少なくとも10倍以上

**離散化手法**:
- 零次ホールド (Zero-Order Hold)
- 双一次変換 (Bilinear Transform)

---

## まとめ (Summary)

この理解者向けノートでは、制御工学の発展的な内容を学習しました：

**状態空間表現の高度な理論**:
- カルマン分解と最小実現
- 状態フィードバックによる極配置
- オブザーバ設計と分離定理

**安定性解析の高度な手法**:
- リアプノフ安定性理論とリアプノフ方程式
- ナイキスト安定判別法の詳細
- 絶対安定性と円判別法

**周波数応答の高度な解析**:
- M円・N円とニコルス線図
- 感度関数とロバスト性
- 多変数系の特異値解析

**フィードバック系の高度な設計**:
- 位相進み・遅れ補償器の体系的設計
- 内部モデル原理
- 2自由度制御系

**複合的な考察問題**:
- 倒立振子の安定化など実践的な設計問題
- 時間遅れ系、非最小位相系などの高度な問題

**最先端トピックへの橋渡し**:
- LQR、$H_\infty$ 制御、非線形制御、ディジタル制御

これらの知識により、実用的な制御系の設計と解析が可能となります。さらに、修士課程以降で学ぶ最先端の制御理論への基礎が確立されました。

---

## 参考文献

**古典的名著**:
- Katsuhiko Ogata, "Modern Control Engineering"
- Gene F. Franklin, J. David Powell, Abbas Emami-Naeini, "Feedback Control of Dynamic Systems"

**現代制御理論**:
- Chi-Tsong Chen, "Linear System Theory and Design"
- Thomas Kailath, "Linear Systems"

**ロバスト制御**:
- Kemin Zhou, John C. Doyle, "Essentials of Robust Control"
- Sigurd Skogestad, Ian Postlethwaite, "Multivariable Feedback Control"

**非線形制御**:
- Hassan K. Khalil, "Nonlinear Systems"
- Alberto Isidori, "Nonlinear Control Systems"

## 次のステップ

**実装と実験**:
- MATLABやPythonでのシミュレーション
- 実機での制御実験

**専門分野への応用**:
- ロボティクス、航空宇宙、プロセス制御など

**研究への道**:
- 最適制御、適応制御、学習制御
- 分散制御、ネットワーク制御
- 確率的制御、予測制御

制御工学は、理論と実践が密接に結びついた学問です。本ノートで学んだ理論を、実際の問題に適用し、さらなる深化を目指してください。
