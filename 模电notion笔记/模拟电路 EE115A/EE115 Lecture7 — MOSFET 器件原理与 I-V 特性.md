# EE115 Lecture7 — MOSFET 器件原理与 I-V 特性

<aside>
📌

**本节主题：** MOSFET 物理结构、通道形成机制、Triode / Saturation 两大工作区、PMOS / CMOS、Circuit symbol、沟道长调制 (CLM) 与输出阻抗 $r_o$、Diode-connected 拓扑 + 两个 DC 偏置例题。

**参考书：** Sedra/Smith — pp. 247–276（Chapter 5 §5.1–5.3）

**核心脉络：** 在 oxide 上加正 $v_{GS}$ → 衬底底部诱导出电子沟道 → 沟道电流 $i_D$ 随 $v_{GS}, v_{DS}$ 演化 → 三个区（cutoff / triode / saturation）→ CLM 让饱和区出现有限 $r_o$ → PMOS 是对偶版 → 拼成 CMOS → DC 偏置例题练手。

</aside>

## 📅 课程行政信息

- [x]  **Homework 3** — Sedra/Smith Ch.3 题 **3.13, 3.20, 3.24** + Ch.5 题 **5.2, 5.7, 5.18, 5.20**（截止：April 8, 2026 11:00 PM）

---

## 1️⃣ MOSFET 是什么（slide 3–4）

