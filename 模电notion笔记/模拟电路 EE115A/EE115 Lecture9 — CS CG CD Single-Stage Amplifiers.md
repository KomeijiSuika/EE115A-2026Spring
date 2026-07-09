# EE115 Lecture9 — CS / CG / CD Single-Stage Amplifiers

<aside>
📌

**本节主题：** Transistor Amplifier 2 —— 三种单管拓扑 (CS / CG / CD) 与两端口模型

**参考书：** Sedra/Smith — pp. 423–452（Chapter 7 §7.3）

**核心脉络：** 两端口模型抽象 ($R_{in}, A_{vo}, R_o$) → CS 负责 “拿增益” → CS + Rs 加负反馈提线性 → 单位增益缓冲 → CG 提供低输入阻 / 高漏输出 (为 cascode 黑笔颓) → CD 作输出驱动缓冲 → Table 6.4 归总。

</aside>

## 📅 课程行政信息

- **Homework 4** — Sedra/Smith Ch.7 题：**7.2, 7.6, 7.25, 7.29, 7.65, 7.77, 7.82, 7.83**：due April 15 23:00。

---

## 1️⃣ 复习：MOSFET 小信号模型（slide 3）

[Slide 3 — Review: Small-signal model for MOSFET](https://app.notion.com)

Slide 3 — Review: Small-signal model for MOSFET

$$
g_m = k_n V_{OV} = \frac{2I_D}{V_{OV}} = \sqrt{2k_n'(W/L)I_D},\quad r_o = \frac{|V_A|}{I_D}
$$

NMOS / PMOS 同一模型，PMOS 取绝对值、$k_n\to k_p$。

---

## 2️⃣ 三种拓扑一眼看（slide 4）

[Slide 4 — Basic Single-Transistor Amplifier Configurations](https://app.notion.com)

Slide 4 — Basic Single-Transistor Amplifier Configurations

| 拓扑 | 输入端 | 输出端 | 接地端 | 脚色 |
| --- | --- | --- | --- | --- |
| **CS** (Common Source) | Gate | Drain | Source | 主力增益级 |
| **CG** (Common Gate) | Source | Drain | Gate | 低输入阻 / 高输出阻 → cascode 基本块 |
| **CD** (Common Drain / Source Follower) | Gate | Source | Drain | 输出缓冲 / 驱动低载 |

<aside>
💡

**记忆口诀**：“**哪个是输入**决定名字”。输入接 Source 叫共源？不对 — “共”指的是**接地端**。CS=Source 接地，CG=Gate 接地，CD=Drain 接地。

</aside>

---

## 3️⃣ 两端口模型（slide 5）

[Slide 5 — Two-Port Model of Amplifiers](https://app.notion.com)

Slide 5 — Two-Port Model of Amplifiers

任何放大器 = $R_{in} + A_{vo}v_i + R_o$。**全体增益**（含信号源与负载）：

$$
G_v = \frac{R_{in}}{R_{sig}+R_{in}}\cdot A_{vo}\cdot\frac{R_L}{R_o+R_L}
$$

### 求 $R_o$ 的标准动作

1. 接地 $v_i$（去除信号源 $R_{sig}$）。
2. 在输出端加测试源 $i_x$ (概念上)。
3. 求出端电压 $v_x$，$R_o=v_x/i_x$。

<aside>
🔑

**十道题十个学生错**：Lecture 课后始终从这个“在输出端加测试源”出题，记必随身。

</aside>

---

## 4️⃣ CS 放大器（slide 6）

[Slide 6 — Common-Source Amplifier](https://app.notion.com)

Slide 6 — Common-Source Amplifier

$$
R_{in}=\infty,\quad A_{vo}=-g_m R_D,\quad R_o=R_D
$$

带负载 $R_L$ 后：$G_v = -g_m(R_D\|R_L)$。如果考虑沟道长调制：$G_v = -g_m(R_D\|R_L\|r_o)$。

<aside>
🎯

**CS 是主力增益级**：$R_{in}=\infty$ 不吃信号源、增益高、拓扑简单。但 $R_o=R_D$ 偏高 → 不能驱动低阻负载 → 后续需要 CD 充当输出缓冲。

</aside>

---

## 5️⃣ T-Equivalent Model（slide 7）

[Slide 7 — T-Equivalent Circuit Model for MOSFET](https://app.notion.com)

Slide 7 — T-Equivalent Circuit Model for MOSFET

从 hybrid-π 等价变形出来的“T 型”：

- Source 侧看进去是 $1/g_m$。
- Drain 侧仍是受控电流源 $g_m v_{gs}$。
- Gate 侧电流 $i_g=0$。

<aside>
💡

**使用法则**：“当有电阻接在 source 上” → 用 T-model；起手是 $v_{gs}=v_g-v_s$，再 KVL/KCL。代价一般会跳过繁冗的 $v_{gs}$ 表达式。

</aside>

---

## 6️⃣ CS + Source Degeneration（slides 8–9）

[Slide 8 — CS with Source Resistance](https://app.notion.com)

Slide 8 — CS with Source Resistance

[Slide 9 — Source Degeneration Continued](https://app.notion.com)

Slide 9 — Source Degeneration Continued

### 增益公式

$$
A_v = -\frac{R_D}{1/g_m + R_s} = -\frac{R_D \text{ in Drain}}{R_s \text{ in Source}}
$$

加负载后：$G_v = -\dfrac{R_D\|R_L}{1/g_m + R_s}$。

### 负反馈的三个好处

1. **稳定电流**：$I_D$ 不再随 $V_{GS}$ 高敏感，开环增益被 $1+g_m R_s$ 除。
2. **提线性**：输入信号被分压，$v_{gs}$ 变小。
3. **拓宽带宽**：主极点频率提高（后期 bode plot 会详述）。

### 踩坑警报

<aside>
⚠️

**Slide 9 上有个印刷错误（老师标记 “这里有错”）：**$v_{gs}$ 表达式的右半应为

$$
v_{gs} = v_i\cdot\frac{1/g_m}{1/g_m+R_s} = \frac{v_i}{1+g_m R_s}
$$

**不是** $v_i \cdot \dfrac{g_m}{1+g_m R_s}$。量纲上也看得出：$v_{gs}$ 是分压后的电压，不能多出一个 $g_m$。题上别拄错。

</aside>

---

## 7️⃣ CG 放大器（slide 10）

[Slide 10 — Common-Gate Amplifier](https://app.notion.com)

Slide 10 — Common-Gate Amplifier

$$
R_{in}=\frac{1}{g_m},\quad A_{vo}=g_m R_D,\quad R_o=R_D
$$

$$
G_v=\frac{1/g_m}{R_{sig}+1/g_m}\,[g_m(R_D\|R_L)] = \frac{R_D\|R_L}{R_{sig}+1/g_m}
$$

### CG 的名句

- **输入阻低** $=1/g_m$、**输出阻高** → 才能做 cascode 中以“上面那个”出现（参 Lecture 12/13）。
- 增益与 CS 同量级但 **不反相**（+ 号）；阂零特性优于 CS。

---

## 8️⃣ 上节间插曲：为什么需要单位增益缓冲（slide 11）

[Slide 11 — Unity-Gain Buffer Voltage Amplifier](https://app.notion.com)

Slide 11 — Unity-Gain Buffer Voltage Amplifier

- **直接驱动**：$R_{sig}=1\ \text{M}\Omega$、$R_L=1\ \text{k}\Omega$ → $v_o\approx 1\ \text{mV}$（信号被拆灭 1000×）。
- **加个** $A_{vo}=1$ **的缓冲**：$R_o=100\ \Omega$ → $v_o\approx 0.9\ \text{V}$。全都靠“阻抗分压学”赢回来。

<aside>
🎯

**加 buffer 不贪增益、只为保全信号**。IC 里这个 buffer = source follower。

</aside>

---

## 9️⃣ CD 放大器 (Source Follower)（slide 12）

[Slide 12 — Common-Drain Amplifier / Source Follower](https://app.notion.com)

Slide 12 — Common-Drain Amplifier / Source Follower

$$
R_{in}=\infty,\quad A_v\equiv\frac{v_o}{v_i}=\frac{R_L}{R_L+1/g_m},\quad R_o=\frac{1}{g_m}
$$

增益总是小于 1，接近 1 需要 $g_m R_L\gg 1$。

作输出级的价值：

- $R_{in}=\infty$ 不拖拽前级。
- $R_o=1/g_m$ 是单管能拿到的最低输出阻之一（不考虑反馈）。

---

## 🔟 对比总结（slides 13–14 / Table 6.4）

[Slide 14 — Summary of MOSFET Amplifiers](https://app.notion.com)

Slide 14 — Summary of MOSFET Amplifiers

| 拓扑 | $R_{in}$ | $A_{vo}$ | $R_o$ | $G_v$ （含 $R_{sig}, R_L$） |
| --- | --- | --- | --- | --- |
| **CS** | $\infty$ | $-g_m R_D$ | $R_D$ | $-g_m(R_D\|R_L)$ |
| **CS + Rs** | $\infty$ | $-\dfrac{g_m R_D}{1+g_m R_s}$ | $R_D$ | $-\dfrac{R_D\|R_L}{1/g_m+R_s}$ |
| **CG** | $1/g_m$ | $+g_m R_D$ | $R_D$ | $\dfrac{R_D\|R_L}{R_{sig}+1/g_m}$ |
| **CD** | $\infty$ | $1$ | $1/g_m$ | $\dfrac{R_L}{R_L+1/g_m}$ |

<aside>
🎯

**设计组装思路**：三口诀走天下 ——

1. **CS 拿增益**；
2. **CG 助 cascode** （低 $R_{in}$ 、高 $R_o$）；
3. **CD 作输出缓冲**。

后期的 OpAmp、OTA、PGA 都只是这三者的组合。

</aside>

---

## 📝 本节总结（本节记三点即可）

<aside>
🎯

1. **两端口抽象** = 看任何放大器只看 $R_{in}, A_{vo}, R_o$ 三个数，$G_v$ 是三个数加两个分压。
2. **Source 接电阻 → 用 T-model**；不接、只看 drain/gate → 用 hybrid-π。
3. **负反馈用** $R_s$ **插在 Source**：稳电流 + 增线性 + 宽带、代价是增益被 $1+g_m R_s$ 除。
</aside>

---

## 🔗 与上下文衔接

- 上一节 Lecture 8 完成了从 I–V 曲线到 hybrid-π 的推导。
- 下节 [EE115 Lecture10 — Biasing & CS Amplifier with Bias](EE115%20Lecture10%20%E2%80%94%20Biasing%20&%20CS%20Amplifier%20with%20Bias.md) 会把 CS + $R_s$ 这套思路落到偏置电路设计上。

## 📚 Homework 4

- [x]  Sedra/Smith 7.2
- [x]  Sedra/Smith 7.6
- [x]  Sedra/Smith 7.25
- [x]  Sedra/Smith 7.29
- [x]  Sedra/Smith 7.65
- [x]  Sedra/Smith 7.77
- [x]  Sedra/Smith 7.82
- [x]  Sedra/Smith 7.83

*Due: April 15 23:00*

[EE115 Lecture9 updated.pdf](EE115%20Lecture9%20%E2%80%94%20CS%20CG%20CD%20Single-Stage%20Amplifiers/EE115_Lecture9_updated.pdf)