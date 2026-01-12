# 第5章 周波数伝達関数 - 理解者向け技術ノート

## 1. 周波数応答の数学的基礎 (Mathematical Foundations of Frequency Response)

### 1.1 LTIシステムの正弦波定常応答 (Sinusoidal Steady-State Response of LTI Systems)

線形時不変システム（LTI: Linear Time-Invariant system）に正弦波入力 $u(t) = A\sin(\omega t)$ を加えたとき、過渡応答が減衰した後の**定常応答 (steady-state response)** を考えます。

システムの伝達関数を $G(s)$ とすると、出力のラプラス変換は：

$$Y(s) = G(s)U(s) = G(s) \cdot \frac{A\omega}{s^2 + \omega^2}$$

部分分数展開により、定常応答成分（極 $s = \pm j\omega$）は以下の形になります：

$$y_{ss}(t) = A|G(j\omega)|\sin(\omega t + \angle G(j\omega))$$

これは、出力が入力と同じ周波数 $\omega$ の正弦波であり、振幅が $|G(j\omega)|$ 倍、位相が $\angle G(j\omega)$ だけシフトすることを示しています。

### 1.2 フーリエ変換との関係 (Relationship with Fourier Transform)

周波数伝達関数 $G(j\omega)$ は、インパルス応答 $h(t)$ の**フーリエ変換 (Fourier transform)** と等価です：

$$G(j\omega) = \int_{0}^{\infty} h(t)e^{-j\omega t}dt$$

これにより、周波数応答は時間領域のインパルス応答と周波数領域の伝達関数を結びつける重要な概念となります。

### 1.3 伝達関数の周波数特性表現 (Frequency Representation of Transfer Functions)

周波数伝達関数は、極座標形式で表現されます：

$$G(j\omega) = |G(j\omega)|e^{j\angle G(j\omega)} = M(\omega)e^{j\phi(\omega)}$$

ここで：
- **ゲイン特性 (gain characteristic)**: $M(\omega) = |G(j\omega)|$
- **位相特性 (phase characteristic)**: $\phi(\omega) = \angle G(j\omega)$

## 2. ゲイン特性の詳細解析 (Detailed Analysis of Gain Characteristics)

### 2.1 帯域幅 (Bandwidth)

**帯域幅 (bandwidth)** $\omega_{BW}$ は、ゲインが低周波数値から **-3 dB 下がる周波数**として定義されます。

-3 dB は振幅比が $1/\sqrt{2} \approx 0.707$ に相当し、パワーが半減する点を示します：

$$|G(j\omega_{BW})| = \frac{|G(0)|}{\sqrt{2}}$$

**工学的意義**：
- 信号がほぼ減衰なく通過する周波数範囲を示す
- 制御系の応答速度の指標（帯域幅が広い→速い応答）
- フィルタ設計の基本パラメータ

### 2.2 カットオフ周波数 (Cutoff Frequency)

1次遅れ系 $G(s) = \frac{K}{1+Ts}$ の場合、**カットオフ周波数 (cutoff frequency)** $\omega_c = 1/T$ において：

$$|G(j\omega_c)| = \frac{K}{\sqrt{2}}, \quad \angle G(j\omega_c) = -45°$$

この周波数は**折れ点周波数 (corner frequency)** とも呼ばれ、Bode線図の漸近線が交わる点です。

### 2.3 共振ピークと共振周波数 (Resonance Peak and Resonance Frequency)

2次系 $G(s) = \frac{\omega_n^2}{s^2 + 2\zeta\omega_n s + \omega_n^2}$ において、減衰比 $\zeta < 1/\sqrt{2}$ のとき、**共振周波数 (resonance frequency)** $\omega_r$ でゲインがピークとなります：

$$\omega_r = \omega_n\sqrt{1-2\zeta^2}$$

**共振ピーク (resonance peak)** の大きさは：

$$M_r = \frac{1}{2\zeta\sqrt{1-\zeta^2}}$$

**物理的意味**：
- 機械系の固有振動数に対応
- $\zeta$ が小さいほど共振が顕著（オーバーシュートが大きい）
- 制御系設計では適度な減衰比（$\zeta = 0.5 \sim 0.7$）が望ましい

