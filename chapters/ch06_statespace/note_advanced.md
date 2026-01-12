# 第6章 状態空間表現 - 理解者編

## 1. はじめに (Introduction)

初級編・中級編では、状態空間表現の基本と状態遷移行列の概念を学びました。この理解者編では、より深い理論的理解を目指して以下の高度なトピックを学習します：

- **行列指数関数の計算法**（対角化、ケイリー・ハミルトン、ラプラス変換）
- **モード分解**と固有値・固有ベクトルの深い理解
- **システム安定性と極の関係**の厳密な理論
- **座標変換**と**正準形**（可制御正準形、可観測正準形）
- **状態フィードバック制御**への橋渡し

これらの理論は、現代制御理論の基盤であり、実用的な制御系設計に不可欠です。

---

## 2. 行列指数関数の計算法 (Computing Matrix Exponentials)

### 2.1 対角化による計算 (Diagonalization Method)

行列指数関数 $e^{\mathbf{A}t}$ を計算する最も基本的な方法は**対角化**です。

#### 対角化可能な場合

行列 $\mathbf{A}$ が対角化可能であるとき、以下のように分解できます：

$$
\mathbf{A} = \mathbf{T}\mathbf{\Lambda}\mathbf{T}^{-1}
$$

ここで：
- $\mathbf{\Lambda} = \text{diag}(\lambda_1, \lambda_2, \ldots, \lambda_n)$：固有値を対角成分に持つ対角行列
- $\mathbf{T} = [\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n]$：固有ベクトルを列に持つ行列

この場合、行列指数関数は次のように計算できます：

$$
e^{\mathbf{A}t} = \mathbf{T}e^{\mathbf{\Lambda}t}\mathbf{T}^{-1}
$$

対角行列の指数関数は簡単に計算できます：

$$
e^{\mathbf{\Lambda}t} = \begin{bmatrix}
e^{\lambda_1 t} & 0 & \cdots & 0 \\
0 & e^{\lambda_2 t} & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & e^{\lambda_n t}
\end{bmatrix}
$$

**例題 2.1**: 次の行列の行列指数関数を対角化により求めよ。

$$
\mathbf{A} = \begin{bmatrix} -1 & 0 \\ 0 & -2 \end{bmatrix}
$$

**解答**:
既に対角行列なので、$\mathbf{T} = \mathbf{I}$ として：

$$
e^{\mathbf{A}t} = \begin{bmatrix} e^{-t} & 0 \\ 0 & e^{-2t} \end{bmatrix}
$$

#### 重複固有値を持つ場合（ジョルダン標準形）

行列が対角化不可能な場合（重複固有値で独立な固有ベクトルが不足）は、**ジョルダン標準形 (Jordan canonical form)** を使用します：

$$
\mathbf{A} = \mathbf{T}\mathbf{J}\mathbf{T}^{-1}
$$

ジョルダンブロック $\mathbf{J}_k(\lambda)$ に対して：

$$
e^{\mathbf{J}_k(\lambda)t} = e^{\lambda t}\begin{bmatrix}
1 & t & \frac{t^2}{2!} & \cdots & \frac{t^{k-1}}{(k-1)!} \\
0 & 1 & t & \cdots & \frac{t^{k-2}}{(k-2)!} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & 1
\end{bmatrix}
$$

### 2.2 ケイリー・ハミルトンの定理による計算 (Cayley-Hamilton Theorem)

**ケイリー・ハミルトンの定理 (Cayley-Hamilton theorem)** は、全ての正方行列がその特性方程式を満たすという重要な定理です。

#### 定理の内容

行列 $\mathbf{A}$ の特性多項式を：

$$
p(\lambda) = \det(\lambda\mathbf{I} - \mathbf{A}) = \lambda^n + a_{n-1}\lambda^{n-1} + \cdots + a_1\lambda + a_0
$$

とするとき、$\mathbf{A}$ は以下を満たします：

$$
p(\mathbf{A}) = \mathbf{A}^n + a_{n-1}\mathbf{A}^{n-1} + \cdots + a_1\mathbf{A} + a_0\mathbf{I} = \mathbf{0}
$$

