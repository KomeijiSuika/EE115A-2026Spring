# EE115 Lecture13 — Cascode & Differential Amplifiers

<aside>
📌

**本节主题：** Building Blocks of IC Amplifier + Differential Amplifiers

**参考书：** Sedra/Smith — pp. 547–556（级联 / cascode 建构块）+ pp. 595–613（差分放大器引言）

**核心脉络：** Cascode 增益提升 → Cascode + Active Load → 设计实例 → Double / Folded Cascode → 差分对基本结构 → 共模与 ICMR → 大信号转移 → 虚负与半电路 → 电流源负载 / cascode 差分放大器

</aside>

## 📅 课程行政信息

- **Homework 6**（截止 **2026-05-20 23:00** ）：Sedra/Smith Ch. 8 — **8.1, 8.4, 8.6, 8.40, 8.43, 8.65, 8.67, 8.71**

---

## 1️⃣ Outline（slides 1–2）

[Slide 1 — Outline](https://app.notion.com)

Slide 1 — Outline

[Slide 2 — Outline](https://app.notion.com)

Slide 2 — Outline

两条主线：

1. **Building blocks of IC amplifier** — Sedra/Smith pp. 547–556
2. **Differential amplifiers** — Sedra/Smith pp. 595–613

---

## 2️⃣ 回顾：怎么提升电压增益？（slide 3）

[Slide 3 — Boosting voltage gain](https://app.notion.com)

Slide 3 — Boosting voltage gain

共源 (CS) 放大器的小信号模型：

$$
v_o = -g_{m1}v_i \cdot (r_{o1}\|R_L)
$$

- 若负载只是单管 $r_{o1}$，则 $A_v=-g_{m1}r_{o1}=-A_0$（本征增益）。
- 想要更高增益 → 把输出阶似上一个「黑盒 K」把负载电阻抹到 $K r_{o1}$，增益变 $-g_{m1}(K r_{o1})$。
- **最自然的「黑盒 K」 → 再迭一个 MOSFET 作为 cascode。**

---

## 3️⃣ 回顾：源极退化→输出阻抗变换（slide 4）

[Slide 4 — Source degeneration](https://app.notion.com)

Slide 4 — Source degeneration

带源极电阻 $R_s$ 的共源管：

$$
R_{\text{out}} = r_o + R_s + g_m r_o R_s = r_o + (1+g_m r_o)R_s \simeq (g_m r_o)\, R_s = A_0\, R_s
$$

<aside>
💡

**口诀（向上看）：** 源极电阻 被「本征增益 $A_0=g_m r_o$」**乘**后出现在漏端。

</aside>

---

## 4️⃣ 回顾：共栅管的阻抗变换（slide 5）

[Slide 5 — CG impedance transform](https://app.notion.com)

Slide 5 — CG impedance transform

共栅 (CG) 拓扑，从源端看漏端负载 $R_L$：

$$
R_{\text{in}} = \frac{r_o + R_L}{1 + g_m r_o} \simeq \frac{1}{g_m} + \frac{R_L}{g_m r_o} = \frac{1}{g_m} + \frac{R_L}{A_0}
$$

<aside>
💡

**口诀（向下看）：** 漏载被 $A_0$ **除/压缩**后出现在源端。

</aside>

<aside>
🔑

**Cascode 灵魂一句话⭐** 上头看下来阻抗 ÷ $A_0$；下头看上去阻抗 × $A_0$。后面整章 cascode 都靠这一条。

</aside>

---

## 5️⃣ MOS Cascode 放大器（slide 6）

[Slide 6 — MOS cascode amplifier](https://app.notion.com)

Slide 6 — MOS cascode amplifier

两层堆叠：$Q_1$（共源、提供 $g_m$）+ $Q_2$（共栅、阻抗变换）。

- 从 $Q_2$ 漏端往下看：$R_o \simeq (g_{m2}r_{o2})\, r_{o1} = A_0\, r_o$
- 若负载为理想电流源（无穷大 $r_L$）：

$$
A_{vo} = -g_{m1} R_o = -(g_{m1} r_{o1})(g_{m2} r_{o2}) = -A_0^2
$$

Cascode 的核心收益：**多一层堆叠 → 本征增益从** $A_0$ **提到** $A_0^2$**。**

---

## 6️⃣ Cascode + 简单 Active Load（slide 7）

[Slide 7 — Cascode + simple active load](https://app.notion.com)

Slide 7 — Cascode + simple active load

负载只是单个 PMOS 电流源 $Q_3$：

$$
R_{\text{out}} = R_o \| R_L = (g_{m2}r_{o2}r_{o1}) \| r_{o3} \simeq r_{o3}
$$

$$
\Rightarrow A_v = -g_{m1}(R_o \| R_L) \simeq -g_{m1}\, r_{o3}
$$

<aside>
⚠️

**踩坑**：input 侧 cascode 把阻抗推到 $A_0^2 r_o$，但**负载侧没跟上 → 总阻抗被负载拉低。要拿** $A_0^2$ **增益必须上下两边都 cascode。**

</aside>

---

## 7️⃣ Cascode + Cascode Current-Source Load（slide 8）

[Slide 8 — Cascode + cascode current load](https://app.notion.com)

Slide 8 — Cascode + cascode current load

上下两侧都用 cascode：

- 输入侧阻抗 $R_{on} = (g_{m2}r_{o2})\, r_{o1}$
- 负载侧阻抗 $R_{op} = (g_{m3}r_{o3})\, r_{o4}$

并联后（NMOS / PMOS 对称时）：

$$
A_v = -g_{m1}(R_{on}\|R_{op}) = -\tfrac{1}{2}(g_m r_o)^2 = -\tfrac{1}{2}A_0^2
$$

名义上比单端 cascode 少一半，但这 **已是 single-stage 拓扑能拿到的最高电压增益**。

---

## 8️⃣ 设计例题：Cascode 电流源（slides 9–11）

[Slide 9 — Design example setup](https://app.notion.com)

Slide 9 — Design example setup

[Slide 10 — Design example calculation](https://app.notion.com)

Slide 10 — Design example calculation

[Slide 11 — Design result](https://app.notion.com)

Slide 11 — Design result

**给定**：0.18 µm CMOS，$V_{DD}=1.8\ \text{V}$，$V_{tp}=-0.5\ \text{V}$，$\mu_p C_{ox}=90\ \mu\text{A/V}^2$，$V_A'=-5\ \text{V/µm}$。

**目标**：Cascode 电流源 $I=100\ \mu\text{A}$，$R_o=500\ \text{k}\Omega$；取 $|V_{OV}|=0.3\ \text{V}$，求 $L$、$W/L$、$V_{G3}, V_{G4}$。

### Step 1 — 由 $R_o$ 求 $L$

$$
R_o = (g_m r_o)\, r_o = \frac{|V_A|}{|V_{OV}|/2} \cdot \frac{|V_A|}{I_D}
$$

$$
500\ \text{k}\Omega = \frac{|V_A|}{0.15} \cdot \frac{|V_A|}{0.1\ \text{mA}} \Rightarrow |V_A|=2.74\ \text{V}
$$

$$
L = \frac{|V_A|}{|V_A'|} = \frac{2.74}{5} \approx 0.55\ \mu\text{m}
$$

约为最小沟道长度的 3 倍。

### Step 2 — 偏压 $V_{G3}, V_{G4}$

- $|V_{GS}| = |V_t| + |V_{OV}| = 0.5 + 0.3 = 0.8\ \text{V}$
- $V_{G4} = V_{DD} - |V_{SG4}| = 1.8 - 0.8 = 1.0\ \text{V}$
- 为让输出摆幅最大，取 $V_{D4} = V_{DD} - |V_{OV}| = 1.5\ \text{V}$
- $V_{SG3} = V_{SG4} = 0.8\ \text{V}$ → $V_{G3} = 1.5 - 0.8 = 0.7\ \text{V}$

### Step 3 — $W/L$

$$
I_D = \tfrac{1}{2}\mu_p C_{ox}\frac{W}{L}|V_{OV}|^2\left(1 + \frac{V_{SD}}{|V_A|}\right)
$$

$$
100 = \tfrac{1}{2}\times 90\times\frac{W}{L}\times 0.3^2\times\left(1 + \frac{0.3}{2.74}\right)
\Rightarrow \frac{W}{L}\approx 22.3
$$

### Step 4 — 输出最大摆幅

$Q_3$ 需要 $|V_{OV}|=0.3\ \text{V}$：$v_{D3,\max} = V_{D4} - 0.3 = 1.2\ \text{V}$

<aside>
💡

**快公式**：$g_m = \dfrac{I_D}{V_{OV}/2}$，$r_o = \dfrac{|V_A|}{I_D}$，$A_0 = g_m r_o = \dfrac{2 V_A' L}{V_{OV}}$ —— **本征增益 ∝ L，反比于** $V_{OV}$。

</aside>

---

## 9️⃣ Double Cascoding（slide 12）

[Slide 12 — Double cascoding](https://app.notion.com)

Slide 12 — Double cascoding

再加一层 $Q_3$ → 输出阻抗变 $A_0^2\, r_o$，理论上 $A_v = -A_0^3$。

<aside>
⚠️

**代价：** 纵向堆 3 个管子 + 1 个电流源，需要的最低 $V_{DD}\ge 4|V_{OV}|+|V_t|$，对低压工艺（1 V 以下）几乎在现代 IC 里不可行。

</aside>

---

## 🔟 Folded Cascode（slide 13）

[Slide 13 — Folded cascode](https://app.notion.com)

Slide 13 — Folded cascode

为了避免堆叠过高，把共栅级 (CG) 「折叠」到 PMOS 上：

- $Q_1$（NMOS 共源）由 $I_1 - I_2$ 偏置
- $Q_2$（PMOS 共栅）由 $I_2$ 偏置
- 输出节点在 $Q_2$ 的漏端

| 优点 | 具体意义 |
| --- | --- |
| 低 $V_{DD}$ 友好 | 纵向只剩两层管子，同样 $A_0^2$ 增益，但所需压降高剩不多 |
| 输入/输出共模分离 | 输入在 NMOS 端（低共模），输出在 PMOS 漏端（高共模）——让 op-amp 设计更灵活 |
| 现代 op-amp 标配 | 今天几乎所有商用 op-amp 输入级都是 folded cascode |

---

# Part II — Differential Amplifiers（slides 14–26）

[Slide 14 — Differential amplifiers intro](https://app.notion.com)

Slide 14 — Differential amplifiers intro

## 1️⃣1️⃣ 为什么要差分？（slide 15）

[Slide 15 — Why differential](https://app.notion.com)

Slide 15 — Why differential

<aside>
🎯

1. **抗噪声 / 抗干扰**：共模噪声在两路相消。
2. **无需大电容耦合**：DC 直接级联，省下面积巨大的 bypass cap。
3. **IC 工艺天然匹配好**：同一 die 上的 MOS 匹配优于晷片级；diff 对多用管在 IC 上不是问题。
</aside>

---

## 1️⃣2️⃣ MOS 差分对：基本结构（slide 16）

[Slide 16 — MOS diff pair basic](https://app.notion.com)

Slide 16 — MOS diff pair basic

- 两个完全匹配的 NMOS $Q_1, Q_2$，源极汇合到尾电流源 $I$。
- 差分输入：$v_{G1} = V_{CM} + \tfrac{v_{id}}{2}$，$v_{G2} = V_{CM} - \tfrac{v_{id}}{2}$。
- 差分输出：$v_{D1}$ 与 $v_{D2}$ 之差。
- 零差分输入时：$V_{GS1}=V_{GS2}=V_t+\sqrt{I/k_n}$ 是固定值。

---

## 1️⃣3️⃣ 共模输入与 ICMR（slides 17–19）

[Slide 17 — Common-mode operation](https://app.notion.com)

Slide 17 — Common-mode operation

[Slide 18 — ICMR limits](https://app.notion.com)

Slide 18 — ICMR limits

[Slide 19 — ICMR numerical example](https://app.notion.com)

Slide 19 — ICMR numerical example

### 共模抑制原理

当 $v_{G1}=v_{G2}=V_{CM}$ 时：

- $V_{GS}$ 保持 $V_t + V_{OV}$，$V_S = V_{CM} - V_{GS}$ 随 $V_{CM}$ 平移。
- $I_{D1}=I_{D2}=I/2$ **保持不变**。
- 于是 $v_{D1}=v_{D2}=V_{DD}-(I/2)R_D$，与 $V_{CM}$ **无关** → 理想 CMRR = ∞。

### 输入共模范围 (ICMR)

- **上限**：$Q_1, Q_2$ 保持饱和 → $v_{D1}\ge v_{G1} - V_t$

$$
V_{CM,\max} = V_t + V_{DD} - \tfrac{I}{2}R_D
$$

- **下限**：尾电流源需要最低压降 $V_{CS}$

$$
V_{CM,\min} = -V_{SS} + V_{CS} + V_t + V_{OV}
$$

### 数值例题（slide 18–19）

$V_{DD}=V_{SS}=1.5\ \text{V}$，$I=0.4\ \text{mA}$，$R_D=2.5\ \text{k}\Omega$，$V_{CS}=0.4\ \text{V}$，$k_n=4\ \text{mA/V}^2$，$V_{tn}=0.5\ \text{V}$：

- $V_{OV}=\sqrt{(I/2)/(k_n/2)}=0.316\ \text{V}$
- $V_{GS}=0.82\ \text{V}$
- $I_{D1}=I_{D2}=0.2\ \text{mA}$
- $V_{D1}=V_{D2}=1.5-0.5=1.0\ \text{V}$

| $V_{CM}$ | $V_S$ | 说明 |
| --- | --- | --- |
| 0 V | −0.82 V | 正常工作点 |
| +1 V | +0.18 V | 共模上抬，$I_D, V_D$ 不变 |
| −0.2 V | −1.02 V | 共模下拉，仍在 ICMR 内 |

**ICMR**：$-0.28\ \text{V} \le V_{CM} \le +1.5\ \text{V}$；只要在范围内，$I_D, V_D$ 全部保持不变。

---

## 1️⃣4️⃣ 大信号差分操作（slides 20–22）

[Slide 20 — Large-signal transfer](https://app.notion.com)

Slide 20 — Large-signal transfer

[Slide 21 — Current steering](https://app.notion.com)

Slide 21 — Current steering

[Slide 22 — Small-signal approximation](https://app.notion.com)

Slide 22 — Small-signal approximation

### 大信号公式

$$
i_{D1,2} = \frac{I}{2}\pm\sqrt{k_n'\tfrac{W}{L}I}\,\left(\frac{v_{id}}{2}\right)\sqrt{1-\left(\frac{v_{id}/2}{V_{OV}}\right)^2}
$$

$$
i_{D1}+i_{D2}=I\quad\text{(尾电流恒定)}
$$

- 当 $|v_{id}|\ge\sqrt{2}V_{OV}$ 时，**一管截止、另一管拿全部** $I$ → 这就是「**电流舵 (current steering)**」的物理基础。

### 小信号线性近似

当 $|v_{id}|\ll V_{OV}$ 时：

$$
i_{D1,2}\simeq \frac{I}{2}\pm\frac{I}{V_{OV}}\cdot\frac{v_{id}}{2}
$$

即每管变化 $\Delta i = g_m \cdot v_{id}/2$，其中 $g_m = I/V_{OV} = 2I_D/V_{OV}$。

<aside>
⚠️

**增益 vs 线性度 trade-off**：$V_{OV}\uparrow$ → 线性范围宽（$\sqrt{2}V_{OV}$ 变大），但 $g_m=I/V_{OV}\downarrow$。这是 diff pair 设计的核心调参点。

</aside>

---

## 1️⃣5️⃣ 小信号操作 & Virtual Ground（slide 23）

[Slide 23 — Small-signal & virtual ground](https://app.notion.com)

Slide 23 — Small-signal & virtual ground

差分输入 + 反对称 → 公共源点 $V_S$ 是 **virtual ground**：

$$
g_m = \frac{2I_D}{V_{OV}} = \frac{I}{V_{OV}},\qquad A_d \equiv \frac{v_{od}}{v_{id}} = g_m R_D
$$

<aside>
💡

**Virtual ground 的妙处**：不用大耦合 / bypass 电容就实现「交流接地」 → **面积小、频响好**。这是 IC 差分拓扑胜过单端拓扑的一个关键原因。

</aside>

---

## 1️⃣6️⃣ 差分半电路（slide 24）

[Slide 24 — Differential half-circuit](https://app.notion.com)

Slide 24 — Differential half-circuit

由于反对称，只需分析**一半**（一边的共源管 + $R_D$，source 直接接地）：

$$
A_d = g_m(R_D \| r_o)
$$

后续所有差分小信号分析（频响、噪声、稳定性）都从半电路展开。

- 💬 Q&A：为什么 source 公共点可以等效成地？
    
    **问题：** slide 24 里差分半电路下面为什么能等效成地？是不是因为左边流进这个点多少，右边就一定流回来多少？
    
    **短答：** 这个理解接近，但要更精确地说：这里的“地”是 **AC virtual ground（交流虚地）**，不是因为这个节点真的接到了物理地，而是因为在纯差分激励下，左右两边对公共源点的交流作用完全对称、大小相等、方向相反，所以公共源点的交流电压变化为 0。
    
    **怎么想：**
    
    - 差分输入是反对称的：
        - 左管 gate 加 $+v_{id}/2$
        - 右管 gate 加 $-v_{id}/2$
    - 两管完全匹配，因此小信号电流变化也是反对称：
        - 左边电流增加 $+g_m v_{id}/2$
        - 右边电流减少 $-g_m v_{id}/2$
    - 所以尾电流源看到的总交流电流：
    
    $$
    	\Delta i_{D1}+\Delta i_{D2}
    =
    g_m\frac{v_{id}}{2}-g_m\frac{v_{id}}{2}
    =
    0
    $$
    
    **关键结论：** 公共源点没有净交流电流需要流进尾电流源，因此这个点不需要上下摆动来“找电流通路”；它的交流电压固定为 0，于是可当作 AC ground。
    
    <aside>
    🔑
    
    **口诀：** 不是“左边流进地、右边从地流回来”这么像真实地线的说法；更准确是：左右电流扰动互相抵消，公共源点电压不动，所以它是 virtual ground。
    
    </aside>
    

---

## 1️⃣7️⃣ 差分对 + 电流源负载（slide 25）

[Slide 25 — Diff pair with current-source load](https://app.notion.com)

Slide 25 — Diff pair with current-source load

把 $R_D$ 换成 PMOS 电流源 $Q_3, Q_4$：

$$
A_d = g_{m1}(r_{o1}\|r_{o3})
$$

比简单电阻负载高一两个量级（$r_o$ 远大于实际 IC 能做的 $R_D$）——这是 **“IC 里用 active load 代替电阻”** 背后的第一原因。

---

## 1️⃣8️⃣ Cascode 差分放大器（slide 26）

[Slide 26 — Cascode differential amplifier](https://app.notion.com)

Slide 26 — Cascode differential amplifier

输入对和负载都用 cascode：

$$
A_d = g_{m1}(R_{on}\|R_{op})
$$

$$
R_{on}=(g_{m3}r_{o3})\, r_{o1},\quad R_{op}=(g_{m5}r_{o5})\, r_{o7}
$$

源管如同如果应对称：$A_d \approx \tfrac{1}{2}A_0^2$。

<aside>
🎯

**单级差分放大器能拿** $60\sim 80\ \text{dB}$。现代 op-amp 的输入级 = **diff-pair + cascode active load**，是所有 OTA 设计的起点。

</aside>

---

## 📝 本节总结（本节记三点即可）

<aside>
🎯

1. **Cascode 是把 IC 增益拉到** $A_0^2$ **的标准武器**：上下两边都要 cascode，否则被负载拖累；低 $V_{DD}$ 时用 folded cascode。
2. **差分对天然抗共模、不需耦合电容**；ICMR 由饱和条件和尾电流源压降共同决定。
3. **大信号 → current steering（达** $\sqrt{2}V_{OV}$ **完全转移）；小信号 → virtual ground 半电路，**$A_d = g_m(R_D\|r_o)$**。** 增益 vs 线性度 由 $V_{OV}$ 调节。
</aside>

---

[Slide 27 — Homework 6 list](https://app.notion.com)

Slide 27 — Homework 6 list

## 📚 Homework 6 待做清单

- [x]  Sedra/Smith 8.1
- [x]  Sedra/Smith 8.4
- [x]  Sedra/Smith 8.6
- [x]  Sedra/Smith 8.40
- [x]  Sedra/Smith 8.43
- [x]  Sedra/Smith 8.65
- [x]  Sedra/Smith 8.67
- [x]  Sedra/Smith 8.71

*Due: 2026-05-20 23:00（Blackboard 提交）*