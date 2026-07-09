# EE115 Lecture6 — PN Junction（PN 结）

<aside>
📘

**本节主题：** PN 结（PN Junction）—— 半导体器件（二极管 / BJT / MOSFET）的共同基石。

**参考教材：** Sedra/Smith *Microelectronic Circuits*，pp. 148–167。

**核心脉络：** 冶金结形成耗尽区 → 内建电势 $V_o$ → 静电分析给出耗尽区宽度 $W$ 与存储电荷 $Q_J$ → 加偏置后 $W$ 随电压变化 → 正偏扩散电流给出二极管 I-V 方程 → 反偏几乎不导通但有耗尽电容（变容）+ 反向击穿。

**一句话总纲：** 「**正偏导通靠扩散、反偏截止存电荷**」—— 一个结、两种工作态、三类电流/电容行为。

</aside>

## 1️⃣ PN 结与三大器件基石

- PN 结 = 一块 **P 型半导体** 与一块 **N 型半导体** 冶金接触（metallurgical junction）形成的界面。
- 它是几乎所有半导体器件的基本构件：
    - **二极管 Diode**（单个 PN 结，anode = P，cathode = N）
    - **双极型晶体管 BJT**（两个背靠背 PN 结）
    - **MOSFET**（源 / 漏与衬底之间同样是 PN 结）

![Slide 3 — PN 结物理结构（Fig 3.8：anode/P 与 cathode/N）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_3.png)

Slide 3 — PN 结物理结构（Fig 3.8：anode/P 与 cathode/N）

<aside>
🎯

**口诀：** 二极管、BJT、MOSFET 全是「PN 结的不同拼法」—— 吃透一个结，后面所有器件都是它的组合。

</aside>

## 2️⃣ 内建电势 Built-in Potential

载流子扩散 → 复合 → 形成耗尽区：

- N 侧自由电子与 P 侧自由空穴在界面 **复合（recombine）**。
- 留下的 **耗尽区（depletion region，耗尽区）** 没有可动载流子，只剩施主 / 受主离子的 **固定电荷（immobile charge）**。
- 固定电荷建立内建电场 → P、N 两侧产生电位差，即 **内建电势** $V_o$**（built-in potential，内建电势）**。

$$
V_o = V_T \ln\!\frac{N_A N_D}{n_i^2}
$$

- $V_T$：热电压（thermal voltage）$=kT/q \approx 26\ \text{mV}$（室温）。
- $N_A$：P 侧受主浓度；$N_D$：N 侧施主浓度。
- $n_i$：本征载流子浓度 $\approx 1.5\times10^{10}\ \text{cm}^{-3}$（室温 Si）。

![Slide 4 — 无外加电压的 PN 结与电位分布（Fig 3.9）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_4.png)

Slide 4 — 无外加电压的 PN 结与电位分布（Fig 3.9）

<aside>
🎯

**口诀：** $V_o$ 随掺杂升高而增大（对数关系），但它是「内建」的 —— 外电路量不到，只能通过器件行为间接体现。

</aside>

## 3️⃣ 耗尽区静电分析 Electrostatic Analysis

电荷中性（charge neutrality）：两侧耗尽电荷大小相等。

$$
|Q_+| = |Q_-| = Q_J \;\Rightarrow\; q A\, x_p N_A = q A\, x_n N_D \;\Rightarrow\; N_A x_p = N_D x_n
$$

- 推论：**掺杂浓度低的一侧，耗尽区伸得更深**（$x \propto 1/N$）。图中 $N_A > N_D$，故 N 侧耗尽更宽。

平衡时耗尽区总宽度：

$$
W = x_n + x_p = \sqrt{\frac{2\varepsilon_s}{q}\left(\frac{1}{N_A}+\frac{1}{N_D}\right)V_o}
$$

存储在耗尽区的电荷：

$$
Q_J = A\, q\,\frac{N_A N_D}{N_A+N_D}\,W = A\sqrt{2\varepsilon_s q\,\frac{N_A N_D}{N_A+N_D}\,V_o}
$$

![Slide 5 — 耗尽区电荷分布与电场（Fig 3.10：载流子浓度 / 电荷 / 电场）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_5.png)

Slide 5 — 耗尽区电荷分布与电场（Fig 3.10：载流子浓度 / 电荷 / 电场）

<aside>
🎯

**口诀：** 「轻掺一侧耗尽宽」+「两侧电荷量相等」是所有耗尽区几何题的两把钥匙。

</aside>

## 4️⃣ 偏置下的耗尽区宽度 Depletion Width under Bias

把外加电压 $V$ 叠加到内建电势上（$V$ 正偏为正、反偏为负）：

$$
W = \sqrt{\frac{2\varepsilon_s}{q}\left(\frac{1}{N_A}+\frac{1}{N_D}\right)(V_o - V)}
$$

- **反偏（**$V<0$**）：** $V_o - V$ 变大 → **耗尽区变宽**，存储电荷 $Q_J$ 增多。
- **正偏（**$V>0$**）：** $V_o - V$ 变小 → 耗尽区变窄、势垒降低，利于扩散导通。

![Slide 6 — 平衡 / 反偏 / 正偏三种状态下的耗尽区（Fig 3.11）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_6.png)