#### 行列指数関数への応用

この定理により、$e^{\mathbf{A}t}$ を次のように $n-1$ 次以下の多項式で表現できます：

$$
e^{\mathbf{A}t} = \sum_{k=0}^{n-1} \alpha_k(t)\mathbf{A}^k
$$

係数 $\alpha_k(t)$ は、固有値 $\lambda_i$ における条件式から決定します：

$$
e^{\lambda_i t} = \sum_{k=0}^{n-1} \alpha_k(t)\lambda_i^k, \quad i = 1, 2, \ldots, n
$$

**例題 2.2**: 次の行列に対し、ケイリー・ハミルトンの定理を用いて $e^{\mathbf{A}t}$ を求めよ。

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}
$$

**解答**:
特性方程式は $\lambda^2 + 3\lambda + 2 = (\lambda + 1)(\lambda + 2) = 0$。固有値は $\lambda_1 = -1, \lambda_2 = -2$。

$e^{\mathbf{A}t} = \alpha_0(t)\mathbf{I} + \alpha_1(t)\mathbf{A}$ と仮定し：

$$
\begin{cases}
e^{-t} = \alpha_0(t) + \alpha_1(t)(-1) \\
e^{-2t} = \alpha_0(t) + \alpha_1(t)(-2)
\end{cases}
$$

これを解くと：

$$
\alpha_0(t) = 2e^{-t} - e^{-2t}, \quad \alpha_1(t) = e^{-t} - e^{-2t}
$$

よって：

$$
e^{\mathbf{A}t} = \begin{bmatrix}
2e^{-t} - e^{-2t} & e^{-t} - e^{-2t} \\
-2e^{-t} + 2e^{-2t} & -e^{-t} + 2e^{-2t}
\end{bmatrix}
$$

### 2.3 ラプラス変換による計算 (Laplace Transform Method)

ラプラス変換を用いると、行列指数関数を次のように計算できます：

$$
e^{\mathbf{A}t} = \mathcal{L}^{-1}\left\{(s\mathbf{I} - \mathbf{A})^{-1}\right\}
$$

#### 計算手順

1. **レゾルベント行列を計算**: $(s\mathbf{I} - \mathbf{A})^{-1}$
2. **各要素の逆ラプラス変換を取る**: 各要素を部分分数展開して逆変換

**例題 2.3**: 前述の行列 $\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}$ に対し、ラプラス変換法で $e^{\mathbf{A}t}$ を求めよ。

**解答**:

$$
s\mathbf{I} - \mathbf{A} = \begin{bmatrix} s & -1 \\ 2 & s+3 \end{bmatrix}
$$

逆行列は：

$$
(s\mathbf{I} - \mathbf{A})^{-1} = \frac{1}{s^2 + 3s + 2}\begin{bmatrix} s+3 & 1 \\ -2 & s \end{bmatrix}
$$

部分分数展開すると：

$$
\frac{s+3}{(s+1)(s+2)} = \frac{2}{s+1} - \frac{1}{s+2}
$$

$$
\frac{1}{(s+1)(s+2)} = \frac{1}{s+1} - \frac{1}{s+2}
$$

逆ラプラス変換により、前述と同じ結果が得られます。

### 2.4 計算法の比較

| 方法 | 長所 | 短所 | 適用場面 |
|-----|------|------|---------|
| **対角化** | 直感的で理解しやすい | 対角化不可能な場合は不可 | 異なる固有値を持つ場合 |
| **ケイリー・ハミルトン** | 常に適用可能、数値計算に向く | 係数計算が煩雑 | 低次元システム |
| **ラプラス変換** | ラプラス変換の知識を活用 | 部分分数展開が必要 | 制御理論との親和性 |
| **テイラー級数** | 一般的に適用可能 | 収束が遅い、精度に注意 | 数値計算ライブラリ |

---

## 3. モード分解と固有値・固有ベクトル (Modal Decomposition)

### 3.1 モード分解の概念

状態空間表現における**モード (mode)** とは、システムの基本的な振る舞いのパターンです。各モードは固有値と固有ベクトルによって特徴づけられます。

