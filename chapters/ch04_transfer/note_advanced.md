# 第4章 伝達関数 - 理解者編

## 1. 伝達関数の等価変換 (Equivalent Transformations)

### 1.1 複雑なブロック線図の簡約化

中級編では基本的なブロック線図の結合を学びましたが、実際の制御系はより複雑な構造を持ちます。複雑なブロック線図を系統的に簡約化する手法を学びます。

**基本的な等価変換規則**：

| 操作 | 変換前 | 変換後 |
|------|--------|--------|
| ブロックの移動（和点通過） | $\xrightarrow{X} \boxed{+} \xrightarrow{} \boxed{G}$ | $\xrightarrow{X} \boxed{G} \xrightarrow{} \boxed{+} \xleftarrow{X/G}$ |
| ブロックの移動（分岐点通過） | $\xrightarrow{} \boxed{G} \xrightarrow{} \circ$ | $\xrightarrow{} \circ \xrightarrow{} \boxed{G}$ |
| フィードバックの移動 | 内側ループ → 外側ループ | 等価変換可能 |

**例題**：次の複雑なブロック線図を簡約化せよ。

```
           ┌──────────[H₁]────────┐
R(s) ──+──→ [G₁] ──+──→ [G₂] ──┴──→ Y(s)
       ↑           |
       └───[H₂]────┘
```

**解法**：

1. 内側のループ（$G_2$ と $H_1$）を簡約：
   $$G_{inner}(s) = \frac{G_2(s)}{1 + G_2(s)H_1(s)}$$

2. $G_1$ と $G_{inner}$ の直列結合：
   $$G_{forward}(s) = G_1(s) \cdot G_{inner}(s) = \frac{G_1(s)G_2(s)}{1 + G_2(s)H_1(s)}$$

3. 外側のフィードバックループ（$H_2$）を適用：
   $$G_{total}(s) = \frac{G_{forward}(s)}{1 + G_{forward}(s)H_2(s)} = \frac{G_1(s)G_2(s)}{1 + G_2(s)H_1(s) + G_1(s)G_2(s)H_2(s)}$$

### 1.2 メイソンの利得公式 (Mason's Gain Formula)

非常に複雑なブロック線図に対しては、**メイソンの利得公式**が有効です。

$$G(s) = \frac{1}{\Delta}\sum_{k=1}^{N} P_k \Delta_k$$

ここで：
- $P_k$：$k$ 番目のフォワードパス（入力から出力への経路）の利得
- $\Delta$：システムの特性行列式
- $\Delta_k$：$k$ 番目のパスに接触しないループの特性行列式
- $N$：フォワードパスの総数

**特性行列式の計算**：
$$\Delta = 1 - \sum L_i + \sum L_i L_j - \sum L_i L_j L_k + \cdots$$

- $\sum L_i$：すべての個別ループ利得の和
- $\sum L_i L_j$：互いに接触しない2つのループ利得の積の和
- $\sum L_i L_j L_k$：互いに接触しない3つのループ利得の積の和

**例題**：次のシステムにメイソンの公式を適用せよ。

```
        ┌─────[G₁]─────┬─────[G₂]─────┐
R(s) ───+              +              ├──→ Y(s)
        ↑              ↑              |
        └────[H₁]──────┴────[H₂]──────┘
```

**解法**：

1. フォワードパス：
   - $P_1 = G_1 G_2$

2. ループ利得：
   - $L_1 = -G_1 H_1$（内側ループ1）
   - $L_2 = -G_2 H_2$（内側ループ2）
   - $L_3 = -G_1 G_2 H_1 H_2$（外側ループ）

3. 特性行列式：
   $$\Delta = 1 - (L_1 + L_2 + L_3) + L_1 L_2$$
   $$= 1 + G_1 H_1 + G_2 H_2 + G_1 G_2 H_1 H_2 + G_1 G_2 H_1 H_2$$

4. $P_1$ に接触しないループはないので $\Delta_1 = 1$

5. 伝達関数：
   $$G(s) = \frac{P_1 \Delta_1}{\Delta} = \frac{G_1 G_2}{1 + G_1 H_1 + G_2 H_2 + 2G_1 G_2 H_1 H_2}$$

