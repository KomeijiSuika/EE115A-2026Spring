# EE115 Lecture8 — Transistor Amplifier 基础与 Small-Signal Model

<aside>
📌

**本节主题：** Transistor Amplifier 1 —— 从 I–V 曲线 → 偏置点 → 小信号模型 → 系统化分析流程

**参考书：** Sedra/Smith — pp. 367–393（Chapter 7 §7.1）

**核心脉络：** 为什么 MOSFET 能做放大器（饱和区是 flat 区）→ VTC 上找个 Q 点 → 迭加小信号 $v_{gs}$ → 用 $g_m, r_o$ 的 hybrid-π 模型取代 MOSFET → 四步走完任意放大器。

</aside>

## 📅 课程行政信息

- Homework 本节为占位（“Homeworkx-due March xx 11p.m.”），实际习题随 Lecture 9 / Lecture 10 发布。

---

## 1️⃣ 为什么饱和区能放大？slides 3–4

### 饱和区的物理图像（slide 3）

[Slide 3 — Transistor Operating Mode in Amplifiers](https://app.notion.com)

Slide 3 — Transistor Operating Mode in Amplifiers

- 条件：$v_{GS} = V_t + V_{OV}$ 且 $v_{DS}\ge V_{OV}$（等价于 $v_{GD}\le V_{tn}$）。
- 现象：沟道在 drain 侧 **pinched off**，漏电压变动不再改变电流。

$$
i_D = \tfrac{1}{2}k_n(v_{GS}-V_t)^2
$$

<aside>
💡

“饱和=平”是放大器的必要条件：$i_D$ 只与 $v_{GS}$ 线性相关，不依赖 $v_{DS}$ → 这使 MOSFET 担任 **跨导 (transconductance)** 放大器。

</aside>

### 从跨导到电压增益（slide 4）

[Slide 4 — Voltage Transfer Curves](https://app.notion.com)

Slide 4 — Voltage Transfer Curves

加上负载电阻 $R_D$后：

$$
v_{DS} = V_{DD} - i_D R_D
$$

这是 **载荷线**，与 $i_D$–$v_{DS}$ 曲线家族的交点定位出 Q 点。

### VTC 三区（slide 4 右下）

| 区间 | $v_{GS}$ 范围 | MOSFET 状态 | $v_{DS}$ | 能否放大？ |
| --- | --- | --- | --- | --- |
| **A (Cut-off)** | $v_{GS}<V_t$ | 截止 | $v_{DS}=V_{DD}$ | ❌ |
| **B (Saturation)** | $V_t<v_{GS}<V_{GS}|_B$ | **饱和** | $v_{DS}=V_{DD}-\tfrac{1}{2}k_n R_D(v_{GS}-V_t)^2$ | ✅（VTC 斜率最大 = 增益） |
| **C (Triode)** | $v_{GS}>V_{GS}|_B$ | 三极（压零） | $v_{DS}\to 0$ | ❌ |

---

## 2️⃣ 偏置与小信号叠加（slide 5）

[Slide 5 — Biasing the amplifier](https://app.notion.com)

Slide 5 — Biasing the amplifier

总输入电压：

$$
v_{GS}(t) = V_{GS} + v_{gs}(t)
$$

饱和区展开（slide 5 右下）：

$$
v_{DS} = V_{DD} - \tfrac{1}{2}k_n R_D(V_{GS}-V_t)^2
$$

Q 点附近的电压增益：

$$
A_v = \left.\frac{d v_{DS}}{d v_{GS}}\right|_{v_{GS}=V_{GS}} = -k_n(V_{GS}-V_t)R_D = -k_n V_{OV}R_D = -g_m R_D
$$

<aside>
🔑

**Q 点斜率 = 电压增益**。增益 ∝ $R_D$，但 $R_D$ 受 $V_{DD}$ 和偏置电流约束→ 后面引出 active load。

</aside>

---

## 3️⃣ Example 7.1 — 完整数值（slides 6–7）

**已知：** $V_t=0.4\ \text{V}$，$k_n'=0.4\ \text{mA/V}^2$，$W/L=10$，$\lambda=0$，$V_{DD}=1.8\ \text{V}$，$R_D=17.5\ \text{k}\Omega$，$V_{GS}=0.6\ \text{V}$。

### (a) 求 $V_{OV}, I_D, V_{DS}, A_v$

$V_{OV}=0.6-0.4=0.2\ \text{V}$

$$
I_D = \tfrac{1}{2}\cdot 0.4\cdot 10\cdot 0.2^2 = 0.08\ \text{mA}
$$

$$
V_{DS} = 1.8 - 17.5\times 0.08 = 0.4\ \text{V}
$$

检查：$V_{DS}=0.4>V_{OV}=0.2$ ✅ 饱和。

$$
A_v = -k_n'\tfrac{W}{L}V_{OV}R_D = -0.4\times 10\times 0.2\times 17.5 = -14\ \text{V/V}
$$

### (b) 最大对称摆幅

- 漏端负向摆幅边界是 $V_{DS}-V_{OV}=0.2\ \text{V}$；正向摆幅边界是 $V_{DD}-V_{DS}=1.4\ \text{V}$。
- 取小者 → $\hat v_{ds}=\pm 0.2\ \text{V}$。
- 转换为输入：$\hat v_{gs}=\hat v_{ds}/|A_v|=0.2/14\approx 14.2\ \text{mV}$。

### 严格版边界检查（slide 7）

要保证 $v_{DS,\min}\ge v_{GS,\max}-V_t$：

$$
0.4-|A_v|\hat v_{gs}\ge 0.6+\hat v_{gs}-0.4
\Rightarrow \hat v_{gs}\le \frac{0.2}{|A_v|+1}\approx 13.3\ \text{mV}
$$

<aside>
⚠️

**踩坑**：快估 14.2 mV 与严格 13.3 mV 差一米，是因为饱和边界本身也随 $v_{GS}$ 动（$v_{GS}-V_t$ 中的 $v_{GS}$ 是瞬时值）。考试里只要填 $\hat v_{gs}=\hat v_{ds}/|A_v|$ 通常够用；但要看似边界题记得加 +1。

</aside>

---

## 4️⃣ 载荷线与偏置点选择（slides 8–10）

### Maximum signal amplitude（slide 8）

[Slide 8 — Maximum Input Signal Amplitude](https://app.notion.com)

Slide 8 — Maximum Input Signal Amplitude

两个波形叠在一起 → 保证 $v_{DS,\min}\ge v_{GS,\max}-V_t$ 始终成立。

### Load line 图解（slide 9）

[Slide 9 — Graphical Analysis with Load Line](https://app.notion.com)

Slide 9 — Graphical Analysis with Load Line

$$
i_D = \frac{V_{DD}}{R_D} - \frac{v_{DS}}{R_D}
$$

- 载荷线斜率 $-1/R_D$；与 $i_D$–$v_{DS}$ 曲线家族交于 Q 点。
- 三个特殊点：A（截止）、Q（饱和中点）、B（饱和/三极边界）、C（三极区）。

### 偏置点选择（slide 10）

[Slide 10 — Consideration for Bias Point](https://app.notion.com)

Slide 10 — Consideration for Bias Point

<aside>
⚠️

**两种坏偏置：**

1. $Q_1$ 太贴 $V_{DD}$ → 正向摆幅被刪。
2. $Q_2$ 太贴三极区 → 负向摆幅被刪。

**经验值**：把 Q 点设在 $V_{DS}\approx V_{DD}/3$ 附近，允许两边各留一些余量。

</aside>

---

## 5️⃣ MOSFET 小信号模型（slide 11）

[Slide 11 — Hybrid-π 小信号模型](https://app.notion.com)

Slide 11 — Hybrid-π 小信号模型

### 三个核心公式

$$
g_m \equiv \left.\frac{\partial i_D}{\partial v_{GS}}\right|_{v_{GS}=V_{GS}} = k_n V_{OV} = \frac{2I_D}{V_{OV}} = \sqrt{2k_n' (W/L) I_D}
$$

$$
V_{OV} = \sqrt{2I_D / (k_n'(W/L))}
$$

$$
r_o = \frac{|V_A|}{I_D}
$$

### 辨识三种 $g_m$ 公式的使用场景

| 公式 | 使用时机 |
| --- | --- |
| $g_m = k_n V_{OV}$ | 已知 $V_{OV}$ |
| $g_m = 2I_D/V_{OV}$ | 已知 $I_D$ 与 $V_{OV}$（最常用） |
| $g_m = \sqrt{2k_n'(W/L)I_D}$ | 已知几何参数与 $I_D$，$V_{OV}$ 未知 |

<aside>
💡

NMOS / PMOS 小信号模型**完全一样**，只要把 $V_{GS}, V_t, V_{OV}, V_A, k_n$ 加绝对值 / 换为 $k_p$ 即可。

</aside>

---

## 6️⃣ 小信号模型的推导（slide 12）

把 $v_{GS}=V_{GS}+v_{gs}$ 代入 $i_D=\tfrac{1}{2}k_n(v_{GS}-V_t)^2$：

$$
i_D = \underbrace{\tfrac{1}{2}k_n(V_{GS}-V_t)^2}_{I_D} + \underbrace{k_n(V_{GS}-V_t)v_{gs}}_{i_d=g_m v_{gs}} + \underbrace{\tfrac{1}{2}k_n v_{gs}^2}_{\text{distortion}}
$$

<aside>
⚠️

**小信号近似的本质**：丢掉 $\tfrac{1}{2}k_n v_{gs}^2$。需要 $v_{gs}\ll V_{OV}$ 才可志；否则产生二次谐波 (HD2)。小信号是**线性化假设**，不是小电压。

</aside>

---

## 7️⃣ 系统化分析步骤（slide 13）⭐

<aside>
🎯

**Notion-style 四步走法：**

1. **DC 分析** ：小信号源置零 → 求 $V_{GS}, I_D, V_{DS}$，确认饱和。
2. **小信号参数**：$g_m=2I_D/V_{OV}$、$r_o=|V_A|/I_D$。
3. **AC 等效电路**：$V_{DD}, V_{SS}$ 在小信号看为地，直流电流源看为开路，耦合 / bypass 电容看为短路，MOSFET 换为 hybrid-π。
4. **电路求解**：KCL / KVL 求 $A_v, R_{in}, R_o$。
</aside>

---

## 8️⃣ Example 7.3 — 完整走一遍（slides 14–16）

**拓扑**：Self-bias CS 放大器。$V_{DD}=+15\ \text{V}$，$R_D=R_L=10\ \text{k}\Omega$，$R_G=10\ \text{M}\Omega$（drain-to-gate 反馈）。

MOSFET：$V_t=1.5\ \text{V}$，$k_n=0.25\ \text{mA/V}^2$，$V_A=50\ \text{V}$。

### (1) DC Bias（slide 14）

[Slide 14 — MOSFET Amplifier Example: DC Bias Point](https://app.notion.com)

Slide 14 — MOSFET Amplifier Example: DC Bias Point

因 $I_G=0$ → $R_G$ 上无压降 → $V_{GS}=V_{DS}$。代入饱和区公式：

$$
V_{DS} = V_{DD} - I_D R_D = V_{DD} - \tfrac{1}{2}k_n(V_{GS}-V_t)^2 R_D
$$

联立解（设 $V_{GS}=V_{DS}=x$）：

$$
x = 15 - \tfrac{1}{2}(0.25)(x-1.5)^2(10)\Rightarrow x\approx 3.5\ \text{V}
$$

得 $V_{GS}=V_{DS}=3.5\ \text{V}$，$V_{OV}=2.0\ \text{V}$，$I_D = (15-3.5)/10\ \text{k}\Omega = 1.15\ \text{mA}$（书上取 $\approx 1.06\ \text{mA}$）。

### (2) 小信号参数

$$
g_m = \frac{2I_D}{V_{OV}} = \frac{2\times 1.06}{2.0}\approx 1.06\ \text{mA/V}
$$

$$
r_o = \frac{V_A}{I_D} = \frac{50}{1.06}\approx 47\ \text{k}\Omega
$$

### (3) AC 等效（slide 15）

[Slide 15 — Solve AC Small Signal Circuit](https://app.notion.com)

Slide 15 — Solve AC Small Signal Circuit

耦合电容为 $\infty$ → 看作短路。$v_{gs}=v_i$（因 $R_G\gg$）。

输出节点负载：

$$
R_L' = R_L\|R_D\|r_o = 10\|10\|47\approx 4.34\ \text{k}\Omega
$$

增益（忽略 $R_G$ 反馈次要项）：

$$
A_v = -g_m R_L' = -1.06\times 4.34\approx -4.6\ \text{V/V}
$$

### (4) Additional Parameters（slide 16）

[Slide 16 — Additional Parameters of Interest](https://app.notion.com)

Slide 16 — Additional Parameters of Interest

$R_G$ 从 drain 接到 gate 是个 Miller 反馈电阻：

$$
R_{in} = \frac{R_G}{1+g_m R_L'} = \frac{10\ \text{M}\Omega}{1+1.06\times 4.34}\approx 1.78\ \text{M}\Omega
$$

$$
R_o = R_D\|r_o\|\frac{R_G}{1+g_m R_{sig}}\approx R_D\|r_o \approx 8.25\ \text{k}\Omega
$$

<aside>
🔑

**Drain-to-gate 反馈带来的代价**：输入阻抗被 Miller 效应拉低、增益也被轻微弱化，**换取的是偏置的自动稳定**（Lecture 10 详谈）。

</aside>

---

## 📝 本节总结（本节记三条）

<aside>
🎯

1. **饱和区 = 平 = 能放大**：$v_{DS}\ge V_{OV}$ 是偏置第一原则，Q 点隐藏足够摆幅。
2. **小信号三口诀**：$g_m=2I_D/V_{OV}$、$r_o=|V_A|/I_D$、$A_v|_{ideal load}=-g_m r_o = -A_0$。
3. **四步法分析任意放大器**：DC → 参数 → AC 等效 → KCL/KVL。只要 hybrid-π 画对了，后面都是线性电路题。
</aside>

---

## 🔗 与上下文衔接

- 下一节 Lecture 9 把本节的四步法走到 CS / CG / CD 三种拓扑上，并引入 T-model。
- 偏置电路的完整设计（包括 source degeneration、drain-to-gate 反馈实现）在 [EE115 Lecture10 — Biasing & CS Amplifier with Bias](EE115%20Lecture10%20%E2%80%94%20Biasing%20&%20CS%20Amplifier%20with%20Bias.md)。

## 📚 Homework（占位）

- [ ]  SEDRA/SMITH Book chapter xx problem xx — due March xx 23:00

[EE115 Lecture8.pdf](EE115%20Lecture8%20%E2%80%94%20Transistor%20Amplifier%20%E5%9F%BA%E7%A1%80%E4%B8%8E%20Small-Si/EE115_Lecture8.pdf)