#### 対角化座標系での表現

行列 $\mathbf{A}$ が対角化可能とし、座標変換 $\mathbf{x} = \mathbf{T}\mathbf{z}$ を行うと：

$$
\dot{\mathbf{z}}(t) = \mathbf{\Lambda}\mathbf{z}(t) + \mathbf{T}^{-1}\mathbf{B}\mathbf{u}(t)
$$

この座標系では、状態方程式が**非結合 (decoupled)** になります：

$$
\dot{z}_i(t) = \lambda_i z_i(t) + [\mathbf{T}^{-1}\mathbf{B}]_i \mathbf{u}(t)
$$

各 $z_i(t)$ を**モード変数 (modal variable)** と呼びます。

### 3.2 零入力応答のモード分解

零入力応答は、各モードの寄与の和として表現されます：

$$
\mathbf{x}(t) = e^{\mathbf{A}t}\mathbf{x}(0) = \mathbf{T}e^{\mathbf{\Lambda}t}\mathbf{T}^{-1}\mathbf{x}(0) = \sum_{i=1}^{n} c_i e^{\lambda_i t}\mathbf{v}_i
$$

ここで：
- $c_i$：初期条件による係数（$c_i = \mathbf{w}_i^T\mathbf{x}(0)$、$\mathbf{w}_i$ は左固有ベクトル）
- $e^{\lambda_i t}$：第 $i$ モードの時間発展
- $\mathbf{v}_i$：第 $i$ モードの空間パターン（固有ベクトル）

### 3.3 固有値の役割

固有値 $\lambda_i = \sigma_i + j\omega_i$ は、各モードの特性を決定します：

| 固有値の性質 | モードの振る舞い | 例 |
|------------|---------------|---|
| $\lambda_i < 0$（実数・負） | 指数減衰 | $e^{-t}$ |
| $\lambda_i > 0$（実数・正） | 指数発散 | $e^{t}$ |
| $\lambda_i = j\omega$（純虚数） | 持続振動 | $e^{j\omega t}$ |
| $\lambda_i = \sigma + j\omega$（$\sigma < 0$） | 減衰振動 | $e^{-t}\cos(\omega t)$ |
| $\lambda_i = \sigma + j\omega$（$\sigma > 0$） | 発散振動 | $e^{t}\cos(\omega t)$ |

### 3.4 固有ベクトルの役割

固有ベクトル $\mathbf{v}_i$ は、各モードの**空間的なパターン**を表します。つまり、各モードが励起されたときに、状態変数がどのような比率で変化するかを示します。

**例題 3.1**: 次のシステムのモード分解を行え。

$$
\mathbf{A} = \begin{bmatrix} -1 & 1 \\ 0 & -2 \end{bmatrix}, \quad \mathbf{x}(0) = \begin{bmatrix} 1 \\ 1 \end{bmatrix}
$$

**解答**:
固有値は $\lambda_1 = -1, \lambda_2 = -2$。対応する固有ベクトルは：

$$
\mathbf{v}_1 = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \quad \mathbf{v}_2 = \begin{bmatrix} 1 \\ -1 \end{bmatrix}
$$

モード分解により：

$$
\mathbf{x}(t) = c_1 e^{-t}\begin{bmatrix} 1 \\ 0 \end{bmatrix} + c_2 e^{-2t}\begin{bmatrix} 1 \\ -1 \end{bmatrix}
$$

初期条件から $c_1 = 0, c_2 = 1$ となり：

$$
\mathbf{x}(t) = e^{-2t}\begin{bmatrix} 1 \\ -1 \end{bmatrix}
$$

---

## 4. システム安定性と極の関係 (Stability and Poles)

### 4.1 安定性の定義

線形時不変システム $\dot{\mathbf{x}} = \mathbf{A}\mathbf{x}$ において、**漸近安定性 (asymptotic stability)** は次のように定義されます：

$$
\lim_{t \to \infty} \mathbf{x}(t) = \mathbf{0} \quad \text{for all } \mathbf{x}(0)
$$

### 4.2 安定性条件

