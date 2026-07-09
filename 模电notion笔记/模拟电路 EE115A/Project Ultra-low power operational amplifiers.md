# Project: Ultra-low power operational amplifiers

- **Research Project**（截止 2026-06-20 23:00）：三选一
    1. Wireless neural stimulators（无线神经刺激器）
    2. Ultra-low voltage DC-DC converter for energy harvesting（能量采集用超低压 DC-DC）
    3. Ultra-low power operational amplifiers（超低功耗运放）
    - 流程：文献综述 → 分析现有方法 → 提出新思路（可选）→ IEEE 格式报告 ≥ 5 页

<aside>
🎯

**选题**：Ultra-Low Power Operational Amplifiers。**Deadline：2026-06-20 23:00**。交付物：IEEE 格式 ≥ 5 页 report。

</aside>

## 1. 速通时间线（写作前准备 ≈ 10–12 h；加上正式写作总计 ≈ 24 h）

<aside>
⚡

**原则**：不做完整 research project 式深挖，只做“足够可信的 review paper”。核心任务是把初稿里的 AI 痕迹修掉：reference 真实、comparison table 不乱填、proposed idea 逻辑自洽。

</aside>

| 阶段 | 时间预算 | 任务 | 交付物 |
| --- | --- | --- | --- |
| Step 1 | 1 h | 读 [Ultra-low-power op-amp project 速通阅读包](Project%20Ultra-low%20power%20operational%20amplifiers/Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85.md)，确认最终主线：low-voltage headroom + low-current efficiency + performance boosting | 确定 report 不再扩题 |
| Step 2 | 3–4 h | 只精读 4 篇核心论文，让三条主线各有锚点：Ferreira 2007（low-voltage headroom，bulk-driven 范例）、Silveira 1996（gm/ID → low-current efficiency 方法论）、Assaad 2009（RFC → performance boosting）、Kulej & Khateb 2020（0.5 V 近期实测，喂 comparison table）；Chatterjee 2005 可只引用不精读。重点看 abstract / figures / performance table | 核心论据 + 关键数字确认 |
| Step 3 | 2–3 h | 修 comparison table：保留 8–10 篇 representative work；查不到的 FoM 写 N/A，不再硬算 | 可信表格 v1 |
| Step 4 | 1–2 h | 确认 proposed idea：weak-inversion 偏置的 bulk-driven input（拉高 gm/ID）+ recycling folded-cascode load + optional class-AB output；只做 qualitative argument，不声称仿真结果 | Proposed Idea 半页素材 |
| Step 5 | 2 h | 重排初稿结构：Background → Topology Review → Comparison → Proposed Direction；删除过细或无关内容 | 可写作大纲 |
| Buffer | 剩余时间 | 补 reference 格式、单位检查、图表 caption；然后进入正式写作 | 写作前最终材料包 |

## 2. Ultra-low power operational amplifier 是什么？

**Operational amplifier（运算放大器，op-amp）** 是模拟电路里最基础的 building block。它通常用来放大很小的模拟信号，也可以在 feedback system 中实现缓冲、滤波、积分、比较等功能。

<aside>
🧠

**一句话理解**：ultra-low power operational amplifier 就是在极低功耗、极低电源电压下仍然能完成基本模拟信号处理的 op-amp / OTA，主要服务于 wearable、implantable、IoT sensor、biomedical front-end、energy harvesting system 这类不能大量耗电的场景。

</aside>

这里的 **ultra-low power** 没有完全统一的硬性边界，但通常意味着：

- **Bias current**：nA–µA 级
- **Power consumption**：nW–µW 级
- **Supply voltage**：常见 0.3–1 V
- **目标应用**：不追求很高速度，而是追求长续航、低功耗、低电压可工作

### 2.1 为什么 ultra-low power op-amp 难？

普通 op-amp 需要同时满足 high gain、足够 bandwidth、低 noise、足够 phase margin、足够 output swing、足够 slew rate、PVT 稳定等指标。但 ultra-low power 条件下，设计会被两个核心约束卡住：

#### 约束一：电源电压太低，headroom 不够

传统 CMOS op-amp 中很多 transistor 是上下堆叠的，例如 differential pair、tail current source、current mirror、cascode stage。普通 gate-driven differential pair 的最低电源电压大致受限于：

$$
V_{DD,\min} \gtrsim V_{TH}+2V_{DSAT}+V_{DSAT,\text{tail}}
$$

