# Ultra-Low-Power Operational Amplifiers: A Review of Low-Voltage and Low-Power Design Techniques

<aside>
🔬

**Revision target** — 这一版按 **IEEE double-column ≥5 pages** 的密度扩充，不再只是“写作提纲”。主线从 application motivation 收回到 **CMOS op-amp / OTA transistor-level design**：voltage headroom、$g_m/I_D$、$g_{mb}$ penalty、intrinsic gain、current reuse、folded / recycling cascode、class-AB slew enhancement、noise / offset cancellation。

**图片原则** — 正文不使用 mermaid 自画图。所有图都作为 “paper figure extraction slots” 标出：**哪篇顶刊 / 顶会文章、哪个原图号、图中应该标注什么、放在本文哪一段**。正式稿中直接从论文 PDF 截图 / extract，再在 caption 中写 “adapted from / reproduced from …”。

</aside>

## 0. Five-page writing budget（避免又不够 5 页）

<aside>
📏

**目标长度**：IEEE conference 双栏 5 页，含 6–7 张图、2 张表、15–20 篇 references。正文需要约 **3200–4200 English words**；如果图较多，正文约 2800–3300 words 也够。下面这版按“正文可直接扩成 5 页”的密度写。

</aside>

| Section | Page budget | Content density target | Figure / table |
| --- | --- | --- | --- |
| Abstract + I. Introduction | 0.6 page | 明确 thesis：ULP op-amp 是 transistor-level resource allocation，不是 application survey | None |
| II. Device-Level Constraints | 1.0 page | $g_m/I_D$, headroom, $g_m r_o$, noise, matching, compensation | Fig. 1, Fig. 2 |
| III. Topology Review | 1.8–2.0 pages | Bulk-driven, QFG, inverter-based, RFC, class-AB, chopper | Fig. 3–Fig. 8 |
| IV. Comparison and Discussion | 0.8 page | 按 mechanism 比较，而不是按 application 比较 | Table I, Table II |
| V. Proposed Direction + VI. Conclusion | 0.6–0.8 page | 提出 coherent qualitative design，不虚构仿真结果 | Optional Fig. 9 |
| References | 0.5–0.7 page | 15–20 篇 IEEE style | — |

## Figure extraction map（正式稿插图清单）

<aside>
🖼️

**执行规则**：正式 Word / LaTeX 稿中，下面每个 Figure slot 都直接从对应论文 PDF 截图 / extract 原图，并在图注中写清楚 “adapted from …”。如果课程要求“不能直接复制图”，则按这些原图重画，但仍在 caption 写 “redrawn based on …”。这页先保留原图来源，方便回源核对。

</aside>

