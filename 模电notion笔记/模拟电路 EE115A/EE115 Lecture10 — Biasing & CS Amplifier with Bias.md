# EE115 Lecture10 — Biasing & CS Amplifier with Bias

<aside>
📌

**本节主题：** Bias Circuit + Capacitively-coupled CS Amplifier

**参考书：** Sedra/Smith — pp. 454–479（Chapter 7 §7.5：放大器的偏置与耦合）

**核心脉络：** 单管放大器复习 → 偏置电路的目的 → 不理想偏置 → 加 $R_s$ 引入负反馈 → 三种实用偏置拓扑 → drain-to-gate feedback → 耦合 / 旁路电容 → 完整 CS 放大器 → 频响

</aside>

## 📅 课程行政信息

- [x]  **Homework 5** — Sedra/Smith Ch.7 题 **7.92, 7.98, 7.102, 7.117, 7.118**（截止：April 22, 2026 11:00 PM）

---

## 1️⃣ Review — 一管放大器的三个基本拓扑（slides 3–7）

### 1.1 饱和区 I-V（slide 3–4）

[Slide 3 — Drain Current vs Gate Voltage](https://app.notion.com)

Slide 3 — Drain Current vs Gate Voltage

[Slide 4 — I-V Curves of NMOS](https://app.notion.com)

Slide 4 — I-V Curves of NMOS

$$
i_D = \tfrac{1}{2}k_n'\!\left(\frac{W}{L}\right)(v_{GS}-V_{tn})^2 = \tfrac{1}{2}k_n'\!\left(\frac{W}{L}\right)V_{OV}^2
$$

- Triode 区：$v_{DS}\le V_{OV}$
- Saturation 区：$v_{DS}\ge V_{OV}$（放大器工作区）
- 边界点：$v_{DS}=V_{OV}$ 时 $i_D=\tfrac{1}{2}k_n'(W/L)v_{DS}^2$

### 1.2 小信号模型（slide 5）

[Slide 5 — Small-signal model for MOSFET](https://app.notion.com)

Slide 5 — Small-signal model for MOSFET

$$
g_m = k_n V_{OV} = \sqrt{2k_n'(W/L)\, I_D},\qquad r_o = \frac{|V_A|}{I_D},\qquad V_{OV}=\sqrt{\frac{2I_D}{k_n'(W/L)}}
$$

NMOS / PMOS 用同一套等效电路；PMOS 把所有量取绝对值并把 $k_n\to k_p$。

### 1.3 三种基本配置（slides 6–7）

[Slide 6 — Basic Single-Transistor Amplifier Configurations](https://app.notion.com)

Slide 6 — Basic Single-Transistor Amplifier Configurations

| 类型 | $R_{\text{in}}$ | $A_{vo}$（开路） | $R_o$ | $A_v$（带 $R_L$） | 用途口诀 |
| --- | --- | --- | --- | --- | --- |
| **Common Source (CS)** | $\infty$ | $-g_m R_D$ | $R_D$ | $-g_m(R_D\|R_L)$ | 电压增益大、反相 |
| **CS with** $R_s$ | $\infty$ | $-\dfrac{g_m R_D}{1+g_m R_s}$ | $R_D$ | $-\dfrac{g_m(R_D\|R_L)}{1+g_m R_s}$ | 降增益换线性度 / 带宽 |
| **Common Gate (CG)** | $1/g_m$ | $g_m R_D$ | $R_D$ | $g_m(R_D\|R_L)$ | 低 $R_{\text{in}}$、电流缓冲 |
| **Source Follower (CD)** | $\infty$ | $1$ | $1/g_m$ | $\dfrac{R_L}{R_L+1/g_m}$ | 电压跟随器 / 低输出阻抗 |

<aside>
💡

**记忆口诀：** CS 给电压增益、CG 给电流缓冲、SF 给低输出阻抗。三者构成 IC 中后续 cascode / 差分 / op-amp 的全部砖块。

</aside>

---

## 2️⃣ 为什么需要偏置电路？（slide 8）

<aside>
🎯

**Bias 的三大目标**：

1. 提供稳定的 DC drain 电流 $I_D$；
2. 对器件参数（$V_t,\ k_n$）变化、温度漂移**不敏感**；
3. 让 MOSFET 落在 I-V 曲线的平坦段（**saturation**），留出足够的输出摆幅。
</aside>

物理来源：$\mu_n$ 与 $V_t$ 都随温度变化（典型 $-2\ \text{mV/K}$ 的 $V_t$ 漂移和 $T^{-3/2}$ 的 $\mu$ 漂移）。

---

## 3️⃣ 反例：电阻分压固定 $V_{GS}$（slide 9）

[Slide 9 — Example of Non-Ideal Bias](https://app.notion.com)

Slide 9 — Example of Non-Ideal Bias

**电路**：$V_{DD}\to R_{G1}\to V_G\to R_{G2}\to \text{GND}$，$V_G=V_{GS}$（栅源直接接 $V_G$，源极接地）。

$$
I_D=\tfrac{1}{2}\mu_n C_{ox}\frac{W}{L}(V_{GS}-V_t)^2
$$

<aside>
⚠️

**踩坑**：$V_{GS}$ 由分压固定 → $(V_{GS}-V_t)$ 中只要 $V_t$ 漂一点，$I_D$ 就**平方放大**地跳变。两支同型管之间的 $I_D$ 差异可达数十 %。**完全不能用**。

</aside>

---

## 4️⃣ 加源极电阻 $R_s$ — 负反馈稳流（slide 10）

[Slide 10 — Adding Source Resistance Rs](https://app.notion.com)

Slide 10 — Adding Source Resistance Rs

**电路**：在源极串入 $R_s$ 到地，栅极接固定 $V_G$。

$$
V_G = V_{GS} + R_s I_D \Rightarrow I_D = \frac{V_G - V_{GS}}{R_s}
$$

图解：在 $i_D\!-\!v_{GS}$ 坐标上画 **负载线** $i_D = (V_G - v_{GS})/R_s$（斜率 $-1/R_s$），它与器件曲线的交点即工作点。

### 负反馈机理

- 若 $I_D\!\uparrow$ → $V_S = R_s I_D\!\uparrow$ → $V_{GS} = V_G - V_S\!\downarrow$ → $I_D\!\downarrow$（拉回）
- 反之亦然。

<aside>
🔑

**核心思想**：让 $V_{GS}$ 不再被外部「钉死」，而由电流自己决定 → 器件 / 温度变化只引起一阶小修正，而非平方放大。

</aside>

---

## 5️⃣ 三种实用偏置拓扑（slide 11）

[Slide 11 — Practical Bias Implementations](https://app.notion.com)

Slide 11 — Practical Bias Implementations

### A. Single Power Supply

栅端 $V_G$ 用 $R_{G1},R_{G2}$ 分压设定，源端串 $R_s$。**最常见**。

### B. Dual Power Supply（$\pm V_{DD}$）

栅直接接地（经大 $R_G$ 抑制 leakage），源经 $R_s$ 接 $-V_{SS}$。**省一颗分压电阻**，常见于实验室电路。

### C. Drain-to-Gate Feedback

$R_G$ 直接把漏端接回栅端。$V_{GS}=V_{DS}$，自动让管子工作在**饱和边界**。详见 §7。

---

## 6️⃣ 例题 1：Single-Supply Bias 设计（slides 12–13）

[Slide 12 — Single-Supply Bias Design Example](https://app.notion.com)

Slide 12 — Single-Supply Bias Design Example

**给定**：$V_{DD}=15\ \text{V}$，$I_D=0.5\ \text{mA}$，$V_t=1\ \text{V}$，$k_n'W/L=1\ \text{mA/V}^2$，$\lambda=0$。

### 设计规则（rule of thumb）

把 $V_{DD}$ 平均分给 $R_D,\ V_{DS},\ R_s$ —— 每段约 1/3：

- $V_D = +10\ \text{V}$，$V_S = +5\ \text{V}$
- $R_D = (V_{DD}-V_D)/I_D = 5/0.5 = 10\ \text{k}\Omega$
- $R_s = V_S/I_D = 5/0.5 = 10\ \text{k}\Omega$

### 求 $V_{GS},\ V_G$

$$
0.5 = \tfrac{1}{2}\cdot 1\cdot V_{OV}^2 \Rightarrow V_{OV} = 1\ \text{V}
$$

$$
V_{GS} = V_t + V_{OV} = 2\ \text{V},\quad V_G = V_S + V_{GS} = 5+2 = 7\ \text{V}
$$

选 $R_{G1}=8\ \text{M}\Omega,\ R_{G2}=7\ \text{M}\Omega$（栅极漏电几乎为零，可以用很大值以保高 $R_{\text{in}}$）。

### Sensitivity check：把 $V_t$ 换成 $1.5\ \text{V}$ 会怎样？

$$
I_D = \tfrac{1}{2}(V_{GS}-1.5)^2,\quad V_G = V_{GS} + 10 I_D = 7
$$

联立 → $I_D = 0.455\ \text{mA}$，相对变化

$$
\frac{\Delta I_D}{I_D} = \frac{-0.045}{0.5} = -9\%
$$

<aside>
💡

**对比**：若没有 $R_s$（纯分压），$V_t$ 漂 $0.5\ \text{V}$ 会让 $I_D$ **跌掉 75 %**。加 $R_s$ 后只 $-9\%$ —— 这就是负反馈的威力。

</aside>

---

## 7️⃣ Drain-to-Gate Feedback Bias（slide 14）

[Slide 14 — Drain-to-Gate Feedback Bias](https://app.notion.com)

Slide 14 — Drain-to-Gate Feedback Bias

**电路**：$V_{DD}\to R_D\to V_D=V_G\to$（管子漏）→ 经 $R_G$ 反馈到栅。栅电流 0，故 $R_G$ 上无压降，$V_G=V_D$。

$$
V_{GS} = V_{DS} = V_{DD} - R_D I_D
$$

$$
V_{DD} = V_{GS} + R_D I_D
$$

性质：

- $V_{DS}=V_{GS}>V_{GS}-V_t=V_{OV}$ 永远成立 → 管子**永远在饱和区**（自偏置）。
- 适合做 current mirror / 单管低增益级（后续 Lecture 11）。

---

## 8️⃣ 耦合电容 / 旁路电容（slides 15–16）

### 8.1 耦合电容 $C_C$（slide 15）

[Slide 15 — Coupling Capacitor Separates AC Signal from DC Bias](https://app.notion.com)

Slide 15 — Coupling Capacitor Separates AC Signal from DC Bias

- DC 上开路、AC 上短路。
- 把信号源 $v_{\text{sig}}$ 与 bias 网络分开 → $v_{\text{sig}}$ 不会拉偏栅极 DC 电平。
- **副作用**：在低频形成 RC 高通 → 引入低频截止 $f_L$。

### 8.2 旁路电容 $C_S$（slide 16）

并联在 $R_s$ 上。

- DC：$R_s$ 仍存在 → 偏置稳定；
- AC：$C_S$ 把 $R_s$ 短到地 → 信号通路上**没有源极退化**，增益拿满 $-g_m R_D$。

<aside>
🔑

**口诀：耦合电容隔 DC、旁路电容旁 AC**。两类电容一起用，**DC 偏置稳、AC 增益高、低频被 cap 限**。

</aside>

### 8.3 完整 CS 放大器分析（slide 16）

[Slide 16 — CS Amplifier with Bias Circuit](https://app.notion.com)

Slide 16 — CS Amplifier with Bias Circuit

$$
R_{\text{in}} = R_{G1}\|R_{G2},\qquad R_o = R_D\|r_o
$$

$$
G_v = -\,\frac{R_{\text{in}}}{R_{\text{in}}+R_{\text{sig}}}\, g_m\,(R_D\|R_L\|r_o)
$$

第一项是输入分压衰减，第二项是真正的器件增益。在 IC 里我们希望 $R_{\text{in}}\gg R_{\text{sig}}$，分压损失可忽略。

---

## 9️⃣ 典型频响（slide 17）

[Slide 17 — Typical Frequency Response of Capacitively Coupled Amplifier](https://app.notion.com)

Slide 17 — Typical Frequency Response of Capacitively Coupled Amplifier

```
|Vo/Vsig|(dB)
        ┌──────────────┐
        │   Midband    │
        │  20·log|A_M| │
   3dB ─┤              ├─ 3dB
       /                \
      / Low band         \ High band
     /  (C_C1,C_C2,C_E)   \ (内部 Cgs,Cgd)
  ──/──────fL──────fH─────\──→ f (log)
```

| 频段 | 主导效应 | 说明 |
| --- | --- | --- |
| Low band（$f<f_L$） | $C_{C1},\ C_{C2},\ C_S(=C_E)$ | 耦合 / 旁路电容阻抗增大 → 增益滑落 |
| Midband（$f_L<f<f_H$） | 所有电容可忽略 | 平坦段，$A_M$ 由 $g_m, R_D, R_L$ 决定 |
| High band（$f>f_H$） | $C_{gs},\ C_{gd}$（Miller 效应） | 器件内寄生电容 → 增益滑落 |

<aside>
⚠️

**踩坑**：discrete CS 才有 $f_L$ 的存在（因为耦合电容是外接的）；IC 里 DC-coupled，$f_L=0$，只看 $f_H$（这就是 Lecture 11+ IC 设计哲学的出发点之一）。

</aside>

---

## 📝 本节总结（本节记三条）

<aside>
🎯

1. **Bias = 三件事**：稳流、抗温漂 / 器件漂、留摆幅。
2. $R_s$ **是 discrete bias 的灵魂**：负反馈让 $I_D$ 对 $V_t$ 漂移从「平方放大」降到「一阶小修正」。Rule of thumb：$V_{DD}$ 平分给 $R_D, V_{DS}, R_s$。
3. **耦合 cap 隔 DC、旁路 cap 旁 AC**：DC 偏置 / AC 增益解耦，代价是带宽下限 $f_L$。IC 设计直接 DC coupled 把它消掉。
</aside>

---

## 📚 Homework 5

- [x]  Sedra/Smith 7.92
- [x]  Sedra/Smith 7.98
- [x]  Sedra/Smith 7.102
- [x]  Sedra/Smith 7.117
- [x]  Sedra/Smith 7.118

*Due: 2026-04-22 23:00*

[EE115 Lecture10addreview.pdf](EE115%20Lecture10%20%E2%80%94%20Biasing%20&%20CS%20Amplifier%20with%20Bias/EE115_Lecture10addreview.pdf)