### 2.4 高周波・低周波でのゲイン漸近挙動 (Asymptotic Gain Behavior)

伝達関数 $G(s) = \frac{b_ms^m + \cdots + b_0}{a_ns^n + \cdots + a_0}$ において：

**低周波域（$\omega \to 0$）**:
- 積分器がなければ: $|G(j\omega)| \to |b_0/a_0|$ （定数）
- 積分器 $r$ 個の場合: $|G(j\omega)| \sim \omega^{-r}$ （傾き $-20r$ dB/decade）

**高周波域（$\omega \to \infty$）**:
- $|G(j\omega)| \sim \omega^{m-n}$ （傾き $20(m-n)$ dB/decade）
- 通常、制御系では $m \leq n$ なので高周波でゲインは減衰

## 3. 位相特性の詳細解析 (Detailed Analysis of Phase Characteristics)

### 3.1 位相遅れ・位相進み (Phase Lag and Phase Lead)

**位相遅れ (phase lag)**: $\angle G(j\omega) < 0$
- 出力が入力より時間的に遅れる
- 積分要素、1次遅れ系が位相遅れを生じる
- 安定性を損なう要因となり得る

**位相進み (phase lead)**: $\angle G(j\omega) > 0$
- 出力が入力より時間的に進む
- 微分要素、1次進み系 $G(s) = 1+Ts$ が位相進みを生じる
- 安定性を改善する効果がある

### 3.2 最小位相系と非最小位相系 (Minimum Phase and Non-Minimum Phase Systems)

**最小位相系 (minimum phase system)**: すべての極と零点が複素平面の左半平面にあるシステム

- ゲイン特性から位相特性が一意に決まる
- 位相遅れが最小（絶対値が最小）
- 多くの物理システムがこれに該当

**非最小位相系 (non-minimum phase system)**: 右半平面に零点を持つシステム

- 同じゲイン特性でもより大きな位相遅れを持つ
- 例: むだ時間系 $e^{-Ls}$、不安定零点を持つ系
- 制御が困難（位相余裕が小さくなる）

### 3.3 全域通過系 (All-Pass System)

**全域通過系 (all-pass system)** は、全周波数でゲインが一定（通常1）で、位相のみが変化するシステムです：

$$G(s) = \frac{s - a}{s + a}, \quad a > 0$$

この場合：
- $|G(j\omega)| = 1$ （すべての $\omega$ で）
- $\angle G(j\omega) = -2\tan^{-1}(\omega/a)$

**応用**: 位相補償、むだ時間の近似、信号処理のフィルタ設計

### 3.4 位相余裕の概念 (Phase Margin Concept)

**位相余裕 (phase margin)** $PM$ は、開ループ伝達関数 $L(s) = G(s)H(s)$ のゲインが0 dBとなる周波数（**ゲイン交差周波数 (gain crossover frequency)** $\omega_{gc}$）における位相の余裕です：

$$PM = 180° + \angle L(j\omega_{gc})$$

**判定基準**:
- $PM > 0$: 安定
- $PM = 0$: 臨界安定
- $PM < 0$: 不安定

**推奨値**: $PM = 30° \sim 60°$ が一般的（$PM = 45°$ が目安）

位相余裕が大きいほど、システムの安定性が高く、ロバスト性が向上します。

## 4. 周波数応答と制御設計 (Frequency Response and Control Design)

### 4.1 ゲイン余裕・位相余裕 (Gain Margin and Phase Margin)

**ゲイン余裕 (gain margin)** $GM$ は、位相が -180° となる周波数（**位相交差周波数 (phase crossover frequency)** $\omega_{pc}$）におけるゲインの余裕です：

$$GM = -20\log_{10}|L(j\omega_{pc})|$$

または、比で表現すると：

$$GM = \frac{1}{|L(j\omega_{pc})|}$$

**関係性**:
- GM と PM は制御系の安定度を定量的に評価
- 両方とも正の値が必要（安定条件）
- 一般的な設計目標: $GM > 6$ dB、$PM > 30°$

### 4.2 開ループ周波数応答から閉ループ特性の推定 (Estimating Closed-Loop Characteristics)

