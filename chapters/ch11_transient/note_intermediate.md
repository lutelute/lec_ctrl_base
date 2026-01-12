# 第11章 制御系の過渡特性 - 中級編

## 1. 2次系の標準形 (Standard Form of Second-Order Systems)

### 1.1 標準形とは

初級編では、1次遅れ系のステップ応答を学びました。中級編では、**2次系 (second-order system)** の詳細な解析を行います。

2次系の**標準形 (standard form)** は以下のように表現されます：

$$G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$$

ここで：
- $\omega_n$ は**固有振動数 (natural frequency)** [rad/s]
- $\zeta$ は**減衰比 (damping ratio)** [無次元]

**例**:
伝達関数 $G(s) = \frac{16}{s^2 + 4s + 16}$ を標準形で表現します：
- $\omega_n^2 = 16$ より $\omega_n = 4$ [rad/s]
- $2\zeta\omega_n = 4$ より $\zeta = 4/(2 \times 4) = 0.5$
- したがって、$G(s) = \frac{4^2}{s^2 + 2(0.5)(4)s + 4^2}$

### 1.2 一般形から標準形への変換

一般的な2次伝達関数：

$$G(s) = \frac{K}{s^2 + as + b}$$

これを標準形に変換するには：
1. 分子を $\omega_n^2$ の形にする：$K = \omega_n^2$ より $\omega_n = \sqrt{b}$
2. 減衰比を求める：$2\zeta\omega_n = a$ より $\zeta = \frac{a}{2\sqrt{b}}$

**例**:
$G(s) = \frac{25}{s^2 + 6s + 25}$ の場合：
- $\omega_n = \sqrt{25} = 5$ [rad/s]
- $\zeta = \frac{6}{2 \times 5} = 0.6$

## 2. 減衰比と固有振動数 (Damping Ratio and Natural Frequency)

### 2.1 減衰比の意味

**減衰比 $\zeta$** は、システムの振動の減衰の度合いを表します：

| 減衰比の範囲 | システムの特性 | 応答の性質 |
|------------|--------------|-----------|
| $\zeta = 0$ | 非減衰 (undamped) | 持続的な振動 |
| $0 < \zeta < 1$ | 不足減衰 (underdamped) | 振動しながら収束 |
| $\zeta = 1$ | 臨界減衰 (critically damped) | 最速で振動なく収束 |
| $\zeta > 1$ | 過減衰 (overdamped) | 振動なく緩やかに収束 |

> **重要**: 制御系設計では、通常 $0.4 < \zeta < 0.8$ の範囲が望ましいとされます。

### 2.2 固有振動数の意味

**固有振動数 $\omega_n$** は、システムの応答速度を決定します：

- $\omega_n$ が大きい → 速い応答
- $\omega_n$ が小さい → 遅い応答

減衰比が同じでも、$\omega_n$ が異なれば応答速度が変わります。

### 2.3 極の位置との関係

2次系の極 (poles) は、特性方程式 $s^2 + 2\zeta\omega_n s + \omega_n^2 = 0$ の解として求められます：

$$s_{1,2} = -\zeta\omega_n \pm j\omega_n\sqrt{1-\zeta^2}$$

不足減衰の場合 ($0 < \zeta < 1$)、極は複素共役対となり：
- 実部：$-\zeta\omega_n$ （減衰の速さ）
- 虚部：$\pm\omega_d = \pm\omega_n\sqrt{1-\zeta^2}$ （振動周波数）

ここで、$\omega_d$ は**減衰振動数 (damped natural frequency)** と呼ばれます。

## 3. 特性値の詳細 (Detailed Characteristic Values)

### 3.1 立ち上がり時間 $T_r$ (Rise Time)

**立ち上がり時間**は、ステップ応答が最終値の10%から90%に達するまでの時間です。

不足減衰系 ($0 < \zeta < 1$) の場合の近似式：

$$T_r \approx \frac{1.8}{\omega_n}$$

より正確には：

$$T_r = \frac{\pi - \beta}{\omega_d}$$

ここで、$\beta = \tan^{-1}\left(\frac{\sqrt{1-\zeta^2}}{\zeta}\right)$ です。