当 $V_{DD}$ 降到 0.3–0.5 V，而 MOSFET 的 threshold voltage $V_{TH}$ 本身也接近 0.3–0.5 V 时，传统输入级会失去足够的 input common-mode range 和 output swing。

所以 low-voltage op-amp 需要一些特殊输入技术：

- **Bulk-driven / body-driven input**：信号从 bulk/body 输入，gate 端固定偏置，从而绕开 gate threshold 对输入 common-mode 的直接限制。
- **Floating-gate / quasi-floating-gate input**：用电容耦合信号，DC bias 单独设定。
- **Rail-to-rail input/output**：尽量扩大输入输出摆幅。
- ❓ Q&A | bulk-driven / floating-gate / rail-to-rail 这几个低压输入技术是什么？
    
    **共同要解决的问题**：约束一说传统 **gate-driven（栅驱动）** 输入级，光一个 $V_{TH}$ 就吃掉一大截 headroom，导致**输入共模范围（input common-mode range, ICMR：两个输入端一起平移、电路仍能正常工作的那段共模电平区间）** 被压得很窄。下面三招都是在回答“怎么把信号喂进差分对、又不被 $V_{TH}$ 卡死”。
    
    **1. Bulk-driven / body-driven（体驱动）。** MOS 有 4 个端：gate / source / drain + **bulk（body，衬底）**。平时信号走 gate，必须先翻过 $V_{TH}$ 这道坎；体驱动反过来——gate 接**固定偏置**让管子先导通，信号改从 bulk 注入，靠**体效应（body effect）** 调制电流。好处：bulk 这条路没有 $V_{TH}$ 门槛 → ICMR 直接做到 rail-to-rail，能在 0.3–0.5 V 工作。代价：体跨导 $g_{mb}\approx 0.2\,g_m$（弱很多）→ 增益小、噪声差，还有 **latch-up（闩锁）** 风险。这是本项目 §4.2 的首选输入级。
    
    **2. Floating-gate（浮栅）/ quasi-floating-gate（准浮栅, QFG）。**
    
    - **浮栅**：gate 完全不接直流，用一个**电容把信号耦合**上去，DC 工作点靠预置电荷设定。好处：DC 偏置可任意设、绕开 $V_{TH}$、input range 大。代价：要大电容（占面积）+ 初始电荷 / 初始化麻烦。
    - **准浮栅 QFG**：用一个超大电阻（实为一只关断的 MOS 当电阻）把浮栅弱弱拉到偏置，免去初始化；但漏电会让 DC 点慢慢漂。
    
    **3. Rail-to-rail（轨到轨）input / output。** “rail” = 两条电源轨（$V_{DD}$ 与 GND / $V_{SS}$）。rail-to-rail 就是**输入共模 / 输出摆幅能一直摆到几乎贴着两条轨**——低压下电压本就少，必须尽量“用满”。输入做 rail-to-rail 常用 NMOS+PMOS **互补双输入对**拼接；输出做 rail-to-rail 常用**共源输出级**（而非源跟随器，后者会再吃一个 $V_{GS}$）。
    
    ```
    gate-driven（信号要翻 VTH 坎）         bulk-driven（信号走衬底, 绕开 VTH）
      in ──┤G                               Vbias ──┤G
            │ D ──►  需 VGS>VTH 才导通               │ D ──►
      ──────┤S                               ───────┤S
      固定 ─┤B                               in ─────┤B  ◄ 信号从 body 进
    ```
    
    <aside>
    🔗
    
    **一句话接回约束一**：当 $V_{DD}\lesssim V_{TH}+2V_{DSAT}+V_{DSAT,\text{tail}}$ 不再成立，这三招就是出路——bulk-driven / floating-gate 把 $V_{TH}$ 从信号通路里踢走，rail-to-rail 把仅剩的电压“用到顶”。
    
    </aside>
    
