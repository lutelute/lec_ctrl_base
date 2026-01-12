# 第6章 状態空間表現 - 中級編

## 1. はじめに (Introduction)

初級編では、状態空間表現の基本概念と簡単な例を学びました。この中級編では、より深い理解を目指して以下を学習します：

- **状態遷移行列** $\Phi(t) = e^{\mathbf{A}t}$ の定義と性質
- **零入力応答**と**零状態応答**による解の分解
- **状態空間表現から伝達関数への変換**
- **可制御性・可観測性**の基本概念

これらの理論を理解することで、状態空間法の強力さを実感できるでしょう。

---

## 2. 状態遷移行列 (State Transition Matrix)

### 2.1 状態遷移行列の定義

状態方程式：

$$
\dot{\mathbf{x}}(t) = \mathbf{A}\mathbf{x}(t) + \mathbf{B}\mathbf{u}(t)
$$

において、入力がゼロ（$\mathbf{u}(t) = 0$）の場合を考えます：

$$
\dot{\mathbf{x}}(t) = \mathbf{A}\mathbf{x}(t)
$$

この斉次方程式の解は、**状態遷移行列 (state transition matrix)** $\Phi(t)$を用いて表されます：

$$
\mathbf{x}(t) = \Phi(t)\mathbf{x}(0)
$$

ここで、状態遷移行列は次のように定義されます：

$$
\Phi(t) = e^{\mathbf{A}t}
$$

これを**行列指数関数 (matrix exponential)** と呼びます。

### 2.2 行列指数関数の定義

行列指数関数は、スカラーの指数関数のテイラー展開を行列に拡張したものです：

$$
e^{\mathbf{A}t} = \mathbf{I} + \mathbf{A}t + \frac{(\mathbf{A}t)^2}{2!} + \frac{(\mathbf{A}t)^3}{3!} + \cdots = \sum_{k=0}^{\infty} \frac{(\mathbf{A}t)^k}{k!}
$$

ここで、$\mathbf{I}$は単位行列、$(\mathbf{A}t)^k$は$\mathbf{A}t$の$k$乗を表します。

> **注意**: 行列の指数は通常の指数とは異なる性質を持ちます。特に、$e^{\mathbf{A}}e^{\mathbf{B}} \neq e^{\mathbf{A}+\mathbf{B}}$が一般には成り立ちません（$\mathbf{A}$と$\mathbf{B}$が可換でない場合）。

### 2.3 状態遷移行列の性質

状態遷移行列$\Phi(t) = e^{\mathbf{A}t}$は以下の重要な性質を持ちます：

**性質1: 初期時刻での値**

$$
\Phi(0) = e^{\mathbf{A} \cdot 0} = \mathbf{I}
$$

これは、時刻0での状態は初期状態そのものであることを意味します。

**性質2: 半群性（加法性）**

$$
\Phi(t_1 + t_2) = \Phi(t_1)\Phi(t_2) = \Phi(t_2)\Phi(t_1)
$$

これは、時刻$t_1$経過した後にさらに$t_2$経過することは、最初から$t_1+t_2$経過することと同じであることを示します。

**性質3: 逆行列**

$$
\Phi^{-1}(t) = \Phi(-t)
$$

つまり、$\Phi(t)\Phi(-t) = \mathbf{I}$が成り立ちます。これは、時間を逆向きに進めることができることを意味します。

**性質4: 微分**

$$
\frac{d}{dt}\Phi(t) = \mathbf{A}\Phi(t) = \Phi(t)\mathbf{A}
$$

これは、$\Phi(t)$が微分方程式$\dot{\Phi}(t) = \mathbf{A}\Phi(t)$の解であることを示します。

### 2.4 簡単な例：対角行列の場合

$\mathbf{A}$が対角行列の場合、行列指数関数は簡単に計算できます：

$$
\mathbf{A} = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}
\quad \Rightarrow \quad
e^{\mathbf{A}t} = \begin{bmatrix} e^{\lambda_1 t} & 0 \\ 0 & e^{\lambda_2 t} \end{bmatrix}
$$

**例**:

$$
\mathbf{A} = \begin{bmatrix} -1 & 0 \\ 0 & -2 \end{bmatrix}
\quad \Rightarrow \quad
\Phi(t) = \begin{bmatrix} e^{-t} & 0 \\ 0 & e^{-2t} \end{bmatrix}
$$

---

## 3. 零入力応答 (Zero-Input Response)

### 3.1 零入力応答の定義

**零入力応答 (zero-input response)** とは、入力がゼロ（$\mathbf{u}(t) = 0$）で、初期状態$\mathbf{x}(0) \neq \mathbf{0}$から始まるシステムの応答です。

状態方程式$\dot{\mathbf{x}} = \mathbf{A}\mathbf{x}$の解は：

$$
\mathbf{x}_{zi}(t) = e^{\mathbf{A}t}\mathbf{x}(0) = \Phi(t)\mathbf{x}(0)
$$

出力は：

$$
\mathbf{y}_{zi}(t) = \mathbf{C}\mathbf{x}_{zi}(t) = \mathbf{C}e^{\mathbf{A}t}\mathbf{x}(0)
$$

### 3.2 計算例：2次系

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -4 & -2 \end{bmatrix}, \quad
\mathbf{x}(0) = \begin{bmatrix} 1 \\ 0 \end{bmatrix}
$$

まず、$\mathbf{A}$の固有値を求めます：

$$
\det(s\mathbf{I} - \mathbf{A}) = \det\begin{bmatrix} s & -1 \\ 4 & s+2 \end{bmatrix} = s(s+2) + 4 = s^2 + 2s + 4 = 0
$$

$$
s = \frac{-2 \pm \sqrt{4 - 16}}{2} = -1 \pm j\sqrt{3}
$$

固有値が複素数なので、応答は減衰振動となります。零入力応答は：

$$
\mathbf{x}_{zi}(t) = e^{-t}\begin{bmatrix} \cos(\sqrt{3}t) + \frac{1}{\sqrt{3}}\sin(\sqrt{3}t) \\ -\frac{4}{\sqrt{3}}\sin(\sqrt{3}t) \end{bmatrix}
$$

> **注意**: 固有値の実部が負なので、システムは安定であり、応答は時間とともにゼロに収束します。

---

## 4. 零状態応答 (Zero-State Response)

### 4.1 零状態応答の定義

**零状態応答 (zero-state response)** とは、初期状態がゼロ（$\mathbf{x}(0) = \mathbf{0}$）で、入力$\mathbf{u}(t)$のみによるシステムの応答です。

状態方程式の解は、**畳み込み積分 (convolution integral)** で表されます：

$$
\mathbf{x}_{zs}(t) = \int_0^t e^{\mathbf{A}(t-\tau)}\mathbf{B}\mathbf{u}(\tau)d\tau = \int_0^t \Phi(t-\tau)\mathbf{B}\mathbf{u}(\tau)d\tau
$$

出力は：

$$
\mathbf{y}_{zs}(t) = \mathbf{C}\mathbf{x}_{zs}(t) + \mathbf{D}\mathbf{u}(t)
$$

### 4.2 完全応答

一般に、初期状態$\mathbf{x}(0)$と入力$\mathbf{u}(t)$が両方とも存在する場合、**完全応答 (complete response)** は零入力応答と零状態応答の和になります：

$$
\mathbf{x}(t) = \mathbf{x}_{zi}(t) + \mathbf{x}_{zs}(t) = e^{\mathbf{A}t}\mathbf{x}(0) + \int_0^t e^{\mathbf{A}(t-\tau)}\mathbf{B}\mathbf{u}(\tau)d\tau
$$