開ループ伝達関数 $L(s) = G(s)H(s)$ から、閉ループ伝達関数 $T(s) = \frac{G(s)}{1+G(s)H(s)}$ の特性を推定できます：

**帯域幅**: 開ループのゲイン交差周波数 $\omega_{gc}$ ≈ 閉ループの帯域幅

**オーバーシュート**: 位相余裕 $PM$ と相関
- $PM \approx 60°$ → オーバーシュート ≈ 10%
- $PM \approx 45°$ → オーバーシュート ≈ 20%
- $PM \approx 30°$ → オーバーシュート ≈ 40%

### 4.3 制御器設計への活用 (Application to Controller Design)

#### 位相進み補償 (Phase Lead Compensation)

位相進み補償器 $G_c(s) = K_c\frac{1+aTs}{1+Ts}$（$a > 1$）を用いて：
- ゲイン交差周波数付近で位相を進める
- 位相余裕を増加させ、安定性を改善
- 応答速度を向上

#### 位相遅れ補償 (Phase Lag Compensation)

位相遅れ補償器 $G_c(s) = K_c\frac{1+Ts}{1+aTs}$（$a > 1$）を用いて：
- 低周波域でゲインを上げる（定常偏差の改善）
- ゲイン交差周波数を下げて安定性を確保
- 高周波ノイズの抑制

## 5. 練習問題と計算例 (Practice Problems and Examples)

### 5.1 複合系のBode線図作成

**問題**: $G(s) = \frac{100}{s(1+0.1s)(1+0.01s)}$ のBode線図を描け。

**解**:
1. 因数分解: $G(s) = \frac{100}{s} \cdot \frac{1}{1+0.1s} \cdot \frac{1}{1+0.01s}$
2. 折れ点周波数: $\omega_1 = 10$ rad/s、$\omega_2 = 100$ rad/s
3. 各要素のゲイン:
   - $100$: $+40$ dB
   - $1/s$: $-20$ dB/decade
   - $1/(1+0.1s)$: $\omega > 10$ で $-20$ dB/decade
   - $1/(1+0.01s)$: $\omega > 100$ で $-20$ dB/decade
4. 合成:
   - $\omega < 10$: 傾き $-20$ dB/decade
   - $10 < \omega < 100$: 傾き $-40$ dB/decade
   - $\omega > 100$: 傾き $-60$ dB/decade

### 5.2 Nyquist線図からの特性読み取り

**問題**: ある系のNyquist線図が点 $(-1, 0)$ の近くを通過する。この系の安定性を論じよ。

**解**: Nyquist安定判別法により、開ループ伝達関数の軌跡が点 $(-1, 0)$ を囲む回数から閉ループの安定性が判定できます。点 $(-1, 0)$ の近くを通過する場合、位相余裕とゲイン余裕が小さく、安定性が低い（または不安定に近い）ことを示します。

### 5.3 実システムの周波数応答解析例

**問題**: DCモータの伝達関数 $G(s) = \frac{K}{s(1+\tau s)}$ において、$K = 10$、$\tau = 0.05$ s のとき、カットオフ周波数と位相余裕を求めよ。

**解**:
- 折れ点周波数: $\omega_c = 1/\tau = 20$ rad/s
- ゲイン交差周波数を求める: $|G(j\omega_{gc})| = 1$ より計算
- 位相余裕: $PM = 180° + \angle G(j\omega_{gc})$

（詳細な数値計算は省略）

---

## まとめ

この理解者向けノートでは、以下の発展的内容を学びました：

- LTIシステムの正弦波定常応答の数学的導出とフーリエ変換との関係
- 帯域幅、カットオフ周波数、共振周波数などのゲイン特性の詳細
- 最小位相系・非最小位相系、全域通過系などの位相特性の理論
- ゲイン余裕・位相余裕による安定性の定量的評価
- 開ループから閉ループ特性の推定方法
- 位相進み/遅れ補償器による制御器設計の基礎
- 複合系のBode線図作成やNyquist線図解析の実践例

これらの知識により、周波数領域での制御系解析と設計の基礎が確立されました。第10章では、これらを活用した安定性判別法（Nyquist安定判別法など）を詳しく学習します。