**定理 4.1** (リャプノフ安定性定理の周波数領域版):
線形時不変システムが漸近安定であるための必要十分条件は、行列 $\mathbf{A}$ の全ての固有値が**左半平面 (left half-plane)** にあることです：

$$
\text{Re}(\lambda_i) < 0 \quad \text{for all } i = 1, 2, \ldots, n
$$

### 4.3 極と固有値の関係

状態空間表現から伝達関数を求めると：

$$
G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}
$$

この伝達関数の**極 (poles)** は、$\det(s\mathbf{I} - \mathbf{A}) = 0$ の根です。これは行列 $\mathbf{A}$ の固有値と一致します（可制御・可観測な極に限る）。

#### 極と安定性の関係

| 極の位置 | システムの挙動 | 安定性 |
|---------|--------------|-------|
| 全ての極が左半平面 | 全てのモードが減衰 | **漸近安定** |
| 虚軸上に単純な極がある | 持続振動 | **限界安定** |
| 右半平面に極がある | 少なくとも1つのモードが発散 | **不安定** |

### 4.4 可制御性・可観測性と極

注意すべき点として、伝達関数には**可制御かつ可観測な極のみ**が現れます。

- **不可制御な極**: 入力から到達できないモード
- **不可観測な極**: 出力に現れないモード

これらの「隠れた極」も安定性には影響します。したがって、**内部安定性 (internal stability)** を保証するには、$\mathbf{A}$ の全固有値が左半平面になければなりません。

**例題 4.1**: 次のシステムの安定性を判定せよ。

$$
\mathbf{A} = \begin{bmatrix} -2 & 1 \\ -1 & -2 \end{bmatrix}
$$

**解答**:
特性方程式：$\det(\lambda\mathbf{I} - \mathbf{A}) = (\lambda + 2)^2 + 1 = \lambda^2 + 4\lambda + 5 = 0$

固有値：$\lambda = -2 \pm j$

両方の固有値の実部が負（$\text{Re}(\lambda) = -2 < 0$）なので、システムは**漸近安定**です。

---

## 5. 座標変換と正準形 (Coordinate Transformation and Canonical Forms)

### 5.1 座標変換の理論

状態空間表現は、座標系の取り方によって見え方が変わります。座標変換 $\mathbf{x} = \mathbf{T}\mathbf{z}$ により：

$$
\dot{\mathbf{z}} = \mathbf{T}^{-1}\mathbf{A}\mathbf{T}\mathbf{z} + \mathbf{T}^{-1}\mathbf{B}\mathbf{u}
$$

$$
\mathbf{y} = \mathbf{C}\mathbf{T}\mathbf{z} + \mathbf{D}\mathbf{u}
$$

変換後のシステムは：

$$
(\mathbf{A}', \mathbf{B}', \mathbf{C}', \mathbf{D}') = (\mathbf{T}^{-1}\mathbf{A}\mathbf{T}, \mathbf{T}^{-1}\mathbf{B}, \mathbf{C}\mathbf{T}, \mathbf{D})
$$

#### 不変量

座標変換で変わらない性質（**不変量**）：
- 固有値（極）
- 伝達関数 $G(s)$
- 可制御性・可観測性
- 安定性

### 5.2 可制御正準形 (Controllable Canonical Form)

**可制御正準形**は、システムの可制御性を明示的に表現する形です。SISO（単入力単出力）システムの伝達関数が：

$$
G(s) = \frac{b_0 s^{n-1} + b_1 s^{n-2} + \cdots + b_{n-1}}{s^n + a_1 s^{n-1} + \cdots + a_n}
$$

のとき、可制御正準形は：

$$
\mathbf{A}_c = \begin{bmatrix}
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & 1 \\
-a_n & -a_{n-1} & -a_{n-2} & \cdots & -a_1
\end{bmatrix}, \quad \mathbf{B}_c = \begin{bmatrix} 0 \\ 0 \\ \vdots \\ 0 \\ 1 \end{bmatrix}
$$

$$
\mathbf{C}_c = \begin{bmatrix} b_{n-1} & b_{n-2} & \cdots & b_0 \end{bmatrix}
$$