Slide 6 — 平衡 / 反偏 / 正偏三种状态下的耗尽区（Fig 3.11）

<aside>
🎯

**口诀：** 反偏 = 拉宽耗尽区（存电荷、当电容）；正偏 = 压窄耗尽区（放载流子、导电流）。

</aside>

## 5️⃣ 正偏 I-V 特性推导 Diode Equation

正偏下，少数载流子（minority carrier）在耗尽区边缘被 **扩散电流** 抬升：

- N 区边缘空穴浓度（注入）：

$$
p_n(x_n) = p_{n0}\,e^{V/V_T},\qquad p_{n0} = \frac{n_i^2}{N_D}
$$

- 空穴扩散电流密度：$J_p = -q D_p \dfrac{dp_n}{dx}$；电子侧同理。
- 合并两侧扩散电流，得 **二极管方程（diode equation）**：

$$
I = I_S\left(e^{V/V_T} - 1\right)
$$

- **饱和电流（saturation current）**：

$$
I_S = A\, q\, n_i^2\left(\frac{D_p}{L_p N_D} + \frac{D_n}{L_n N_A}\right)
$$

![Slide 7 — 正偏 PN 结中的少子分布（Fig 3.12，$N_A \gg N_D$）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_7.png)

Slide 7 — 正偏 PN 结中的少子分布（Fig 3.12，$N_A \gg N_D$）

<aside>
🎯

**口诀：** 二极管电流的本质是「少子扩散」；指数项 $e^{V/V_T}$ 来自玻尔兹曼分布，$I_S \propto n_i^2$ 对温度极敏感。

</aside>

## 6️⃣ I-V 曲线与反向击穿 Breakdown

- **正偏区：** 电流随 $V$ 指数上升，导通压降约 0.6–0.7 V（Si）。
- **反偏区：** 仅有极小的 $-I_S$（漏电），近似截止。
- **反向击穿（reverse breakdown，击穿）：** 反压超过击穿电压后反向电流急剧增大（雪崩 / 齐纳）；齐纳二极管正是工作在此区做稳压。

![Slide 8 — PN 结 I-V 特性与反向击穿（Fig 3.13 & 3.14）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_8.png)

Slide 8 — PN 结 I-V 特性与反向击穿（Fig 3.13 & 3.14）

<aside>
🎯

**口诀：** 正偏指数导通、反偏微漏截止、过压雪崩击穿 —— 三段式记牢 I-V 曲线。

</aside>

## 7️⃣ 结电容：耗尽电容 vs 扩散电容

PN 结有两种电容，分别主导不同偏置：

| 电容 | 主导偏置 | 物理来源 | 表达式 |
| --- | --- | --- | --- |
| **耗尽电容** $C_j$（depletion / junction cap） | 反偏为主 | 耗尽区固定电荷随 $V_R$ 变化 | $C_j=\dfrac{A\varepsilon_s}{W}=\dfrac{C_{j0}}{\sqrt{1+V_R/V_o}}$ |
| **扩散电容** $C_d$（diffusion cap） | 正偏为主 | 少子存储电荷随正向电流变化 | $C_d=\dfrac{\tau_T}{V_T}\,I$ |
- 反偏耗尽电容随反压 **减小**（$W$ 变宽 → $C_j$ 变小）→ 这正是 **变容二极管（varactor）** 的原理：用反向电压调电容。

![Slide 9 — 耗尽层电荷随反向电压 $V_R$ 的变化（Fig 1.42）](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_9.png)

Slide 9 — 耗尽层电荷随反向电压 $V_R$ 的变化（Fig 1.42）

<aside>
🎯

**口诀：** 反偏看耗尽电容（电压控容，变容管）；正偏看扩散电容（电流控容，限制开关速度）。

</aside>

## 9️⃣ 附录：内建电势推导

老师在附录里给出了 $V_o$ 的完整推导：从平衡时 **漂移电流 = 扩散电流**、用爱因斯坦关系 $D/\mu = V_T$，对电场积分得到 P、N 两侧电位差。原 slide 推导如下：

![Slide 11 — 附录：PN 结电位推导](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/slide_11.png)

Slide 11 — 附录：PN 结电位推导

<aside>
🧠

**本节总结（PN 结三句话）：**

1. **一个结两种态：** 正偏靠少子扩散指数导通 $I=I_S(e^{V/V_T}-1)$；反偏几乎只剩 $-I_S$。
2. **耗尽区是核心几何：** 宽度 $W=\sqrt{\tfrac{2\varepsilon_s}{q}(\tfrac{1}{N_A}+\tfrac{1}{N_D})(V_o-V)}$，轻掺一侧耗尽更深；反偏拉宽、正偏压窄。
3. **两种电容分管两端：** 反偏耗尽电容 $C_j\propto 1/W$（变容），正偏扩散电容 $C_d\propto I$（限速）。
</aside>

## 📎 原始 Slides

[EE115 Lecture6.pdf](EE115%20Lecture6%20%E2%80%94%20PN%20Junction%EF%BC%88PN%20%E7%BB%93%EF%BC%89/EE115_Lecture6.pdf)