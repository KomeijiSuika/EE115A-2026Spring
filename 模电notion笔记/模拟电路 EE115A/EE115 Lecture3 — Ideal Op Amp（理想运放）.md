# EE115 Lecture3 — Ideal Op Amp（理想运放）

<aside>
📘

**主题：** 理想运放（Ideal Op Amp）—— 从理想运放的五大特性出发，用「虚短 + 虚断」分析反相 / 同相 / 加权求和 / 跟随器 / 差分五大经典拓扑。

**教材：** Sedra/Smith — *Microelectronic Circuits*，pp.59–81（Ch.2）。

**核心脉络：** 理想运放（$R_i=\infty$、$R_o=0$、$A=\infty$、无限带宽、无限 CMRR）→ 用 **虚短（**$v_+=v_-$**）+ 虚断（输入不抽流）** 两条黄金法则 → 推导反相 $-R_2/R_1$ / 同相 $1+R_2/R_1$ / 加权求和 / 跟随器（buffer）/ 差分放大器（CMRR）。这是后面所有运放应用的分析起点。

</aside>

## 1️⃣ 理想运放五大特性

- **无限输入阻抗** $R_i=\infty$ → 输入端不抽电流（**虚断**）。
- **零输出阻抗** $R_o=0$。
- **无限开环增益** $A=\infty$ → 配合**负反馈**才能定出有限闭环增益（否则输出饱和）。
- **无限带宽**、**无限共模抑制（CMRR）**。

两条分析黄金法则（闭环、$A=\infty$ 时）：

$$
\boxed{v_+ = v_-\ (\text{虚短})\qquad i_+ = i_- = 0\ (\text{虚断})}
$$

