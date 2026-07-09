# EE115 Lecture11 — IC Building Blocks: Current Mirror & Intrinsic Gain

<aside>
📌

**本节主题：** Building Blocks of IC Amplifier — Current Mirror / Current Steering / Intrinsic Gain Cell

**参考书：** Sedra/Smith — pp. 509–529（Chapter 8 §8.1–8.2）

**核心脉络：** IC 设计哲学（与 discrete 的差异） → 电流镜上的物理（饱和同 $V_{GS}$） → channel-length modulation 对电流镜的影响→电流舵 (current steering)→小信号模型重读 → 理想电流源负载下的 intrinsic gain $A_0=g_m r_o$ → 现实电流源负载。

</aside>

## 📅 课程行政信息

- Homework 本节为占位（“Homework x — due April xx”），听老师另行发布。

---

## 1️⃣ IC 设计哲学（slides 3–4）

|  | **Discrete 电路** | **IC 电路** |
| --- | --- | --- |
| 元件选择 | 电阻 / 电容广用 | 几乎都是管子（R / C 费面积） |
| 耦合方式 | AC coupled（大耦合 cap） | DC coupled（没大 cap） |
| 电源 | $V_{DD}\sim10$–$30\ \text{V}$ | $V_{DD}\sim1\ \text{V}$ |
| 器件尺寸 | 取决于可用型号 | $W/L$ 可任意定制，**matching** 与 **阵列比例** 是重要设计手段 |
| 器件类型 | BJT 为主 | CMOS 为主（BiCMOS 提供 BJT） |

<aside>
🔑

**三句话 IC 哲学**：用管代替 R/C、低压 DC-coupled、用「匹配 + W/L 比例」作为设计变量。

</aside>

---

## 2️⃣ 电流镜（slides 5–6）

### 2.1 拓扑

两个 NMOS $Q_1, Q_2$ 栅极接在一起、源极接地；$Q_1$ 的栅与漏短路（「diode-connected」），由 $I_{\text{REF}}$（一个电阻 $R$ 从 $V_{DD}$ 接入）注入。$Q_2$ 的漏输出 $I_O$。