これは、状態方程式$\dot{\mathbf{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{u}$の一般解です。

### 4.3 計算例：ステップ入力

1次系：

$$
\dot{x} = -ax + bu, \quad y = x, \quad x(0) = 0
$$

にステップ入力$u(t) = 1$を加えたときの応答を求めます。

零状態応答：

$$
x_{zs}(t) = \int_0^t e^{-a(t-\tau)} \cdot b \cdot 1 \, d\tau = b e^{-at} \int_0^t e^{a\tau} d\tau
$$

$$
= b e^{-at} \left[ \frac{e^{a\tau}}{a} \right]_0^t = \frac{b}{a} e^{-at} (e^{at} - 1) = \frac{b}{a}(1 - e^{-at})
$$

これは、定常値$\frac{b}{a}$に時定数$\frac{1}{a}$で近づく応答です。

---

## 5. 状態空間表現から伝達関数への変換 (State-Space to Transfer Function)

### 5.1 変換公式の導出

単入力単出力（SISO）システムの状態空間表現：

$$
\begin{aligned}
\dot{\mathbf{x}}(t) &= \mathbf{A}\mathbf{x}(t) + \mathbf{B}u(t) \\
y(t) &= \mathbf{C}\mathbf{x}(t) + Du(t)
\end{aligned}
$$

にラプラス変換を適用します（初期条件はゼロ）：

$$
\begin{aligned}
s\mathbf{X}(s) &= \mathbf{A}\mathbf{X}(s) + \mathbf{B}U(s) \\
Y(s) &= \mathbf{C}\mathbf{X}(s) + DU(s)
\end{aligned}
$$

第1式を整理：

$$
(s\mathbf{I} - \mathbf{A})\mathbf{X}(s) = \mathbf{B}U(s)
$$

$$
\mathbf{X}(s) = (s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B}U(s)
$$

これを第2式に代入：

$$
Y(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B}U(s) + DU(s)
$$

$$
Y(s) = \left[\mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D\right]U(s)
$$

したがって、伝達関数は：

$$
G(s) = \frac{Y(s)}{U(s)} = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D
$$

> **重要**: この公式により、状態空間表現$\{\mathbf{A}, \mathbf{B}, \mathbf{C}, D\}$から伝達関数$G(s)$を計算できます。

### 5.2 計算例：1次系

RC回路の例（初級編より）：

$$
\mathbf{A} = -\frac{1}{RC}, \quad \mathbf{B} = \frac{1}{RC}, \quad \mathbf{C} = 1, \quad D = 0
$$

伝達関数を求めます：

$$
s\mathbf{I} - \mathbf{A} = s + \frac{1}{RC}
$$

$$
(s\mathbf{I} - \mathbf{A})^{-1} = \frac{1}{s + \frac{1}{RC}} = \frac{RC}{sRC + 1}
$$

$$
G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D = 1 \cdot \frac{RC}{sRC + 1} \cdot \frac{1}{RC} + 0
$$

$$
G(s) = \frac{1}{sRC + 1}
$$

これは、時定数$\tau = RC$の1次遅れ系の伝達関数です。

### 5.3 計算例：2次系

バネ-マス-ダンパ系（初級編より）：

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -\frac{k}{m} & -\frac{c}{m} \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 0 \\ \frac{1}{m} \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 1 & 0 \end{bmatrix}, \quad
D = 0
$$

まず、$(s\mathbf{I} - \mathbf{A})^{-1}$を計算：

$$
s\mathbf{I} - \mathbf{A} = \begin{bmatrix} s & -1 \\ \frac{k}{m} & s + \frac{c}{m} \end{bmatrix}
$$

$$
\det(s\mathbf{I} - \mathbf{A}) = s\left(s + \frac{c}{m}\right) + \frac{k}{m} = s^2 + \frac{c}{m}s + \frac{k}{m}
$$

逆行列の公式より：

$$
(s\mathbf{I} - \mathbf{A})^{-1} = \frac{1}{s^2 + \frac{c}{m}s + \frac{k}{m}} \begin{bmatrix} s + \frac{c}{m} & 1 \\ -\frac{k}{m} & s \end{bmatrix}
$$

伝達関数：

$$
G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} = \begin{bmatrix} 1 & 0 \end{bmatrix} \cdot \frac{1}{s^2 + \frac{c}{m}s + \frac{k}{m}} \begin{bmatrix} s + \frac{c}{m} & 1 \\ -\frac{k}{m} & s \end{bmatrix} \begin{bmatrix} 0 \\ \frac{1}{m} \end{bmatrix}
$$

$$
= \frac{1}{s^2 + \frac{c}{m}s + \frac{k}{m}} \begin{bmatrix} 1 & 0 \end{bmatrix} \begin{bmatrix} \frac{1}{m} \\ \frac{s}{m} \end{bmatrix} = \frac{1}{m(s^2 + \frac{c}{m}s + \frac{k}{m})}
$$

$$
G(s) = \frac{1}{ms^2 + cs + k}
$$

これは、標準的な2次系の伝達関数です。

---

## 6. 可制御性と可観測性 (Controllability and Observability)

### 6.1 可制御性の概念

**可制御性 (controllability)** とは、「任意の初期状態から、有限時間内に、適切な入力を加えることで任意の目標状態に到達できる」という性質です。

システム$\{\mathbf{A}, \mathbf{B}\}$が**可制御 (controllable)** であるとは、**可制御性行列 (controllability matrix)** が正則（フルランク）であることです：

$$
\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{A}\mathbf{B} & \mathbf{A}^2\mathbf{B} & \cdots & \mathbf{A}^{n-1}\mathbf{B} \end{bmatrix}
$$

$$
\text{rank}(\mathcal{C}) = n
$$

ここで、$n$は状態の次数です。

**直感的理解**: 可制御性は、「入力によってシステムの全ての状態を自由に操作できるか」を表します。もし可制御でない状態があれば、その状態は入力によって制御できません。

### 6.2 可観測性の概念

**可観測性 (observability)** とは、「出力の測定値から、有限時間内に初期状態を一意に決定できる」という性質です。

システム$\{\mathbf{A}, \mathbf{C}\}$が**可観測 (observable)** であるとは、**可観測性行列 (observability matrix)** が正則（フルランク）であることです：

$$
\mathcal{O} = \begin{bmatrix} \mathbf{C} \\ \mathbf{C}\mathbf{A} \\ \mathbf{C}\mathbf{A}^2 \\ \vdots \\ \mathbf{C}\mathbf{A}^{n-1} \end{bmatrix}
$$

$$
\text{rank}(\mathcal{O}) = n
$$

**直感的理解**: 可観測性は、「出力の測定からシステムの内部状態を推定できるか」を表します。もし可観測でない状態があれば、その状態は出力からは見えません。

### 6.3 可制御性の例

**例1**:

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

可制御性行列：

$$
\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{A}\mathbf{B} \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}
$$