この形式では：
- 状態変数が直列に接続
- 入力が最後の状態変数にのみ作用
- 状態フィードバック設計が容易

### 5.3 可観測正準形 (Observable Canonical Form)

**可観測正準形**は、システムの可観測性を明示的に表現します：

$$
\mathbf{A}_o = \begin{bmatrix}
0 & 0 & \cdots & 0 & -a_n \\
1 & 0 & \cdots & 0 & -a_{n-1} \\
0 & 1 & \cdots & 0 & -a_{n-2} \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & \cdots & 1 & -a_1
\end{bmatrix}, \quad \mathbf{B}_o = \begin{bmatrix} b_{n-1} \\ b_{n-2} \\ \vdots \\ b_0 \end{bmatrix}
$$

$$
\mathbf{C}_o = \begin{bmatrix} 0 & 0 & \cdots & 0 & 1 \end{bmatrix}
$$

この形式では：
- 出力が最後の状態変数のみから得られる
- 観測器（オブザーバ）設計が容易

### 5.4 対角正準形 (Diagonal Canonical Form)

固有値が全て異なる場合、対角正準形が存在します：

$$
\mathbf{A}_d = \begin{bmatrix}
\lambda_1 & 0 & \cdots & 0 \\
0 & \lambda_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_n
\end{bmatrix}
$$

この形式では、各状態変数が独立したモードを表します。

**例題 5.1**: 伝達関数 $G(s) = \frac{2s + 3}{s^2 + 3s + 2}$ の可制御正準形を求めよ。

**解答**:

$$
\mathbf{A}_c = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix}, \quad \mathbf{B}_c = \begin{bmatrix} 0 \\ 1 \end{bmatrix}, \quad \mathbf{C}_c = \begin{bmatrix} 3 & 2 \end{bmatrix}, \quad \mathbf{D} = 0
$$

---

## 6. 状態フィードバック制御への橋渡し (Bridge to State Feedback Control)

### 6.1 状態フィードバックの概念

状態空間表現の最大の利点は、**状態フィードバック制御**の設計が可能になることです。

制御則を次のように設計します：

$$
\mathbf{u}(t) = -\mathbf{K}\mathbf{x}(t) + \mathbf{r}(t)
$$

ここで：
- $\mathbf{K}$：**フィードバックゲイン行列**
- $\mathbf{r}(t)$：参照入力

閉ループシステムは：

$$
\dot{\mathbf{x}} = (\mathbf{A} - \mathbf{B}\mathbf{K})\mathbf{x} + \mathbf{B}\mathbf{r}
$$

### 6.2 極配置法 (Pole Placement)

システムが可制御であれば、フィードバックゲイン $\mathbf{K}$ を適切に選ぶことで、閉ループ系の極（$\mathbf{A} - \mathbf{B}\mathbf{K}$ の固有値）を**任意の位置に配置**できます。

**定理 6.1** (極配置定理):
システム $(\mathbf{A}, \mathbf{B})$ が可制御であるとき、任意の多項式 $p(s) = s^n + p_1 s^{n-1} + \cdots + p_n$ に対して、閉ループ系 $\mathbf{A} - \mathbf{B}\mathbf{K}$ の特性多項式が $p(s)$ となるような $\mathbf{K}$ が存在する。

これにより、応答速度、減衰比、安定性などの性能を自由に設計できます。

### 6.3 最適制御 (Optimal Control)

より体系的なアプローチとして、**最適制御理論**があります。性能指標：

$$
J = \int_0^\infty (\mathbf{x}^T\mathbf{Q}\mathbf{x} + \mathbf{u}^T\mathbf{R}\mathbf{u}) dt
$$

を最小化する制御則は、**リカッチ方程式 (Riccati equation)** を解くことで得られます（**LQR: Linear Quadratic Regulator**）。

### 6.4 オブザーバ（状態推定器）

実際には全ての状態変数を測定できないことが多いため、**オブザーバ (observer)** を設計して状態を推定します：

$$
\dot{\hat{\mathbf{x}}} = \mathbf{A}\hat{\mathbf{x}} + \mathbf{B}\mathbf{u} + \mathbf{L}(\mathbf{y} - \mathbf{C}\hat{\mathbf{x}})
$$