> **重要**: メイソンの公式は、複雑なブロック線図を体系的に解析できる強力な手法です。

---

## 2. 状態空間表現との関係 (Relation to State-Space Representation)

### 2.1 状態空間表現から伝達関数へ

制御系の別の表現方法として**状態空間表現 (state-space representation)** があります：

$$\begin{aligned}
\dot{\mathbf{x}}(t) &= \mathbf{A}\mathbf{x}(t) + \mathbf{B}u(t) \\
y(t) &= \mathbf{C}\mathbf{x}(t) + \mathbf{D}u(t)
\end{aligned}$$

ここで：
- $\mathbf{x}(t) \in \mathbb{R}^n$：状態ベクトル
- $u(t)$：入力（スカラー）
- $y(t)$：出力（スカラー）
- $\mathbf{A} \in \mathbb{R}^{n \times n}$：システム行列
- $\mathbf{B} \in \mathbb{R}^{n \times 1}$：入力行列
- $\mathbf{C} \in \mathbb{R}^{1 \times n}$：出力行列
- $\mathbf{D} \in \mathbb{R}$：直達項

**伝達関数への変換**：

ラプラス変換を適用すると：
$$s\mathbf{X}(s) = \mathbf{A}\mathbf{X}(s) + \mathbf{B}U(s)$$

整理して：
$$(s\mathbf{I} - \mathbf{A})\mathbf{X}(s) = \mathbf{B}U(s)$$

したがって：
$$\mathbf{X}(s) = (s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B}U(s)$$

出力方程式より：
$$Y(s) = \mathbf{C}\mathbf{X}(s) + \mathbf{D}U(s) = \left[\mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}\right]U(s)$$

よって、伝達関数は：
$$G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}$$

### 2.2 具体例：2次系

2次系の状態空間表現：
$$\mathbf{A} = \begin{bmatrix} 0 & 1 \\ -\omega_n^2 & -2\zeta\omega_n \end{bmatrix}, \quad \mathbf{B} = \begin{bmatrix} 0 \\ \omega_n^2 \end{bmatrix}, \quad \mathbf{C} = \begin{bmatrix} 1 & 0 \end{bmatrix}, \quad \mathbf{D} = 0$$

$(s\mathbf{I} - \mathbf{A})$ を計算：
$$s\mathbf{I} - \mathbf{A} = \begin{bmatrix} s & -1 \\ \omega_n^2 & s + 2\zeta\omega_n \end{bmatrix}$$

逆行列：
$$(s\mathbf{I} - \mathbf{A})^{-1} = \frac{1}{s^2 + 2\zeta\omega_n s + \omega_n^2} \begin{bmatrix} s + 2\zeta\omega_n & 1 \\ -\omega_n^2 & s \end{bmatrix}$$