**例**:
$\omega_n = 4$ [rad/s]、$\zeta = 0.5$ の場合：
- $T_r \approx \frac{1.8}{4} = 0.45$ [s]

### 3.2 ピーク時間 $T_p$ (Peak Time)

**ピーク時間**は、ステップ応答が最初のピークに達するまでの時間です。

$$T_p = \frac{\pi}{\omega_d} = \frac{\pi}{\omega_n\sqrt{1-\zeta^2}}$$

**例**:
$\omega_n = 4$ [rad/s]、$\zeta = 0.5$ の場合：
- $\omega_d = 4\sqrt{1-0.5^2} = 4 \times 0.866 = 3.464$ [rad/s]
- $T_p = \frac{\pi}{3.464} = 0.907$ [s]

> **注意**: ピーク時間は $\zeta$ が小さいほど短くなります。

### 3.3 整定時間 $T_s$ (Settling Time)

**整定時間**は、応答が最終値の±2%（または±5%）の範囲内に収まるまでの時間です。

**2%基準**:
$$T_s \approx \frac{4}{\zeta\omega_n}$$

**5%基準**:
$$T_s \approx \frac{3}{\zeta\omega_n}$$

**例**:
$\omega_n = 4$ [rad/s]、$\zeta = 0.5$ の場合：
- $T_s \approx \frac{4}{0.5 \times 4} = 2.0$ [s] （2%基準）
- $T_s \approx \frac{3}{0.5 \times 4} = 1.5$ [s] （5%基準）

**重要な関係**:
- $\zeta$ が小さい → $T_s$ が長い（振動が長く続く）
- $\zeta$ が大きい → $T_s$ が短い（速く収束）

### 3.4 最大オーバーシュート $M_p$ (Maximum Overshoot)

**最大オーバーシュート**は、応答が最終値を超える最大の割合です（パーセント表示）。

$$M_p = e^{-\frac{\zeta\pi}{\sqrt{1-\zeta^2}}} \times 100\%$$

**例**:
$\zeta = 0.5$ の場合：
- $M_p = e^{-\frac{0.5\pi}{\sqrt{1-0.5^2}}} \times 100\% = e^{-0.907} \times 100\% = 40.4\%$

**減衰比とオーバーシュートの関係**:

| $\zeta$ | $M_p$ |
|---------|-------|
| 0.2 | 52.7% |
| 0.4 | 25.4% |
| 0.5 | 16.3% |
| 0.6 | 9.5% |
| 0.7 | 4.6% |
| 0.8 | 1.5% |
| 1.0 | 0% |

> **設計指針**: 一般的に $M_p < 20\%$ となるよう $\zeta > 0.45$ を目標とします。

### 3.5 特性値のまとめ表

| 特性値 | 記号 | 公式（$0 < \zeta < 1$） | 依存性 |
|-------|------|----------------------|--------|
| 立ち上がり時間 | $T_r$ | $\approx \frac{1.8}{\omega_n}$ | $\omega_n$ に反比例 |
| ピーク時間 | $T_p$ | $\frac{\pi}{\omega_n\sqrt{1-\zeta^2}}$ | $\omega_n$、$\zeta$ |
| 整定時間 | $T_s$ | $\frac{4}{\zeta\omega_n}$ | $\zeta\omega_n$ に反比例 |
| 最大オーバーシュート | $M_p$ | $e^{-\frac{\zeta\pi}{\sqrt{1-\zeta^2}}} \times 100\%$ | $\zeta$ のみ |

## 4. 極の位置と応答特性 (Pole Location and Response Characteristics)

### 4.1 s平面上の極の位置

複素平面（s平面）上の極の位置が、システムの応答を決定します：

$$s_{1,2} = -\zeta\omega_n \pm j\omega_n\sqrt{1-\zeta^2}$$

極座標表現では：
- 極から原点までの距離：$|s| = \omega_n$
- 極と負の実軸のなす角度：$\theta = \cos^{-1}(\zeta)$

### 4.2 極の位置と減衰比の関係

s平面上で考えると：

