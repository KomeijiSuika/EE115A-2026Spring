# EE115 Lecture17 — 期末复习：MOSFET → 单级 → 差分 → 运放 → 反馈

<aside>
📌

**本节主题：** 全课程总复习（Review of the course）—— 从 MOSFET 器件，到单级放大器、电流镜 / cascode、差分对、两级 CMOS 运放，最后落到负反馈基础。

**参考书：** Sedra/Smith — *Microelectronic Circuits*（Ch.5 器件 / Ch.7 单级 / Ch.8 IC building blocks & diff pair / Ch.9 op-amp / Ch.10 feedback）。

**核心脉络：** 器件 I-V → 小信号 $g_m, r_o$ → CS/CG/CD 三拓扑 → 电流镜 & 本征增益 $A_0$ → 阻抗变换 & cascode（$A_0^2$）→ 差分对（virtual ground / 半电路）→ 两级 CMOS 运放 → 负反馈四大好处。

</aside>

<aside>
🧭

**这是期末复习课**，每一节都是把前面 Lecture 的核心一句话拎出来。配套详细笔记见课程主页 [模拟电路 EE115A](../%E6%A8%A1%E6%8B%9F%E7%94%B5%E8%B7%AF%20EE115A.md)。临近的 **Course Project（≥5 页 IEEE 格式）** 截止 June 20, 2026 11:00 PM。

</aside>

---

## 1️⃣ MOSFET 器件与工作区（slides 3–11）

### 器件结构

![Slide 3 — Enhancement-type NMOS 结构](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_3.png)

Slide 3 — Enhancement-type NMOS 结构

- **N 沟道增强型 MOSFET**：4 端器件 —— Source(源 n+) / Drain(漏 n+) / Gate(栅，金属 + 绝缘层) / Body(衬底 p 型)。
- 栅压为正 → 在衬底表面感应出**电子反型层（沟道）**；电子从 S 流向 D，故电流 $i_D$ 方向 **D→S**。

### 三个工作区

$$
V_{OV} \equiv V_{GS} - V_t \quad(\text{过驱动电压，overdrive})
$$

| 区域 | 条件 | $i_D$ 表达式 |
| --- | --- | --- |
| **截止 Cutoff** | $V_{GS}<V_t$ | $i_D=0$ |
| **三极管 Triode** | $v_{DS}<V_{OV}$ | $i_D=k_n'\tfrac{W}{L}\big[V_{OV}v_{DS}-\tfrac12 v_{DS}^2\big]$ |
| **饱和 Saturation** | $v_{DS}\ge V_{OV}$ | $i_D=\tfrac12 k_n'\tfrac{W}{L}V_{OV}^2(1+\lambda v_{DS})$ |

![Slide 8 — NMOS 的 iD–vDS 特性族](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_8.png)

Slide 8 — NMOS 的 iD–vDS 特性族

<aside>
💡

**夹断（Pinch-off）：** 当 $v_{DS}=V_{OV}$ 时漏端沟道电荷为零，沟道在漏端被夹断；继续增大 $v_{DS}$，$i_D$ 基本不再随 $v_{DS}$ 变 → 进入饱和区。放大器都工作在**饱和区**。

</aside>

### 沟长调制（CLM）与输出电阻

![Slide 11 — CLM 导致有限输出电阻](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_11.png)

Slide 11 — CLM 导致有限输出电阻

$v_{DS}$ 越大，夹断点向源端移动 → 有效沟长 $\downarrow$ → $i_D$ 微升，这就是 **沟长调制**，体现为有限输出电阻：

$$
r_o = \frac{|V_A|}{I_D} = \frac{1}{\lambda I_D},\qquad V_A = V_A'\,L
$$

<aside>
🔑

**记忆点：** $r_o\propto L$ —— 想要高 $r_o$（高增益），就把沟道做长一点。

</aside>

---

## 2️⃣ 小信号模型（slide 12）

