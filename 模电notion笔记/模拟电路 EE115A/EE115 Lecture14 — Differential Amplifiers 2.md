# EE115 Lecture14 — Differential Amplifiers 2

<aside>
📌

**本节主题：** Differential Amplifiers 2（differential amplifier 第二讲）

**参考书：** Sedra/Smith — pages 627–663

**核心脉络：** common-mode analysis → 失配与失调 → differential pair + current-mirror load → 两级 CMOS 运放

</aside>

## 📅 课程行政信息

- **Research Project**（截止 2026-06-20 23:00）：三选一
    1. Wireless neural stimulators（无线神经刺激器）
    2. Ultra-low voltage DC-DC converter for energy harvesting（能量采集用超低压 DC-DC）
    3. Ultra-low power operational amplifiers（超低功耗运放）
    - 流程：文献综述 → 分析现有方法 → 提出新思路（可选）→ IEEE 格式报告 ≥ 5 页
- **Homework 7**（截止 2026-05-27 23:00）：Sedra/Smith Ch. 9 — 9.2, 9.6, 9.19, 9.59, 9.60, 9.71

---

## 1️⃣ common-mode input下的 AC 等效电路

[Slide 4 — AC Equivalent Circuit for Common Mode Input](https://app.notion.com)

Slide 4 — AC Equivalent Circuit for Common Mode Input

- 实际的tail current source = **理想电流源 ∥ 大电阻** $R_{SS}$（非理想模型）。
- common-mode excitation下，两侧对称同相，$Q_1$ 和 $Q_2$ 各流过相同的小信号电流 $i$，公共源端流出 $2i$ 经过 $R_{SS}$ 到地。
- 这是后面所有common-mode analysis的「底图」。

---

## 2️⃣ Common-Mode "Half Circuit"

[Slide 5 — Common Mode Half Circuit](https://app.notion.com)

Slide 5 — Common Mode Half Circuit

<aside>
💡

**differential vs common-mode 的 half circuit 区别：**

- **differential input**：两半电路 *anti-symmetric*，source 是 **virtual ground**，$R_{SS}$ 不出现在半电路里。
- **common-mode input**：两半电路 *symmetric*，source **不再是 virtual ground**。
- 把 $R_{SS}$ 看成两个 $2R_{SS}$ 并联 → 每个 CM 半电路的 source 串 $2R_{SS}$ 到地。
</aside>

这其实就是带 **source degeneration** 的 **common-source amplifier**。

---

## 3️⃣ 理想情况下的 CM 输出电压

[Slide 6 — Ideal CM Output Voltage](https://app.notion.com)

Slide 6 — Ideal CM Output Voltage

<aside>
🧩

$v_{icm}$ = input common-mode voltage，也就是两个输入端**同时加上的同相小信号电压**。在 common-mode analysis 里，通常有 $v_{i1}=v_{i2}=v_{icm}$，所以电路两边会做相同变化，理想情况下不会产生 differential output。

$v_{od}$ = output differential voltage，也就是两个输出端之间的差值，定义为 $v_{od}=v_{o2}-v_{o1}$。如果两边 output 完全一样，$v_{od}=0$；如果两边 output 不一样，就说明 common-mode signal 被转换成了 differential output。

</aside>

$$
v_{o1}=v_{o2}=-\frac{R_D}{1/g_m+2R_{SS}}v_{icm}\simeq-\frac{R_D}{2R_{SS}}v_{icm}
$$

$$
v_{od}=v_{o2}-v_{o1}=0
$$

**理想differential pair** $v_{od}=0$ 的两个前提：

1. 晶体管、电阻完全匹配；
2. common-mode voltage不能过大，保证 $Q_1, Q_2$ 仍在saturation region。

---

## 4️⃣ $R_D$ 失配下的common-mode gain与 CMRR

[Slide 7 — Common Mode Gain with Mismatched RD](https://app.notion.com)

Slide 7 — Common Mode Gain with Mismatched RD

<aside>
🔎

**这里的** $R_D$ **是什么？**

- $R_D$ = drain resistor（漏极电阻），也就是每个 transistor drain 端接到 $V_{DD}$ 的 load resistor。
- 它的作用是把 differential pair 里的 drain current 变化转换成 output voltage 变化：$v_o \approx -i_d R_D$。所以 $R_D$ 越大，同样的电流变化造成的 voltage swing / gain 越大。
- 为什么会有 $R_D$？因为这版 differential amplifier 用的是 **resistive load**：上方不是 current mirror，而是两个电阻 $R_{D1}, R_{D2}$ 作为负载。
- 在 **ideal matched case** 里，$R_{D1}$ 和 $R_{D2}$ 是一样的：$R_{D1}=R_{D2}=R_D$。这里的 $R_D$ 可以理解为「设计目标值 / nominal value」。
- 在 **mismatched case** 里，它们就**不一样**了。课件为了方便推导，把左边当成参考值：$R_{D1}=R_D$，右边写成 $R_{D2}=R_D+\Delta R_D$。
- 所以 $\Delta R_D$ 就是两边 load resistor 的差值：$\Delta R_D=R_{D2}-R_{D1}$。如果 $\Delta R_D=0$，两边完全匹配；如果 $\Delta R_D\neq 0$，两边输出对同一个 common-mode input 的放大程度就不同。
- 直观地说：common-mode input 本来应该让两边 output 一起升 / 一起降；但如果 $R_{D1}\neq R_{D2}$，两边 voltage drop 不一样，于是 $v_{o1}\neq v_{o2}$，最后就会出现非零 $v_{od}=v_{o2}-v_{o1}$，也就是 common-mode gain。
</aside>

设 $R_{D1}=R_D,\ R_{D2}=R_D+\Delta R_D$，则：

$$
v_{o1}\simeq-\frac{R_D}{2R_{SS}}v_{icm},\quad v_{o2}\simeq-\frac{R_D+\Delta R_D}{2R_{SS}}v_{icm}
$$

$$
A_{cm}\equiv\frac{v_{od}}{v_{icm}}=-\left(\frac{R_D}{2R_{SS}}\right)\left(\frac{\Delta R_D}{R_D}\right)
$$

$$
\text{CMRR}=\frac{|A_d|}{|A_{cm}|}=\frac{2g_m R_{SS}}{\Delta R_D / R_D}
$$

> 🐱 重点：CMRR 想要大，就要 **大** $R_{SS}$（好tail current source，往往用 cascode 实现）+ **好匹配**（layout 上的 common-centroid、dummy 等）。
> 
- ❓ 疑问记录：失配项到底是 $\frac{\Delta R_D}{R_D}$ 还是 $\frac{\Delta R_D}{\Delta R_D+R_D}$？
    
    是 $\dfrac{\Delta R_D}{R_D}$，课件没写错。关键是：$A_{cm}$ 的**本体**其实只含 $\Delta R_D$，「除以谁」只是选哪只电阻当 reference(参考) 的写法。
    
    **① 精确结果只跟** $\Delta R_D$ **有关**
    
    两边各是带 source degeneration 的 common-source：
    
    $$
    v_{o1}\simeq-\frac{R_{D1}}{2R_{SS}}v_{icm},\qquad v_{o2}\simeq-\frac{R_{D2}}{2R_{SS}}v_{icm}
    $$
    
    相减：
    
    $$
    A_{cm}=\frac{v_{od}}{v_{icm}}=-\frac{R_{D2}-R_{D1}}{2R_{SS}}=-\frac{\Delta R_D}{2R_{SS}}
    $$
    
    分母就是 $2R_{SS}$，**根本没有** $\Delta R_D+R_D$。
    
    **② 拆成「单端增益 × 相对失配」只是上下同乘** $R_D$
    
    $$
    -\frac{\Delta R_D}{2R_{SS}}=\underbrace{\left(-\frac{R_D}{2R_{SS}}\right)}_{\text{单端 CM 增益}}\times\underbrace{\frac{\Delta R_D}{R_D}}_{\text{相对失配}}
    $$
    
    因为约定取 nominal 值 $R_{D1}=R_D$ 当参考，所以相对失配写成 $\dfrac{\Delta R_D}{R_D}$。
    
    **③** $\frac{\Delta R_D}{\Delta R_D+R_D}$ **是怎么来的**
    
    如果改用 $R_{D2}=R_D+\Delta R_D$ 当参考往外提：
    
    $$
    -\frac{\Delta R_D}{2R_{SS}}=\left(-\frac{R_D+\Delta R_D}{2R_{SS}}\right)\times\frac{\Delta R_D}{R_D+\Delta R_D}
    $$
    
    也对，只是参考基准换成了「大的那只电阻」。而 $\Delta R_D\ll R_D$，到一阶 $\dfrac{\Delta R_D}{R_D+\Delta R_D}\approx\dfrac{\Delta R_D}{R_D}$，几乎相等。
    
    > 🐱 记忆点：$A_{cm}$ 本体永远是 $-\dfrac{\Delta R_D}{2R_{SS}}$；约定拿 nominal $R_D$ 当 reference → 相对失配就是 $\dfrac{\Delta R_D}{R_D}$。
    > 
- ❓ 疑问记录：为什么课件把 $A_{cm}$ 定义成 $v_{od}/v_{icm}$，和「$-R_D/2R_{SS}$」那个不一样？
    
    不矛盾——这其实是**两个不同的 common-mode 增益**，对应**两种输出**，读者把它们混在一起了。
    
    **① CM → CM（共模输入 → 共模 / 单端输出）**：第 3️⃣ 节（Slide 6）那个，每个输出端各自的增益：
    
    $$
    \frac{v_{o1}}{v_{icm}}=\frac{v_{o2}}{v_{icm}}=\frac{v_{oc}}{v_{icm}}\simeq-\frac{R_D}{2R_{SS}}
    $$
    
    描述「两个输出一起上下浮动」多少，**即使完全匹配也存在**。对话里我先给的 $-R_D/2R_{SS}$ 就是这一个。
    
    **② CM → DM（共模输入 → 差分输出）**：第 4️⃣ 节（Slide 7）定义的 $A_{cm}$，也是**进 CMRR 的那个**：
    
    $$
    A_{cm}\equiv\frac{v_{od}}{v_{icm}}=-\left(\frac{R_D}{2R_{SS}}\right)\left(\frac{\Delta R_D}{R_D}\right)
    $$
    
    它**在完全匹配时 = 0**（因为 $v_{od}=v_{o2}-v_{o1}=0$），只有失配（$\Delta R_D$ 或 $\Delta g_m$）才让它非零。
    
    **两者关系（一眼看穿）：**
    
    $$
    \underbrace{A_{cm}}_{\text{CM→DM}}=\underbrace{\left(-\frac{R_D}{2R_{SS}}\right)}_{\text{CM→单端}}\times\underbrace{\frac{\Delta R_D}{R_D}}_{\text{相对失配}}
    $$
    
    即「差分输出的共模增益 = 单端共模增益 × 相对失配」。
    
    **为什么 CMRR 用** $v_{od}/v_{icm}$ **而不是** $v_{oc}/v_{icm}$**？** 因为下一级差分放大器只读**差分输出** $v_{od}$；纯共模输出（两边一起动）会被后级**天然抑制**掉。真正污染信号的，是「共模被转换成差分」这条 CM→DM 漏道。所以衡量抑制能力的 CMRR 必须用 $A_d/A_{cm}^{(od)}$。
    
    > 🐱 记忆点：$-R_D/2R_{SS}$ 是「两边一起动多少」（matched 也有）；课件的 $A_{cm}=v_{od}/v_{icm}$ 是「失配漏成差分多少」（matched = 0）。CMRR 只关心后者。
    > 

---

## 5️⃣ $g_m$ 失配下的common-mode gain

[Slide 8 — Common Mode Gain with Mismatched gm](https://app.notion.com)

Slide 8 — Common Mode Gain with Mismatched gm

设 $g_{m1}=g_m+\tfrac{1}{2}\Delta g_m,\ g_{m2}=g_m-\tfrac{1}{2}\Delta g_m$，则 $g_{m1}-g_{m2}=\Delta g_m$，得：

$$
A_{cm}\simeq\left(\frac{R_D}{2R_{SS}}\right)\left(\frac{\Delta g_m}{g_m}\right),\quad \text{CMRR}=\frac{2g_m R_{SS}}{\Delta g_m/g_m}
$$

<aside>
🧮

**简要推导：为什么** $g_m$ **mismatch 会产生 common-mode gain？**

1. common-mode input 下，两边输入一样：$v_{i1}=v_{i2}=v_{icm}$。如果 $g_{m1}=g_{m2}$，两边 drain current 变化一样，两个 output 也一样，所以 $v_{od}=0$。
2. 但现在 $g_{m1}\neq g_{m2}$，两边 transconductance 不同，同一个 $v_{icm}$ 会产生不同的小信号 drain current：

$$
\Delta i_d \approx (g_{m1}-g_{m2})v_{gs,\text{eff}}
$$

1. common-mode half circuit 里 source degeneration 很强，effective gate-source voltage 大约被 $2g_mR_{SS}$ 压小：

$$
v_{gs,\text{eff}}\approx \frac{v_{icm}}{2g_mR_{SS}}
$$

1. 因此两边输出差为：

$$
																								v_{od}\approx R_D(g_{m1}-g_{m2})\frac{v_{icm}}{2g_mR_{SS}}
=R_D\Delta g_m\frac{v_{icm}}{2g_mR_{SS}}
$$

所以：

$$
A_{cm}=\frac{v_{od}}{v_{icm}}\approx \frac{R_D}{2R_{SS}}\frac{\Delta g_m}{g_m}
$$

再用 differential gain $A_d\approx g_mR_D$，得到：

$$
																								\text{CMRR}=\frac{|A_d|}{|A_{cm}|}\approx\frac{g_mR_D}{(R_D/2R_{SS})(\Delta g_m/g_m)}
=\frac{2g_mR_{SS}}{\Delta g_m/g_m}
$$

</aside>

注意两种失配的 CMRR 公式**形式完全一致**，只是失配源不同 → 实际电路中 $\Delta R_D/R_D$ 与 $\Delta g_m/g_m$ 会同时出现，可按 RMS 叠加估计。

- ❓ 疑问记录：两种 mismatch 里的 $A_d$ 是不是一样？是什么？
    
    **一样。** $R_D$ 失配和 $g_m$ 失配两条 CMRR 里用的 $A_d$ 都是这版 resistive-load 差分对的**标称差分增益**：
    
    $$
    A_d=\frac{v_{od}}{v_{id}}=g_m R_D
    $$
    
    **为什么是这个、而且两次都一样：**
    
    - 这是 **diff-in → diff-out 的满增益**，不是单端的 $\tfrac12 g_m R_D$。因为 CMRR 里 $A_{cm}\equiv v_{od}/v_{icm}$ 用的是**差分输出**，分子 $A_d$ 也必须用差分输出才一致。
    - **失配不进** $A_d$。$\Delta R_D$ / $\Delta g_m$ 都是一阶小扰动，只污染 $A_{cm}$ 那条 CM→DM 漏道；$A_d$ 取**匹配时的标称值** $g_m R_D$ 就够了（再算失配修正是二阶小量，可忽）。
    - 所以两条 CMRR 分子都凑成 $2g_m R_{SS}$：
    
    $$
    \text{CMRR}=\frac{|A_d|}{|A_{cm}|}=\frac{g_m R_D}{\left(\dfrac{R_D}{2R_{SS}}\right)\cdot(\text{相对失配})}=\frac{2g_m R_{SS}}{\text{相对失配}}
    $$
    
    里面的 $R_D$ 正好约掉，只是分母的「相对失配」从 $\Delta R_D/R_D$ 换成 $\Delta g_m/g_m$。
    
    > 🐱 记忆点：$A_d=g_m R_D$ 是「好东西」，两种失配都共用；失配只改变分母的 $A_{cm}$。⚠️ 只有换成 current-mirror load（第 7⃣ 节）时 $A_d$ 才变成 $g_m(r_{o2}\|r_{o4})$，不再是 $g_m R_D$。
    > 
- ❓ 疑问记录：Slide 8 在考虑 $r_o$ 有限时是怎么样的？
    
    Slide 8 默认 $r_o\to\infty$（输入管是理想跨导电流源）。把有限 $r_o$ 加进去后，CM→DM 通道会多出一个**即使** $R_{SS}=\infty$ **也消不掉**的本征项。
    
    **Step 1：用共源节点** $v_s$ **建模（不再硬拆** $2R_{SS}$**）**
    
    因为 $g_{m1}\neq g_{m2}$，两边电流并不严格相等，half-circuit trick 只是近似。直接写两支管：
    
    $$
    i_{d1}=\dfrac{g_{m1}(v_{icm}-v_s)-v_s/r_o}{1+R_D/r_o},\quad i_{d2}=\dfrac{g_{m2}(v_{icm}-v_s)-v_s/r_o}{1+R_D/r_o}
    $$
    
    **Step 2：源节点 KCL**$\;i_{d1}+i_{d2}=v_s/R_{SS}$，记 $G=g_{m1}+g_{m2}=2g_m$：
    
    $$
    v_s=\dfrac{G\,v_{icm}}{G+\dfrac{2}{r_o}+\dfrac{1+R_D/r_o}{R_{SS}}}
    $$
    
    **Step 3：差分输出**$\;v_{od}=-R_D(i_{d2}-i_{d1})$：
    
    $$
    v_{od}=\dfrac{R_D\,\Delta g_m\,(v_{icm}-v_s)}{1+R_D/r_o}
    $$
    
    在 $2g_m\gg 2/r_o,\ (1+R_D/r_o)/R_{SS}$ 时化简：
    
    $$
    v_{icm}-v_s\simeq\dfrac{v_{icm}}{2g_m}\!\left[\dfrac{2}{r_o}+\dfrac{1+R_D/r_o}{R_{SS}}\right]
    $$
    
    **最终公式：**
    
    $$
    A_{cm}\simeq\dfrac{\Delta g_m}{g_m}\!\left[\dfrac{R_D}{r_o+R_D}+\dfrac{R_D}{2R_{SS}}\right]
    $$
    
    **两项物理含义对照：**
    
    - $\dfrac{R_D}{2R_{SS}}\cdot\dfrac{\Delta g_m}{g_m}$：tail 阻抗有限引起的 CM→DM（Slide 8 原项，$r_o\to\infty$ 时唯一保留）。
    - $\dfrac{R_D}{r_o+R_D}\cdot\dfrac{\Delta g_m}{g_m}$：**新增项**，输入管 $r_o$ 有限造成的本征 CM→DM；即使 $R_{SS}=\infty$ 也不会消失。
    
    **对应 CMRR**（用 $|A_d|=g_m(R_D\|r_o)$）：
    
    $$
    \text{CMRR}=\dfrac{g_m\cdot g_mr_o}{\Delta g_m}\cdot\dfrac{1}{1+\dfrac{r_o+R_D}{2R_{SS}}}
    $$
    
    - $R_{SS}\to\infty$：$\text{CMRR}\to g_m r_o/(\Delta g_m/g_m)$，被**本征增益** $g_m r_o$ **卡死**。
    - $r_o\to\infty$：回到 Slide 8 的 $\text{CMRR}=2g_mR_{SS}/(\Delta g_m/g_m)$。
    
    **记忆点：** Slide 8 是「$R_{SS}$-limited」极限；真实 CMRR 上限其实是 $\min(2g_mR_{SS},\ g_mr_o)/(\Delta g_m/g_m)$。这也解释了为什么用 cascode tail 把 $R_{SS}$ 抬到很高之后，CMRR 反而被输入管 $r_o$ 顶住——后续要继续涨，得换 cascode 输入对或 chopper / auto-zero 这类技巧。
    

---

## 6️⃣ DC Offset

[Slide 9 — DC Offset (concept)](https://app.notion.com)

Slide 9 — DC Offset (concept)

- 即使 $v_{id}=0$（两输入接地），由于 $R_D$ 失配，输出 $V_O\neq 0$。
- 为了让输出归零，需要在输入端施加 **input offset voltage** $V_{OS}=V_O/A_d$。

[Slide 10 — DC Offset (derivation)](https://app.notion.com)

Slide 10 — DC Offset (derivation)

设 $R_{D1}=R_D+\tfrac{\Delta R_D}{2},\ R_{D2}=R_D-\tfrac{\Delta R_D}{2}$：

$$
V_O=V_{D2}-V_{D1}=\left(\frac{I}{2}\right)\Delta R_D
$$

再用 $g_m=\dfrac{2I_D}{V_{OV}}=\dfrac{I}{V_{OV}}$ 与 $A_d=g_m R_D$，得：

$$
V_{OS}=\left(\frac{V_{OV}}{2}\right)\left(\frac{\Delta R_D}{R_D}\right)
$$

> 🐱 直观理解：$V_{OV}$ 越小（亚阈值偏置）失调越小，但牺牲带宽；这是 low-offset OpAmp 设计的一个经典 tradeoff。
> 

<aside>
🧠

**为什么 DC offset 的结果是这个形式？**

- 先看物理原因：当 $v_{id}=0$ 时，理想差分对两边电流各为 $I/2$。如果 $R_{D1}=R_D+\Delta R_D/2,\ R_{D2}=R_D-\Delta R_D/2$，两边电阻压降不同，所以输出 DC 电平不同。
- 因为 $V_D=V_{DD}-I_D R_D$，所以：

$$
																						V_O=V_{D2}-V_{D1}
=\left(V_{DD}-\frac{I}{2}R_{D2}\right)-\left(V_{DD}-\frac{I}{2}R_{D1}\right)
=\frac{I}{2}(R_{D1}-R_{D2})
=\frac{I}{2}\Delta R_D
$$

- **input offset voltage** $V_{OS}$ 的定义是：把内部失配造成的输出误差，折算成输入端需要多大的等效 differential voltage。也就是：

$$
V_{OS}=\frac{V_O}{A_d}
$$

- 对 resistive-load differential pair，$A_d\simeq g_mR_D$。又因为每支管 $I_D=I/2$，所以：

$$
g_m=\frac{2I_D}{V_{OV}}=\frac{I}{V_{OV}}
$$

代入可得：

$$
																						V_{OS}
=\frac{(I/2)\Delta R_D}{(I/V_{OV})R_D}
=\left(\frac{V_{OV}}{2}\right)\left(\frac{\Delta R_D}{R_D}\right)
$$

所以这个结论的结构很合理：offset 与相对失配 $\Delta R_D/R_D$ 成正比，也与输入管的过驱电压 $V_{OV}$ 成正比。

</aside>

---

## 7️⃣ differential input、single-ended output（从 diff pair 到 OpAmp）

### (a) 多级级联（概念）

三级 differential-in / differential-out 串接，最后一级取单端 → 实现 differential → single-ended。

### (b) 单级实现：MOS differential pair + current-mirror load ⭐

[Slide 12 — MOS Differential Pair with Current Mirror Load](https://app.notion.com)

Slide 12 — MOS Differential Pair with Current Mirror Load

核心结论：

$$
A_d\equiv\frac{v_o}{v_{id}}=G_m R_o=g_{m1,2}(r_{o2}\|r_{o4})
$$

<aside>
🧮

**这个** $A_d$ **公式怎么推导？**

1. 对 differential input：

$$
v_{g1}=+\frac{v_{id}}{2},\qquad v_{g2}=-\frac{v_{id}}{2}
$$

所以两支输入 NMOS 的小信号 drain current 变化为：

$$
\Delta i_{D1}=+g_m\frac{v_{id}}{2},\qquad \Delta i_{D2}=-g_m\frac{v_{id}}{2}
$$

1. 如果只看右边单端输出，$Q_2$ 少拉了 $\frac{1}{2}g_mv_{id}$ 的电流，等效为输出节点多出一份向上的小信号电流：

$$
+\frac{1}{2}g_mv_{id}
$$

1. 左边 $Q_1$ 多拉的电流会流过 diode-connected PMOS $Q_3$，再由 current mirror 复制到 $Q_4$。因此 $Q_4$ 也会在右边输出节点多提供一份：

$$
+\frac{1}{2}g_mv_{id}
$$

1. 两部分在输出节点相加：

$$
i_o=\frac{1}{2}g_mv_{id}+\frac{1}{2}g_mv_{id}=g_mv_{id}
$$

这就是 current-mirror load 的关键：左半边的电流变化没有浪费，而是被镜像到右边，所以 single-ended gain **不打半折**。

1. 输出节点看到的电阻是：

$$
R_o=r_{o2}\|r_{o4}
$$

因此：

$$
v_o=i_oR_o=g_mv_{id}(r_{o2}\|r_{o4})
$$

最后得到：

$$
A_d=\frac{v_o}{v_{id}}=g_m(r_{o2}\|r_{o4})
$$

</aside>

注意：尽管输出是**单端**的，differential gain却**没有打折**（不是 $\tfrac{1}{2}g_m R$），因为current mirror把左侧的 $-i$ 翻转后送到右侧节点，与右侧的 $+i$ 相加，电流被「**镜像复用**」。

### (c) 单管等效模型 ⭐（该图中的等效对吗？）

<aside>
✅

**结论：对的！** 把「differential pair + current-mirror load」整块压缩成**一个 common-source 单管**，正是教科书的标准等效。只要记住这组对应关系就完全正确：

- $v_{in}=v_{id}$（差分输入），输出 $v_O$ 单端 —— 这是一个**反相**放大级。
- 等效单管跨导 = 输入对管的 $g_m=g_{m1,2}$（**满** $g_m$**，没打半折**，靠 current mirror 把左半电流镜过来）。
- 等效单管**自身**输出电阻 = $r_{o2}$（来自 $Q_2$）。
- 该图中的 $R_O$ = **镜像负载的输出电阻** $r_{o4}$（来自 $Q_4$）—— 它不是真电阻，而是 active load 的小信号 $r_o$。
- 所以输出节点总电阻 = $R_O\|r_{o2}=r_{o2}\|r_{o4}$，增益：

$$
A_d=-g_m(r_{o2}\|r_{o4}),\qquad\text{接外部负载时 } A_d=-g_m(r_{o2}\|r_{o4}\|R_L)
$$

</aside>

```
                    V_DD
                     │
                   ┌─┴─┐
                   │R_O│     R_O ≡ r_o4  (Q4 镜像负载的输出电阻)
                   └─┬─┘
                     │
        v_O ●────────●─────────────┄┄┄┄┐ ← 虚线: 外接负载
                     │ (drain)          │
                gate │                ┌─┴─┐
  v_in ●──────────┤▏ M                │R_L│
                     │ (source)       └─┬─┘
                     │  g_m = g_m1,2     │
                     │  自身 r_o = r_o2  ═╧═ GND
                     │
                    -V_SS

A_d = -g_m·(R_O ‖ r_o) = -g_m·(r_o2 ‖ r_o4)      (接负载再 ‖ R_L)
```

- ❓ 疑问记录：这个单管等效图有哪些坑要注意？
    1. $R_O$ **是** $r_{o4}$ **的化身**：别把它当成「在 $r_o$ 之外又串了一只真电阻」。等效单管自身的 $r_o=r_{o2}$ 仍要并进去，输出节点总电阻是 $r_{o2}\|r_{o4}$，不是只看 $R_O$。
    2. **这是 small-signal（AC）等效**：小信号里 $V_{DD}$ 和 $-V_{SS}$ 都是 AC ground，所以图上「上接 $V_{DD}$、下到 $-V_{SS}$」在交流分析时其实都回到地，$R_O$ 的上端等效接 AC ground。
    3. **满** $g_m$ **的来历**：右边 $Q_2$ 出 $\tfrac12 g_m v_{id}$，左边 $Q_1$ 经 mirror 再补 $\tfrac12 g_m v_{id}$，两份相加才凑成完整 $g_m$ —— 这正是单管 CS 等效「跨导不打折」的根据。
    4. **只是一阶低频等效**：这张图只刻画 DC / 低频增益，没含频率响应、零极点（那是 Stage 2 + Miller $C_C$ 要单独分析的东西）。
- ❓ 疑问记录：Slide 12 小信号模型里，$R_L\to\infty$（输出开路）为什么 $v_o$ 节点还会有电流？
    
    这个问题戳到 small-signal model 最容易混的一点：**「**$R_L$ **无穷大」和「节点没有电流」是两回事**。关键是分清**两种完全不同的电阻**，以及 transconductance 到底是「电流源」还是「被电压驱动的电阻」。
    
    **① 两种电阻别混：外部** $R_L$ **≠ 器件自身** $r_o$
    
    - $R_L$ = 你**外接**在输出端的负载（下一级输入、测量仪器…）。Slide 12 算的是 **open-circuit / intrinsic gain**，所以默认 $R_L\to\infty$（输出开路，不接任何外部负载）。
    - $r_{o2}, r_{o4}$ = $Q_2$、$Q_4$ **自身**的输出电阻（来自 channel-length modulation / Early effect），是**有限值**，不会因为 $R_L=\infty$ 而消失。
    - 所以输出节点对地的总阻抗不是 $\infty$，而是 $R_o=r_{o2}\|r_{o4}$（有限）。
    
    **②** $g_m v_{gs}$ **是受控电流源，不是「被** $v_o$ **驱动的电阻」**
    
    小信号模型里每个管子拆成「受控电流源 $g_m v_{gs}$ ∥ $r_o$」。电流源的特点是：**只要** $v_{gs}$ **在，它就硬往外灌这么多电流，跟输出端接什么、**$R_L$ **多大毫无关系。**
    
    - 对**电阻**：$i=v/R$，$R=\infty$ 时 $i=0$ —— 常见直觉可能是。
    - 对**电流源**：$i$ 由 $v_{gs}$ 定，$R_L$ 再大也照灌；这股电流必须找地方流回去，走的就是有限的 $r_{o2}\|r_{o4}$。
    
    **③ 这股电流哪来的、流去哪**
    
    differential input 下 $v_{g1}=+\tfrac{v_{id}}{2},\ v_{g2}=-\tfrac{v_{id}}{2}$：
    
    - $Q_2$ 直接往输出节点灌 $+\tfrac12 g_m v_{id}$；
    - $Q_1$ 的 $-\tfrac12 g_m v_{id}$ 经 $Q_3\to Q_4$ 镜像，又补一份 $+\tfrac12 g_m v_{id}$；
    - 合计**短路输出电流** $i_{o,\text{sc}}=g_m v_{id}$。
    
    即使 $R_L=\infty$，这 $g_m v_{id}$ 也不归零，它**全部流进** $r_{o2}\|r_{o4}$，于是：
    
    $$
    v_o=i_o\,(r_{o2}\|r_{o4})=g_m v_{id}(r_{o2}\|r_{o4})
    $$
    
    **④ 用 Norton 一句话收束**
    
    $g_m v_{id}$ 就是 **Norton 短路电流**，$R_o=r_{o2}\|r_{o4}$ 就是 **Norton 输出电阻**。输出开路（$R_L=\infty$）时，短路电流全压在 $R_o$ 上 → 得到**开路电压增益** $A_d=g_m(r_{o2}\|r_{o4})$。若外接有限 $R_L$，它就并进来：$v_o=g_m v_{id}(r_{o2}\|r_{o4}\|R_L)$，增益反而更小。
    
    > 🐱 记忆点：**「**$R_L=\infty$**」只是说没接外部负载，不代表节点零电流。** 电流由 $g_m v_{gs}$ 这个电流源决定，它永远从有限的 $r_{o2}\|r_{o4}$ 流回去 —— 这正是 intrinsic gain $g_m r_o$ 的来源。要是器件真的理想（$r_o\to\infty$）且 $R_L\to\infty$，$v_o$ 才会发散（增益→∞），现实里被 $r_o$ 卡住。
    > 
- ❓ 疑问记录：这个 $i$ 流过 $R_L$ 后 $v_o$ 不是 +∞ 吗？还是它流过等效的 $R_o$（求等效图）？
    
    好问题——这两个选项里**第二个才对**：$i$ **不**流过 $R_L$，它流过等效输出电阻 $R_o=r_{o2}\|r_{o4}$（也就是这版电路里「$R_D$ 的替身」）。下面把为什么讲清楚。
    
    **① 输出节点其实是一个 current divider**
    
    输出节点同时挂着两条到 AC ground 的**并联**路径：
    
    - 内部：$R_o=r_{o2}\|r_{o4}$（器件自身输出电阻，有限）；
    - 外部：$R_L$（你接的负载）。
    
    电流源 $i=g_m v_{id}$ 注入后按**并联分流**：
    
    $$
    v_o=i\,(R_o\|R_L),\qquad i_{R_L}=\frac{v_o}{R_L},\quad i_{R_o}=\frac{v_o}{R_o}
    $$
    
    **② 为什么** $R_L=\infty$ **时** $v_o$ **不会 +∞**
    
    $R_L=\infty$ = **开路**。开路支路上 $i_{R_L}=v_o/\infty=0$ —— **一点电流都流不过去**。所以全部 $i$ 改走有限的 $R_o$：
    
    $$
    v_o=i\,(R_o\|\infty)=i\,R_o=g_m v_{id}(r_{o2}\|r_{o4})\quad(\text{有限!})
    $$
    
    你担心的「$v_o=iR_L\to\infty$」只有在**强行让全部电流挤过那条无穷大支路**时才成立；但现实里永远有个有限的 $r_o$ 并在旁边兜着，电流会**优先走低阻的** $R_o$，不会去挤开路。
    
    **③ 「等效** $R_D$**」就是** $R_o$**（这个直觉对了）**
    
    - resistive-load 差分对：电流真的流过物理电阻 $R_D$ → $v_o=iR_D$。
    - current-mirror-load 差分对：没有物理 $R_D$，它的角色由 $r_{o2}\|r_{o4}$ 顶替 → $v_o=i(r_{o2}\|r_{o4})$。
    
    所以「$i$ 流过等效后的 $R_D$」这句话完全正确，只要把「等效 $R_D$」理解成 $R_o=r_{o2}\|r_{o4}$。
    
    **④ 等效图（Norton current divider）**
    
    ```
              ┌────────────┬────────────┐ ── v_o
              │             │             │
            ↑                ┌─┴─┐         ┌─┴─┐
       i = g_m·v_id           │   │ R_o     │   │ R_L = ∞
     (受控电流源)             │   │ =r_o2‖r_o4│   │ (开路)
              │                └─┬─┘         └─┬─┘
              │             │             │
              └────────────┴────────────┘ ── GND (AC)
    
    分流:  i_Ro = i (全部电流)        i_RL = v_o / ∞ = 0
    ⇒  v_o = i·(R_o ‖ R_L) = i·R_o = g_m·v_id·(r_o2‖r_o4)   ← 有限
    ```
    
    > 🐱 记忆点：电流是「懒人」，永远挑**电阻最小**的路走。$R_L=\infty$ 是断头路（0 电流），$R_o=r_{o2}\|r_{o4}$ 才是真正承载 $i$ 的电阻。只有 $r_o\to\infty$ 同时 $R_L\to\infty$（两条路都断），节点才没了泄流通道，$v_o$ 才会数学上发散。
    > 
- ❓ 疑问记录（KCL/KVL）：$v_o$ 是不是流出 $2i$ 经 $R_L$ 到地，正好让顶上 AC ground 流出的 $2i$ 闭环？
    
    对！只要输出端**有一条到地的通路**（这里就是 $R_L$），这套 KCL/KVL 完全成立。下面列出每条支路电流和回路数（记每管信号电流 $i=\tfrac12 g_m v_{id}$，输出总电流 $=2i=g_m v_{id}$；先按理想管 $r_o\to\infty$ 看清走向）。
    
    **① 输入支路（差分激励，**$v_s=0$ **是 virtual ground）**
    
    - $Q_1$：$i_{d1}=+i$ —— 多拉 $i$，电流从 diode 节点经 $Q_1$ 下到 tail；
    - $Q_2$：$i_{d2}=-i$ —— 少拉 $i$，等效为电流从 tail 经 $Q_2$ **向上**灌进输出节点 $+i$。
    
    **② tail 节点 KCL（理想尾流源 = AC 开路）**
    
    $$
    i_{d1}+i_{d2}=(+i)+(-i)=0
    $$
    
    尾流源 AC 电流为 0 ✓。物理上：$i$ 从 $Q_1$ 下来，又立刻从 $Q_2$ 上去，tail 只是个「掉头点」。
    
    **③ 镜像支路**
    
    $Q_1$ 多拉的 $i$ 全部由 $V_{DD}$ 经 diode-connected $Q_3$ 供给 → mirror 复制给 $Q_4$ → $Q_4$ 从 $V_{DD}$ 往输出节点**下灌** $+i$。
    
    **④ 输出节点 KCL**
    
    进节点：$Q_4$ 给 $+i$、$Q_2$ 给 $+i$ → 合计 $2i$。这 $2i$ 必须出节点，唯一出口是 $R_L$：
    
    $$
    i_{R_L}=2i=g_m v_{id},\qquad v_o=2i\,R_L=g_m v_{id}R_L
    $$
    
    **⑤ 顶上 AC ground（**$V_{DD}$**）总账**
    
    $V_{DD}$ 一共吐出 $2i$：$i$ 走 $Q_3$，$i$ 走 $Q_4$。两路最后都汇到输出节点，再一起经 $R_L$ 回到地 —— 所以 $V_{DD}$ 吐的 $2i$ 完整经 $R_L$ 回流，**每个节点 KCL、每个回路 KVL 全闭环 ✓**。
    
    **电流回路图（理想** $r_o\to\infty$**，只留** $R_L$ **通路）：**
    
    ```mermaid
    flowchart TB
        VDD["V_DD<br>顶部 AC ground<br>共提供 i + i = 2i"]
    
        subgraph LOOP_A["回路 A：镜像输入支路（电流 i）"]
            Q3["Q3<br>diode-connected PMOS"]
            Q1["Q1<br>多拉 i"]
            TAIL["tail node<br>理想尾源 = AC open<br>净小信号电流 i - i = 0"]
            Q2["Q2<br>少拉 i<br>等效向 v_o 上灌 i"]
        end
    
        subgraph LOOP_B["回路 B：镜像输出支路（电流 i）"]
            Q4["Q4<br>current-mirror 输出管<br>向 v_o 下灌 i"]
        end
    
        VO(("v_o<br>输出节点<br>KCL：i + i = 2i"))
        RL["R_L<br>回地通路"]
        GND["GND"]
    
        VDD -->|"i"| Q3 -->|"i"| Q1 -->|"i"| TAIL -->|"i"| Q2 -->|"i"| VO
        VDD -->|"i"| Q4 -->|"i"| VO
        VO -->|"2i = g_m v_id"| RL --> GND
    
        classDef key fill:#fff4d6,stroke:#f2a900,stroke-width:1.4px,color:#111;
        classDef device fill:#eef6ff,stroke:#2f80ed,stroke-width:1.2px,color:#111;
        class VDD,VO key;
        class Q1,Q2,Q3,Q4,TAIL,RL,GND device;
    ```
    
    **读图要点：**
    
    - **回路 A**：$V_{DD}\to Q_3\to Q_1\to tail\to Q_2\to v_o\to R_L\to GND$，左支路多拉的 $i$ 通过 tail / $Q_2$ 等效送到输出节点。
    - **回路 B**：$V_{DD}\to Q_4\to v_o\to R_L\to GND$，current mirror 把左边的电流变化复制到右边，再向输出节点贡献一份 $i$。
    - **输出节点 KCL**：两份电流在 $v_o$ 汇合，所以 $i_o=2i=g_m v_{id}$，最后经 $R_L$ 回到地。
    
    **⚠️ 关键前提：这套闭环需要一条「回地通路」。** 如果 $R_L=\infty$ **且**把 $r_o$ 也理想化（$r_o\to\infty$），那 $2i$ 就**无处可去** → KCL 直接冲突 → $v_o\to\infty$。现实里正是有限的 $r_{o2}\|r_{o4}$ 顶上这条回地通路，所以 $R_L=\infty$ 时电流改走 $r_o$，$v_o$ 仍有限。换句话说：**你这张「**$2i$ **经负载回地」的图永远对，只是「负载」可以是外部** $R_L$**、也可以是器件自己的** $r_o$ **—— 总得有一条。**
    
- ❓ 疑问记录（归谬法）：$R_L=\infty$、$i$ 有限 → $v_o=2iR_L$ 不就 =∞ 了？那「只走 $R_L$」不就错了？
    
    这一步推得太漂亮了 —— 你做的其实是一个**反证法（归谬）**，而且结论完全正确：
    
    **① 该推理（假设「电流只能走** $R_L$**」）**
    
    代入 $R_L\to\infty$、$i$ 有限：
    
    $$
    v_o=2i\cdot R_L=2i\cdot\infty=\infty
    $$
    
    真实运放输出不可能无穷大 → 出现矛盾 → **「只走** $R_L$**」这个假设在** $R_L=\infty$ **时就是错的**。
    
    **② 错在哪**
    
    「电流只走 $R_L$」这张图把管子当成了**理想电流源**（$r_o=\infty$）。这个理想化**只在** $R_L$ **有限时**才站得住；一旦 $R_L\to\infty$，它自相矛盾，恰好在提醒你：$r_o$ **不能扔。**
    
    **③ 真相：到地永远有两条路**
    
    管子永远带着有限的 $r_o$，并在输出节点旁边。所以输出节点到地其实是 $R_L$ 和 $r_{o2}\|r_{o4}$ **并联**：
    
    $$
    v_o=2i\,(R_L\|r_{o2}\|r_{o4})
    $$
    
    **④ 三种情形对号入座**
    
    - $R_L$ 有限且 $R_L\ll r_o$：电流几乎全走 $R_L$ → $v_o\approx 2iR_L$，**你那句话近似成立**。
    - $R_L=\infty$（开路）：$R_L$ 这条断了，电流全走 $r_o$ → $v_o=2i(r_{o2}\|r_{o4})$，**有限**！这正是开路增益 $A_d=g_m(r_{o2}\|r_{o4})$。
    - 一般情况：两条路按电阻分流。
    
    > 🐱 一句话：你那句「只走 $R_L$」**不是错，是有条件的** —— 条件是 $R_L$ 有限。$R_L\to\infty$ 把条件捣破了，$v_o\to\infty$ 这个荒谬结果，就是电路在喊「快把 $r_o$ 请回来」。
    > 
- ❓ 疑问记录：为什么 $A_d=G_m R_o$？这是怎么等效出来的？
    
    这个问题很关键 —— $A_d=G_m R_o$ 不是为这一个电路单独发明的公式，而是**任何 single-ended amplifier 的通用 Norton 等效**：从输入端 $v_{id}$ 看进去，整块电路被缩成一个「**受控电流源 ‖ 输出电阻**」的小盒子。
    
    **两步等效（套路化）：**
    
    1. **找** $G_m$**（"总跨导"）：** 把输出端短路接地，看输入 $v_{id}$ 在输出节点产生多少小信号电流 $i_{o,\text{sc}}$。
        - $Q_2$ 漏端直接灌出 $+\tfrac{1}{2}g_m v_{id}$；
        - $Q_1$ 漏端少灌的 $-\tfrac{1}{2}g_m v_{id}$ 经 $Q_3\to Q_4$ 镜像，在输出节点再补一份 $+\tfrac{1}{2}g_m v_{id}$；
        - 两份相加 → $i_{o,\text{sc}}=g_m v_{id}$，所以 $G_m=g_m$（即输入对管的 $g_{m1,2}$）。
    2. **找** $R_o$**（输出节点对地电阻）：** 把输入接地，在输出端加测试源 $v_x$。
        - 往上看到 $Q_4$ 的 $r_{o4}$；
        - 往下经 $Q_2$（公共 source 跟随之后）看到 $r_{o2}$；
        - 二者并联 → $R_o=r_{o2}\|r_{o4}$（这就是 Slide 13 的推导）。
    
    把两步合起来就是标准的 Norton → 输出电压：
    
    $$
    v_o=i_{o,\text{sc}}\cdot R_o=G_m v_{id}\cdot R_o\;\Rightarrow\;A_d=\frac{v_o}{v_{id}}=G_m R_o=g_m(r_{o2}\|r_{o4})
    $$
    
    **等效抽象（原电路 → Norton 盒子）：**
    
    ```mermaid
    flowchart LR
        subgraph orig ["原电路（看起来很复杂）"]
            direction LR
            IN1["v_id<br>(差分输入)"] --> BOX["Q1, Q2 差分对<br>+ Q3, Q4 电流镜负载"]
            BOX --> OUT1["v_o<br>(单端输出)"]
        end
        subgraph eq ["Norton 单端等效（其实就这么简单）"]
            direction LR
            IN2["v_id"] --> SRC["受控电流源<br>i = G_m · v_id<br>G_m = g_m (输入对)"]
            SRC --> NODE(("输出节点"))
            RO["R_o = r_o2 ‖ r_o4"] --- NODE
            NODE --> OUT2["v_o = G_m·R_o·v_id"]
        end
        orig ==等效化简==> eq
    ```
    
    **真实电路里的「电流源 ‖ 电阻」长这样：**
    
    ```jsx
                ●──────────────● ── v_o
                │              │
                │              │
    (输入)    (↑) G_m·v_id   ▢ R_o = r_o2 ‖ r_o4
     v_id       │              │
                │              │
                ●──────────────● ── GND (AC)
    ```
    
    **口诀：** *差分对 + current mirror = 一个把* $v_{id}$ *变成* $g_m v_{id}$ *的电流源，挂在* $r_{o2}\|r_{o4}$ *这个阻抗上 →* $v_o$ *= 电流 × 阻抗 =* $g_m(r_{o2}\|r_{o4})\cdot v_{id}$*。所有 single-ended 输出的放大器，最后都能压缩成这个* $G_m R_o$ *形式，cascode、folded cascode、telescopic 都一样，只是* $G_m$ *和* $R_o$ *算法不同。*
    
- ❓ 疑问记录：Slide 12 上方接地节点是不是不符合 KCL？
    
    不违反 KCL。Slide 12 里上方画成 ground 的点，本质上是 $V_{DD}$ 在 **small-signal model** 里的 **AC ground**，不是一个只能被动“收电流”的普通地线。理想电压源 / 去耦后的电源轨可以提供或吸收小信号电流，只要它的电压保持为 0 小信号即可。
    
    更完整地说：
    
    1. **DC 电路里**：$V_{DD}$ 通过 PMOS current mirror（$Q_3,Q_4$）向下提供电流，电流再经 NMOS differential pair 和 tail current source 回到 $V_{SS}$，最后通过电源闭合回路返回。并不是“地凭空输出电流”。
    2. **小信号电路里**：独立 DC 电源的电压增量为 0，所以 $V_{DD}$ 被替换成 AC ground。但这个 ground 后面隐含着一个理想电压源；KCL 里还要算上“电源支路”流入 / 流出的电流。
    3. $Q_3,Q_4$ 电流都画向下，只是表示 PMOS load 从电源轨向电路内部提供电流。对上方 AC-ground 节点写 KCL 时，电源支路提供的电流正好等于流入两个 PMOS source 的电流之和。
    
    **记忆点：** AC ground 不是“电流黑洞 / 白洞”，而是“电压固定为 0 的理想源节点”。KCL 永远成立，只是电源支路常常被图省略了。
    

### output resistance $R_o$ 的推导

[Slide 13 — Output Resistance](https://app.notion.com)

Slide 13 — Output Resistance

用 $v_x, i_x$ test 源在输出端：

$$
i_x=2i+\frac{v_x}{r_{o4}}=2\frac{v_x}{2r_{o2}}+\frac{v_x}{r_{o4}}=v_x\!\left(\frac{1}{r_{o2}}+\frac{1}{r_{o4}}\right)
$$

$$
\Rightarrow R_o=r_{o2}\|r_{o4}
$$

- ❓ 疑问记录：为什么 $Q_4$ 上面写 $i+\frac{v_x}{r_{o4}}$，而 $Q_2$ 下面只写 $i$？
    
    这里不能把输出节点简单看成上下两个完全对称的 resistor。$Q_4$ 是 current mirror 的输出管，所以它对输出节点有两部分小信号电流：
    
    1. 由左边 $Q_1$ 经 $Q_3,Q_4$ mirror 复制过来的电流 $i$；
    2. $Q_4$ 自己有限输出电阻 $r_{o4}$ 上的电流 $\frac{v_x}{r_{o4}}$。
    
    因此上方贡献写成：
    
    $$
    i+\frac{v_x}{r_{o4}}
    $$
    
    而下面 $Q_2$ 只写 $i$，是因为这个 $i$ 已经是考虑 $Q_2$ 的 $r_{o2}$、公共 source node 跟随之后得到的净小信号电流；如果再额外加 $\frac{v_x}{r_{o2}}$，就会 double count。
    
    在 output-resistance test 中，输入 gate 置零，输出端加 $v_x$，source node 会随动，所以右支路得到：
    
    $$
    i=\frac{v_x}{2r_{o2}}
    $$
    
    - 这里的 $i$ **不是**平时求跨导电流时的 $g_m v_{ov}$。因为现在是在求 **output resistance**，输入端被置零，所以：
    
    $$
    v_{g1}=v_{g2}=0
    $$
    
    - 更准确地说，公共 source node 会轻微上升，但通常不是 $v_x/2$；它上升的作用是让 $g_m v_{gs}$ 电流抵消掉一部分由 $r_{o2}$ 引起的电流。
    - 设 $Q_1,Q_2$ 匹配，$g_m=g_{m1}=g_{m2}$，$r_o=r_{o1}=r_{o2}$。右边 drain 加测试电压 $v_x$，左边 drain 小信号近似固定在 AC ground，两个 gate 也为 AC ground。于是：
    
    $$
    i_{d2}=g_m(0-v_s)+\frac{v_x-v_s}{r_o}
    $$
    
    $$
    i_{d1}=g_m(0-v_s)+\frac{0-v_s}{r_o}
    $$
    
    - 这里的 $i_d$ 指 **某一支输入 NMOS 的 drain 小信号电流**，也就是从 drain 流向 source 的小信号电流增量；不是尾电流 $I$，也不是输出端测试源总电流 $i_x$。
    - 具体地说，$i_{d2}$ 是右边 $Q_2$ 的 drain 小信号电流；$i_{d1}$ 是左边 $Q_1$ 的 drain 小信号电流。它们都由两部分组成：
    
    $$
    i_d=g_m v_{gs}+\frac{v_d-v_s}{r_o}
    $$
    
    - 后面课件写的 $i$，就是右支路这个净 drain 小信号电流 $i_{d2}$，在推导后等于：
    
    $$
    i=i_{d2}=\frac{v_x}{2r_{o2}}
    $$
    
    - 理想 tail current source 在小信号下等效为 open circuit，所以公共 source node 不能有净小信号电流流入 tail：
    
    $$
    i_{d1}+i_{d2}=0
    $$
    
    代入得：
    
    $$
    -2g_mv_s+\frac{v_x-2v_s}{r_o}=0
    $$
    
    因此：
    
    $$
    v_s=\frac{v_x}{2(1+g_mr_o)}\approx \frac{v_x}{2g_mr_o}
    $$
    
    - 右支路的净 drain current 为：
    
    $$
    i=i_{d2}=-g_mv_s+\frac{v_x-v_s}{r_o}
    $$
    
    代入后可精确得到：
    
    $$
    i=\frac{v_x}{2r_o}=\frac{v_x}{2r_{o2}}
    $$
    
    - 所以 $i=v_x/(2r_{o2})$ 不是因为 $r_{o2}$ 两端电压简单变成 $v_x/2$，而是因为 $r_{o2}$ 的电流和 source node 反馈产生的 $g_m v_{gs}$ 电流相互抵消后，右支路对外只剩下一半。
    - 也可以用课件里的阻抗变换公式理解。对 $Q_2$ 来说，从 drain 看进去的输出电阻为：
    
    $$
    R_{o2}=r_{o2}+R_{in1}+g_{m2}r_{o2}R_{in1}
    $$
    
    - 这个公式来自一个标准小信号推导：对一个 gate 接 AC ground、source 接电阻 $R_S$ 到地的 NMOS，在 drain 加测试电压 $v_x$，测试电流为 $i_x$。因为 source 端电压为 $v_s$，所以从 drain 进入晶体管的电流是：
    
    $$
    												i_x=\frac{v_x-v_s}{r_o}+g_m(0-v_s)
    =\frac{v_x-v_s}{r_o}-g_mv_s
    $$
    
    source 端只有 $R_S$ 到地，因此流过 $R_S$ 的电流也是 $i_x$：
    
    $$
    v_s=i_xR_S
    $$
    
    代回上式：
    
    $$
    i_x=\frac{v_x-i_xR_S}{r_o}-g_mi_xR_S
    $$
    
    整理：
    
    $$
    i_x\left(1+\frac{R_S}{r_o}+g_mR_S\right)=\frac{v_x}{r_o}
    $$
    
    所以：
    
    $$
    \frac{v_x}{i_x}=r_o+R_S+g_mr_oR_S
    $$
    
    这就是 source 端有电阻时，从 drain 看进去的等效输出电阻公式。
    
    - 在 Slide 13 这里，把 $R_S$ 换成 $R_{in1}$，就得到：
    
    $$
    R_{o2}=r_{o2}+R_{in1}+g_{m2}r_{o2}R_{in1}
    $$
    
    其中 $R_{in1}$ 是从公共 source node 往左边 $Q_1$ 支路看进去的等效小信号电阻。因为 $Q_1$ 的 gate 和 drain 在这个测试里近似为 AC ground，所以：
    
    $$
    R_{in1}\approx \frac{1}{g_{m1}}
    $$
    
    若 $Q_1,Q_2$ 匹配，$g_{m1}=g_{m2}=g_m$，且 $g_mr_o\gg1$，则：
    
    $$
    R_{o2}\approx r_o+\frac{1}{g_m}+g_mr_o\frac{1}{g_m}\approx 2r_o
    $$
    
    因此右边 $Q_2$ 支路电流就是：
    
    $$
    i=\frac{v_x}{R_{o2}}\approx\frac{v_x}{2r_{o2}}
    $$
    
    左支路变化再经 current mirror 复制到 $Q_4$，又贡献一份 $i$。所以：
    
    $$
    																				i_x=i+i+\frac{v_x}{r_{o4}}
    =2\frac{v_x}{2r_{o2}}+\frac{v_x}{r_{o4}}
    =\frac{v_x}{r_{o2}}+\frac{v_x}{r_{o4}}
    $$
    
    最后：
    
    $$
    R_o=\frac{v_x}{i_x}=r_{o2}\|r_{o4}
    $$
    
    **记忆点：** $Q_4$ 那边是「mirror current + 自身 $r_o$ 电流」；$Q_2$ 那边的 $i$ 已经包含 $r_{o2}$ 效应。两个 $i$ 加起来才等效成 $\frac{v_x}{r_{o2}}$。
    

---

## 8️⃣ 复习：common-gate amplifier的阻抗变换（cascode 的基础）

[Slide 14 — Common Gate: Load → Input](https://app.notion.com)

Slide 14 — Common Gate: Load → Input

**负载侧 → 输入侧**：负载阻抗 $R_L$ 被 **本征增益** $g_m r_o$ 除（「压缩」）后看到。

$$
R_{in}=\frac{r_o+R_L}{1+g_m r_o}\simeq\frac{1}{g_m}+\frac{R_L}{g_m r_o}
$$

[Slide 15 — Common Gate: Source → Output](https://app.notion.com)

Slide 15 — Common Gate: Source → Output

**源侧 → 输出侧**：源极电阻 $R_s$ 被 **本征增益** $g_m r_o$ 乘（「放大」）后看到。

$$
R_{out}=r_o+R_s+g_m r_o R_s=r_o+(1+g_m r_o)R_s\simeq(g_m r_o)R_s
$$

<aside>
🔑

**Cascode 的灵魂一句话：** 上头看下来阻抗变 $\times A_v$，下头看上去阻抗变 $\div A_v$。后面学 cascode current mirror / cascode amplifier 就是反复利用这条。

</aside>

---

## 9️⃣ 两级 CMOS 运放

### 整体结构

[Slide 16 — Two-Stage CMOS Op-Amp Circuit](https://app.notion.com)

Slide 16 — Two-Stage CMOS Op-Amp Circuit

### 分块解读

[Slide 17 — Stages Scrutinize](https://app.notion.com)

Slide 17 — Stages Scrutinize

三大功能块：

| 模块 | 元件 | 作用 |
| --- | --- | --- |
| **Current Mirrors** | $Q_8 \to Q_5,\ Q_5\to Q_7$ | 由 $I_{REF}$ 复制偏置电流给一级、二级 |
| **Stage 1: Diff Pair + Mirror Load** | $Q_1,Q_2$（输入对）+ $Q_3,Q_4$（current-mirror load） | differential input → single-ended output，提供主要增益 |
| **Stage 2: Common-Source** | $Q_6$（驱动）+ $Q_7$（电流源负载） | 进一步放大并提供output swing |
| **Miller 补偿** | $C_C$ 跨在 $Q_6$ 的栅-漏（D2–D6）之间 | 频率补偿，保证闭环稳定 |

### 增益公式

[Slide 18 — Voltage Gains](https://app.notion.com)

Slide 18 — Voltage Gains

$$
A_1=-g_{m1}(r_{o2}\|r_{o4}),\quad A_2=-g_{m6}(r_{o6}\|r_{o7})
$$

$$
A_o=A_1 A_2
$$

两级都是 negative gain，所以 total gain 为 positive，这是经典 Sedra/Smith 两级 OpAmp 拓扑。

---

## 🔟 例题：完整数值计算

[Slide 19 — Example Problem](https://app.notion.com)

Slide 19 — Example Problem

**给定：** $I_{REF}=90\ \mu\text{A}$，$V_{tn}=0.7\ \text{V}$，$V_{tp}=-0.8\ \text{V}$，$\mu_n C_{ox}=160\ \mu\text{A/V}^2$，$\mu_p C_{ox}=40\ \mu\text{A/V}^2$，$|V_A|=10\ \text{V}$，$V_{DD}=V_{SS}=2.5\ \text{V}$。

**求：** 各管的 $I_D, |V_{OV}|, |V_{GS}|, g_m, r_o$，total gain，input common-mode range，output swing。

### 解答

[Slide 21 — Example Solution Table](https://app.notion.com)

Slide 21 — Example Solution Table

**Step 1: 电流分配**

- $Q_8, Q_5$ 匹配 → $I=I_{REF}=90\ \mu\text{A}$
- $Q_1,Q_2,Q_3,Q_4$ 各流 $I/2=45\ \mu\text{A}$
- $Q_7$ 与 $Q_5,Q_8$ 匹配 → $I_{Q_7}=90\ \mu\text{A}$，$Q_6$ 同流 $90\ \mu\text{A}$

**Step 2:** $V_{OV}, V_{GS}, g_m, r_o$**（见表 9.1）**

所有管 $|V_{OV}|\approx 0.3\ \text{V}$。NMOS（$Q_1$–$Q_4, Q_6$）：$g_m=0.3\ \text{mA/V}$ 或 $0.6\ \text{mA/V}$；PMOS（$Q_5, Q_7, Q_8$）类似。

$r_o=|V_A|/I_D$ → 流 $45\ \mu\text{A}$ 的管 $r_o=222\ \text{k}\Omega$；流 $90\ \mu\text{A}$ 的管 $r_o=111\ \text{k}\Omega$。

**Step 3: 增益**

$$
A_1=-0.3\times(222\|222)=-33.3\ \text{V/V}
$$

$$
A_2=-0.6\times(111\|111)=-33.3\ \text{V/V}
$$

$$
A_o=(-33.3)\times(-33.3)=1109\ \text{V/V}\approx 61\ \text{dB}
$$

**Step 4: input common-mode range**

- 下限：$Q_1,Q_2$ 离开饱和的临界。$Q_1$ 漏端电压 $=-V_{SS}+V_{GS3}=-2.5+1=-1.5\ \text{V}$，再低 $|V_{tp}|$ 即 $0.8\ \text{V}$ 离开饱和 → $v_{ICM,\min}=-2.3\ \text{V}$。
- 上限：$Q_5$ 需满足 $V_{SD5}\ge |V_{OV5}|=0.3\ \text{V}$ → $Q_5$ 漏端最高 $+2.2\ \text{V}$，故 $v_{ICM,\max}=2.2-|V_{GS1}|=2.2-1.1=1.1\ \text{V}$。

**Step 5: output swing**

- 上限：$Q_7$ 离开饱和 → $V_{DD}-|V_{OV7}|=2.5-0.3=+2.2\ \text{V}$。
- 下限：$Q_6$ 离开饱和 → $-V_{SS}+V_{OV6}=-2.5+0.3=-2.2\ \text{V}$。
- 故output swing $-2.2\ \text{V}\sim +2.2\ \text{V}$。

---

## 📝 本节总结（本节记三点即可）

<aside>
🎯

1. **CMRR 由** $R_{SS}$ **与器件匹配共同决定** → cascode tail current source是工程上提高 $R_{SS}$ 的标准做法。
2. **Current-mirror load 让differential pair实现 diff-in / single-out，且增益不打折**：$A_d=g_m(r_{o2}\|r_{o4})$。
3. **两级 CMOS 运放 = differential gain级 + 共源增益级 + Miller 补偿**，是几乎所有教科书运放设计的原型。
</aside>

---

## 📚 Homework 7 待做清单

- [ ]  Sedra/Smith 9.2
- [ ]  Sedra/Smith 9.6
- [ ]  Sedra/Smith 9.19
- [ ]  Sedra/Smith 9.59
- [ ]  Sedra/Smith 9.60
- [ ]  Sedra/Smith 9.71

*Due: 2026-05-27 23:00*

---

## 📎 原始幻灯片

[EE115 Lecture14 原始 PDF](EE115%20Lecture14%20%E2%80%94%20Differential%20Amplifiers%202/EE115_Lecture14.pdf)

EE115 Lecture14 原始 PDF