```
        虚軸 (jω)
          ↑
          |
    × ←---+--→ θ = cos⁻¹(ζ)
    |     |
----+-----+----→ 実軸 (σ)
    |     |
    × ←---+
          |
```

- $\zeta = 0$：虚軸上（$\theta = 90°$）
- $\zeta = 0.5$：$\theta = 60°$
- $\zeta = 0.707$：$\theta = 45°$（よく使われる設計値）
- $\zeta = 1$：実軸上（$\theta = 0°$）

### 4.3 極の実部と整定時間

極の実部 $\sigma = -\zeta\omega_n$ は、指数関数的な減衰の速さを決定します：

$$e^{\sigma t} = e^{-\zeta\omega_n t}$$

整定時間は実部に反比例します：
$$T_s \approx \frac{4}{|\sigma|} = \frac{4}{\zeta\omega_n}$$

> **設計指針**: 速い応答には、極を左側（実部が大きく負）に配置します。

### 4.4 極の虚部と振動周波数

極の虚部 $\omega_d = \omega_n\sqrt{1-\zeta^2}$ は、振動の周波数を決定します：

- 虚部が大きい → 振動が速い
- 虚部が小さい → 振動が遅い
- 虚部がゼロ → 振動なし

## 5. 応用例 (Application Examples)

### 5.1 設計仕様からパラメータを決定

**問題**: 以下の仕様を満たす2次系を設計せよ：
- 整定時間：$T_s = 2$ [s]（2%基準）
- 最大オーバーシュート：$M_p = 10\%$

**解答**:

ステップ1: $M_p = 10\%$ から $\zeta$ を求める：
$$0.10 = e^{-\frac{\zeta\pi}{\sqrt{1-\zeta^2}}}$$

この式を解くと：$\zeta \approx 0.59$

ステップ2: $T_s = 2$ [s] と $\zeta = 0.59$ から $\omega_n$ を求める：
$$2 = \frac{4}{\zeta\omega_n} = \frac{4}{0.59 \times \omega_n}$$

よって、$\omega_n = \frac{4}{0.59 \times 2} = 3.39$ [rad/s]

ステップ3: 伝達関数を構成：
$$G(s) = \frac{11.5}{s^2 + 4.0s + 11.5}$$

### 5.2 サーボモータの位置制御

サーボモータの位置制御系では、以下のような仕様がよく使われます：

- $\zeta = 0.7$（適度な減衰、オーバーシュート約5%）
- $\omega_n = 10$ [rad/s]（速い応答）

この場合の特性値：
- $T_r \approx \frac{1.8}{10} = 0.18$ [s]
- $T_p = \frac{\pi}{10\sqrt{1-0.7^2}} = 0.44$ [s]
- $T_s \approx \frac{4}{0.7 \times 10} = 0.57$ [s]
- $M_p = 4.6\%$

### 5.3 温度制御システム

温度制御系では、オーバーシュートを避けるため：

- $\zeta = 1.0$ または $\zeta > 1$（臨界減衰または過減衰）
- $\omega_n$ は応答速度の要求から決定

例：$\zeta = 1.0$、$\omega_n = 0.5$ [rad/s]の場合：
- オーバーシュートなし
- $T_s \approx \frac{4}{1.0 \times 0.5} = 8$ [s]

---

## まとめ

この中級編では、以下を学びました：

- 2次系の標準形 $G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$
- 減衰比 $\zeta$ と固有振動数 $\omega_n$ の物理的意味
- 4つの主要な特性値の計算式：
  - 立ち上がり時間 $T_r \approx \frac{1.8}{\omega_n}$
  - ピーク時間 $T_p = \frac{\pi}{\omega_n\sqrt{1-\zeta^2}}$
  - 整定時間 $T_s \approx \frac{4}{\zeta\omega_n}$
  - 最大オーバーシュート $M_p = e^{-\frac{\zeta\pi}{\sqrt{1-\zeta^2}}} \times 100\%$
- 極の位置と応答特性の関係
- 実際の制御系設計への応用

次の上級編では、特性値の数学的導出、根軌跡法の基礎、高次システムへの拡張について学習します。