伝達関数：
$$G(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

これは中級編で学んだ2次遅れ要素と一致します。

### 2.3 可制御性と可観測性

状態空間表現では、**可制御性 (controllability)** と**可観測性 (observability)** という重要な概念があります。

**可制御性**：すべての状態を有限時間で任意の値に制御できるか

可制御性行列：
$$\mathcal{C} = \begin{bmatrix} \mathbf{B} & \mathbf{A}\mathbf{B} & \mathbf{A}^2\mathbf{B} & \cdots & \mathbf{A}^{n-1}\mathbf{B} \end{bmatrix}$$

$\text{rank}(\mathcal{C}) = n$ なら可制御。

**可観測性**：出力から状態を推定できるか

可観測性行列：
$$\mathcal{O} = \begin{bmatrix} \mathbf{C} \\ \mathbf{C}\mathbf{A} \\ \mathbf{C}\mathbf{A}^2 \\ \vdots \\ \mathbf{C}\mathbf{A}^{n-1} \end{bmatrix}$$

$\text{rank}(\mathcal{O}) = n$ なら可観測。

> **注意**: 伝達関数表現では極零相殺により情報が失われる場合がありますが、状態空間表現では完全な情報を保持できます。

---

## 3. 多入力多出力系への拡張 (Extension to MIMO Systems)

### 3.1 伝達関数行列

これまでは1入力1出力（SISO: Single-Input Single-Output）システムを扱ってきましたが、実際の制御系は複数の入力と出力を持つ場合があります。

**多入力多出力 (MIMO: Multi-Input Multi-Output)** システムの伝達関数は行列として表現されます：

$$\mathbf{G}(s) = \begin{bmatrix}
G_{11}(s) & G_{12}(s) & \cdots & G_{1m}(s) \\
G_{21}(s) & G_{22}(s) & \cdots & G_{2m}(s) \\
\vdots & \vdots & \ddots & \vdots \\
G_{p1}(s) & G_{p2}(s) & \cdots & G_{pm}(s)
\end{bmatrix}$$

ここで：
- $G_{ij}(s)$：$j$ 番目の入力から $i$ 番目の出力への伝達関数
- $m$：入力の数
- $p$：出力の数

入出力の関係：
$$\mathbf{Y}(s) = \mathbf{G}(s)\mathbf{U}(s)$$

### 3.2 2入力2出力系の例

次のようなシステムを考えます：

$$\mathbf{G}(s) = \begin{bmatrix}
\frac{1}{s+1} & \frac{2}{s+2} \\
\frac{3}{s+3} & \frac{4}{s+4}
\end{bmatrix}$$

入力：
$$\mathbf{U}(s) = \begin{bmatrix} U_1(s) \\ U_2(s) \end{bmatrix}$$

出力：
$$\mathbf{Y}(s) = \begin{bmatrix} Y_1(s) \\ Y_2(s) \end{bmatrix} = \begin{bmatrix}
\frac{1}{s+1}U_1(s) + \frac{2}{s+2}U_2(s) \\
\frac{3}{s+3}U_1(s) + \frac{4}{s+4}U_2(s)
\end{bmatrix}$$

**物理的解釈**：
- $U_1$ は $Y_1$ と $Y_2$ の両方に影響を与える
- $U_2$ も $Y_1$ と $Y_2$ の両方に影響を与える
- これは**干渉 (coupling)** と呼ばれます

### 3.3 状態空間表現からMIMO伝達関数行列へ

MIMO系の状態空間表現：
$$\begin{aligned}
\dot{\mathbf{x}}(t) &= \mathbf{A}\mathbf{x}(t) + \mathbf{B}\mathbf{u}(t) \\
\mathbf{y}(t) &= \mathbf{C}\mathbf{x}(t) + \mathbf{D}\mathbf{u}(t)
\end{aligned}$$

ここで $\mathbf{B} \in \mathbb{R}^{n \times m}$、$\mathbf{C} \in \mathbb{R}^{p \times n}$、$\mathbf{D} \in \mathbb{R}^{p \times m}$

伝達関数行列：
$$\mathbf{G}(s) = \mathbf{C}(s\mathbf{I} - \mathbf{A})^{-1}\mathbf{B} + \mathbf{D}$$

### 3.4 非干渉化 (Decoupling)

MIMO系では、各入力が特定の出力にのみ影響するように設計することを**非干渉化**と言います。

理想的には：
$$\mathbf{G}(s) = \begin{bmatrix}
G_{11}(s) & 0 & \cdots & 0 \\
0 & G_{22}(s) & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & G_{pp}(s)
\end{bmatrix}$$

非干渉化制御器の設計：
$$\mathbf{K}(s) = \mathbf{G}(s)^{-1}\mathbf{G}_d(s)$$

ここで $\mathbf{G}_d(s)$ は望ましい対角伝達関数行列。

> **重要**: MIMO系の制御は現代制御理論の中心的なトピックであり、ロボット制御、プロセス制御、航空機制御などで不可欠です。

---

## 4. 安定性との関係 (Relationship with Stability)

### 4.1 極配置と安定性

中級編で学んだように、システムの安定性は極の位置で決まります。ここでは、より深く掘り下げます。

**BIBO安定性 (Bounded-Input Bounded-Output Stability)**：

有界な入力に対して出力が有界であるとき、システムはBIBO安定です。

**定理**：線形時不変システムがBIBO安定である必要十分条件は、すべての極が左半平面（$\text{Re}(s) < 0$）にあることです。

**証明の概略**：

伝達関数 $G(s) = \frac{N(s)}{D(s)}$ の部分分数展開：
$$G(s) = \sum_{i=1}^{n} \frac{A_i}{s - p_i}$$

逆ラプラス変換：
$$g(t) = \sum_{i=1}^{n} A_i e^{p_i t}$$

- $\text{Re}(p_i) < 0$ なら $e^{p_i t} \to 0$ （$t \to \infty$）
- $\text{Re}(p_i) > 0$ なら $e^{p_i t} \to \infty$ （$t \to \infty$）

したがって、すべての極が左半平面にあれば、インパルス応答は減衰し、システムは安定です。

### 4.2 ラウス・フルビッツの安定判別法

特性方程式の係数から極を計算せずに安定性を判定する方法です。

特性方程式：
$$D(s) = a_n s^n + a_{n-1} s^{n-1} + \cdots + a_1 s + a_0 = 0$$

**ラウス配列**：

```
s^n     | a_n      a_{n-2}    a_{n-4}    ...
s^{n-1} | a_{n-1}  a_{n-3}    a_{n-5}    ...
s^{n-2} | b_1      b_2        b_3        ...
s^{n-3} | c_1      c_2        c_3        ...
...
s^1     | e_1
s^0     | f_1
```

ここで：
$$b_1 = \frac{a_{n-1} a_{n-2} - a_n a_{n-3}}{a_{n-1}}$$

**判定規則**：第1列の符号変化の回数が、右半平面にある極の個数に等しい。

**例**：特性方程式 $s^3 + 6s^2 + 11s + 6 = 0$ の安定性を判定。

ラウス配列：
```
s^3 | 1    11
s^2 | 6    6
s^1 | 10   0
s^0 | 6
```

第1列：$1, 6, 10, 6$ すべて正 → 符号変化なし → 安定

### 4.3 ナイキスト安定判別法への橋渡し

フィードバック系の安定性を周波数領域で判定する**ナイキスト安定判別法**は、次章（第5章）で詳しく学びます。

フィードバック系：
$$G_{cl}(s) = \frac{G(s)}{1 + G(s)H(s)}$$

特性方程式：
$$1 + G(s)H(s) = 0$$

**ナイキスト線図**は、$G(j\omega)H(j\omega)$ を複素平面上にプロットしたものです。

**ナイキスト安定判別法の概要**：
- $G(s)H(s)$ が右半平面に極を持たない場合：ナイキスト線図が点 $(-1, 0)$ を囲まなければ閉ループ系は安定
- この判別法は、周波数応答から直接安定性を判定できる強力な手法

> **次章へ**：第5章では、ボード線図やナイキスト線図を用いた周波数領域での安定性解析と制御系設計を学びます。

### 4.4 相対安定性と安定余裕

単に「安定」か「不安定」かだけでなく、「どれくらい安定か」を評価する指標が**安定余裕 (stability margin)** です。

**ゲイン余裕 (Gain Margin, GM)**：
- フィードバックゲインをどれだけ増やせるか

**位相余裕 (Phase Margin, PM)**：
- 位相をどれだけ遅らせることができるか

これらは周波数応答から求められ、次章で詳しく学びます。

**推奨値**：
- ゲイン余裕：6 dB 以上
- 位相余裕：30° ～ 60°

---

## 5. 実践的な制御系設計 (Practical Control System Design)

### 5.1 PID制御器のチューニング

中級編でPID制御器の伝達関数を学びましたが、実際にパラメータ（$K_p$, $K_i$, $K_d$）をどう決定するかを考えます。

**Ziegler-Nicholsの第2法**（周波数応答法）：

1. $K_i = 0$, $K_d = 0$ として $K_p$ のみで制御
2. $K_p$ を増やして系を限界安定にする
3. そのときの $K_p = K_u$（限界ゲイン）と振動周期 $T_u$ を測定
4. 次の表に従ってパラメータを決定

| 制御器タイプ | $K_p$ | $T_i$ | $T_d$ |
|------------|-------|-------|-------|
| P | $0.5 K_u$ | - | - |
| PI | $0.45 K_u$ | $T_u/1.2$ | - |
| PID | $0.6 K_u$ | $T_u/2$ | $T_u/8$ |

ここで $K_i = K_p/T_i$、$K_d = K_p T_d$

**例**：
- $K_u = 10$, $T_u = 2$ 秒のとき
- PID制御器：$K_p = 6$, $T_i = 1$, $T_d = 0.25$
- したがって：$K_p = 6$, $K_i = 6$, $K_d = 1.5$

### 5.2 リード・ラグ補償器の設計

システムの過渡応答や周波数特性を改善するために、**リード補償器 (lead compensator)** や**ラグ補償器 (lag compensator)** を使用します。

**リード補償器**（位相進み補償）：
$$G_c(s) = K_c \frac{1 + \alpha Ts}{1 + Ts}, \quad \alpha > 1$$

- 目的：位相余裕の改善、応答の高速化
- 効果：中高周波数帯域でゲインと位相を増加

**ラグ補償器**（位相遅れ補償）：
$$G_c(s) = K_c \frac{1 + Ts}{1 + \beta Ts}, \quad \beta > 1$$

- 目的：定常偏差の改善
- 効果：低周波数帯域でゲインを増加

**リード・ラグ補償器**（両者の組み合わせ）：
$$G_c(s) = K_c \frac{(1 + \alpha T_1 s)(1 + T_2 s)}{(1 + T_1 s)(1 + \beta T_2 s)}$$

### 5.3 極配置法による設計

状態フィードバック制御：
$$u(t) = -\mathbf{K}\mathbf{x}(t) + r(t)$$

閉ループ系：
$$\dot{\mathbf{x}}(t) = (\mathbf{A} - \mathbf{B}\mathbf{K})\mathbf{x}(t) + \mathbf{B}r(t)$$

**極配置定理**：システムが可制御なら、フィードバックゲイン $\mathbf{K}$ を適切に選ぶことで、閉ループ系の極を任意の位置に配置できる。

**設計手順**：
1. 望ましい極の位置を決定（例：$s = -2 \pm 2j$）
2. 望ましい特性方程式を作成
3. $\det(s\mathbf{I} - (\mathbf{A} - \mathbf{B}\mathbf{K})) = $ 望ましい特性方程式
4. 係数比較により $\mathbf{K}$ を求める

**例**：
$$\mathbf{A} = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix}, \quad \mathbf{B} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$$