$$
\det(\mathcal{C}) = -1 \neq 0 \quad \Rightarrow \quad \text{可制御}
$$

**例2**:

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}
$$

可制御性行列：

$$
\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{A}\mathbf{B} \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}
$$

$$
\text{rank}(\mathcal{C}) = 1 < 2 \quad \Rightarrow \quad \text{可制御でない}
$$

この場合、第2状態（$x_2$）は入力によって直接制御できません。

### 6.4 可観測性の例

**例**:

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -4 & -2 \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 1 & 0 \end{bmatrix}
$$

可観測性行列：

$$
\mathcal{O} = \begin{bmatrix} \mathbf{C} \\ \mathbf{C}\mathbf{A} \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}
$$

$$
\det(\mathcal{O}) = 1 \neq 0 \quad \Rightarrow \quad \text{可観測}
$$

位置のみを測定しても、速度を推定できることを意味します。

### 6.5 双対性 (Duality)

可制御性と可観測性には、以下の**双対性 (duality)** が成り立ちます：

- $\{\mathbf{A}, \mathbf{B}\}$が可制御 $\Leftrightarrow$ $\{\mathbf{A}^T, \mathbf{C}^T\}$が可観測
- $\{\mathbf{A}, \mathbf{C}\}$が可観測 $\Leftrightarrow$ $\{\mathbf{A}^T, \mathbf{B}^T\}$が可制御

この性質は、制御理論において非常に重要です。

---

## 7. 実践的な計算のまとめ (Summary of Practical Calculations)

状態空間法における主要な計算手順をまとめます：

### 7.1 状態遷移行列の計算

**手順**:
1. $\mathbf{A}$の固有値$\lambda_i$を求める
2. $\mathbf{A}$が対角化可能なら、$e^{\mathbf{A}t} = \mathbf{T}e^{\Lambda t}\mathbf{T}^{-1}$
3. 対角化不可能なら、ケイリー・ハミルトンの定理やラプラス変換を使用（理解者編で学習）

### 7.2 伝達関数の計算

**手順**:
1. $(s\mathbf{I} - \mathbf{A})$を計算
2. 逆行列$(s\mathbf{I} - \mathbf{A})^{-1}$を求める（余因子行列を使用）
3. $G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D$を計算

