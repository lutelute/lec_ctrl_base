# 第2章 複素数とラプラス変換 - 中級編

## 1. 複素数の極座標表現 (Polar Form of Complex Numbers)

### 1.1 極座標表現とは

初級編では、複素数を**直交座標形式** $z = a + jb$ で表現しました。中級編では、**極座標形式 (polar form)** を学びます。

複素数 $z = a + jb$ は、以下の極座標形式で表現できます：

$$z = r(\cos\theta + j\sin\theta)$$

ここで：
- $r$ は**絶対値 (magnitude)** または**モジュラス (modulus)** と呼ばれ、$r = |z| = \sqrt{a^2 + b^2}$
- $\theta$ は**偏角 (argument)** または**位相 (phase)** と呼ばれ、$\theta = \tan^{-1}(b/a)$

**例**:
複素数 $z = 3 + 4j$ の極座標表現を求めます：
- $r = \sqrt{3^2 + 4^2} = \sqrt{9 + 16} = 5$
- $\theta = \tan^{-1}(4/3) \approx 53.13°$ または $0.927$ ラジアン
- したがって、$z = 5(\cos 0.927 + j\sin 0.927)$

### 1.2 オイラーの公式と指数形式 (Euler's Formula and Exponential Form)

**オイラーの公式 (Euler's formula)** は、数学において最も美しい公式の一つです：

$$e^{j\theta} = \cos\theta + j\sin\theta$$

この公式を使うと、複素数を**指数形式 (exponential form)** で表現できます：

$$z = re^{j\theta}$$

これは極座標表現の最もコンパクトな形です。

**例**:
- $z = 3 + 4j = 5e^{j0.927}$
- $z = 1 + j = \sqrt{2}e^{j\pi/4}$

> **注意**: 指数形式は、複素数の乗算・除算を非常に簡単にします。

### 1.3 複素数の乗算・除算

指数形式を使うと、乗算と除算が簡単になります：

**乗算**:
$$z_1 z_2 = r_1 e^{j\theta_1} \cdot r_2 e^{j\theta_2} = r_1 r_2 e^{j(\theta_1 + \theta_2)}$$

**除算**:
$$\frac{z_1}{z_2} = \frac{r_1 e^{j\theta_1}}{r_2 e^{j\theta_2}} = \frac{r_1}{r_2} e^{j(\theta_1 - \theta_2)}$$

**直感的な理解**:
- 乗算：絶対値は掛け算、偏角は足し算
- 除算：絶対値は割り算、偏角は引き算

## 2. ラプラス変換の性質 (Properties of Laplace Transform)

### 2.1 線形性 (Linearity)

ラプラス変換は**線形演算子**です：

$$\mathcal{L}\{af(t) + bg(t)\} = aF(s) + bG(s)$$

ここで、$a, b$ は定数です。

**例**:
$$\mathcal{L}\{3e^{-2t} + 5t\} = 3 \cdot \frac{1}{s+2} + 5 \cdot \frac{1}{s^2}$$

### 2.2 時間シフト (Time Shifting)

関数が時間軸上でシフトされる場合：

$$\mathcal{L}\{f(t-a)u(t-a)\} = e^{-as}F(s)$$

ここで、$u(t)$ は単位ステップ関数です。

### 2.3 周波数シフト (Frequency Shifting)

時間領域で指数関数が掛けられる場合：

$$\mathcal{L}\{e^{-at}f(t)\} = F(s+a)$$

**例**:
- $\mathcal{L}\{\sin(\omega t)\} = \frac{\omega}{s^2 + \omega^2}$ より
- $\mathcal{L}\{e^{-at}\sin(\omega t)\} = \frac{\omega}{(s+a)^2 + \omega^2}$

### 2.4 微分定理 (Differentiation Theorem)

時間領域の微分は、$s$領域では $s$ の乗算になります：

$$\mathcal{L}\left\{\frac{df(t)}{dt}\right\} = sF(s) - f(0^-)$$

初期条件がゼロの場合：
$$\mathcal{L}\left\{\frac{df(t)}{dt}\right\} = sF(s)$$

**重要性**: この性質により、微分方程式が代数方程式に変換されます。

### 2.5 積分定理 (Integration Theorem)

時間領域の積分は、$s$領域では $s$ による除算になります：

$$\mathcal{L}\left\{\int_0^t f(\tau)d\tau\right\} = \frac{F(s)}{s}$$

### 2.6 主要な性質のまとめ表

| 性質 | 時間領域 | ラプラス領域 |
|------|---------|-------------|
| 線形性 | $af(t) + bg(t)$ | $aF(s) + bG(s)$ |
| 時間シフト | $f(t-a)u(t-a)$ | $e^{-as}F(s)$ |
| 周波数シフト | $e^{-at}f(t)$ | $F(s+a)$ |
| 微分 | $\frac{df}{dt}$ | $sF(s) - f(0^-)$ |
| 積分 | $\int_0^t f(\tau)d\tau$ | $\frac{F(s)}{s}$ |

## 3. 制御工学への応用

### 3.1 伝達関数での利用

複素数の極座標表現とラプラス変換の性質は、**伝達関数 (transfer function)** の解析に不可欠です：

- 極と零点の位置が、システムの安定性と応答特性を決定します
- 周波数応答は、$s = j\omega$ を代入することで得られます
- ボード線図の作成には、絶対値と偏角の計算が必要です

### 3.2 システム応答の計算例

1次遅れ系の伝達関数 $G(s) = \frac{1}{s+1}$ に単位ステップ入力を与える場合：

- 入力: $U(s) = \frac{1}{s}$
- 出力: $Y(s) = G(s)U(s) = \frac{1}{s(s+1)}$
- 部分分数展開と逆ラプラス変換により、時間応答 $y(t) = 1 - e^{-t}$ を得ます

---

## まとめ

この中級編では、以下を学びました：

- 複素数の極座標表現 $z = r(\cos\theta + j\sin\theta)$
- オイラーの公式と指数形式 $z = re^{j\theta}$
- 指数形式を使った複素数の乗算・除算
- ラプラス変換の重要な性質（線形性、時間シフト、周波数シフト、微分・積分定理）
- 制御工学における実際の応用

次の上級編では、伝達関数の詳細な解析や安定性判別について学習します。