![Slide 12 — MOSFET 小信号 hybrid-π 模型](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_12.png)

Slide 12 — MOSFET 小信号 hybrid-π 模型

在饱和区直流工作点附近线性化，得到统一的小信号原子：

$$
g_m = k_n'\frac{W}{L}V_{OV} = \frac{2I_D}{V_{OV}} = \sqrt{2k_n'\tfrac{W}{L}I_D},\qquad r_o=\frac{|V_A|}{I_D}
$$

$$
\boxed{A_0 \equiv g_m r_o = \frac{2I_D}{V_{OV}}\cdot\frac{|V_A|}{I_D} = \frac{2|V_A|}{V_{OV}} = \frac{2V_A' L}{V_{OV}}}\quad(\text{本征增益})
$$

<aside>
💡

PMOS 用同一套模型，只是所有参数取绝对值（$|V_{GS}|,|V_t|,|V_{OV}|,|V_A|$）并把 $k_n$ 换成 $k_p$。

</aside>

---

## 3️⃣ 单级放大器：CS / CG / CD（slides 13–19）

<aside>
🧪

**晶体管放大器分析四步法（slide 14）：** ① 做直流分析（关掉小信号源）；② 算 $g_m, r_o$；③ 画交流小信号等效（DC 电压源短路、DC 电流源开路、晶体管换 hybrid-π / T 模型）；④ 解电路求增益等指标。

</aside>

### 共源 CS（slide 15）

![Slide 15 — Common-Source 放大器](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_15.png)

Slide 15 — Common-Source 放大器

$$
G_v = -g_m(R_D\|R_L\|r_o),\qquad R_{in}=\infty,\qquad R_o = R_D\|r_o
$$

用 hybrid-π 模型「几乎一眼写出」$A_v=-g_mR_D$，比死套 MOSFET 方程省事得多。

### CS + 源极退化（Source Degeneration，slides 16–17）

![Slide 16 — CS with source resistance（T 模型）](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_16.png)

Slide 16 — CS with source resistance（T 模型）

源端接电阻时改用 **T 模型** 最快：

$$
A_v = -\frac{R_D}{\tfrac{1}{g_m}+R_s} = -\frac{\text{漏端总电阻}}{\text{源端总电阻}}
$$

$R_s$ 引入**负反馈**：稳定 $I_D$、压低 $v_{gs}=v_i\frac{1}{1+g_mR_s}$ 提高线性度、展宽带宽。代价是增益下降。

### 共栅 CG（slide 18）

![Slide 18 — Common-Gate 放大器](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_18.png)

Slide 18 — Common-Gate 放大器

$R_{sig}$ 接源端 → 用 T 模型。CG **同相、低输入阻抗、好高频**：

$$
R_{in}=\frac{1}{g_m},\qquad A_v=+g_m R_L\ (\text{忽略 }r_o)
$$

### 共漏 CD / 源极跟随器（slide 19）

![Slide 19 — Common-Drain / Source Follower](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_19.png)

Slide 19 — Common-Drain / Source Follower

$R_L$ 接源端 → T 模型。**电压跟随、低输出阻抗、做 buffer**：

$$
A_v=\frac{R_L}{R_L+\tfrac{1}{g_m}}\approx 1,\qquad R_{out}=\frac{1}{g_m}
$$

| 拓扑 | $R_{in}$ | $R_{out}$ | 增益 | 用途 |
| --- | --- | --- | --- | --- |
| **CS** | $\infty$ | $R_D\|r_o$ | 大、反相 | 主增益级 |
| **CG** | $1/g_m$（小） | 高 | $+g_mR_L$ | cascode 上管 / 高频 |
| **CD** | $\infty$ | $1/g_m$（小） | $\approx 1$ | 电压 buffer |

---

## 4️⃣ 电流镜与本征增益（slides 20–22）

### 电流镜（slide 20）

![Slide 20 — Current Mirror](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_20.png)

Slide 20 — Current Mirror

忽略沟长调制时，输出电流由 $W/L$ 之比镜像：

$$
\frac{I_O}{I_{REF}} = \frac{(W/L)_2}{(W/L)_1}
$$

带参考源 + 考虑 CLM 时，$I_O$ 会随输出端电压略变（受 $r_o$ 限制）—— 这正是后面要用 cascode 把电流源做「更理想」的动机。

### 理想电流源负载的基本增益单元（slide 22）

![Slide 22 — Basic gain cell（ideal current-source load）](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_22.png)

Slide 22 — Basic gain cell（ideal current-source load）

用理想电流源（$r_L\to\infty$）替代电阻负载，CS 增益就被推到**本征增益**：

$$
A_v = -g_m r_o = -A_0
$$

这是单管能拿到的增益上限 → 想更高，必须靠阻抗变换 / cascode。

---

## 5️⃣ 阻抗变换 → Cascode（slides 23–29）

<aside>
🔑

**Cascode 灵魂两句话⭐**

- **源极退化（向上看）：** 源端电阻被本征增益**乘** → $R_{out}\simeq A_0 R_s$。
- **共栅（向下看）：** 漏端负载被本征增益**除** → $R_{in}\simeq \tfrac{1}{g_m}+\tfrac{R_L}{A_0}$。
</aside>

### 两条阻抗变换规律（slides 24–25）

![Slide 24 — 源极阻抗向输出端变换（×A₀）](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_24.png)

Slide 24 — 源极阻抗向输出端变换（×A₀）

$$
R_{out}=r_o+R_s+g_m r_o R_s \simeq (g_m r_o)R_s = A_0 R_s
$$

![Slide 25 — 共栅负载向输入端变换（÷A₀）](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_25.png)

Slide 25 — 共栅负载向输入端变换（÷A₀）

$$
R_{in}=\frac{r_o+R_L}{1+g_m r_o}\simeq \frac{1}{g_m}+\frac{R_L}{A_0}
$$

### MOS Cascode 放大器（slide 27）

![Slide 27 — MOS Cascode Amplifier](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_27.png)

Slide 27 — MOS Cascode Amplifier

$Q_1$ 共源提供 $g_m$，$Q_2$ 共栅做阻抗变换：

$$
R_o \simeq (g_{m2}r_{o2})\,r_{o1}=A_0 r_o,\qquad A_{vo}=-g_{m1}R_o = -A_0^2
$$

**多堆一层 → 增益从** $A_0$ **提到** $A_0^2$**。**

### Cascode + 简单 active load（slide 28）

![Slide 28 — Cascode + simple active load](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_28.png)

Slide 28 — Cascode + simple active load

<aside>
⚠️

**踩坑：** 输入侧阻抗被推到 $A_0^2 r_o$，但负载只是单个电流源 $r_{o3}$ → 并联后 $R_{out}\simeq r_{o3}$，增益被负载拉回 $-g_{m1}r_{o3}$。**要拿** $A_0^2$**，上下两边都得 cascode。**

</aside>

### Cascode + cascode 电流源负载（slide 29）

![Slide 29 — Cascode + cascode current-source load](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_29.png)

Slide 29 — Cascode + cascode current-source load

$$
A_v = -g_{m1}(R_{on}\|R_{op}) = -\tfrac{1}{2}A_0^2\ (\text{NMOS/PMOS 对称})
$$

虽比单端少一半，但这已是 **single-stage 能拿的最高电压增益**。

---

# Part II — 差分放大器 & 运放（slides 30–39）

## 6️⃣ MOS 差分对（slides 30–36）

### 基本结构（slide 30）

![Slide 30 — MOS Differential Pair 基本结构](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_30.png)

Slide 30 — MOS Differential Pair 基本结构

- 两个匹配 NMOS $Q_1,Q_2$，源极汇合到尾电流源 $I$。
- 差分输入 $v_{G1}=V_{CM}+\tfrac{v_{id}}{2}$、$v_{G2}=V_{CM}-\tfrac{v_{id}}{2}$；差分输出取 $v_{D1}-v_{D2}$。
- 零差分输入时两管 $V_{GS}$ 固定、$I_{D1}=I_{D2}=I/2$。

### 小信号操作 & Virtual Ground（slide 31）

![Slide 31 — Small-signal & Virtual Ground](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_31.png)

Slide 31 — Small-signal & Virtual Ground

差分信号下电路**反对称**，公共源点电位不动 → **虚地（virtual ground）**：

$$
g_m=\frac{2I_D}{V_{OV}}=\frac{I}{V_{OV}}
$$

<aside>
💡

**妙处：** 不用大 bypass 电容就实现「交流接地」→ 面积小、频响好。这是 IC 里差分拓扑胜过单端的关键。

</aside>

### 差分半电路（slide 32）

![Slide 32 — Differential Half Circuit](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_32.png)

Slide 32 — Differential Half Circuit

因反对称 + 源端虚地，只需分析**一半**（一个共源管，source 直接接地）：

$$
A_d = g_m(R_D\|r_o)
$$

### 电流源负载差分对（slide 33）

![Slide 33 — Diff pair with current-source loads](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_33.png)

Slide 33 — Diff pair with current-source loads

把 $R_D$ 换成 PMOS 电流源 → 负载电阻变 $r_o$，增益跳一两个量级：

$$
A_d = g_{m1}(r_{o1}\|r_{o3})
$$

### Cascode 差分放大器（slide 34）

![Slide 34 — Cascode Differential Amplifier](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_34.png)

Slide 34 — Cascode Differential Amplifier

输入对和负载都用 cascode：

$$
A_d=g_{m1}(R_{on}\|R_{op})\approx \tfrac12 A_0^2
$$

单级差分放大器可达 $60\sim 80\ \text{dB}$。

### 电流镜负载差分对（slides 35–36）⭐

![Slide 35 — Diff pair with current-mirror load](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_35.png)

Slide 35 — Diff pair with current-mirror load

用电流镜（$Q_3,Q_4$）做有源负载，把**差分输入 → 单端输出**，且电流相加使 $G_m$ 拿满 $g_{m1}$：

$$
G_m=g_{m1},\qquad A_d = g_{m1}(r_{o2}\|r_{o4})
$$

<aside>
🎯

**这就是运放第一级标准形态** —— 差分对 + 电流镜负载，天然完成「双端转单端」且不损失增益。

</aside>

## 7️⃣ 两级 CMOS 运放（slides 37–39）

![Slide 38 — 两级 CMOS Op-Amp 三个组成模块](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_38.png)

Slide 38 — 两级 CMOS Op-Amp 三个组成模块

经典两级运放 = **电流镜偏置 + 差分对（电流镜负载）增益级 + 共源增益级**：

$$
A_1=-g_{m1}(r_{o2}\|r_{o4})\quad(\text{第一级})
$$

$$
A_2=-g_{m6}(r_{o6}\|r_{o7})\quad(\text{第二级 CS})
$$

$$
\boxed{A_o=A_1A_2}
$$

<aside>
🎯

这是所有模拟信号链的「原型」：后续的带宽 / Miller 补偿 / 噪声 / 稳定性分析都围绕它展开。

</aside>

---

# Part III — 负反馈基础（slides 40–48）

## 8️⃣ 一般反馈结构（slides 40–42）

![Slide 40 — The general feedback structure](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_40.png)

Slide 40 — The general feedback structure

$$
A_f=\frac{A}{1+A\beta}
$$

- $A$：开环增益；$\beta$：反馈系数；$A\beta$：**环路增益（loop gain）**。
- $A\beta$ 的**符号**决定反馈极性；**大小**决定 $A_f$ 多接近理想值 $1/\beta$。
- $A\beta\gg1$ 时 $A_f\approx 1/\beta$ —— 闭环增益**只由反馈网络决定**。

<aside>
🔑

**去敏因子（desensitivity factor）**$1+A\beta$**：** 闭环增益对开环漂移的灵敏度被压低 $(1+A\beta)$ 倍。

$$
\frac{dA_f/A_f}{dA/A}=\frac{1}{1+A\beta}
$$

</aside>

## 负反馈四大好处（slides 43–45）

### 1) 展宽带宽（slide 43）

![Slide 43 — Feedback bandwidth extension](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_43.png)

Slide 43 — Feedback bandwidth extension

$$
\omega_{Hf}=\omega_H(1+A\beta),\qquad \omega_{Lf}=\frac{\omega_L}{1+A\beta}
$$

增益 ÷$(1+A\beta)$、带宽 ×$(1+A\beta)$ → **增益带宽积守恒**。

### 2) 抑制干扰 / 提高 SNR（slide 44）

负反馈对「**前级注入**」的干扰相对信号有改善（取决于前级增益），从而提高有效信噪比。

### 3) 减小非线性失真（slide 45）

![Slide 45 — Reduction in nonlinear distortion](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_45.png)

Slide 45 — Reduction in nonlinear distortion

开环转移曲线斜率（增益）忽大忽小 → 加反馈后被「拉直」，闭环斜率近似恒为 $1/\beta$，失真被压低 $(1+A\beta)$ 倍。

### 4) 稳定增益（即上面的去敏）。

## 系统化分析：Series–Shunt 反馈放大器（slides 46–48）

![Slide 46 — Ideal series–shunt（A 电路 / β 电路）](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/slide_46.png)

Slide 46 — Ideal series–shunt（A 电路 / β 电路）

理想 series–shunt（电压采样、电压混合）反馈放大器 = **单向开环 A 电路 + 理想 β 网络**。分析套路：把负载 / 源 / 反馈网络的 loading 折进 A 电路，得到 $A,R_i,R_o$，再用 $A_f=\dfrac{A}{1+A\beta}$ 与端口电阻公式求闭环指标。

---

## 📝 本节总结（复习只背这几条）

<aside>
🎯

1. **一切的原子：** 饱和区 + $g_m=\tfrac{2I_D}{V_{OV}}$、$r_o=\tfrac{|V_A|}{I_D}$、本征增益 $A_0=g_mr_o=\tfrac{2V_A'L}{V_{OV}}$。
2. **三拓扑分工：** CS 主增益、CG 低输入阻抗 / 做 cascode 上管、CD 低输出阻抗做 buffer；「$A_v=-\tfrac{\text{漏端总阻}}{\text{源端总阻}}$」。
3. **高增益 = 高输出阻抗**：$A_v=g_mR_{out}$，cascode 把 $R_{out}$ 推到 $A_0^2 r_o$（上下都要 cascode）。
4. **差分对：** 抗共模、虚地省电容、半电路分析；电流镜负载完成双端转单端且 $G_m=g_{m1}$。
5. **两级运放：** 差分级 + CS 级，$A_o=A_1A_2$。
6. **负反馈：** $A_f=\tfrac{A}{1+A\beta}$，$(1+A\beta)$ 同时带来「稳增益 / 展带宽 / 降失真 / 抑干扰」四大好处，代价是增益变小。
</aside>

---

## 📎 原始 Slides

[EE115 Lecture17.pdf](EE115%20Lecture17%20%E2%80%94%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%EF%BC%9AMOSFET%20%E2%86%92%20%E5%8D%95%E7%BA%A7%20%E2%86%92%20%E5%B7%AE%E5%88%86%20%E2%86%92%20%E8%BF%90%E6%94%BE%20%E2%86%92%20%E5%8F%8D%E9%A6%88/EE115_Lecture17.pdf)