[Slide 5 — 电流镜基本拓扑（电阻参考 IREF）](https://app.notion.com)

Slide 5 — 电流镜基本拓扑（电阻参考 IREF）

### 2.2 忽略 channel-length modulation（$\lambda=0$）

两管 $V_{GS}$ 相同：

$$
I_{D1} = I_{\text{REF}} = \tfrac{1}{2}k_n'\!\left(\frac{W}{L}\right)_{\!1}(V_{GS}-V_{tn})^2
$$

$$
I_O = I_{D2} = \tfrac{1}{2}k_n'\!\left(\frac{W}{L}\right)_{\!2}(V_{GS}-V_{tn})^2
$$

$$
\boxed{\;\frac{I_O}{I_{\text{REF}}} = \frac{(W/L)_2}{(W/L)_1}\;}
$$

<aside>
💡

**闪光点**：电流镜就是「IC 里用 W/L 比例复制电流」的原型。一个 $I_{\text{REF}}$ 可以让门外几十个管同时拿到指定比例的偏置电流。

</aside>

### 2.3 考虑 channel-length modulation

$$
i_D = \tfrac{1}{2}k_n'\frac{W}{L}(v_{GS}-V_{tn})^2(1+\lambda v_{DS})
$$

参考管 $Q_1$ 是 diode-connected，$V_{DS1}=V_{GS}$。输出管 $Q_2$ 的 $V_{DS2}=V_O$ 可以不同：

$$
I_O = \frac{(W/L)_2}{(W/L)_1}I_{\text{REF}}\left(1 + \frac{V_O - V_{GS}}{V_{A2}}\right)
$$

从 $V_O$ 看进去：输出电流随 $V_O$ 以斜率 $1/r_{o2}$ 增加（$r_{o2}=|V_{A2}|/I_O$）。最低可用 $V_O = V_{GS}-V_{tn} = V_{OV}$，低于此值 $Q_2$ 进入 triode。

[Slide 6 — 含沟道长度调制时 IO–VO 特性，斜率 = 1/ro](https://app.notion.com)

Slide 6 — 含沟道长度调制时 IO–VO 特性，斜率 = 1/ro

---

## 3️⃣ 例题：电流镜设计（slides 7–8）

**给定**：$V_{DD}=3\ \text{V}$，$I_{\text{REF}}=100\ \mu\text{A}$，$Q_1, Q_2$ 匹配，$L=1\ \mu\text{m}$，$W=10\ \mu\text{m}$，$V_t=0.7\ \text{V}$，$k_n'=200\ \mu\text{A/V}^2$，$V_A'=20\ \text{V/}\mu\text{m}$。设计 $R$，求 $V_{O,\min}$、$r_{o2}$、$\Delta I_O$（$V_O$ 变 $+1\ \text{V}$）。

[Slide 7 — 例题电路（Fig. 8.1 电流镜）](https://app.notion.com)

Slide 7 — 例题电路（Fig. 8.1 电流镜）

### Step 1—用 $I_{D1}$ 求 $V_{OV}$

$$
100 = \tfrac{1}{2}\times 200\times 10\times V_{OV}^2 \Rightarrow V_{OV}=0.316\ \text{V}
$$

### Step 2—求 $V_{GS}$ 与 $R$

$$
V_{GS} = V_t + V_{OV} = 0.7 + 0.316 \simeq 1\ \text{V}
$$

$$
R = \frac{V_{DD}-V_{GS}}{I_{\text{REF}}} = \frac{3-1}{0.1\ \text{mA}} = 20\ \text{k}\Omega
$$

### Step 3—输出范围

$$
V_{O,\min} = V_{OV} \simeq 0.3\ \text{V}
$$

以上此值，$Q_2$ 始终在饱和区，才是理想电流源。

### Step 4—输出阻抗

$$
V_A = V_A'\cdot L = 20\times 1 = 20\ \text{V}
$$

$$
r_{o2} = \frac{V_A}{I_O} = \frac{20\ \text{V}}{100\ \mu\text{A}} = 0.2\ \text{M}\Omega
$$

### Step 5—$V_O$ 变 1V 引起的 $\Delta I_O$

$$
\Delta I_O = \frac{\Delta V_O}{r_{o2}} = \frac{1\ \text{V}}{0.2\ \text{M}\Omega} = 5\ \mu\text{A}
$$

相对 $100\ \mu\text{A}$ 中的 5%。

<aside>
💡

**记忆组合**：$V_A=V_A'\cdot L$，$r_o=V_A/I_D$。想要更准的电流源 → 只能 $L$ 加大或上 cascode（下一节）。

</aside>

---

## 4️⃣ MOS Current Steering Circuits（slide 9）

[Slide 9 — MOS 电流舵：NMOS 镜链 + PMOS 镜链](https://app.notion.com)

Slide 9 — MOS 电流舵：NMOS 镜链 + PMOS 镜链

一个 $I_{\text{REF}}$ 同时给“一阵”管子供偏置：

- $Q_1$ diode-connected 生成 $V_{GS1}$；$Q_2, Q_3$ 共享该 $V_{GS1}$ 拿到不同比例的电流：

$$
I_2 = I_{\text{REF}}\frac{(W/L)_2}{(W/L)_1},\qquad I_3 = I_{\text{REF}}\frac{(W/L)_3}{(W/L)_1}
$$

- $Q_3$ 的漏连接一个 PMOS 镜 $Q_4$（栅漏短），$Q_4$ 生成 $V_{SG4}=V_{SG5}$，让 PMOS 镜 $Q_5$ 输出另一个电流 $I_5$：

$$
I_5 = I_4\frac{(W/L)_5}{(W/L)_4}
$$

<aside>
🔑

**「奇骑兵式」偏置**：一条 NMOS-mirror 链 + 一条 PMOS-mirror 链 → 门外任意节点都能拿到任意比例的 sink 或 source。这就是后面差分对 / op-amp 里跟随起伏那些“看似独立”的电流源的真面目。

</aside>

---

## 5️⃣ 电流镜的小信号模型（slide 10）

[Slide 10 — 电流镜小信号模型（Rin≈1/gm1, Ro=ro2, Ais=gm2/gm1）](https://app.notion.com)

Slide 10 — 电流镜小信号模型（Rin≈1/gm1, Ro=ro2, Ais=gm2/gm1）

点亮 $Q_1, Q_2$ 后加入小信号激励 $i_i$ 到 $Q_1$ 的漏点（= $Q_2$ 的栅点），看 $Q_2$ 的漏电流 $i_o$。

$$
R_{\text{in}} = r_{o1}\|\frac{1}{g_{m1}}\simeq \frac{1}{g_{m1}}
$$

$$
R_o = r_{o2}
$$

$$
A_{is}\equiv\frac{i_o}{i_i}\bigg|_{v_{d2}=0} = \frac{g_{m2}}{g_{m1}} = \frac{(W/L)_2}{(W/L)_1}
$$

<aside>
💡

**含义**：电流镜**不只传递 DC**，也传递 AC 小信号 —— 增益是 $(W/L)$ 比例；输入阻 $1/g_m$ 低（diode-connected 万用法则），输出阻 $r_o$ 高（饱和管点看进去）。

</aside>

---

## 6️⃣ 理想电流源负载的增益单元（slides 11–12）

### 6.1 拓扑

$Q_1$ 共源，负载是理想电流源 $I$（阻抗 $=\infty$）。

[Slide 11 — 理想电流源负载增益单元及其小信号模型](https://app.notion.com)

Slide 11 — 理想电流源负载增益单元及其小信号模型

$$
A_{vo} = -g_m r_o \equiv -A_0
$$

$$
R_{\text{in}} = \infty,\quad R_o = r_o
$$

### 6.2 三个表式 $A_0$ 都要记住

$$
g_m = \frac{I_D}{V_{OV}/2},\quad r_o = \frac{V_A}{I_D} = \frac{V_A'L}{I_D},\quad g_m=\sqrt{2\mu_n C_{ox}(W/L)\, I_D}
$$

$$
A_0 = g_m r_o = \frac{V_A}{V_{OV}/2} = \boxed{\;\frac{2V_A'L}{V_{OV}}\;}
$$

$$
A_0 = \frac{V_A'\sqrt{2\mu_n C_{ox}(WL)}}{\sqrt{I_D}}\quad\Rightarrow\quad A_0\propto\frac{1}{\sqrt{I_D}}
$$

### 6.3 $A_0$ 与 $I_D$ 的关系（子阈值 vs 强反型）

[Slide 12 — A0 随 ID 变化（log-log）：强反型斜率 −1/2，亚阈值平台](https://app.notion.com)

Slide 12 — A0 随 ID 变化（log-log）：强反型斜率 −1/2，亚阈值平台

- **强反型区 (strong inversion)**：$A_0\propto I_D^{-1/2}$，log-log 上斜率 $-1/2$。
- **亚阈值区 (subthreshold)**：$A_0$ 达到上限平台（限于 $V_A/\phi_T$），再减小 $I_D$ 也不提高了。

<aside>
⚠️

**Trade-off**：$I_D\downarrow\Rightarrow A_0\uparrow$，但 $g_m\downarrow$ → 带宽与噪声全部变差。IC 设计的总原则：**在带宽 / 增益 / 功耗 / 噪声四者折中**。

</aside>

---

## 7️⃣ 现实电流源负载（slide 13）

用一个 PMOS 管 $Q_2$（栅接偏置 $V_G$）作为主管 $Q_1$（NMOS）的负载。

[Slide 13 — 现实电流源（PMOS）负载及小信号等效](https://app.notion.com)

Slide 13 — 现实电流源（PMOS）负载及小信号等效

$Q_2$ 提供的输出阻：

$$
r_{o2} = \frac{|V_{A2}|}{I}
$$

$$
A_v = \frac{v_o}{v_i} = -g_{m1}(r_{o1}\|r_{o2})
$$

若 NMOS / PMOS 取的 $r_o$ 量级相近，则 $A_v\sim -\tfrac{1}{2}A_0$。

<aside>
💡

**实用同估**：0.18 µm、$V_{OV}=0.2\sim 0.3\ \text{V}$ 时，intrinsic gain $A_0\approx 30\sim 60$ 倍；加实际 active load 后 $A_v\approx 15\sim 30$ 倍。想拿更高增益 → 下一节的 **cascode**。

</aside>

---

## 📝 本节总结（本节记三条）

<aside>
🎯

1. **电流镜是 IC 里万用偏置机**：同 $V_{GS}$ + W/L 比例复制电流；包含 channel-length modulation 后输出斜率 = $1/r_o$。
2. **Intrinsic gain** $A_0 = g_m r_o = 2V_A'L/V_{OV}$是一切后续拓扑的上限。$L\uparrow$ 或 $V_{OV}\downarrow$ 能拿更高增益，但带宽 / 匹配 / 噪声被牺牲。
3. **增益 =** $g_m\times R_{\text{out}}$。要怅原宝可以走两条路：加大 $g_m$（= $\sqrt{2k_n'(W/L)I_D}$），或加大 $R_{\text{out}}$（= cascode）。IC 设计的几乎所有招数都在这两条路上。
</aside>

---

## 📚 Homework（占位，等实际发布）

- [ ]  SEDRA/SMITH Book chapter x problem xx — due April xx 23:00

---

## 📎 原始 Slides

[EE115 Lecture11.pdf](EE115%20Lecture11%20%E2%80%94%20IC%20Building%20Blocks%20Current%20Mirro/EE115_Lecture11.pdf)