ここで $\mathbf{L}$ は**オブザーバゲイン**です。システムが可観測であれば、推定誤差を任意の速さで収束させられます。

### 6.5 分離定理 (Separation Principle)

線形システムでは、**制御器の設計**と**オブザーバの設計**を独立に行えます（**分離定理**）。これにより、複雑な制御系を体系的に設計できます。

---

## 7. まとめ (Summary)

この理解者編では、以下の高度なトピックを学習しました：

### 7.1 行列指数関数の計算法

- **対角化法**: 最も直感的、$e^{\mathbf{A}t} = \mathbf{T}e^{\mathbf{\Lambda}t}\mathbf{T}^{-1}$
- **ケイリー・ハミルトンの定理**: 特性多項式を利用して低次多項式で表現
- **ラプラス変換法**: $e^{\mathbf{A}t} = \mathcal{L}^{-1}\{(s\mathbf{I} - \mathbf{A})^{-1}\}$

### 7.2 モード分解

- システムの応答は各固有モードの重ね合わせ
- 固有値がモードの時間発展を決定
- 固有ベクトルがモードの空間パターンを決定

### 7.3 安定性理論

- **安定性条件**: 全固有値の実部が負
- 極と固有値の関係（可制御・可観測な極）
- 内部安定性の重要性

### 7.4 座標変換と正準形

- **可制御正準形**: 状態フィードバック設計に適した形
- **可観測正準形**: オブザーバ設計に適した形
- **対角正準形**: モード分解を明示的に表現

### 7.5 次章への展望

状態空間表現の理論的基礎を理解したことで、次の段階に進めます：

- **第7章**: 状態フィードバック制御と極配置法
- **第8章**: 最適制御理論（LQR, LQG）
- **第9章**: ロバスト制御とH∞制御

状態空間法は、現代制御理論の基盤です。伝達関数では扱えない多入力多出力（MIMO）システム、状態制約、最適性などを体系的に扱えます。

---

## 練習問題 (Practice Problems)

### 問題1: 行列指数関数の計算

次の行列に対し、3つの方法（対角化、ケイリー・ハミルトン、ラプラス変換）のうち2つを使って $e^{\mathbf{A}t}$ を計算し、結果が一致することを確認せよ。

$$
\mathbf{A} = \begin{bmatrix} 1 & -1 \\ 2 & 4 \end{bmatrix}
$$

### 問題2: モード分解

次のシステムのモード分解を行い、各モードの特性（減衰率、振動周波数）を求めよ。

$$
\mathbf{A} = \begin{bmatrix} -1 & 4 \\ -1 & -1 \end{bmatrix}
$$

### 問題3: 安定性判定

次のシステムについて：
(a) 固有値を求めよ
(b) 安定性を判定せよ
(c) 不安定な場合、どのモードが発散するか説明せよ

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ -6 & -11 & -6 \end{bmatrix}
$$

### 問題4: 可制御正準形

伝達関数 $G(s) = \frac{s + 4}{s^2 + 5s + 6}$ を可制御正準形で表現せよ。

### 問題5: 座標変換

次のシステムを対角正準形に変換せよ。

$$
\mathbf{A} = \begin{bmatrix} -3 & 1 \\ -2 & 0 \end{bmatrix}, \quad \mathbf{B} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \quad \mathbf{C} = \begin{bmatrix} 1 & 0 \end{bmatrix}
$$

---

## 参考文献 (References)

1. Katsuhiko Ogata, "Modern Control Engineering", 5th Edition
2. Gene F. Franklin, J. David Powell, Abbas Emami-Naeini, "Feedback Control of Dynamic Systems"
3. Chi-Tsong Chen, "Linear System Theory and Design"
4. 吉川恒夫、井村順一「現代制御論」昭晃堂
5. 足立修一「システム同定の基礎」東京電機大学出版局

---

**次の学習**: 第7章「状態フィードバック制御と極配置法」では、この章で学んだ理論を実際の制御系設計に応用します。可制御性の条件のもとで、極を任意に配置して所望の性能を達成する方法を学びます。