| 本文图号 | 放置位置 | 直接摘取来源 | 原图号 | 正文标注重点 | 原文链接 |
| --- | --- | --- | --- | --- | --- |
| Fig. 1 | Section II-A | F. Silveira, D. Flandre, and P. G. A. Jespers, “A $g_m/I_D$ based methodology for the design of CMOS analog circuits and its application to the synthesis of a silicon-on-insulator micropower OTA,” **IEEE JSSC**, 1996. | Fig. 1 | $g_m/I_D$ versus normalized current curve；标 weak / moderate / strong inversion。 | [ieeexplore.ieee.org/document/535416](http://ieeexplore.ieee.org/document/535416)（DOI 10.1109/4.535416） |
| Fig. 2 | Section II-F | Silveira, Flandre, and Jespers, **JSSC 1996**. | Fig. 2 | SOI micropower OTA basic schematic；标 input pair、cascode、current mirror 的 $g_m/I_D$ sizing role。 | [ieeexplore.ieee.org/document/535416](http://ieeexplore.ieee.org/document/535416)（DOI 10.1109/4.535416） |
| Fig. 3 | Section III-A | S. Chatterjee, Y. Tsividis, and P. R. Kinget, “0.5-V analog circuit techniques and their application in OTA and filter design,” **IEEE JSSC**, 2005. | Fig. 1 | Fully differential body-input gain stage with local CMFB；标 body input、gate bias、CMFB。 | [ieeexplore.ieee.org/document/1546214](http://ieeexplore.ieee.org/document/1546214)（DOI 10.1109/JSSC.2005.856280） |
| Fig. 4 | Section III-A | Chatterjee, Tsividis, and Kinget, **JSSC 2005**. | Fig. 4 | 0.5-V biasing arrangement；标 body bias / common-mode loop。 | [ieeexplore.ieee.org/document/1546214](http://ieeexplore.ieee.org/document/1546214)（DOI 10.1109/JSSC.2005.856280） |
| Fig. 5 | Section III-B | L. H. C. Ferreira, T. C. Pimenta, and R. L. Moreno, “An ultra-low-voltage ultra-low-power CMOS Miller OTA with rail-to-rail input/output swing,” **IEEE TCAS-II**, 2007. | 原文 OTA topology schematic（打开 IEEE PDF 后最终确认图号） | Bulk-driven / threshold-lowering path、tail-current treatment、Miller compensation。 | [ieeexplore.ieee.org/document/4349224](http://ieeexplore.ieee.org/document/4349224) |
| Fig. 6 | Section III-C | B. Nauta, “A CMOS transconductance-C filter technique for very high frequencies,” **IEEE JSSC**, 1992. | Fig. 2(d) | Six-inverter transconductor；标 Inv1/Inv2 V-to-I conversion，Inv3–Inv6 CM control / negative resistance。 | [ieeexplore.ieee.org/document/127337](http://ieeexplore.ieee.org/document/127337)（DOI 10.1109/4.127337） |
| Fig. 7 | Section III-D | R. S. Assaad and J. Silva-Martínez, “The recycling folded cascode: A general enhancement of the folded cascode amplifier,” **IEEE JSSC**, 2009. | Fig. 1 | Conventional folded cascode vs. recycling folded cascode；标 “idle devices” 如何进入 signal path。 | [ieeexplore.ieee.org/document/5226701](http://ieeexplore.ieee.org/document/5226701) |
| Fig. 8 | Section III-D | Assaad and Silva-Martínez, **JSSC 2009**. | Fig. 2 | Proposed RFC OTA schematic；标 split input drivers、mirror gain $K$、output node。 | [ieeexplore.ieee.org/document/5226701](http://ieeexplore.ieee.org/document/5226701) |
| Fig. 9 | Section III-E / V | T. Kulej and F. Khateb, “A compact 0.3-V class AB bulk-driven OTA,” **IEEE TVLSI**, 2020. | Fig. 1 | 0.3-V class-AB bulk-driven OTA schematic；标 body-driven input、class-AB branch、bias current path。 | [ieeexplore.ieee.org/document/8836116](http://ieeexplore.ieee.org/document/8836116) |
| Backup Fig. | Noise subsection | C. C. Enz and G. C. Temes, “Circuit techniques for reducing the effects of op-amp imperfections: autozeroing, correlated double sampling, and chopper stabilization,” **Proc. IEEE**, 1996. | Chopper principle figure（最终以原 PDF 图号为准） | 只在版面允许时加入；标 input chopper、offset / $1/f$ noise modulation、demodulator。 | [ieeexplore.ieee.org/document/542410](http://ieeexplore.ieee.org/document/542410)（DOI 10.1109/5.542410） |

<aside>
✅

**正式 5 页建议保留**：Fig. 1, Fig. 3, Fig. 5, Fig. 6, Fig. 7, Fig. 8, Fig. 9 共 7 张。

**如果版面不够**：Fig. 2 和 chopper backup figure 可以删；但 Fig. 1 / Fig. 7 / Fig. 8 最好保留，因为它们最能体现 transistor-level 主线。

</aside>

## Abstract

As CMOS supply voltages approach the transistor threshold voltage and bias currents are reduced to the nanoampere–microampere range, operational amplifiers and operational transconductance amplifiers (OTAs) become constrained primarily by transistor-level voltage headroom, transconductance efficiency, output resistance, and noise. This review surveys ultra-low-power CMOS op-amp techniques from a circuit-design perspective rather than from an application-driven perspective. The discussion first establishes the device-level limits imposed by $g_m/I_D$, weak-inversion operation, intrinsic gain $g_m r_o$, MOS stacking, thermal noise, flicker noise, and mismatch. It then reviews mature low-voltage / low-power topology families, including bulk-driven input stages, floating-gate and quasi-floating-gate inputs, inverter-based current-reuse OTAs, folded and recycling folded cascodes, class-AB output stages, and dynamic offset-cancellation techniques. Representative figures from JSSC, TCAS, TVLSI, and related solid-state circuit literature are selected as visual evidence, with each figure tied to a specific transistor-level mechanism. Reported silicon results confirm that these techniques sustain useful amplifier operation at supply voltages down to 0.3 V and power budgets from the hundred-microwatt level down to the tens-of-nanowatt level. The central conclusion is that ultra-low-power op-amp design is not merely the reduction of bias current; it is the deliberate redistribution of voltage headroom and reuse of bias current so that each nanoampere contributes maximum useful transconductance, gain, slew rate, or noise reduction.

*Index Terms* — Ultra-low-power op-amp, OTA, CMOS analog design, $g_m/I_D$, weak inversion, bulk-driven MOSFET, current reuse, inverter-based OTA, recycling folded cascode, class-AB, chopper stabilization.

- 📖 中文译文 — Abstract
    
    随着 CMOS 电源电压逐渐逼近晶体管阈值电压、偏置电流降至纳安–微安（nanoampere–microampere）量级，运算放大器与运算跨导放大器（OTA）的性能主要受限于晶体管级的电压裕度（voltage headroom）、跨导效率（transconductance efficiency）、输出电阻与噪声。本文从电路设计的角度、而非应用驱动的角度，综述超低功耗 CMOS 运放设计技术。讨论首先建立由 $g_m/I_D$、弱反型（weak-inversion）工作、本征增益 $g_m r_o$、MOS 器件堆叠、热噪声、闪烁噪声与失配所决定的器件级极限。随后综述成熟的低电压/低功耗拓扑族，包括体驱动（bulk-driven）输入级、浮栅与准浮栅（floating-gate / quasi-floating-gate）输入、基于反相器的电流复用 OTA、折叠式与回收型折叠共源共栅（folded / recycling folded cascode）、AB 类（class-AB）输出级，以及动态失调消除（dynamic offset-cancellation）技术。本文从 JSSC、TCAS、TVLSI 及相关固态电路文献中选取代表性插图作为可视化证据，并将每一张图与特定的晶体管级机制相对应。已报道的流片（silicon）结果证实，这些技术能够在电源电压低至 0.3 V、功耗预算从百微瓦量级直至数十纳瓦量级的条件下维持有效的放大器工作。核心结论是：超低功耗运放设计并非单纯地降低偏置电流，而是有意地重新分配电压裕度、复用偏置电流，使每一纳安电流都能贡献出最大可用的跨导、增益、压摆率（slew rate）或噪声抑制。
    
    **索引词** — 超低功耗运放、OTA、CMOS 模拟设计、$g_m/I_D$、弱反型、体驱动 MOSFET、电流复用、基于反相器的 OTA、回收型折叠共源共栅、AB 类、斩波稳定（chopper stabilization）。
    

## I. Introduction

Operational amplifiers are among the most important analog building blocks because they appear inside active filters, data converters, sensor interfaces, voltage references, low-dropout regulators, and mixed-signal feedback loops. In conventional analog design, an op-amp specification is usually approached by selecting a topology and then spending enough supply voltage and bias current to meet gain, bandwidth, slew-rate, and noise requirements. Ultra-low-power design reverses this freedom. The current budget is often fixed first, sometimes in the nanoampere or low-microampere range, and the supply voltage may be comparable to the MOS threshold voltage. Under these conditions, an op-amp cannot be designed by simply scaling down a classical two-stage or telescopic topology. The designer must instead decide how every transistor terminal, every current branch, and every stacked device contributes to the final small-signal or large-signal performance.

This review therefore treats ultra-low-power op-amp design as a **transistor-level resource-allocation problem**. The first scarce resource is voltage headroom. A conventional gate-driven differential pair requires at least one threshold voltage plus overdrive margins for the input devices, the tail current source, and the load devices. When $V_{DD}$ is reduced to 0.3–0.6 V, this stack can no longer support a useful input common-mode range or output swing. The second scarce resource is bias current. Lowering current reduces power, but it also lowers transconductance, bandwidth, slew rate, and thermal-noise efficiency. The third scarce resource is device area and capacitance. Increasing transistor dimensions improves matching and sometimes noise, but it increases parasitic capacitance and can reduce the transition frequency.

The technical question of ultra-low-power op-amp design can therefore be phrased as follows:

<aside>
🎯

How can a CMOS OTA maintain useful gain, bandwidth, slew rate, and noise performance when both available voltage headroom and available bias current are severely limited?

</aside>

This question is fundamentally circuit-level. Energy-harvesting nodes, biomedical implants, and wearable systems provide motivation, but they are not the core of the review. The core is how low-voltage analog circuits restructure MOS operation: using weak inversion to maximize $g_m/I_D$, using body or floating-gate inputs to avoid threshold-voltage headroom, using inverter-based current reuse to obtain $g_{mn}+g_{mp}$, using folded or recycling cascodes to recover gain and speed, and using class-AB branches to separate quiescent current from transient current.

The contributions of this review are threefold. First, it organizes ultra-low-power op-amp techniques around the scarce transistor-level resources—voltage headroom, transconductance efficiency, output resistance, transient current, and low-frequency precision—rather than around application domains. Second, it ties every reviewed topology to measured silicon results and to specific original figures from JSSC, TCAS, and TVLSI papers, so that every quantitative claim is traceable to its source. Third, it condenses the comparison into mechanism-oriented tables with explicit figures of merit and derives conditional design guidelines, culminating in a coherent qualitative design proposal.

The remainder of this paper is organized as follows. Section II derives the device-level constraints that dominate ultra-low-power design. Section III reviews the main topology families and explains the transistor-level mechanism behind each one. Section IV compares these techniques using configuration and performance-oriented tables. Section V proposes a qualitative design direction combining weak-inversion bulk-driven input devices, a recycling folded-cascode load, and optional class-AB output drive. Section VI concludes.

- 📖 中文译文 — I. Introduction（引言）
    
    运算放大器是最重要的模拟构建模块之一，它出现在有源滤波器、数据转换器、传感器接口、电压基准、低压差稳压器（LDO）以及混合信号反馈环路之中。在传统模拟设计中，运放指标通常的实现方式是：先选定一个拓扑，再投入足够的电源电压与偏置电流以满足增益、带宽、压摆率与噪声要求。超低功耗设计则颠倒了这种自由度：电流预算往往被首先固定——有时被限定在纳安或低微安量级——而电源电压可能与 MOS 阈值电压相当。在这种条件下，运放无法通过简单缩小经典两级或套筒式（telescopic）拓扑来设计，设计者必须转而决定每一个晶体管端子、每一条电流支路、每一个堆叠器件如何贡献于最终的小信号或大信号性能。
    
    **晶体管级资源分配视角**——因此本文把超低功耗运放设计视为一个晶体管级资源分配问题。第一种稀缺资源是电压裕度：经典栅驱动差分对要求输入器件满足栅过驱动、尾电流源保持饱和、负载器件维持输出电阻；当 $V_{DD}$ 降至 0.3–0.6 V 时，该堆叠便无法再支撑有用的输入共模范围或输出摆幅。第二种稀缺资源是偏置电流：降低电流可减小功耗，但同时降低跨导、带宽、压摆率与热噪声效率。第三种稀缺资源是器件面积与电容：增大晶体管尺寸可改善匹配（有时也改善噪声），但会增大寄生电容并可能降低特征频率。
    
    **核心问题**——当可用的电压裕度与可用的偏置电流都受到严格限制时，CMOS OTA 如何维持有用的增益、带宽、压摆率与噪声性能？
    
    这一问题本质上是电路级的。能量收集节点、生物医学植入体与可穿戴系统提供应用动机，但它们并非本综述的核心。核心在于低电压模拟电路如何重构 MOS 工作方式：用弱反型最大化 $g_m/I_D$，用体输入或浮栅输入规避阈值电压裕度，用基于反相器的电流复用获得 $g_{mn}+g_{mp}$，用折叠式或回收型共源共栅恢复增益与速度，用 AB 类支路把静态电流与瞬态电流分离。
    
    本综述的贡献有三。第一，它围绕稀缺的晶体管级资源——电压裕度、跨导效率、输出电阻、瞬态电流与低频精度——来组织技术，而非围绕应用领域。第二，它将每种拓扑都与实测流片结果以及 JSSC、TCAS、TVLSI 论文中的特定原始插图相对应，使每项定量论断都可溯源。第三，它将比较凝练为面向机制、带明确品质因数（FoM）的表格，并推导条件式设计准则，最终归结为一个连贯的定性设计方案。
    
    本文余下部分组织如下：第 II 节推导主导超低功耗设计的器件级约束；第 III 节综述主要拓扑族并解释各自的晶体管级机制；第 IV 节用配置与性能导向的表格比较这些技术；第 V 节提出结合弱反型体驱动输入、回收型折叠共源共栅负载与可选 AB 类输出的定性设计方向；第 VI 节作结。
    

## II. Device-Level Constraints in Ultra-Low-Power OTAs

### A. $g_m/I_D$ as the central low-power design variable

The most compact way to describe low-power analog design is through the transconductance efficiency

$$
\frac{g_m}{I_D}.
$$

This ratio measures how much small-signal transconductance is obtained per unit of drain current. Since the gain-bandwidth product of a single-pole OTA driving a load capacitor is approximately

$$
GBW \approx \frac{G_m}{2\pi C_L},
$$

improving $g_m/I_D$ directly improves the bandwidth available per unit current. In weak inversion,

$$
I_D \approx I_0\frac{W}{L}\exp\left(\frac{V_{GS}-V_{TH}}{nU_T}\right),
\qquad
g_m=\frac{I_D}{nU_T},
$$

and thus

$$
\left.\frac{g_m}{I_D}\right|_{\text{weak inversion}}=\frac{1}{nU_T}.
$$

At room temperature, this gives a typical maximum around 25–30 V$^{-1}$ in bulk CMOS, depending on the subthreshold slope factor $n$. In strong inversion, by contrast,

$$
\frac{g_m}{I_D}\approx\frac{2}{V_{OV}},
$$

so an overdrive of 200 mV gives only about 10 V$^{-1}$. This comparison explains why many ultra-low-power OTAs bias their input devices near weak or moderate inversion.

However, the maximum $g_m/I_D$ point is not automatically the best operating point. As devices move toward weak inversion, the required device width for a fixed current can increase, parasitic capacitances become more significant, and the transition frequency falls. In addition, weak-inversion current depends exponentially on threshold voltage and temperature, so PVT sensitivity and mismatch become more severe. Thus $g_m/I_D$ is not merely a formula for maximizing efficiency; it is a coordinate system for choosing an operating point that balances efficiency, speed, gain, area, and robustness.

<aside>
🖼️

**Insert Fig. 1 here** — Silveira, Flandre & Jespers, **JSSC 1996**, Fig. 1: calculated and measured $g_m/I_D$ versus normalized drain current.

**Caption instruction**: “$g_m/I_D$ as a device-level design coordinate. Weak inversion provides maximum current efficiency, while moderate inversion offers a practical speed–power compromise.”

</aside>

### B. Minimum supply voltage and MOS stack budget

The most visible limitation at ultra-low supply voltage is not current but stack height. A classical NMOS input differential pair with a tail current source and active load requires the input devices to satisfy gate overdrive, the tail device to remain in saturation, and the load devices to maintain output resistance. A simplified lower bound is

$$
V_{DD,\min}\gtrsim V_{TH}+2V_{DSAT}+V_{DSAT,\text{tail}}.
$$

The exact expression depends on topology, input common-mode level, load type, and output swing requirement, but the conclusion is robust: classical gate-driven input stages consume one threshold voltage plus several saturation margins. If $V_{TH}$ is 0.4–0.5 V, a supply of 0.5 V leaves almost no room for traditional stacking.

This is why ultra-low-voltage op-amps often avoid one or more classical assumptions:

- The input signal may be applied to the **bulk** instead of the gate.
- The input signal may be capacitively coupled to a **floating gate** or **quasi-floating gate**.
- The **tail current source** may be removed or replaced by pseudo-differential operation.
- A high-gain telescopic stack may be replaced by a **folded cascode**, **gain-boosted branch**, or **recycling structure**.
- A two-stage amplifier may use compensation carefully so that the required output resistance is achieved without excessive voltage stacking.

The important design mindset is to draw the vertical transistor stack and ask how many overdrive voltages are required from rail to rail. This stack budget often determines feasibility before small-signal gain equations are even considered.

### C. Intrinsic gain, output resistance, and the gain–headroom conflict

The low-frequency gain of an OTA can be approximated as

$$
A_0 \approx G_mR_{out}.
$$

For a single transistor, the intrinsic gain is $g_mr_o$, where $r_o\approx 1/(\lambda I_D)$ under a simple channel-length modulation model. Low current tends to increase $r_o$, which seems beneficial, but scaled CMOS processes often have reduced intrinsic gain because channel-length modulation and short-channel effects degrade $r_o$. Therefore, obtaining high gain at low power is not guaranteed.

Cascode devices are the classical solution for increasing output resistance. A telescopic cascode can provide high gain and speed, but it stacks many devices between supply rails and is poorly suited for sub-0.6-V operation. A folded cascode relaxes the input common-mode constraint by folding signal current into a different branch, but it usually consumes more current than a telescopic structure. Recycling folded cascodes and gain-boosting structures are attempts to recover the benefits of high output resistance without simply increasing current or stack height.

<aside>
⚖️

**Core trade-off**: high gain wants large $R_{out}$ and therefore cascodes; low supply voltage forbids too many stacked devices. Ultra-low-power op-amp design is largely the art of resolving this contradiction.

</aside>

### D. Noise, offset, and matching at low current

Thermal noise in a MOS transconductor scales approximately as

$$
\overline{v_{n,\text{th}}^2}\propto\frac{1}{g_m}.
$$

For a fixed current, using weak inversion improves $g_m$ and therefore helps thermal-noise efficiency. But low-frequency flicker noise and offset do not disappear. Larger input devices can reduce mismatch and flicker noise, but they increase capacitance and area. Chopper stabilization and auto-zeroing can reduce offset and $1/f$ noise, but they introduce switches, clock feedthrough, ripple, aliasing, and additional design complexity.

The key point is that noise must be discussed at the circuit level, not just as an application requirement. For example, a biomedical front-end may motivate a low-frequency noise target, but the transistor-level response is larger input devices, current reuse, chopping, correlated double sampling, or auto-zeroing. The final report should therefore describe noise in terms of $g_m$, device area, switching artifacts, and input-referred quantities.

A useful quantitative bridge between noise and supply current is the noise efficiency factor (NEF),

$$
NEF=V_{ni,\text{rms}}\sqrt{\frac{2I_{tot}}{\pi U_T\cdot 4kT\cdot BW}},
$$

which compares the input-referred noise of an amplifier against that of a single ideal bipolar transistor consuming the same total current. NEF makes the noise–current trade-off explicit and comparable across designs. A classical reference point is the weak-inversion neural amplifier of Harrison and Charles, which achieves 2.2 µVrms input-referred noise over 0.025 Hz–7.2 kHz with only 80 µW of power dissipation by deliberately operating each transistor in the inversion region that minimizes its noise contribution per unit current. Although the NEF originated in biomedical front-ends, the metric itself is application-independent and should accompany any low-noise ultra-low-power comparison.

### E. Large-signal behavior and slew-rate efficiency

Small-signal bandwidth is set by $G_m/C_L$, but large-signal response is limited by available charging or discharging current:

$$
SR=\frac{I_{\max}}{C_L}.
$$

If $I_{\max}$ is equal to the quiescent bias current, an ultra-low-power OTA will slew slowly even if its small-signal bandwidth is acceptable. This motivates class-AB output stages and adaptive-biasing techniques. Their purpose is to maintain small quiescent current at rest while allowing much larger transient current during large input steps. This separates small-signal current efficiency from large-signal drive efficiency.

### F. From device curves to actual OTA sizing

The $g_m/I_D$ methodology is valuable because it connects device operation to actual transistor dimensions. Once a designer chooses $g_m/I_D$, drain current, and channel length, the normalized current curves determine the required $W/L$. Input devices may be placed in weak or moderate inversion for efficiency, while current mirrors may be placed in stronger inversion for matching and robustness. This means an ultra-low-power OTA is not uniformly “weak inversion everywhere”; different devices may deliberately operate in different inversion regions.

<aside>
🖼️

**Insert Fig. 2 here** — Silveira, Flandre & Jespers, **JSSC 1996**, Fig. 2: SOI micropower OTA basic schematic.

**Caption instruction**: “Example of mapping $g_m/I_D$ design choices onto a practical micropower OTA. Input and cascode devices can be assigned different inversion levels according to gain, speed, and matching requirements.”

</aside>

- 📖 中文译文 — II. Device-Level Constraints（器件级约束）
    
    **A.** $g_m/I_D$**：低功耗设计的核心变量**
    
    描述低功耗模拟设计最简洁的方式是跨导效率 $g_m/I_D$，它衡量每单位漏极电流所获得的小信号跨导。由于单极点 OTA 驱动负载电容时增益带宽积近似为 $GBW \approx G_m/(2\pi C_L)$，提高 $g_m/I_D$ 便直接提高单位电流可获得的带宽。在弱反型区，由漏极电流的指数关系可得 $g_m=I_D/(nU_T)$，于是 $g_m/I_D=1/(nU_T)$。室温下体硅 CMOS 中该值典型上限约 25–30 V⁻¹，取决于亚阈值斜率因子 $n$。相比之下强反型区 $g_m/I_D\approx 2/V_{OV}$，200 mV 过驱动仅给出约 10 V⁻¹。这解释了为何许多超低功耗 OTA 把输入器件偏置在弱反型或中反型附近。
    
    然而 $g_m/I_D$ 的最大值点并不自动是最佳工作点。趋向弱反型时，固定电流下所需器件宽度增大、寄生电容更显著、特征频率下降；且弱反型电流对阈值电压与温度呈指数依赖，PVT 敏感性与失配更严重。因此 $g_m/I_D$ 不只是最大化效率的公式，更是一个在效率、速度、增益、面积与鲁棒性之间权衡选点的坐标系。
    
    **B. 最低电源电压与 MOS 堆叠预算**
    
    超低电源电压下最显著的限制不是电流而是堆叠高度。经典 NMOS 输入差分对加尾电流源与有源负载，要求输入器件满足栅过驱动、尾管保持饱和、负载维持输出电阻，简化下界为 $V_{DD,\min}\gtrsim V_{TH}+2V_{DSAT}+V_{DSAT,\text{tail}}$。确切表达式取决于拓扑、输入共模电平、负载类型与输出摆幅，但结论稳健：经典栅驱动输入级要消耗一个阈值电压加若干饱和裕度。若 $V_{TH}$ 为 0.4–0.5 V，0.5 V 电源几乎不留传统堆叠余地。
    
    正因如此，超低电压运放常规避一个或多个经典假设：输入信号施加于体（bulk）而非栅；输入信号经电容耦合至浮栅或准浮栅；尾电流源被去除或被伪差分工作替代；高增益套筒式堆叠被折叠共源共栅、增益自举支路或回收型结构替代；两级放大器谨慎使用补偿，在不过度堆叠电压下获得所需输出电阻。重要的设计思维是：画出纵向晶体管堆叠，自问从一条电源轨到另一条需要多少过驱动电压——这一堆叠预算往往在考虑小信号增益方程之前就决定了方案是否可行。
    
    **C. 本征增益、输出电阻与「增益–裕度」矛盾**
    
    OTA 低频增益近似 $A_0\approx G_mR_{out}$。单个晶体管本征增益为 $g_mr_o$，简单沟道长度调制模型下 $r_o\approx 1/(\lambda I_D)$。低电流倾向增大 $r_o$ 看似有利，但缩小工艺 CMOS 往往本征增益更低，因为沟道长度调制与短沟道效应劣化 $r_o$；因此低功耗下获得高增益并无保证。共源共栅是提高输出电阻的经典方案：套筒式电流高效但摆幅差；折叠共源共栅放宽输入共模约束但耗流更多；回收型折叠共源共栅与增益自举则试图在不单纯增大电流或堆叠高度下恢复高输出电阻的好处。**核心权衡**：高增益需要大 $R_{out}$、从而需要共源共栅，而低电源电压禁止堆叠太多器件——超低功耗运放设计在很大程度上就是化解这一矛盾的艺术。
    
    **D. 低电流下的噪声、失调与匹配**
    
    MOS 跨导器件热噪声近似正比于 $1/g_m$。固定电流下用弱反型提高 $g_m$ 有利于热噪声效率，但低频闪烁噪声与失调不会消失。增大输入器件可减小失配与闪烁噪声却增大电容与面积；斩波稳定与自调零可降低失调与 $1/f$ 噪声，但引入开关、时钟馈通、纹波、混叠与额外复杂度。关键在于噪声须在电路级讨论：生物医学前端可能给出低频噪声目标，但晶体管级的应对是增大输入器件、电流复用、斩波、相关双采样或自调零。一个有用的定量桥梁是噪声效率因子 $NEF=V_{ni,\text{rms}}\sqrt{2I_{tot}/(\pi U_T\cdot 4kT\cdot BW)}$，它把放大器输入折合噪声与消耗相同总电流的理想双极性晶体管相比较，使噪声–电流权衡显式且可跨设计比较。经典参照是 Harrison 与 Charles 的弱反型神经记录放大器：在 0.025 Hz–7.2 kHz 内实现 2.2 µVrms 输入折合噪声、功耗仅 80 µW（做法是让每个晶体管工作在使其单位电流噪声贡献最小的反型区）。NEF 虽源自生物医学前端，但本身与应用无关，应伴随任何低噪声超低功耗比较。
    
    **E. 大信号行为与压摆率效率**
    
    小信号带宽由 $G_m/C_L$ 决定，但大信号响应受可用充放电电流限制：$SR=I_{\max}/C_L$。若 $I_{\max}$ 等于静态偏置电流，则即便小信号带宽尚可，OTA 压摆也很慢。这促成 AB 类输出级与自适应偏置：静止时维持小静态电流，大输入阶跃时允许大得多的瞬态电流，从而把小信号电流效率与大信号驱动效率分离。
    
    **F. 从器件曲线到实际 OTA 尺寸设计**
    
    $g_m/I_D$ 方法的价值在于把器件工作与实际晶体管尺寸联系起来：一旦选定 $g_m/I_D$、漏极电流与沟道长度，归一化电流曲线即确定所需 $W/L$。输入器件可置于弱/中反型以求效率，电流镜可置于更强反型以求匹配与鲁棒性。这意味着超低功耗 OTA 并非一律「处处弱反型」，不同器件可被有意置于不同反型区工作。
    

## III. Topology Review

### A. Bulk-driven input stages: reducing threshold-voltage headroom

Bulk-driven input stages are among the most direct responses to ultra-low supply voltage. Instead of applying the input signal to the gate, the signal is applied to the body terminal while the gate is held at a suitable DC bias. Since the input signal no longer needs to create a gate-source voltage larger than $V_{TH}$, the input common-mode range can extend closer to the rails. This makes bulk-driven OTAs attractive when $V_{DD}$ is comparable to or even below the threshold voltage.

The drawback is body transconductance:

$$
g_{mb}=\eta g_m,
$$

where $\eta$ is typically much less than one. A value around 0.1–0.3 is common, depending on process and bias. This means that a bulk-driven input stage usually has lower transconductance, lower gain-bandwidth, and higher input-referred noise than a gate-driven input stage using the same current. Bulk-driven design therefore trades transconductance efficiency for input headroom.

<aside>
🖼️

**Insert Fig. 3 here** — Chatterjee, Tsividis & Kinget, **JSSC 2005**, Fig. 1: fully differential body-input gain stage with local common-mode feedback.

**Caption instruction**: “A body-input gain stage for 0.5-V analog operation. The input signal enters through the body terminals, while gate and common-mode biases set the operating point.”

</aside>

This figure should be annotated carefully. Mark the body input terminals, gate bias nodes, local common-mode feedback path, and output nodes. The caption should emphasize that the topology solves a headroom problem but introduces a transconductance penalty.

Biasing is equally important. In low-voltage circuits, every MOS terminal must be biased in a safe and useful region. Body terminals cannot be moved arbitrarily because of junction constraints and latch-up concerns. The gate must still establish a correct channel condition, and the common-mode feedback loop must maintain the output operating point over PVT variation.

<aside>
🖼️

**Insert Fig. 4 here** — Chatterjee, Tsividis & Kinget, **JSSC 2005**, Fig. 4: biasing arrangement for 0.5-V operation.

**Caption instruction**: “Low-voltage operation requires not only a new signal path but also a biasing strategy that stabilizes body, gate, and common-mode voltages.”

</aside>

### B. Floating-gate and quasi-floating-gate inputs

Floating-gate and quasi-floating-gate techniques attack the same threshold-voltage problem from a different direction. The input signal is capacitively coupled to a floating or quasi-floating gate, while the DC operating point is set independently. This allows the designer to separate signal swing from DC bias. In principle, a floating-gate input can support a large input common-mode range even when the supply voltage is very small.

The cost is practical implementation complexity. Coupling capacitors consume area, floating nodes require initialization or leakage paths, and quasi-floating-gate pseudo-resistors may introduce very slow time constants or drift. These techniques are therefore useful when rail-to-rail input range is essential, but they complicate layout, startup, and long-term bias stability. In the final report, this subsection should be shorter than the bulk-driven subsection because the proposed direction uses bulk-driven input rather than floating-gate input.

### C. Ultra-low-voltage Miller OTAs: complete low-voltage amplifier examples

A single body-driven input stage is not a complete op-amp. A useful review should also discuss full amplifier structures that include gain stages, output stages, and compensation. Ferreira, Pimenta, and Moreno's ultra-low-voltage Miller OTA is valuable for this reason. It demonstrates that low-voltage operation requires coordinated choices: rail-to-rail input/output swing, low-headroom biasing, and frequency compensation must all be addressed together.

<aside>
🖼️

**Insert Fig. 5 here** — Ferreira, Pimenta & Moreno, **IEEE TCAS-II 2007**, topology schematic of the ultra-low-voltage Miller OTA（最终图号以 IEEE PDF 为准）.

**Caption instruction**: “A complete ultra-low-voltage Miller OTA. The figure should be annotated to show the input signal path, low-headroom biasing, compensation capacitor, and output swing path.”

</aside>

The discussion should focus on “stack surgery.” Which devices are removed, folded, or biased differently compared with a conventional two-stage OTA? Which node sets the dominant pole? How does Miller compensation trade bandwidth for phase margin? How does rail-to-rail swing affect gain and distortion? These questions keep the subsection grounded in transistor-level design rather than application motivation. Experimentally, the fabricated OTA operates from a minimum supply of 600 mV—below the device threshold voltage—while consuming only 550 nW and preserving almost rail-to-rail input and output swing, demonstrating that a complete two-stage Miller amplifier remains feasible at sub-threshold supply levels.

### D. Inverter-based current reuse: obtaining $g_{mn}+g_{mp}$

Inverter-based OTAs are attractive because they use both NMOS and PMOS devices as active signal transistors. Under proper biasing, the effective transconductance can be approximated as

$$
G_{m,\text{eff}}\approx g_{mn}+g_{mp}.
$$

This is a current-reuse principle: the same current path produces transconductance from both complementary devices. Compared with a single NMOS differential pair, the inverter-based structure can provide higher $G_m$ per current, which improves bandwidth and noise efficiency.

Nauta's CMOS transconductor is historically associated with high-frequency continuous-time filters, not necessarily ultra-low-power design. Nevertheless, it is an important figure in this review because it shows the structural origin of inverter-based current reuse and common-mode control with few internal nodes.

<aside>
🖼️

**Insert Fig. 6 here** — Nauta, **IEEE JSSC 1992**, Fig. 2(d): complete six-inverter transconductor.

**Caption instruction**: “Inverter-based transconductor using complementary devices for V-to-I conversion and additional inverters for common-mode control and differential load shaping.”

</aside>

The limitation is bias sensitivity. A CMOS inverter has its highest small-signal gain near the switching point, but that point depends on PMOS / NMOS strength, threshold voltages, temperature, and supply voltage. Modern inverter-based OTAs therefore need common-mode feedback, self-biasing, or replica-bias circuits. In a low-voltage environment, the lack of internal nodes is helpful, but the operating point must be carefully stabilized.

### E. Folded cascode and recycling folded cascode

Cascode structures increase gain by increasing output resistance, but they consume voltage headroom. A telescopic cascode is efficient in current but poor in input/output swing. A folded cascode reduces the input common-mode constraint by steering signal current into a folded branch, but the folded branches consume additional current. In low-power design, this creates a question: can the folded branch current do more signal work?

The recycling folded cascode answers yes. Assaad and Silva-Martínez observed that some devices in a conventional folded cascode are underused from a signal-transconductance perspective. By splitting input drivers and current mirrors, the recycling folded cascode brings previously idle devices into the signal path. The result is higher effective transconductance, gain-bandwidth, and slew rate without a proportional increase in static current.

<aside>
🖼️

**Insert Fig. 7 here** — Assaad & Silva-Martínez, **IEEE JSSC 2009**, Fig. 1: conventional folded cascode and recycling folded cascode comparison.

**Caption instruction**: “Recycling folded cascode concept. Devices that mainly bias the conventional folded cascode are reused to contribute signal transconductance.”

</aside>

<aside>
🖼️

**Insert Fig. 8 here** — Assaad & Silva-Martínez, **IEEE JSSC 2009**, Fig. 2: proposed recycling folded-cascode OTA.

**Caption instruction**: “Implementation of the RFC OTA. Mark the split input paths, mirror gain factor $K$, and output node used for gain and bandwidth enhancement.”

</aside>

A simplified expression for the improvement is

$$
G_{m,\text{RFC}}\approx (1+K)G_{m,\text{FC}},
$$

where $K$ is related to the current mirror ratio or recycling gain. This equation should not be presented as an exact universal law; it is a compact way to explain why recycling increases effective transconductance. In the measured 0.18-µm prototypes, both amplifiers consume the same 0.8-mA total current from a 1.8-V supply and occupy the same area, yet the RFC achieves a 60.9-dB DC gain, a 134.2-MHz gain-bandwidth product, and a 94.1-V/µs average slew rate when driving 5.6 pF—almost twice the bandwidth and more than twice the slew rate of the conventional folded cascode. In the final report, this should be one of the longest topology subsections because it directly supports the proposed design direction.

### F. Class-AB output stages and dynamic current boosting

Weak-inversion and bulk-driven input stages solve small-signal current efficiency and headroom problems, but they do not automatically solve large-signal output drive. If the maximum charging current is limited to the static bias current, the slew rate is small:

$$
SR=\frac{I_{\max}}{C_L}.
$$

Class-AB output stages solve this by allowing the current to increase dynamically during large-signal transitions. The quiescent current remains low, but the transient current can be much larger. This improves slew-rate efficiency without paying the full static-power cost.

A modern compact example is the 0.3-V class-AB bulk-driven OTA by Kulej and Khateb. It is useful in this review because it combines several low-voltage mechanisms: bulk-driven input, subthreshold operation, and class-AB current enhancement. The measured 0.3-V prototype dissipates only 12.6 nW while achieving a 64.7-dB voltage gain, a 2.96-kHz gain-bandwidth product, and a 4.15-V/ms average slew rate at a 30-pF load—figures that translate into some of the highest current-efficiency FoMs reported for ultra-low-voltage OTAs.

<aside>
🖼️

**Insert Fig. 9 here** — Kulej & Khateb, **IEEE TVLSI 2020**, Fig. 1: proposed 0.3-V class-AB bulk-driven OTA.

**Caption instruction**: “A 0.3-V class-AB bulk-driven OTA combining body input and dynamic current boosting. Annotate the input path, bias current path, class-AB branch, and output node.”

</aside>

This subsection should not be written as “this circuit is good for biomedical signals.” It should be written as “this circuit separates input headroom from transient output drive.” That framing keeps the review focused on transistor-level design.

### G. Chopper stabilization and auto-zeroing

Offset and flicker noise are serious in low-frequency, low-current OTAs. Chopper stabilization modulates the input signal to a higher frequency before amplification, so that amplifier offset and $1/f$ noise are shifted away from the signal band and filtered after demodulation. Auto-zeroing samples offset and subtracts it in a later phase. Correlated double sampling is closely related.

These techniques are not “application tricks”; they are switching-circuit methods for reshaping low-frequency nonidealities. Their cost is also circuit-level: switches add charge injection and clock feedthrough; chopping creates ripple; auto-zeroing can alias wideband noise. A good final paragraph should state both sides: dynamic offset cancellation can make ultra-low-power precision amplifiers practical, but it adds clock-dependent artifacts that must be included in the system-level noise budget.

- 📖 中文译文 — III. Topology Review（拓扑综述）
    
    **A. 体驱动输入级：降低阈值电压裕度**——把输入信号施加到衬底（body）而非栅极，栅极保持适当直流偏置。由于输入信号不再需要建立大于 $V_{TH}$ 的栅源电压，输入共模范围可更接近电源轨，因此在 $V_{DD}$ 接近甚至低于阈值电压时颇具吸引力。代价是体跨导 $g_{mb}=\eta g_m$，其中 $\eta$ 通常远小于一（常见 0.1–0.3），故同等电流下体驱动输入级的跨导、增益带宽更低、输入参考噪声更高——以跨导效率换取输入裕度。低压电路中每个 MOS 端都须偏置在安全有效区，衬底因结约束与闩锁不能任意移动，共模反馈环须在 PVT 下维持输出工作点。
    
    **B. 浮栅与准浮栅输入**——从另一方向解决阈值电压问题：信号经电容耦合到（准）浮栅，直流工作点独立设定，从而把信号摆幅与直流偏置分离，原则上在极低电源下也能支持大输入共模范围。代价是实现复杂：耦合电容占面积，浮节点需初始化或漏电通路，准浮栅伪电阻可能引入极慢时间常数或漂移。由于提出的方向用体驱动而非浮栅，本小节在正式稿中应更短。
    
    **C. 超低压 Miller OTA：完整低压放大器范例**——单个体驱动输入级并非完整运放。Ferreira、Pimenta 与 Moreno 的超低压 Miller OTA 表明低压工作需协调多项选择：轨到轨输入/输出摆幅、低裕度偏置与频率补偿须同时解决。讨论应聚焦“堆叠手术”：哪些器件被移除/折叠/改偏，哪个节点设主极点，Miller 补偿如何以带宽换相位裕度，轨到轨摆幅如何影响增益与失真。实测中该 OTA 在低至 600 mV（低于器件阈值电压）下工作、仅耗 550 nW 并保持近轨到轨摆幅，证明完整两级 Miller 放大器在亚阈值电源下仍可行。
    
    **D. 基于反相器的电流复用：获得** $g_{mn}+g_{mp}$——反相器型 OTA 同时把 NMOS 与 PMOS 作为有源信号管，适当偏置下有效跨导约为 $G_{m,\text{eff}}\approx g_{mn}+g_{mp}$，即同一电流路径由互补器件共同产生跨导，相比单 NMOS 差分对提供更高的单位电流 $G_m$，改善带宽与噪声效率。Nauta 跨导器历史上用于高频连续时间滤波，但展示了反相器电流复用与少内部节点共模控制的结构来源。局限是偏置敏感：反相器在翻转点附近增益最高，而该点依赖 PMOS/NMOS 强度、阈值、温度与电源，故现代反相器 OTA 需共模反馈、自偏置或复制偏置。
    
    **E. 折叠共源共栅与回收型折叠共源共栅**——共源共栅以提升输出电阻换增益但消耗电压裕度；伸缩式（telescopic）省电流但输入/输出摆幅差；折叠式经把信号电流导入折叠支路放宽输入共模约束，但折叠支路额外耗流。回收型折叠共源共栅（RFC）的回答是：通过拆分输入驱动与电流镜，把原本闲置的器件引入信号路径，在不成比例增加静态电流下提升有效跨导、增益带宽与压摆率，简化表达 $G_{m,\text{RFC}}\approx(1+K)G_{m,\text{FC}}$（$K$ 与电流镜比/回收增益相关，非精确普适律）。0.18 µm 实测原型中两放大器同耗 0.8 mA、同面积，RFC 在驱动 5.6 pF 时达 60.9 dB 直流增益、134.2 MHz 增益带宽、94.1 V/µs 平均压摆——近两倍带宽、两倍多压摆率。该小节因直接支撑提出的设计方向，应为最长之一。
    
    **F. AB 类输出级与动态电流增强**——弱反型与体驱动输入解决小信号电流效率与裕度，但不自动解决大信号输出驱动；若最大充电电流受限于静态偏置电流，压摆率 $SR=I_{\max}/C_L$ 很小。AB 类输出级让电流在大信号过渡时动态增大，静态电流仍低而瞬态电流可大得多，从而在不付全部静态功耗的前提下改善压摆效率。Kulej 与 Khateb 的 0.3 V AB 类体驱动 OTA 结合体驱动输入、亚阈值工作与 AB 类电流增强：0.3 V 原型仅耗 12.6 nW，在 30 pF 负载下达 64.7 dB 增益、2.96 kHz 增益带宽、4.15 V/ms 平均压摆，是已报道超低压 OTA 中电流效率 FoM 最高者之一。该小节应写成“分离输入裕度与瞬态输出驱动”，而非“适合生物医学信号”。
    
    **G. 斩波稳定与自调零**——失调与闪烁噪声在低频、低电流 OTA 中很严重。斩波把输入信号调制到更高频再放大，使放大器失调与 $1/f$ 噪声移出信号带、解调后滤除；自调零在后续相位采样并扣除失调，相关双采样与之密切相关。它们并非“应用技巧”，而是重塑低频非理想性的开关电路方法，代价同样在电路层：开关引入电荷注入与时钟馈通，斩波产生纹波，自调零可能混叠宽带噪声。好的收尾应两面陈述：动态失调消除能使超低功耗精密放大器可行，但引入须计入系统噪声预算的时钟相关伪象。
    

## IV. Comparison and Discussion

### A. Mechanism-oriented comparison

| Technique | Main transistor-level problem solved | Primary benefit | Main penalty | Best figure evidence |
| --- | --- | --- | --- | --- |
| $g_m/I_D$ methodology | Choosing inversion level under current constraint | Systematic sizing; efficiency–speed trade-off visible | Requires process curves / characterization | Silveira 1996 Fig. 1, Fig. 2 |
| Bulk-driven input | Input common-mode headroom near $V_{TH}$ | Sub-0.5-V / rail-to-rail input possible | $g_{mb}\ll g_m$, noise and gain penalty | Chatterjee 2005 Fig. 1 |
| Floating / quasi-floating gate | DC bias separated from signal input | Large input range at low supply | Capacitor area, leakage, initialization | Ramírez-Angulo / QFG papers |
| Inverter-based current reuse | Low $G_m$ under current constraint | $g_{mn}+g_{mp}$ from same current path | Common-mode and PVT sensitivity | Nauta 1992 Fig. 2(d) |
| Folded cascode | Gain and input swing conflict | High gain with improved input/output range | More current than telescopic cascode | Assaad 2009 baseline FC |
| Recycling folded cascode | Inefficient folded-branch current use | Higher $G_m$, GBW, SR without proportional current | More complex mirrors and matching constraints | Assaad 2009 Fig. 1, Fig. 2 |
| Class-AB output | Low slew rate under low quiescent current | Transient current exceeds static current | Stability and nonlinear settling complexity | Kulej & Khateb 2020 Fig. 1 |
| Chopper / auto-zero | Offset and low-frequency noise | Reduced offset / $1/f$ noise | Ripple, aliasing, clock feedthrough | Enz & Temes 1996 chopper figures |

### B. Performance-table policy

The comparison table in the final report must be conservative. It is better to write N/A than to fabricate a number. For every measured metric, record the original unit and the converted unit before computing the FoM:

$$
FoM_S=\frac{GBW[\text{MHz}]C_L[\text{pF}]}{I_{DD}[\text{mA}]},
\qquad
FoM_L=\frac{SR[\text{V}/\mu\text{s}]C_L[\text{pF}]}{I_{DD}[\text{mA}]}.
$$

| Work | $V_{DD}$ | Power / current | Gain | GBW | SR | $C_L$ | $FoM_S$ / $FoM_L$ | 核对状态 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Chatterjee 2005 body-input OTA [4] | 0.5 V | 110 µW（0.5 V 实测电流 220 µA） | 52 dB | 2.5 MHz | N/A† | 20 pF（每端，原文设计值） | ≈227 / N/A† | ✅ 已对照原文 PDF 全文核实 |
| Chatterjee 2005 gate-input OTA [4] | 0.5 V | 75 µW（≈150 µA） | 62 dB（含自动增益增强） | 10 MHz | N/A† | N/A†（开环测试用 50 kΩ 电阻负载） | N/A† | ✅ 增益 / GBW / 功耗已对照原文 PDF 核实 |
| Ferreira 2007 Miller OTA [5] | 0.6 V（最低实测） | 550 nW（≈0.9 µA） | N/A† | N/A† | N/A† | N/A† | N/A† | ✅ VDD / 功耗 / 近 rail-to-rail 摆幅已核实；其余在付费墙内（2005 会议版为 600 mV / 420 nW，0.35 µm） |
| Assaad 2009 conventional FC（基准）[10] | 1.8 V | 同 RFC（总电流 0.8 mA，等功耗、等面积） | N/A† | ≈½× RFC（论文结论） | ＜½× RFC（论文结论） | 5.6 pF | N/A† | 🔶 倍率与等功耗条件已核实；绝对值见原文 Table |
| Assaad 2009 RFC [10] | 1.8 V | 1.44 mW（0.8 mA） | 60.9 dB | 134.2 MHz | 94.1 V/µs（avg） | 5.6 pF | ≈939 / ≈659 | ✅ 经第三方文献对比表二次核实 |
| Kulej & Khateb 2020 class-AB BD OTA [11] | 0.3 V | 12.6 nW（≈42 nA） | 64.7 dB（@1 Hz） | 2.96 kHz | 4.15 V/ms（avg） | 30 pF（含 PCB 寄生） | ≈2114 / ≈2964 | ✅ 全部已二次核实 |
| Khateb & Kulej 2019 DDA（0.3-V 版本）[16] | 0.3 V | 22 nW（≈73 nA） | ＞60 dB | 1.85 kHz | 1.55 V/ms | 20 pF | ≈505 / ≈423 | ✅ 全部已二次核实 |
| Rodovalho 2020 inverter-based OTA-A [15] | ULV（亚 0.5 V 级，N/A†） | 600 pW | 39 dB | N/A† | — | N/A† | 不参与排名（simulation only） | ✅ 仿真数据已二次核实 |

†：公开渠道（原文 PDF 镜像 / 摘要 / 第三方对比表）未能核实的单项；提交前可对照 IEEE 原文把 N/A† 替换为实数，否则保留 N/A，禁止猜测。FoM 计算明细（保留 source unit → converted unit）：Chatterjee body-input $FoM_S=2.5\times20/0.22\approx227$；Assaad RFC $FoM_S=134.2\times5.6/0.8\approx939$、$FoM_L=94.1\times5.6/0.8\approx659$；Kulej 2020（2.96 kHz = 0.00296 MHz，4.15 V/ms = 0.00415 V/µs，42 nA = 4.2×10⁻⁵ mA）$FoM_S=0.00296\times30/0.000042\approx2114$、$FoM_L=0.00415\times30/0.000042\approx2964$；DDA $FoM_S=0.00185\times20/0.000073\approx505$、$FoM_L=0.00155\times20/0.000073\approx423$。所有「✅」数据于 2026-06-11 二次核实。

<aside>
⚠️

**Unit trap**：nW / µW / mW、kHz / MHz、V/ms / V/µs、pF / nF 都会造成 $10^3$ 到 $10^6$ 级错误。正式表格必须保留 “source unit → converted unit” 的草稿列。

</aside>

### C. Discussion: what trend should the final report claim?

The final discussion should avoid saying that one topology is universally best. Instead, it should make conditional claims:

1. **If minimum supply voltage is the dominant constraint**, bulk-driven and floating-gate inputs are attractive because they relax the input common-mode requirement. Their penalty is reduced effective transconductance and higher noise.
2. **If current efficiency is the dominant constraint**, weak / moderate inversion and inverter-based current reuse are attractive because they maximize useful $G_m$ per current. Their penalty is speed, PVT sensitivity, and common-mode bias complexity.
3. **If gain and bandwidth are insufficient**, folded and recycling folded cascodes are attractive because they improve $R_{out}$ and effective $G_m$. Their penalty is topology complexity and sometimes additional current branches.
4. **If large-signal slew rate is limiting**, class-AB output stages are attractive because they increase transient current without increasing quiescent current. Their penalty is stability and nonlinear settling complexity.
5. **If low-frequency precision is limiting**, chopping and auto-zeroing are attractive because they suppress offset and flicker noise. Their penalty is ripple, switching artifacts, and aliasing.

These conditional claims can now be quantified from the verified rows of the performance table. The 0.3-V, 12.6-nW class-AB bulk-driven OTA of Kulej and Khateb reaches $FoM_S\approx 2114$ MHz·pF/mA and $FoM_L\approx 2964$ V/µs·pF/mA—roughly an order of magnitude above the $\approx 227$ MHz·pF/mA of the 0.5-V, 110-µW body-input OTA—even though its absolute GBW is three orders of magnitude smaller (2.96 kHz versus 2.5 MHz). The 0.3-V differential-difference amplifier confirms the same trend at $FoM_S\approx 505$. At the opposite end of the power range, the recycling folded cascode shows that current reuse also pays at milliwatt levels: at identical 0.8-mA consumption and identical area, the RFC reaches 134.2 MHz and 94.1 V/µs ($FoM_S\approx 939$), almost twice the bandwidth and more than twice the slew rate of its conventional counterpart. The central quantitative message of the review is therefore consistent across five decades of power: weak-inversion, bulk-driven, and current-recycling design does not make amplifiers fast in absolute terms; it makes every nanoampere disproportionately productive, and the application then determines whether kilohertz-class bandwidth is acceptable.

This conditional structure is much more defensible than claiming that a single “ultra-low-power op-amp topology” is optimal.

- 📖 中文译文 — IV. Comparison and Discussion（比较与讨论）
    
    **A. 面向机制的比较**——上表按“所解决的晶体管级问题”而非按应用来组织：$g_m/I_D$ 方法解决电流约束下的反型区选择；体驱动输入解决 $V_{TH}$ 附近的输入共模裕度（代价是 $g_{mb}\ll g_m$ 的噪声与增益损失）；浮栅/准浮栅把直流偏置与信号输入分离（代价是电容面积、漏电与初始化）；基于反相器的电流复用从同一电流路径获得 $g_{mn}+g_{mp}$（代价是共模与 PVT 敏感）；折叠共源共栅以更多电流换取增益与输入摆幅；回收型折叠共源共栅在不成比例增加电流下提高 $G_m$、GBW 与 SR（代价是电流镜更复杂、匹配约束更严）；AB 类输出使瞬态电流超过静态电流（代价是稳定性与非线性建立）；斩波/自调零降低失调与 $1/f$ 噪声（代价是纹波、混叠、时钟馈通）。
    
    **B. 性能表原则**——正式表格必须保守：宁写 N/A 不猜数。每项实测指标在计算 FoM 前都要记录原始单位与转换单位：$FoM_S=GBW[\text{MHz}]C_L[\text{pF}]/I_{DD}[\text{mA}]$，$FoM_L=SR[\text{V}/\mu\text{s}]C_L[\text{pF}]/I_{DD}[\text{mA}]$。表中所有“✅”项均已对照原文 PDF 二次核实，“N/A†”为公开渠道未能核实的单项，提交前需对照 IEEE 原文补齐或保留 N/A。**单位陷阱**：nW/µW/mW、kHz/MHz、V/ms·V/µs、pF/nF 之间的转换会造成 $10^3$–$10^6$ 级误差。
    
    **C. 讨论：最终报告应提出什么趋势？**——最终讨论不应声称某一拓扑普适最优，而应作出条件式论断：① 若最低电源电压是主导约束，体驱动与浮栅输入具吸引力（代价是有效跨导降低、噪声升高）；② 若电流效率是主导约束，弱/中反型与反相器电流复用具吸引力（代价是速度、PVT、共模偏置复杂度）；③ 若增益/带宽不足，折叠与回收型折叠共源共栅具吸引力（代价是拓扑复杂度与额外电流支路）；④ 若大信号压摆率受限，AB 类输出级具吸引力（代价是稳定性与非线性建立）；⑤ 若低频精度受限，斩波与自调零具吸引力（代价是纹波、开关伪象、混叠）。这些条件式论断可由性能表中已核实的行量化：Kulej 与 Khateb 的 0.3 V、12.6 nW AB 类体驱动 OTA 达到 $FoM_S\approx 2114$、$FoM_L\approx 2964$，约为 0.5 V、110 µW 体输入 OTA（$FoM_S\approx 227$）的一个数量级，尽管其绝对 GBW 小三个数量级（2.96 kHz 对 2.5 MHz）；0.3 V 差分差分放大器以 $FoM_S\approx 505$ 印证同一趋势；另一极端，回收型折叠共源共栅在等电流 0.8 mA、等面积下达 134.2 MHz、94.1 V/µs（$FoM_S\approx 939$），说明电流复用在毫瓦级同样获益。本综述的核心定量信息因此跨越五个数量级的功耗都一致：弱反型、体驱动与电流回收设计并不使放大器在绝对意义上变快，而是使每一纳安电流产出不成比例地高，应用再决定千赫兹级带宽是否可接受。这种条件式结构远比声称“某单一超低功耗拓扑最优”更站得住脚。
    

## V. Proposed Direction: Bulk-Driven Input + Recycling Folded-Cascode Load + Class-AB Output

The proposed direction should be framed as a qualitative circuit hypothesis, not a simulated design. A coherent candidate OTA core is:

<aside>
🧩

**Weak-inversion-biased bulk-driven input pair + recycling folded-cascode load + optional class-AB output stage.**

</aside>

The motivation is that each block addresses a different transistor-level bottleneck. The bulk-driven input pair addresses voltage headroom by moving the signal path away from the gate-threshold constraint. Weak-inversion biasing maximizes current efficiency:

$$
\frac{g_{mb}}{I_D}=\eta\frac{g_m}{I_D}.
$$

Because $\eta$ is smaller than one, the body-driven input alone would suffer a transconductance penalty. The recycling folded-cascode load is therefore used to recover effective transconductance:

$$
G_{m,\text{eff}}\approx(1+K)g_{mb}.
$$

This expression should be presented as an intuitive first-order argument: recycling does not change the fact that the input is body-driven, but it can reuse branch currents so that the effective signal transconductance is larger than the raw $g_{mb}$ of the input pair.

The optional class-AB output stage addresses a different issue. Even if the small-signal $G_m$ is acceptable, a nanopower OTA can have poor slew rate because output current is limited. A class-AB branch allows the transient charging current to exceed the quiescent bias current:

$$
I_{\max,\text{transient}}\gg I_Q,
\qquad
SR=\frac{I_{\max,\text{transient}}}{C_L}.
$$

Thus the proposed architecture separates three problems: input headroom, small-signal transconductance efficiency, and large-signal drive.

### A. What the proposed section may claim

- It may claim that the combination is **topologically coherent**.
- It may claim that each block addresses a distinct bottleneck.
- It may claim that the idea is supported qualitatively by existing literature on bulk-driven inputs, RFC, and class-AB OTAs.

### B. What the proposed section must not claim

- Do not claim a specific gain, GBW, SR, power, or FoM without simulation.
- Do not claim novelty if a similar combination already exists.
- Do not claim that the design is optimized for biomedical or energy harvesting; those are possible use cases, not the technical proof.

### C. Final-report paragraph

A possible future direction is to combine a weak-inversion bulk-driven input stage with a recycling folded-cascode load and an optional class-AB output stage. The bulk-driven input relaxes the input common-mode and threshold-voltage constraint under sub-0.5-V supplies, while weak-inversion biasing maximizes current efficiency. Since the body transconductance $g_{mb}$ is only a fraction of the gate transconductance $g_m$, a recycling folded-cascode load can be used to recover effective $G_m$ by reusing branch currents that would otherwise contribute weakly to the signal path. Finally, a class-AB output stage can improve large-signal slew rate without increasing quiescent current. This combination directly targets the three transistor-level bottlenecks of ultra-low-power OTAs: voltage headroom, transconductance efficiency, and transient drive capability.

- 📖 中文译文 — V. Proposed Direction（提出的设计方向）
    
    提出的方向应被表述为定性的电路假设，而非经过仿真的设计。一个连贯的候选 OTA 核心是：**弱反型偏置的体驱动输入对 + 回收型折叠共源共栅负载 + 可选 AB 类输出级。** 其动机在于每个模块各自解决一个不同的晶体管级瓶颈。体驱动输入对通过把信号路径移离栅–阈值约束来解决电压裕度问题；弱反型偏置最大化电流效率：$g_{mb}/I_D=\eta\,(g_m/I_D)$。由于 $\eta$ 小于一，单凭体驱动输入会承受跨导损失，因此用回收型折叠共源共栅负载恢复有效跨导：$G_{m,\text{eff}}\approx(1+K)g_{mb}$。该式应作为直观的一阶论证：回收并不改变输入为体驱动这一事实，但能复用支路电流，使有效信号跨导大于输入对原始的 $g_{mb}$。可选 AB 类输出级解决另一问题：即便小信号 $G_m$ 尚可，纳瓦功耗 OTA 仍可能因输出电流受限而压摆很差；AB 类支路允许瞬态充电电流超过静态偏置电流：$I_{\max,\text{transient}}\gg I_Q$，$SR=I_{\max,\text{transient}}/C_L$。于是该架构把三个问题分离：输入裕度、小信号跨导效率与大信号驱动。
    
    **A. 该节可以声称什么**：可声称该组合在拓扑上是连贯的；可声称每个模块解决一个独立瓶颈；可声称该思路在体驱动输入、RFC 与 AB 类 OTA 的既有文献中得到定性支持。
    
    **B. 该节不得声称什么**：不得在无仿真情况下声称具体增益、GBW、SR、功耗或 FoM；若类似组合已存在则不得声称新颖性；不得声称该设计为生物医学或能量收集而优化——那些只是可能的用例，而非技术证明。
    
    **C. 最终报告段落**：一个可能的未来方向是把弱反型体驱动输入级与回收型折叠共源共栅负载、可选 AB 类输出级相结合。体驱动输入在亚 0.5 V 电源下放宽输入共模与阈值电压约束，弱反型偏置最大化电流效率；由于体跨导 $g_{mb}$ 仅为栅跨导 $g_m$ 的一部分，可用回收型折叠共源共栅负载复用本会对信号路径贡献微弱的支路电流来恢复有效 $G_m$；最后，AB 类输出级可在不增加静态电流的前提下提升大信号压摆率。该组合直接针对超低功耗 OTA 的三个晶体管级瓶颈：电压裕度、跨导效率与瞬态驱动能力。
    

## VI. Conclusion

Ultra-low-power op-amp design is best understood as transistor-level resource allocation under simultaneous voltage and current constraints. The $g_m/I_D$ methodology provides the device-level language for choosing inversion region and sizing transistors. Bulk-driven and floating-gate inputs address the threshold-voltage headroom problem, but they pay a transconductance and noise penalty. Inverter-based OTAs reuse current by adding NMOS and PMOS transconductances in the same current path, but require careful common-mode stabilization. Folded cascodes recover output resistance at the cost of current, while recycling folded cascodes improve effective transconductance and slew rate by placing otherwise idle devices into the signal path. Class-AB stages separate quiescent current from transient drive, and chopping / auto-zeroing suppress low-frequency nonidealities at the cost of switching artifacts.

The strongest final report should therefore foreground circuit schematics, transistor operating regions, current paths, node voltages, and measured topology-level trade-offs. Applications should remain in the introduction as motivation, but the technical body should be organized around headroom, $g_m/I_D$, $g_{mb}$, $g_mr_o$, current reuse, slew-rate current, and noise-shaping mechanisms.

- 📖 中文译文 — VI. Conclusion（结论）
    
    超低功耗运放设计最好理解为在电压与电流同时受限下的晶体管级资源分配。$g_m/I_D$ 方法提供了选择反型区与设计晶体管尺寸的器件级语言。体驱动与浮栅输入解决阈值电压裕度问题，但付出跨导与噪声代价；基于反相器的 OTA 通过在同一电流路径叠加 NMOS 与 PMOS 跨导来复用电流，但需谨慎的共模稳定；折叠共源共栅以电流换取输出电阻，回收型折叠共源共栅则通过把原本闲置的器件置入信号路径来提升有效跨导与压摆率；AB 类级把静态电流与瞬态驱动分离，斩波/自调零以开关伪象为代价抑制低频非理想性。
    
    因此最强的最终报告应突出电路原理图、晶体管工作区、电流路径、节点电压与实测的拓扑级权衡。应用应留在引言中作为动机，而技术主体应围绕电压裕度、$g_m/I_D$、$g_{mb}$、$g_mr_o$、电流复用、压摆电流与噪声整形机制来组织。
    

## References

[1] E. Vittoz and J. Fellrath, “CMOS analog integrated circuits based on weak inversion operation,” *IEEE J. Solid-State Circuits*, vol. 12, no. 3, pp. 224–231, Jun. 1977.

[2] F. Silveira, D. Flandre, and P. G. A. Jespers, “A $g_m/I_D$ based methodology for the design of CMOS analog circuits and its application to the synthesis of a silicon-on-insulator micropower OTA,” *IEEE J. Solid-State Circuits*, vol. 31, no. 9, pp. 1314–1319, Sep. 1996.

[3] B. J. Blalock, P. E. Allen, and G. A. Rincon-Mora, “Designing 1-V op amps using standard digital CMOS technology,” *IEEE Trans. Circuits Syst. II*, vol. 45, no. 7, pp. 769–780, Jul. 1998.

[4] S. Chatterjee, Y. Tsividis, and P. R. Kinget, “0.5-V analog circuit techniques and their application in OTA and filter design,” *IEEE J. Solid-State Circuits*, vol. 40, no. 12, pp. 2373–2387, Dec. 2005.

[5] L. H. C. Ferreira, T. C. Pimenta, and R. L. Moreno, “An ultra-low-voltage ultra-low-power CMOS Miller OTA with rail-to-rail input/output swing,” *IEEE Trans. Circuits Syst. II: Express Briefs*, vol. 54, no. 10, pp. 843–847, Oct. 2007.

[6] B. Nauta, “A CMOS transconductance-C filter technique for very high frequencies,” *IEEE J. Solid-State Circuits*, vol. 27, no. 2, pp. 142–153, Feb. 1992.

[7] J. Ramírez-Angulo, A. J. López-Martín, R. G. Carvajal, and F. M. Chavero, “Very low-voltage analog signal processing based on quasi-floating gate transistors,” *IEEE J. Solid-State Circuits*, vol. 39, no. 3, pp. 434–442, Mar. 2004.

[8] R. G. Carvajal *et al.*, “The flipped voltage follower: A useful cell for low-voltage low-power circuit design,” *IEEE Trans. Circuits Syst. I*, vol. 52, no. 7, pp. 1276–1291, Jul. 2005.

[9] C. C. Enz and G. C. Temes, “Circuit techniques for reducing the effects of op-amp imperfections: autozeroing, correlated double sampling, and chopper stabilization,” *Proc. IEEE*, vol. 84, no. 11, pp. 1584–1614, Nov. 1996.

[10] R. S. Assaad and J. Silva-Martínez, “The recycling folded cascode: A general enhancement of the folded cascode amplifier,” *IEEE J. Solid-State Circuits*, vol. 44, no. 9, pp. 2535–2542, Sep. 2009.

[11] T. Kulej and F. Khateb, “A compact 0.3-V class AB bulk-driven OTA,” *IEEE Trans. Very Large Scale Integr. (VLSI) Syst.*, vol. 28, no. 1, pp. 224–232, Jan. 2020.

[12] C. C. Enz, F. Krummenacher, and E. A. Vittoz, “An analytical MOS transistor model valid in all regions of operation and dedicated to low-voltage and low-current applications,” *Analog Integr. Circuits Signal Process.*, vol. 8, pp. 83–114, 1995.

[13] R. R. Harrison and C. Charles, “A low-power low-noise CMOS amplifier for neural recording applications,” *IEEE J. Solid-State Circuits*, vol. 38, no. 6, pp. 958–965, Jun. 2003.

[14] T. Denison *et al.*, “A 2 µW 100 nV/√Hz chopper-stabilized instrumentation amplifier for chronic measurement of neural field potentials,” *IEEE J. Solid-State Circuits*, vol. 42, no. 12, pp. 2934–2945, Dec. 2007.

[15] L. H. Rodovalho, O. Aiello, and C. R. Rodrigues, “Ultra-low-voltage inverter-based operational transconductance amplifiers with voltage gain enhancement by improved composite transistors,” *Electronics*, vol. 9, no. 9, art. 1410, Sep. 2020.

[16] F. Khateb and T. Kulej, “Design and implementation of a 0.3-V differential difference amplifier,” *IEEE Trans. Circuits Syst. I: Regular Papers*, vol. 66, no. 2, pp. 513–523, Feb. 2019.（卷期页码提交前对照 IEEE 页面再核一次）

## Submission checklist

- [ ]  正文是否已经按 5 页密度展开，而不是只列提纲？
- [ ]  Introduction 是否只用 application 做 motivation，没有让应用抢走主线？
- [ ]  Section II 是否完整覆盖 $g_m/I_D$、headroom、gain、noise、slew、sizing？
- [ ]  Section III 是否按 topology mechanism 展开，而不是只列名字？
- [ ]  每张图是否明确到 paper + original figure number + 标注重点？
- [ ]  Comparison 是否按 mechanism / trade-off 比较？
- [ ]  FoM 表是否只填已核 PDF 的数据？
- [ ]  Proposed direction 是否没有虚构仿真结果？