[Slide 3 — 理想运放接 dc 电源（Fig 2.2）与等效电路（Fig 2.3）](https://app.notion.com)

Slide 3 — 理想运放接 dc 电源（Fig 2.2）与等效电路（Fig 2.3）

<aside>
🎯

**口诀：** 一切运放分析都从「虚短 + 虚断」两句话出发；无限增益靠**负反馈**驯服成有限闭环增益。

</aside>

## 2️⃣ 反相放大器 Inverting Amplifier

- 同相端接地 → 虚短使反相端成为 **虚地（virtual ground,** $v_-=0$**）**。
- 电流路径：$i = \dfrac{v_i}{R_1}$ 全部流过 $R_2$（虚断）→ 闭环增益：

$$
\frac{v_o}{v_i} = -\frac{R_2}{R_1}
$$

- **考虑有限开环增益** $A$**：**

$$
\frac{v_o}{v_i} = \frac{-R_2/R_1}{1 + \dfrac{1}{A}\left(1+\dfrac{R_2}{R_1}\right)} \xrightarrow{A\to\infty} -\frac{R_2}{R_1}
$$

- **输入 / 输出电阻：** 因虚地，$R_{in}=R_1$；理想运放 $R_{out}=0$。

[Slide 4 — 反相闭环配置（Fig 2.5）与虚地分析步骤（Fig 2.6）](https://app.notion.com)

Slide 4 — 反相闭环配置（Fig 2.5）与虚地分析步骤（Fig 2.6）

<aside>
🎯

**口诀：** 反相接法看「虚地」—— 增益 $=-R_2/R_1$，输入阻抗仅 $R_1$（会衰减源）。

</aside>

## 3️⃣ 加权求和器 Weighted Summer

多路输入通过各自电阻汇到虚地点，电流叠加后流过 $R_f$：

$$
v_o = -\left(\frac{R_f}{R_1}v_1 + \frac{R_f}{R_2}v_2 + \cdots + \frac{R_f}{R_n}v_n\right)
$$

- 每路增益独立可调（靠各自 $R_k$），虚地使各路互不干扰。

[Slide 7 — 加权求和器（Fig 2.10）](https://app.notion.com)

Slide 7 — 加权求和器（Fig 2.10）

<aside>
🎯

**口诀：** 虚地 = 「汇流节点」，各路独立加权、互不干扰—— 模拟加法器的雏形。

</aside>

## 4️⃣ 同相放大器 Non-Inverting Amplifier

- 输入接同相端，虚短使 $v_-=v_i$，由分压得：

$$
\frac{v_o}{v_i} = 1 + \frac{R_2}{R_1}
$$

- **考虑有限开环增益** $A$**：**

$$
\frac{v_o}{v_i} = \frac{1+R_2/R_1}{1 + \dfrac{1}{A}\left(1+\dfrac{R_2}{R_1}\right)} \xrightarrow{A\to\infty} 1+\frac{R_2}{R_1}
$$

- **输入 / 输出电阻：** $R_{in}=\infty$（输入端不抽流），$R_{out}=0$。

[Slide 8 — 同相配置（Fig 2.12）与分析步骤（Fig 2.13）](https://app.notion.com)

Slide 8 — 同相配置（Fig 2.12）与分析步骤（Fig 2.13）

<aside>
🎯

**口诀：** 同相接法增益 $1+R_2/R_1$（总 $\ge1$），且 $R_{in}=\infty$ —— 不衰减信号源，是高输入阻抗放大首选。

</aside>

## 5️⃣ 电压跟随器 Voltage Follower（Buffer）

同相放大器令 $R_2=0$（或 $R_1=\infty$）的特例：

$$
\frac{v_o}{v_i} = 1
$$

- 电压增益 = 1，但**功率增益很大**。
- 用途：**阻抗变换 / 缓冲（buffer）** —— 对信号源呈现 $\infty$ 输入阻抗、对负载呈现 $0$ 输出阻抗；也可作功率驱动级提升驱动电流。

[Slide 11 — 单位增益缓冲 / 跟随器（Fig 2.14a）及等效模型（Fig 2.14b）](https://app.notion.com)

Slide 11 — 单位增益缓冲 / 跟随器（Fig 2.14a）及等效模型（Fig 2.14b）

<aside>
🎯

**口诀：** 跟随器增益为 1，卖的不是电压而是**阻抗隔离**—— 高阶抵低阶，防止后级拖垮前级。

</aside>

## 6️⃣ 差分放大器 Differential Amplifier

### 定义与 CMRR

差分放大器只放大两输入的**差分量**，理想上抑制**共模量**。任意输入可分解为差模 $v_{Id}=v_{I1}-v_{I2}$ 与共模 $v_{Icm}=\tfrac12(v_{I1}+v_{I2})$：

$$
v_o = A_d\,v_{Id} + A_{cm}\,v_{Icm}
$$

$$
\text{CMRR} = 20\log\frac{|A_d|}{|A_{cm}|}\ \text{[dB]}
$$

[Slide 12 — 把输入分解为差模 + 共模分量（Fig 2.15）](https://app.notion.com)

Slide 12 — 把输入分解为差模 + 共模分量（Fig 2.15）

### 差模增益与共模增益（叠加法分析）

标准差分放大器（Fig 2.16，取 $R_3=R_1,\ R_4=R_2$）：

$$
v_o = \frac{R_2}{R_1}\,(v_{I2}-v_{I1}) \quad\Rightarrow\quad A_d = \frac{R_2}{R_1}
$$

- **共模增益：** 桥路平衡（$R_4/R_3 = R_2/R_1$）时 $A_{cm}=0$ → CMRR $\to\infty$；电阻失配才产生有限 $A_{cm}$。
- **输入电阻：** 差模输入电阻 $R_{id}=2R_1$（$R_3=R_1,R_4=R_2$ 时）—— 偏低，是该结构的主要缺点。

[Slide 13 — 差分放大器（Fig 2.16）与叠加法分析（Fig 2.17）](https://app.notion.com)

Slide 13 — 差分放大器（Fig 2.16）与叠加法分析（Fig 2.17）

<aside>
🎯

**口诀：** 差分放大器 = 「只放差、不放和」；CMRR 靠**电阻匹配**（$R_4/R_3=R_2/R_1$）撑高。代价：差模输入阻抗仅 $2R_1$。

</aside>

## 📝 作业 / Deadline

- [x]  **Homework 1** — Sedra/Smith Ch.1 题 1.19, 1.43, 1.68；Ch.2 题 2.8, 2.51, 2.60：截止 March 18, 2026 11:00 PM。

## 🧩 本节总结

<aside>
🧠

**理想运放三句话：**

1. **两条法则：** 虚短（$v_+=v_-$）+ 虚断（输入不抽流）是一切运放分析的起点，靠负反馈把无限增益变成有限闭环增益。
2. **五大拓扑：** 反相 $-R_2/R_1$（虚地，$R_{in}=R_1$）、同相 $1+R_2/R_1$（$R_{in}=\infty$）、加权求和、跟随器（buffer，隔离阻抗）、差分放大器。
3. **差分 & CMRR：** $v_o=A_d v_{Id}+A_{cm}v_{Icm}$，$\text{CMRR}=20\log\frac{|A_d|}{|A_{cm}|}$；电阻匹配越好共模抑制越强。
</aside>

## 📎 原始 Slides

[EE115 Lecture3.pdf](EE115%20Lecture3%20%E2%80%94%20Ideal%20Op%20Amp%EF%BC%88%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/EE115_Lecture3.pdf)