望ましい極：$s = -1 \pm j$

望ましい特性方程式：$(s+1-j)(s+1+j) = s^2 + 2s + 2 = 0$

$\mathbf{K} = [k_1 \quad k_2]$ とすると：
$$\mathbf{A} - \mathbf{B}\mathbf{K} = \begin{bmatrix} 0 & 1 \\ -k_1 & -k_2 \end{bmatrix}$$

特性方程式：
$$\det(s\mathbf{I} - (\mathbf{A} - \mathbf{B}\mathbf{K})) = s^2 + k_2 s + k_1 = 0$$

係数比較：$k_2 = 2$, $k_1 = 2$

したがって：$\mathbf{K} = [2 \quad 2]$

### 5.4 実際の設計例：倒立振子

倒立振子は制御理論の古典的な問題です。

**システムモデル**（線形化後）：
$$\mathbf{A} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ \frac{(M+m)g}{Ml} & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ -\frac{mg}{M} & 0 & 0 & 0 \end{bmatrix}, \quad \mathbf{B} = \begin{bmatrix} 0 \\ -\frac{1}{Ml} \\ 0 \\ \frac{1}{M} \end{bmatrix}$$

ここで：
- $M$：台車の質量
- $m$：振子の質量
- $l$：振子の長さ
- $g$：重力加速度