- ❓ Q&A | $V_{DD,\min}$ 这个公式怎么来的？（模电基础补充）
    
    **先回顾 3 个模电基础概念：**
    
    - **Threshold voltage** $V_{TH}$：MOSFET 要 $V_{GS}>V_{TH}$ 才会形成沟道、真正“导通”，典型值 0.3–0.5 V。
    - **Overdrive / 饱和压降** $V_{DSAT}$：晶体管要待在 saturation 区（当受控电流源用、能放大）必须满足 $V_{DS}\ge V_{GS}-V_{TH}\equiv V_{ov}$；这个最小 $V_{DS}$ 记作 $V_{DSAT}$，典型 0.1–0.2 V。每个管子都得“分到”这么多电压才能正常工作。
    - **为什么会堆叠**：op-amp 输入级 = 一个 differential pair（两个输入管）+ 底下 tail current source（定偏置电流）+ 顶上 load，三者从一条 rail 到另一条 rail 竖着叠起来。
    
    **把电压预算从下往上加一遍**（NMOS 输入对，从 ground 叠到 $V_{DD}$）：
    
    - 尾电流源 tail：要分到 $V_{DSAT,\text{tail}}$
    - 输入管的 $V_{GS}$：$V_{TH}+V_{DSAT}$
    - 顶上负载 / 输出余量：再要一个 $V_{DSAT}$
    
    三段相加必须 $\le V_{DD}$，于是：
    
    $$
    V_{DD,\min}\gtrsim V_{TH}+2V_{DSAT}+V_{DSAT,\text{tail}}
    $$
    
    **代数感受**：取 $V_{TH}=0.4$ V、每个 $V_{DSAT}=0.15$ V → $V_{DD,\min}\approx 0.4+2(0.15)+0.15=0.85$ V。
    
    <aside>
    🎯
    
    **结论**：传统 gate-driven 输入级光一个 $V_{TH}$ 就吃掉 ~0.4 V，再叠 3 个饱和压降，最低也要 ~0.8–1 V。当 $V_{DD}$ 只有 0.3–0.5 V 时这条不等式根本不成立 → 必须把 $V_{TH}$ 从信号通路里踢出去，这就是 bulk-driven（信号走 body、gate 只给固定偏置）能在 0.3–0.5 V 工作的根本原因。
    
    </aside>
    

#### 约束二：电流太小，$g_m$、速度、噪声都会变差

- ❓ Q&A | 这里的 $g_m$ 和“小信号模型”到底是什么？（模电 L2–L4 基础补充）
    
    **1. 大信号 vs 小信号。** 晶体管是非线性器件：$I_D$ 随 $V_{GS}$ 是一条弯曲的曲线，不能直接套线性电路法。先定一个直流**静态工作点（operating point / Q 点）**，再只看它附近的微小摆动——这一小段曲线 ≈ 它的切线；把器件换成“切线斜率”等效电路就是**小信号模型（small-signal model）**。你在 L2 见过的 $i_C(t)=I_C+i_c(t)$（直流偏置 + 交流小信号）正是这个思想。
    
    **2. 跨导** $g_m$**（transconductance）= 放大引擎。** 栅压变一点、漏电流变多少：
    
    $g_m=\left.\frac{\partial I_D}{\partial V_{GS}}\right|_{Q}$
    
    单位 A/V，即西门子 (S)。小栅压 $v_{gs}$ 产生漏电流 $i_d=g_m v_{gs}$；所以晶体管本质是“压进、流出”的**跨导放大器**（正好对应 L2 表里的 transconductance amplifier）。
    
    **3. 输出电阻** $r_o$**。** 漏电流还随漏压轻微变化（**沟道长度调制 channel-length modulation**），其斜率倒数 $r_o=1/(\lambda I_D)$ 很大（几十~几百 kΩ）。
    
    **4. MOS 小信号等效电路：** 栅源口几乎不抽流（输入电阻 ≈ ∞），漏源口 = 受控电流源 $g_m v_{gs}$ 并联 $r_o$：
    
    ```
    gate ○───────┐                 ┌──────○ drain
                 │                 │
               v_gs      (↓) g_m·v_gs     r_o
                 │                 │
    src  ○───────┴─────────────────┴──────○ src
    ```
    
    **5. 单管增益（共源 common-source）：** 漏端接负载 $R_L$ 时
    
    $A_v=\frac{v_{out}}{v_{in}}=-g_m\,(r_o\parallel R_L)$
    
    负号 = 反相；只剩 $r_o$ 时的 $g_m r_o$ 叫**本征增益（intrinsic gain）**，是单级增益的上限。
    
    <aside>
    🔗
    
    **接回理想运放：** L3 里被当成“无穷大”的开环增益 $A_0$，真身就是 $A_0\approx g_m\cdot R_{out}$（输入差分对的 $g_m$ × 输出高阻节点电阻）。本项目反复出现的 $g_m/I_D$ 就是“每 1 A 电流换来多少 $g_m$”的**跨导效率**——**弱反型（weak inversion）** 区它最高，所以超低功耗运放把管子偏置在那里；又因 GBW $=g_m/(2\pi C_L)$ 也由 $g_m$ 定，于是“电流太小 → $g_m$ 小 → 增益、速度都差”。
    
    </aside>
    