### 7.3 可制御性・可観測性の判定

**手順**:
1. 可制御性行列$\mathcal{C}$または可観測性行列$\mathcal{O}$を構成
2. ランクを計算（行列式がゼロでないか、またはランク計算）
3. ランク$= n$なら可制御/可観測

---

## まとめ (Summary)

この中級編では、以下を学びました：

- **状態遷移行列**: $\Phi(t) = e^{\mathbf{A}t}$ - 状態の時間発展を表す行列
  - 性質: $\Phi(0) = \mathbf{I}$, $\Phi(t_1 + t_2) = \Phi(t_1)\Phi(t_2)$, $\Phi^{-1}(t) = \Phi(-t)$
- **零入力応答**: $\mathbf{x}_{zi}(t) = e^{\mathbf{A}t}\mathbf{x}(0)$ - 初期状態のみによる応答
- **零状態応答**: $\mathbf{x}_{zs}(t) = \int_0^t e^{\mathbf{A}(t-\tau)}\mathbf{B}\mathbf{u}(\tau)d\tau$ - 入力のみによる応答
- **完全応答**: 零入力応答と零状態応答の和
- **伝達関数への変換**: $G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + D$
- **可制御性**: 可制御性行列$\mathcal{C}$がフルランクかどうか
- **可観測性**: 可観測性行列$\mathcal{O}$がフルランクかどうか

これらの概念は、状態フィードバック制御やオブザーバ設計の基礎となります。

次の**理解者編**では、行列指数関数の計算法（対角化、ケイリー・ハミルトンの定理、ラプラス変換）、モード分解、システム安定性の詳細、座標変換と正準形について学習します。

---

## 練習問題 (Practice Problems)

理解を深めるために、以下の問題に取り組んでみましょう：

### 問題1: 状態遷移行列

次の行列の状態遷移行列$e^{\mathbf{A}t}$を求めなさい：

$$
\mathbf{A} = \begin{bmatrix} -2 & 0 \\ 0 & -3 \end{bmatrix}
$$

<details>
<summary>解答</summary>

対角行列なので：

$$
e^{\mathbf{A}t} = \begin{bmatrix} e^{-2t} & 0 \\ 0 & e^{-3t} \end{bmatrix}
$$
</details>

### 問題2: 伝達関数

次の状態空間表現の伝達関数を求めなさい：

$$
\mathbf{A} = \begin{bmatrix} -1 & 0 \\ 0 & -2 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}, \quad
\mathbf{C} = \begin{bmatrix} 1 & 2 \end{bmatrix}, \quad
D = 0
$$

<details>
<summary>解答</summary>

$$
s\mathbf{I} - \mathbf{A} = \begin{bmatrix} s+1 & 0 \\ 0 & s+2 \end{bmatrix}
$$

$$
(s\mathbf{I} - \mathbf{A})^{-1} = \begin{bmatrix} \frac{1}{s+1} & 0 \\ 0 & \frac{1}{s+2} \end{bmatrix}
$$

$$
G(s) = \begin{bmatrix} 1 & 2 \end{bmatrix} \begin{bmatrix} \frac{1}{s+1} & 0 \\ 0 & \frac{1}{s+2} \end{bmatrix} \begin{bmatrix} 1 \\ 1 \end{bmatrix} = \frac{1}{s+1} + \frac{2}{s+2}
$$
</details>

### 問題3: 可制御性

次のシステムは可制御か判定しなさい：

$$
\mathbf{A} = \begin{bmatrix} 1 & 2 \\ 0 & 3 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}
$$

<details>
<summary>解答</summary>

$$
\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{A}\mathbf{B} \end{bmatrix} = \begin{bmatrix} 1 & 3 \\ 1 & 3 \end{bmatrix}
$$

$$
\det(\mathcal{C}) = 3 - 3 = 0 \quad \Rightarrow \quad \text{可制御でない}
$$
</details>

---

**次のステップ**:
- インタラクティブツールで状態遷移行列の計算を視覚化してみましょう
- 理解者編で、より高度な計算法と理論を学習しましょう
- 実際の制御システム設計への応用（次章以降）に進みましょう

これで中級編は完了です。お疲れ様でした！