**伝達関数**（台車への力 $u$ から振子の角度 $\theta$ へ）：
$$G(s) = \frac{\Theta(s)}{U(s)} = \frac{-1/(Ml)}{s^2 - (M+m)g/(Ml)}$$

**特徴**：
- 極が右半平面にある（不安定）
- フィードバック制御が必須

**設計目標**：
- すべての極を左半平面に配置
- 速い応答と適度な減衰

**実装**：状態フィードバック制御または最適レギュレータ（LQR）を使用

---

## 6. 高度なトピックへの展望 (Advanced Topics)

### 6.1 ロバスト制御

実際のシステムには**不確かさ (uncertainty)** が存在します：
- パラメータの変動
- モデル化誤差
- 外乱

**ロバスト制御**は、これらの不確かさに対しても性能を保証する制御手法です。

代表的な手法：
- **H∞制御**：最悪の外乱に対する性能を最適化
- **μ解析**：構造化不確かさに対するロバスト性評価
- **スライディングモード制御**：不確かさに対する不変性

### 6.2 適応制御

システムのパラメータが時間とともに変化する場合、**適応制御 (adaptive control)** が有効です。

- **モデル規範適応制御 (MRAC)**：理想モデルに追従
- **自己調整制御 (STC)**：システム同定とフィードバック制御を組み合わせ