op-amp 的核心小信号能力来自 transconductance：

$$
g_m=\frac{\partial I_D}{\partial V_{in}}
$$

$g_m$ 越大，通常越容易得到更高 gain、更高 GBW、更低 input-referred noise。但 $g_m$ 往往需要用 bias current 换。ultra-low power 设计把 current 压到 nA–µA 级后，会带来：

- bandwidth 下降
- slew rate 下降
- noise 变差
- gain 可能不够

因此 ultra-low power analog design 特别重视：

$$
\frac{g_m}{I_D}
$$

也就是 **transconductance efficiency**，可以理解成“每单位电流能换来多少跨导”。在 weak inversion / subthreshold 区域，MOSFET 的 $g_m/I_D$ 最高，因此很多 ultra-low-power amplifier 会把 transistor 偏置在 weak inversion。

但 weak inversion 的代价也明显：

- speed 慢
- PVT 敏感
- matching 较差
- bandwidth 通常不高
- ❓ Q&A | 这里的 "amplifier" 是单指运放，还是也包括单管 MOSFET 的跨导？
    
    **先分清两个层级：**
    
    - **器件级（device level）**：单个 MOSFET，用跨导 $g_m=\partial I_D/\partial V_{in}$ 描述它"把输入电压变化转成输出电流变化"的放大能力。这是放大的物理根源，但一般叫 transistor / gain device，不会单独叫一个 amplifier。
    - **电路块级（block level）**：op-amp / OTA，是由很多 transistor 搭出来的**完整放大器模块**。
    
    **这一页的默认语境：** 出现的 "amplifier / op-amp / OTA" 都指**电路块级的运放**，不是把单个 MOSFET 叫 amplifier。运放的放大能力归根结底来自内部 transistor 的 $g_m$。
    
    <aside>
    🎯
    
    **特别注意 OTA：** OTA = Operational **Transconductance** Amplifier，输出 $I_{out}=G_m\cdot V_{in}$。这里的 $G_m$ 是**整个放大器的等效跨导**，由内部多个管子的 $g_m$ 组合而成（例如 §4.4 current-reuse 让 $G_m=g_{mn}+g_{mp}$）。所以 OTA 语境下 "amplifier" 和 "transconductance" 关系最近，但它仍是 block 级的 $G_m$，不是单管 $g_m$。
    
    </aside>
    
    **一句话：** 这页的 amplifier = 运放(op-amp/OTA) 这个块；单管 $g_m$ 是构成它的器件级放大机制。严格说单个 transistor 接成 CS 等结构也是 single-stage amplifier，所以两者是"整体 vs 最小单元"的关系。
    

### 2.2 这个 project 真正要回答什么？

这个 project 不是要完整设计一个商业 op-amp，而是做 review。核心问题是：

<aside>
🎯

在 ultra-low voltage / ultra-low power 条件下，现有 op-amp / OTA topology 如何解决 **headroom、电流效率、噪声和动态性能** 之间的 trade-off？

</aside>

可以把相关论文归成四类：

| 核心问题 | 解决思路 | 代表方向 |
| --- | --- | --- |
| 电压太低，输入级没法工作 | 改变输入方式，降低 threshold/headroom 限制 | bulk-driven, floating-gate, rail-to-rail input |
| 电流太小，$g_m$ 不够 | 提高 transconductance efficiency | weak inversion, $g_m/I_D$ methodology |
| 同样电流下性能不够 | 复用电流 / 提升有效 $G_m$ | inverter-based, current-reuse, recycling folded cascode |
| 大信号和低频噪声表现差 | 动态增强 / 调制降噪 | class-AB, FVF, chopper, auto-zero |

### 2.3 初稿主线一句话

<aside>
🧩

**Ultra-low-power op-amp design is about spending every nanoampere efficiently**：weak inversion 最大化 $g_m/I_D$，bulk-driven input 解决 low-voltage headroom，current-reuse / recycling folded cascode 提升 effective transconductance，class-AB 或 chopper 技术补 slew-rate 与 low-frequency noise 的短板。

</aside>