[Slide 4 — NMOSFET 物理结构与栅控沟道](https://app.notion.com)

Slide 4 — NMOSFET 物理结构与栅控沟道

- **MOSFET = Metal–Oxide–Semiconductor Field-Effect Transistor**：栅极 (Gate) 通过 oxide 层隔离地操控沟道电流。
- 典型几何尺寸（决定速度、面积、噪声）：
    - 沟道长度 $L \sim 3\ \text{nm} \sim 0.35\ \mu\text{m}$
    - 沟道宽度 $W \sim 0.05\ \mu\text{m} \sim 100\ \mu\text{m}$
    - 氧化层厚度 $t_{ox} \sim 1\sim 10\ \text{nm}$

### NMOS 四端结构

| 端子 | 掺杂 | 角色 |
| --- | --- | --- |
| **Source (S, 源)** | n+ | 电子来源 |
| **Drain (D, 漏)** | n+ | 电子去向 |
| **Gate (G, 栅)** | 金属 / 多晶硅（通过 oxide 隔离） | 电场操控沟道 |
| **Body (B, 衬底)** | p-doped | 第 4 端，通常接最低电位 |

<aside>
💡

**N 沟道**靠电子导电；电流方向：**D → S**（注意：电流逆电子方向）。NMOS 的 D 永远比 S 电位高。

</aside>

---

## 2️⃣ 通道是怎么形成的（slide 5）

- MOS = 一颗以 oxide 为介质的电容器。栅极加正电压时，电场把衬底里的少数载流子 (电子) 拉到 oxide 下方聚集。
- 当 $v_{GS} > V_t$（阈值电压）时，电子浓度足够高 → 形成连通 S/D 的 **inversion channel (反型层)**。
- **栅电容**：$C_{ox} = \varepsilon_{ox}/t_{ox}$，其中 $\varepsilon_{ox} = 3.9\varepsilon_0$。
- 沟道里诱导出的总电荷（绝对值）：

$$
|Q| = C_{ox}\cdot WL\cdot (v_{GS}-V_t)
$$

- **过驱电压 (Overdrive Voltage)**：$v_{OV}\equiv v_{GS}-V_t$ —— 后面所有 MOSFET 公式的核心变量。

<aside>
🔑

**口诀**：$V_t$ 是"开门门槛"，$v_{OV}$ 是"开门程度"。$v_{OV}\uparrow$ → 沟道电荷 ∝ $v_{OV}$，电流 ∝ $v_{OV}^2$。

</aside>

---

## 3️⃣ 小 $v_{DS}$ —— MOSFET 当电阻用（slide 6–7）

[Slide 6 — 小 vDS 下的线性 iD-vDS 特性](https://app.notion.com)

Slide 6 — 小 vDS 下的线性 iD-vDS 特性

沟道形成后，再加一个小的 $v_{DS}$：

- 沟道里**线性**电荷密度（沿沟道）：$|Q|/L = C_{ox}W\, v_{OV}$
- 沿沟道电场（小 $v_{DS}$ 时近似均匀）：$|E| = v_{DS}/L$
- 电子漂移速度 $v = \mu_n |E|$，故漏电流 = 电荷密度 × 速度：

$$
i_D = (\mu_n C_{ox})\frac{W}{L}\, v_{OV}\, v_{DS} = k_n'\frac{W}{L}\, v_{OV}\, v_{DS}
$$

### 关键参数

- **工艺跨导参数**：$k_n' \equiv \mu_n C_{ox}$（process transconductance parameter，单位 $\text{A/V}^2$，由工艺决定，不能改）
- **器件跨导参数**：$k_n \equiv k_n'(W/L)$（设计可调，靠版图 $W/L$）
- **栅控电阻**：$r_{DS} = \dfrac{1}{k_n'(W/L)\, v_{OV}}$

<aside>
💡

**在小** $v_{DS}$ **段，MOSFET 就是一个 「**$v_{OV}$ **控阻」的可变电阻**——这正是 switch / pass transistor 在 digital 设计里的用法。

</aside>

---

## 4️⃣ Triode Region（$v_{DS} < v_{OV}$，slide 8）

[Slide 8 — Triode 区沟道电位与积分推导](https://app.notion.com)

Slide 8 — Triode 区沟道电位与积分推导

当 $v_{DS}$ 不再小时，沟道沿 $x$ 方向的电位不再为常数：

- Source 端电位为 0、Drain 端为 $v_{DS}$。
- 沟道每一点的局部 $v_{OV}(x) = v_{OV} - v(x)$。
- 对沟道积分得完整 Triode 区电流：

$$
i_D = k_n'\frac{W}{L}\left[(v_{GS}-V_t)v_{DS} - \tfrac{1}{2}v_{DS}^2\right] = k_n'\frac{W}{L}\left(V_{OV}-\tfrac{1}{2}v_{DS}\right)v_{DS}
$$

- 物理图像：drain 端因电位高，**沟道更窄、电荷更少** → 整体电流不再线性于 $v_{DS}$，而是抛物线。

---

## 5️⃣ Pinch-Off → Saturation（$v_{DS} \ge v_{OV}$，slide 9–11）

[Slide 10 — Pinch-off 物理图像与饱和区电流](https://app.notion.com)

Slide 10 — Pinch-off 物理图像与饱和区电流

- 当 $v_{DS} = v_{OV}$ 时，drain 端 $v_{GD}=V_t$ → 该处局部电荷为零 → 沟道**夹断 (pinch-off)**。
- 继续增大 $v_{DS}$，**夹断点向 source 侧轻微移动 (ΔL)**，但**电流不再增加**（夹断点附近电场极高 → 电子被快速扫过，限速由 source 端沟道决定）。

### 饱和区电流（**放大器工作区**）

$$
\boxed{\;i_D = \tfrac{1}{2}k_n'\frac{W}{L}(v_{GS}-V_t)^2 = \tfrac{1}{2}k_n'\frac{W}{L}V_{OV}^2\;}
$$

- 物理意义：$v_{DS}$ 对电流影响只剩**轻微的 CLM 修正**（见 §8）。
- 这正是 MOSFET 能做**跨导放大器**的根本原因：$i_D$ 几乎只由 $v_{GS}$ 决定。

---

## 6️⃣ I-V 总图：三大工作区（slide 12 / 16）

[Slide 12 — NMOS 完整 iD–vDS 曲线族](https://app.notion.com)

Slide 12 — NMOS 完整 iD–vDS 曲线族

| 区域 | 条件 | $i_D$ | 用法 |
| --- | --- | --- | --- |
| **Cutoff** | $v_{GS} \le V_t$ | $\approx 0$ | Digital OFF 态 |
| **Triode** | $v_{GS}>V_t$ 且 $v_{DS}<v_{OV}$ | $k_n'\dfrac{W}{L}(V_{OV}-\tfrac{1}{2}v_{DS})v_{DS}$ | Digital ON / switch / PSU |
| **Saturation** | $v_{GS}>V_t$ 且 $v_{DS}\ge v_{OV}$ | $\tfrac{1}{2}k_n'\dfrac{W}{L}V_{OV}^2$（× CLM 修正） | **Analog amplifier** |

[Slide 16 — NMOS 端子电压条件示意](https://app.notion.com)

Slide 16 — NMOS 端子电压条件示意

<aside>
🎯

**分区记忆口诀**：

1. 先看 $v_{GS}$ 跟 $V_t$ —— 决定开/关。
2. 再看 $v_{DS}$ 跟 $v_{OV}$ —— 决定 triode 还是 saturation。

**模拟做放大器** → 永远保证「**饱和**」（$v_{DS}\ge v_{OV}$，即 drain 比 source 高出至少 $V_{OV}$）。

</aside>

---

## 7️⃣ Drain Current vs Gate Voltage（slide 17）

[Slide 17 — iD–vGS 在饱和区的平方律](https://app.notion.com)

Slide 17 — iD–vGS 在饱和区的平方律

保持 $v_{DS}$ 足够大让管子处于饱和：

$$
i_D = \tfrac{1}{2}k_n'\frac{W}{L}(v_{GS}-V_t)^2
$$

- **平方律**：$v_{GS}$ 每增 $\Delta$，$i_D$ 不是线性增长，而是按 $(v_{GS}-V_t)^2$。
- 横轴换成 $v_{OV}=v_{GS}-V_t$ → 曲线变成经过原点的纯抛物线。
- 实验测 $V_t$：作 $\sqrt{i_D}\sim v_{GS}$ 图，外推到 $\sqrt{i_D}=0$ 的横轴截距 = $V_t$。

---

## 8️⃣ CLM 与 MOSFET 输出阻抗（slide 18–19）

[Slide 18 — Channel-Length Modulation 示意](https://app.notion.com)

Slide 18 — Channel-Length Modulation 示意

### 物理来源

- $v_{DS}>v_{OV}$ 后，**pinch-off 点向 source 平移 ΔL** → 有效沟道长度 $L-\Delta L$ → 电流增加约 $L/(L-\Delta L)$ 倍。
- 一阶近似（与 $v_{DS}$ 成线性）：

$$
i_D = \tfrac{1}{2}k_n'\frac{W}{L}(v_{GS}-V_t)^2\,(1+\lambda v_{DS})
$$

- $\lambda \equiv 1/V_A$，$V_A$（**Early voltage**）∝ 沟道长度 $L$（**长沟道 → 高** $V_A$ **→ 高** $r_o$）。

[Slide 19 — 输出阻抗 r_o 与 V_A 的几何意义](https://app.notion.com)

Slide 19 — 输出阻抗 r_o 与 V_A 的几何意义

### 小信号输出阻抗

$$
r_o \equiv \left.\frac{\partial i_D}{\partial v_{DS}}\right|_{v_{GS}}^{-1} = \frac{1}{\lambda I_D} = \frac{|V_A|}{I_D}
$$

<aside>
⚠️

**踩坑**：

1. 短沟道工艺（$L \le 65\ \text{nm}$）的 $V_A$ 很小、$r_o$ 很低 → 模拟设计宁可加长 $L$ 也要保住增益（intrinsic gain $A_0 = g_m r_o$ ∝ $L$）。
2. 长沟道做分析时常忽略 CLM 取 $r_o\to\infty$，但凡涉及 **本征增益 / 输出阻抗 / cascode**，CLM 绝不可忽略。
</aside>

---

## 9️⃣ NMOS / PMOS / CMOS 对比（slide 13–14, 15, 20, 22）

[Slide 14 — CMOS Cross-Section（n-well 工艺）](https://app.notion.com)

Slide 14 — CMOS Cross-Section（n-well 工艺）

现代 IC 全部基于 **CMOS** —— 同时把 NMOS 和 PMOS 做在一颗 die 上。一种类型必然要做在 **well** 里（图示是 n-well 工艺，PMOS 处于 n-well 中）。

### NMOS 符号（slide 15）

[Slide 15 — NMOS 电路符号三种画法](https://app.notion.com)

Slide 15 — NMOS 电路符号三种画法

| 画法 | 说明 | 箭头 |
| --- | --- | --- |
| (a) 四端完整 | 显式画出 Body | Body 箭头**指向**沟道 → p-type 衬底 |
| (b) Source 箭头 | 把 Body 隐去，箭头标在 Source 上 | Source 箭头**离开** S（电流流向） |
| (c) 简化 | S–B 短接、忽略 body effect | 无箭头 |

### PMOS 符号（slide 20）

[Slide 20 — PMOS 电路符号三种画法](https://app.notion.com)

Slide 20 — PMOS 电路符号三种画法

- Body 箭头**从沟道指出** → n-type 衬底。
- 电流 S → D；惯例**把 Source 画在上面**（电流向下）。

### PMOS 工作区与端子电压（slide 22）

[Slide 22 — PMOS 端子电压条件示意](https://app.notion.com)

Slide 22 — PMOS 端子电压条件示意

所有公式把 NMOS 的量取**绝对值**并把 $k_n'\to k_p'$ 即可：

$$
i_D = \tfrac{1}{2}k_p'\frac{W}{L}|V_{OV}|^2,\quad |V_{OV}| = |V_{GS}|-|V_{tp}|,\quad |V_{DS}|\ge|V_{OV}|\ \text{(饱和)}
$$

<aside>
💡

**对偶口诀**：把 NMOS 倒挂、所有量加绝对值就是 PMOS。但工艺上 $\mu_p \approx \tfrac{1}{2}\mu_n$ → 同样 $I_D$ 下 PMOS 的 $W$ 要做 NMOS 的 2~3 倍。

</aside>

---

## 🔟 Diode-Connected 拓扑（slide 23）

[Slide 23 — Diode-Connected Transistor](https://app.notion.com)

Slide 23 — Diode-Connected Transistor

把 Gate 短接到 Drain → 变成两端器件。

- 因 $V_{GS} = V_{DS}$，永远满足 $V_{DS} \ge V_{GS}-V_t = V_{OV}$ → **始终在饱和区**（自偏置）。
- $v\!-\!i$ 关系：$i = \tfrac{1}{2}k_n'(W/L)(v-V_t)^2$。
- 从电路角度看，等效是一个**电压依赖电阻** $r_{eq}=1/g_m$（小信号），这是后面 current mirror（Lecture 11）和 active load 的核心积木。

---

## 1️⃣1️⃣ Example 5.3 — Source/Drain 偏置电阻设计（slide 24）

[Slide 24 — Example 5.3 电路与题目](https://app.notion.com)

Slide 24 — Example 5.3 电路与题目

**已知**：$V_{DD}=+2.5\ \text{V}$，$V_{SS}=-2.5\ \text{V}$；NMOS 参数 $V_t=0.7\ \text{V}$、$\mu_n C_{ox}=100\ \mu\text{A/V}^2$、$L=1\ \mu\text{m}$、$W=32\ \mu\text{m}$；**目标**：$I_D=0.4\ \text{mA}$、$V_D=0.5\ \text{V}$。

### Step 1 — 由饱和电流求 $V_{OV}$

$$
I_D = \tfrac{1}{2}k_n'\frac{W}{L}V_{OV}^2
\Rightarrow 0.4 = \tfrac{1}{2}\cdot 0.1\cdot 32\cdot V_{OV}^2
\Rightarrow V_{OV} \approx 0.5\ \text{V}
$$

所以 $V_{GS}=V_t+V_{OV}=1.2\ \text{V}$。

### Step 2 — 求 $V_S$ 并算 $R_S$

栅极接地（$V_G=0$），故 $V_S = V_G - V_{GS} = -1.2\ \text{V}$。

$$
R_S = \frac{V_S - V_{SS}}{I_D} = \frac{-1.2-(-2.5)}{0.4\ \text{mA}} = \frac{1.3}{0.4} = 3.25\ \text{k}\Omega
$$

### Step 3 — 求 $R_D$

$$
R_D = \frac{V_{DD} - V_D}{I_D} = \frac{2.5-0.5}{0.4} = 5\ \text{k}\Omega
$$

### Step 4 — 验证饱和

$V_{DS} = V_D - V_S = 0.5-(-1.2) = 1.7\ \text{V} > V_{OV}=0.5\ \text{V}$ ✅

<aside>
🔑

**套路**：饱和区 DC 偏置题永远三步走 ——

1. 由 $I_D$ 倒推 $V_{OV}$、$V_{GS}$。
2. 用 KVL 把 $V_S, V_D$ 锁住，求出 $R_S, R_D$。
3. **永远最后回头检查** $V_{DS}\ge V_{OV}$ 是否成立 —— 如果不在饱和，全部要重做（按 triode 公式联立）。
</aside>

---

## 1️⃣2️⃣ Example 5.6 — 分压栅极 + 源极电阻偏置（slide 25–26）

[Slide 25 — Example 5.6 电路与化简](https://app.notion.com)

Slide 25 — Example 5.6 电路与化简

**已知**：$V_{DD}=+10\ \text{V}$，$R_{G1}=R_{G2}=10\ \text{M}\Omega$，$R_D=R_S=6\ \text{k}\Omega$，$V_{tn}=1\ \text{V}$，$k_n'(W/L)=1\ \text{mA/V}^2$，忽略 CLM。

### Step 1 — 分压设定 $V_G$

栅极电流 $I_G=0$ → $R_{G1}, R_{G2}$ 只起分压器作用：

$$
V_G = V_{DD}\cdot\frac{R_{G2}}{R_{G1}+R_{G2}} = 10\cdot\frac{10}{20} = 5\ \text{V}
$$

### Step 2 — KVL：由 $V_{GS}$ 与 $I_D$ 自洽

$V_S = R_S I_D = 6 I_D$（$I_D$ 单位 mA、电阻单位 kΩ，结果为 V）

$$
V_{GS} = V_G - V_S = 5 - 6 I_D
$$

代入饱和区电流方程：

$$
I_D = \tfrac{1}{2}\cdot 1\cdot (5-6I_D-1)^2 = \tfrac{1}{2}(4-6I_D)^2
$$

展开：$18 I_D^2 - 25 I_D + 8 = 0$。

### Step 3 — 解二次方程并取物理解

$$
I_D = \frac{25\pm\sqrt{625-576}}{36} = \frac{25\pm 7}{36} \Rightarrow I_D = 0.89\ \text{or}\ 0.5\ \text{mA}
$$

- $I_D=0.89\ \text{mA}$ → $V_S = 5.34\ \text{V} > V_G=5\ \text{V}$ → $V_{GS}<0$，管子截止 → **舍去**。
- $I_D = 0.5\ \text{mA}$ → $V_S=3\ \text{V}$，$V_{GS}=2\ \text{V}$，$V_{OV}=1\ \text{V}$，$V_D = 10-6\cdot 0.5 = 7\ \text{V}$，$V_{DS}=4\ \text{V}>V_{OV}$ ✅ 饱和。

[Slide 26 — Example 5.6 完整解答](https://app.notion.com)

Slide 26 — Example 5.6 完整解答

<aside>
⚠️

**踩坑**：解二次方程一定要**回代验饱和 + 验物理合理**。两根都可能数学成立，但只有一个对应饱和。常见错误：取 $I_D$ 大的根却没注意 $V_S>V_G$ 已经把管子关掉了。

</aside>

---

## 📝 本节总结（本节记三条）

<aside>
🎯

1. **MOSFET 的三大区由两条不等式决定**：先 $v_{GS}\gtrless V_t$ 决定 cutoff，再 $v_{DS}\gtrless v_{OV}$ 决定 triode 还是 saturation。**模拟永远在饱和区**。
2. **饱和区平方律**：$i_D = \tfrac{1}{2}k_n'(W/L)V_{OV}^2(1+\lambda v_{DS})$。$V_{OV}$ 是设计的核心抓手，$\lambda$ 带来有限 $r_o$ 与 intrinsic gain $A_0=g_m r_o$。
3. **NMOS / PMOS 完全对偶**：所有量取绝对值即可；CMOS 工艺中 PMOS 在 n-well 里。Diode-connected 是 current mirror 的种子拓扑（→ Lecture 11）。
</aside>

---

## 📚 Homework 3

- [x]  Sedra/Smith 3.13
- [x]  Sedra/Smith 3.20
- [x]  Sedra/Smith 3.24
- [x]  Sedra/Smith 5.2
- [x]  Sedra/Smith 5.7
- [x]  Sedra/Smith 5.18
- [x]  Sedra/Smith 5.20

*Due: 2026-04-08 23:00*

---

## 📎 原 Slide PDF

[EE115 Lecture7 — MOSFET 原 Slide](EE115%20Lecture7%20%E2%80%94%20MOSFET%20%E5%99%A8%E4%BB%B6%E5%8E%9F%E7%90%86%E4%B8%8E%20I-V%20%E7%89%B9%E6%80%A7/lecture7.pdf)

EE115 Lecture7 — MOSFET 原 Slide

## 🔗 与上下文衔接

- 本节是整门课的**器件起点**：$V_t, V_{OV}, k_n', W/L, V_A$ 是所有后续小信号分析的输入参数。
- 下一节 [EE115 Lecture8 — Transistor Amplifier 基础与 Small-Signal Model](EE115%20Lecture8%20%E2%80%94%20Transistor%20Amplifier%20%E5%9F%BA%E7%A1%80%E4%B8%8E%20Small-Si.md) 把饱和区曲线在 Q 点处线性化 → 引入 hybrid-π 与四步分析法。
- CLM / $r_o$ 的真正威力会在 [EE115 Lecture12 — Cascode Amplifier](EE115%20Lecture12%20%E2%80%94%20Cascode%20Amplifier.md) 通过 cascode 拉到 $A_0^2$ 才完全体现。