### 6.3 非線形制御

伝達関数は線形システムに適用されますが、実際のシステムの多くは非線形です。

非線形制御の手法：
- **フィードバック線形化**：座標変換により線形化
- **リアプノフ法**：安定性の直接証明
- **バックステッピング**：再帰的な制御器設計

### 6.4 最適制御

与えられた性能指標（コスト関数）を最小化する制御入力を求めます。

**LQR (Linear Quadratic Regulator)**：
$$J = \int_0^\infty (\mathbf{x}^T \mathbf{Q} \mathbf{x} + u^T \mathbf{R} u) dt$$

を最小化する $u = -\mathbf{K}\mathbf{x}$ を求める。

解：リカッチ方程式を解く
$$\mathbf{A}^T \mathbf{P} + \mathbf{P}\mathbf{A} - \mathbf{P}\mathbf{B}\mathbf{R}^{-1}\mathbf{B}^T\mathbf{P} + \mathbf{Q} = 0$$

フィードバックゲイン：
$$\mathbf{K} = \mathbf{R}^{-1}\mathbf{B}^T\mathbf{P}$$

---

## まとめ

この理解者編では、以下の高度な内容を学びました：

- **伝達関数の等価変換**：
  - 複雑なブロック線図の系統的な簡約化
  - メイソンの利得公式による体系的解析

- **状態空間表現との関係**：
  - 伝達関数と状態空間表現の相互変換
  - 可制御性・可観測性の概念

- **多入力多出力系 (MIMO)**：
  - 伝達関数行列
  - 干渉と非干渉化

- **安定性との深い関係**：
  - BIBO安定性の理論
  - ラウス・フルビッツの安定判別法
  - ナイキスト安定判別法への橋渡し
  - 安定余裕（ゲイン余裕・位相余裕）

- **実践的な制御系設計**：
  - PID制御器のチューニング（Ziegler-Nichols法）
  - リード・ラグ補償器の設計
  - 極配置法による設計
  - 倒立振子などの実例

- **高度なトピックへの展望**：
  - ロバスト制御
  - 適応制御
  - 非線形制御
  - 最適制御（LQR）

伝達関数は、古典制御理論の中核をなす概念であり、現代制御理論への橋渡しとなる重要な基礎です。初級編から理解者編まで学んだ知識を活用して、インタラクティブツールで実際に手を動かし、理論と実践の両面から制御システムへの理解を深めてください。

**次のステップ**：
- **第5章 周波数応答解析**：ボード線図、ナイキスト線図を用いた周波数領域での制御系設計
- **第6章 根軌跡法**：フィードバックゲインの変化に対する極の軌跡
- **第7章 状態空間法**：現代制御理論の基礎、状態フィードバック、オブザーバ設計
- **第8章 ディジタル制御**：離散時間システム、z変換、サンプル値制御系

制御工学の学習を続け、理論と実践の両面からスキルを磨いていきましょう！