换句话说，低功耗 op-amp 的本质不是“把普通 op-amp 的电流简单调小”，而是整个 topology 都要围绕 **低电压、低电流、低噪声、足够动态性能** 重新设计。

## 3. IEEE Conference Paper 结构 (≈ 5 页 double column)

1. **Abstract** (~150 words)
2. **I. Introduction** (~0.5 页) — motivation：IoT / biomedical implant / energy-harvesting system 对 nW–µW 级 amplifier 的需求
3. **II. Background & Design Challenges** (~1 页) — supply scaling、gm/ID、noise-power tradeoff、PVT 鲁棒性
4. **III. Review of Mature Topologies** (~1.5 页) — §4 的几大类
5. **IV. Comparison & Discussion** (~1 页) — FoM 表 + 趋势图
6. **V. Proposed Idea / Direction**（可选，~0.5 页）
7. **VI. Conclusion** (~0.2 页)
8. **References** (15–25 篇，IEEE 格式)

### 3.1 关键 Figures of Merit（务必在 comparison table 里用）

$$
\text{FoM}_S = \frac{\text{GBW}\,[\text{MHz}] \cdot C_L\,[\text{pF}]}{I_{DD}\,[\text{mA}]}
$$

$$
\text{FoM}_L = \frac{\text{SR}\,[\text{V/}\mu\text{s}] \cdot C_L\,[\text{pF}]}{I_{DD}\,[\text{mA}]}
$$

外加常见指标：DC gain (dB)、PM (°)、CMRR / PSRR、input-referred noise、THD、area、supply voltage。

- ❓ Q&A | FoM_S / FoM_L 是什么？为什么要两个？
    
    **FoM = Figure of Merit（品质因数）**：把“性能 / 代价”压成一个数，方便不同 paper 公平 PK。代价用电流 $I_{DD}$（≈ 功耗）衡量，性能用速度衡量，再乘负载 $C_L$（驱动越大的电容越难）。
    
    **先认 4 个量：**
    
    - $I_{DD}$：总电流，乘上 $V_{DD}$ 就是功耗 → 越小越省电。
    - $C_L$：要驱动的负载电容，越大越难。
    - **GBW（gain–bandwidth product）**：小信号速度，单极点近似下 $\text{GBW}=g_m/(2\pi C_L)$。
    - **SR（slew rate）**：大信号速度，$\text{SR}=I_{\max}/C_L$，即输出最快能多快地摆。
    
    **两个公式 = 两种“速度效率”：**
    
    $$
    \text{FoM}_S=\frac{\text{GBW}\,[\text{MHz}]\cdot C_L\,[\text{pF}]}{I_{DD}\,[\text{mA}]}\quad(\text{小信号})
    $$
    
    $$
    \text{FoM}_L=\frac{\text{SR}\,[\text{V/}\mu\text{s}]\cdot C_L\,[\text{pF}]}{I_{DD}\,[\text{mA}]}\quad(\text{大信号})
    $$
    
    直观读法：**每 1 mA 电流、驱动 1 pF 负载能换来多少速度**，数越大越高效。
    
    <aside>
    🔗
    
    **和** $g_m/I_D$ **的隐藏联系**：把 $\text{GBW}=g_m/(2\pi C_L)$ 代进 FoM_S，$C_L$ 直接约掉 → $\text{FoM}_S=g_m/(2\pi I_{DD})\propto g_m/I_D$。所以小信号 FoM 本质就是跨导效率 $g_m/I_D$ —— 这正是 ultra-low-power 要把管子推到 weak inversion（$g_m/I_D$ 最高）的原因。
    
    </aside>
    
    **为什么要两个**：有的技巧只补一种速度——class-AB 大信号电流暴增，主要拉高 $\text{FoM}_L$（SR）；current-reuse / RFC 提升 $g_m$，主要拉高 $\text{FoM}_S$。两个都列，才能看出一个设计是“小信号快”还是“大信号快”。
    

## 4. 成熟方案（Topology Review 主体）

按“低电源电压”和“低功耗”两条主轴归纳：

- ❓ Q&A | 实际工业里运放怎么“堆叠”？微电流又怎么玩？（衔接 EE115A 课本 → 工业实践）
    
    **你课本已掌握**：单管 $g_m$ / $r_o$、CS/CG/CD、电流镜、cascode→$A_0^2$、差分对、两级运放。课本默认两件事：① 管子在**强反型（strong inversion）**、平方律 $g_m=2I_D/V_{OV}$；② 电流源是理想的。工业超低功耗设计恰恰要打破这两条。
    
    **1. 微电流 → 从平方律掉进指数律。** 把电流压到 nA–µA，$V_{OV}$ 趋近 0，管子进入**弱反型 / 亚阈值（subthreshold）**：$I_D\propto e^{V_{GS}/nV_T}$、$g_m=I_D/(nV_T)$。此时 $g_m/I_D$ 触顶到约 25–30 V⁻¹（即 $1/(nV_T)$）——每安培电流换来的跨导最大。代价：① 速度（**特征频率 transit frequency** $f_T\propto g_m/C_{gs}$）掉到 kHz–MHz；② **失配（mismatch）** 更严重，弱反型增益对 $V_{TH}$ 指数敏感 → 必须大面积管 + **共质心版图（common-centroid layout）**；③ 对温度 / PVT 指数敏感。
    
    **2. 微电流怎么“造”出来。** 低压下不能拿普通电阻定 nA。工业用**恒跨导偏置（constant-gm / β-multiplier）** 自偏置 + 大电阻（常片外或开关电容）+ 电流分流镜把 µA 参考除成 nA，再加启动电路。
    
    **3. 堆叠（stacking）：为什么贵 + 工业怎么排。** 每多叠一个管子就多吃一个 $V_{DSAT}$（见约束一）。
    
    - **套筒式 cascode（telescopic cascode）**：差分对 + cascode + 尾管全挤在一列，增益 $\sim A_0^2$ 最高，但串 ~5 管 → 需要高 $V_{DD}$、输出摆幅极小。
    - **折叠式 cascode（folded cascode）**：把信号电流“折”到一条相反类型的并联支路，cascode 管不再压在输入管头上 → rail-to-rail 间串联管变少，**用 ~2× 电流换回 headroom 与摆幅**，是低压模拟的主力。
    - **循环折叠 cascode（RFC，见 §4.5）**：把折叠支里“只导流不出力”的电流源管复用成第二对输入管，等效 $g_m$ 白赚 2–3×。
    - 当 $V_{DD}$ 只剩 0.3–0.5 V，连折叠都嫌高 → 干脆不 cascode，改 **bulk-driven / inverter-based / current-reuse（推挽 push-pull）** 的单层结构。
    
    ```
    套筒 Telescopic（单列, ~5 层, 吃 headroom）     折叠 Folded（拆两支, ~3 层, 省 headroom）
      VDD ─ PMOS 电流源                            VDD ─┬─ 电流源 ──┐
           └ PMOS cascode                              │           └ cascode 负载
           └ NMOS cascode                          输入对 ───────┘ (信号折到旁支)
           └ NMOS 输入对  ◄ 信号                       │
           └ NMOS 尾电流源                          尾电流源
      GND                                          GND
      串 5 管 → 要高 VDD                            串 ~3 管 → 低 VDD 也能跑
    ```
    
    <aside>
    🔗
    
    **接回本项目 §4**：这些“堆叠玩法”正是 §4 各小节的答案——subthreshold（微电流）、bulk-driven（砍掉 $V_{TH}$ 省 headroom）、folded / recycling cascode（聪明地堆）、current-reuse / inverter（复用电流）、class-AB（不加静态电流也能提 slew）。
    
    </aside>
    

### 4.1 Subthreshold / Weak-Inversion Operation

- **思路**：把 MOSFET 偏置到 $V_{GS} \lt V_{TH}$，此时 $g_m/I_D$ 达到理论上限（约 25–28 V⁻¹）。
- **优点**：单位电流换最大 $g_m$，是 nW 级 op-amp 的基础。
- **代价**：带宽小（kHz–MHz）、PVT 敏感、matching 差。
- **典型架构**：subthreshold two-stage Miller、subthreshold folded cascode。
- **代表文献**：Vittoz & Fellrath, *JSSC* 1977（开山之作）；后续大量 biomedical front-end。

### 4.2 Bulk-Driven (Body-Driven) Op-Amp

- **思路**：信号从 bulk 端注入，gate 端给固定偏置。$V_{TH}$ 不再限制 input common-mode range → 可以做到 $V_{DD}\le 0.5\,\text{V}$。
- **优点**：true rail-to-rail，可在 0.3–0.5 V 工作。
- **代价**：$g_{mb}\approx 0.2\,g_m$，gain 和噪声变差；存在 latch-up 风险。
- **代表文献**：
    - Blalock, Allen & Rincon-Mora, *TCAS-II* 1998 — 1-V CMOS op-amp
    - Chatterjee, Tsividis & Kinget, *JSSC* 2005 — 0.5-V analog circuits
    - Ferreira & Pimenta, *TCAS-I* 2007 — ultra-low-voltage ultra-low-power CMOS op-amp

### 4.3 Floating-Gate / Quasi-Floating-Gate (QFG) Input

- **思路**：用 capacitor 把信号耦合到一个浮栅，DC bias 单独设定。
- **优点**：input range 大，可任意设定 input CM。
- **代价**：需要大电容（面积大）、初始化问题；QFG 用一颗大电阻替代，但漏电会漂移。
- **代表文献**：Ramirez-Angulo, Lopez-Martin 等一系列工作（2003–2010）。

### 4.4 Current-Reuse / Inverter-Based / Push-Pull

- **思路**：让 NMOS 和 PMOS 共享同一支 bias current，$g_{m,\text{eff}} = g_{mn} + g_{mp}$，电流利用率翻倍。
- **优点**：FoM 显著提高，是近 10 年低功耗 amplifier 的主流之一。
- **代价**：input common-mode range 受限、需要 PVT 鲁棒的 bias。
- **代表文献**：Nauta inverter-based OTA (1992)；近年大量 biomedical neural recording amplifier。

### 4.5 Recycling Folded Cascode (RFC) 及其变体

- **思路**：把传统 folded cascode 里"浪费"的电流重新利用，等效 $g_m$ 提升 2–3×。
- **优点**：同样电流下 GBW 和 SR 大幅提升，是非常受欢迎的"免费午餐"。
- **代价**：稍微增加电路复杂度。
- **代表文献**：Assaad & Silva-Martinez, *JSSC* 2009 — "The Recycling Folded Cascode"。

### 4.6 Class-AB Output Stage

- **思路**：让输出级在大信号时电流远大于 quiescent 电流，提升 slew rate 而不增 static power。
- **常用实现**：FVF (Flipped Voltage Follower)、local common-mode feedback、cross-coupled pair。
- **代价**：稳定性设计复杂。
- **代表文献**：Carvajal *et al.*, *TCAS-I* 2005 — FVF and applications。

### 4.7 gm/ID-Based Systematic Design Methodology

- **思路**：不是 topology，而是设计方法。把 $g_m/I_D$ 当 design variable，跨 strong–moderate–weak inversion 统一处理。
- **价值**：在 ultra-low-power 设计里几乎是标配。
- **代表文献**：Silveira, Flandre & Jespers, *JSSC* 1996。

### 4.8 Chopper-Stabilized / Auto-Zero（针对低频低噪声场景）

- **思路**：用调制把 1/f 噪声搬到高频再滤掉。
- **适用**：biomedical（EEG、ECG）front-end，nA 级 bias。
- **代表文献**：Enz & Temes, *Proc. IEEE* 1996（综述）；Denison *et al.*, *JSSC* 2007（neural recording chip）。

## 5. Comparison Table 模板（拆成两表）

主键用 **Ref. / Year + Topology**，两表一一对应。

### 5.1 表 A — Configuration（工艺与功耗）

| Ref. / Year | Topology | Tech (nm) | V_DD (V) | I_DD |
| --- | --- | --- | --- | --- |
| 填入 10–12 行 representative work |  |  |  |  |

### 5.2 表 B — Performance & FoM

| Ref. / Year | GBW | SR | C_L | Gain (dB) | PM (°) | FoM_S | FoM_L |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 与表 A 同顺序填写 |  |  |  |  |  |  |  |

## 6. "Proposed Idea" 的几个可行兜底方向

如果不想做 tape-out 级创新，可以选下面任一作为 §V，配 schematic + 定性分析即可：

1. **Bulk-driven + Recycling Folded Cascode 结合** — 把 RFC 的电流复用思想搬到 bulk-driven 输入级，论证 $g_m/I_D$ 的提升。
2. **Inverter-based input + class-AB FVF output** — 给出 push-pull 全程的 schematic，比较 quiescent vs. transient current。
3. **Subthreshold two-stage with adaptive biasing** — 加一条 sense 支路，让 bias 随 load 自适应。
4. **改进 compensation 方案** — 用 Ahuja-style 或 indirect feedback compensation 替换 Miller，论证 PM/area tradeoff。

建议挑 ①或 ②，文献支撑最强。

## 7. 推荐文献检索路径

- **数据库**：IEEE Xplore（首选）、Google Scholar、Scopus
- **关键 journals/conferences**：
    - *IEEE JSSC* (Journal of Solid-State Circuits)
    - *IEEE TCAS-I / TCAS-II*
    - *IEEE TBioCAS*
    - *ISSCC*, *VLSI Symposium*, *CICC*, *ESSCIRC*
- **关键搜索词**（建议组合检索）：
    - `"ultra-low-power" AND (operational amplifier OR OTA)`
    - `"sub-threshold" OR "weak inversion" amplifier`
    - `"bulk-driven" OR "body-driven" op-amp`
    - `"nano-power" amplifier`
    - `"current reuse" OR "inverter-based" OTA`
    - `recycling folded cascode`
- **起步综述**（先读这类，能快速建立全景）：
    - Ferreira *et al.*, *TCAS-I* 2007
    - Rodovalho, Aiello & Rodrigues, *Electronics* 2021 — "Ultra-Low-Voltage CMOS Amplifiers" survey

## 8. Checklist（提交前自查）

- [ ]  5 页满（含 References），IEEE 双栏模板（`IEEEtran.cls`）
- [ ]  两个 comparison table 均 ≥ 8 行，含 FoM
- [ ]  每个引用 schematic 都用自己重画的图，不要直接截 paper
- [ ]  References 全部用 IEEE 格式（编号 + 缩写期刊名）
- [ ]  Abstract、结论与正文一致
- [ ]  拼写、单位（μ、Ω）、上下标检查

[Ultra-Low-Power Operational Amplifiers: A Review of Low-Voltage and Low-Power Design Techniques](Project%20Ultra-low%20power%20operational%20amplifiers/Ultra-Low-Power%20Operational%20Amplifiers%20A%20Review%20of.md)

[Ultra-low-power op-amp project 速通阅读包](Project%20Ultra-low%20power%20operational%20amplifiers/Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85.md)

## 9. 当前进度评估与冲刺计划（2026-06-11 更新）

<aside>
📊

**一句话结论**：写作前准备完成约 60%。骨架全齐——主线、IEEE 结构、proposed idea、初稿 v2 + figure extraction map 都已就位；卡在三件硬活上：**精读核数（Step 2）→ comparison table 实数（Step 3）→ 论文原图截取（Buffer）**。距 6/20 23:00 还有 9 天，按下方冲刺计划走完全来得及。

</aside>

| 阶段 | 状态 | 评估 |
| --- | --- | --- |
| Step 1 主线确认 | ✅ 完成 | 速通阅读包已建好，三条主线（headroom / efficiency / boosting）已锁定，不再扩题 |
| Step 2 精读 4 篇核心 | 🔶 半完成 | 4 篇核心论文的速读笔记子页已备好；仍需核对 abstract / figures / performance table 中的关键数字 |
| Step 3 修 comparison table | ❌ 未开始 | 初稿 v2 的 performance 表里大量「Verify」占位——这是当前最大的 AI 痕迹，也是可信度短板 |
| Step 4 Proposed idea | ✅ 完成 | weak-inversion bulk-driven + RFC + class-AB 已定性成稿，「不虚构仿真结果」的边界声明也写好了 |
| Step 5 结构重排 | ✅ 完成 | 初稿 v2 已按 IEEE 结构全文展开，含 5 页篇幅预算和 figure extraction map |
| Buffer / 正式写作 | ❌ 未开始 | 7 张论文原图未截取；LaTeX / Word 正式稿未动工 |

### 9.1 冲刺计划（已同步 Plan 页面 + 日历；统一排上午，避开 CS110 / EE115A 复习块）

- [ ]  June 12, 2026 → June 13, 2026：精读 4 篇核心论文（只看 abstract / figures / performance table），核实 GBW、SR、C_L、I_DD 关键数字
- [ ]  June 14, 2026：把初稿 v2 两张表的「Verify」全部替换成原文实数或 N/A，不再硬算
- [ ]  June 16, 2026 → June 17, 2026：按 figure extraction map 截取 / 标注 7 张论文原图（工作流 A：pdftoppm + 裁切复核；6/15 整天让位 EE115B HW4）
- [ ]  June 18, 2026 → June 19, 2026：IEEEtran 双栏正式写作 + 按 checklist 自查
- [ ]  June 20, 2026：终检（单位 / 图注 / references 格式），23:00 前提交