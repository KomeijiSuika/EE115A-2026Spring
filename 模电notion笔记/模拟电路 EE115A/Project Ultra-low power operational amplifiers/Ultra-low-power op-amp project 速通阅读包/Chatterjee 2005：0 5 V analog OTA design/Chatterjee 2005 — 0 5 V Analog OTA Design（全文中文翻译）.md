# Chatterjee 2005 — 0.5 V Analog/OTA Design（全文中文翻译）

**0.5-V Analog Circuit Techniques and Their Application in OTA and Filter Design**

*0.5 V 模拟电路技术及其在 OTA 与滤波器设计中的应用*

Shouri Chatterjee, Yannis Tsividis, Peter Kinget — IEEE Journal of Solid-State Circuits, Vol. 40, No. 12, pp. 2373–2387, December 2005.

---

## 摘要（Abstract）

本文提出了一组设计技术，使模拟电路能够在极低的供电电压（low supply voltage，低至 0.5 V）下工作。文中以运算跨导放大器（operational transconductance amplifier, OTA）与滤波器设计为载体来引入这些技术。作者设计了两种 OTA：一种采用体输入（body input），另一种采用栅输入（gate input）。为在工艺、电压和温度（process, voltage, and temperature, PVT）变化下维持共模电压（common-mode voltage）并获得最大信号摆幅（signal swing），文中提出了相应的偏置策略。

原型芯片在 0.18 µm CMOS 工艺上、采用标准 0.5 V 器件制造。体输入 OTA 实测直流增益（DC gain）为 52 dB，增益带宽积（gain-bandwidth, GBW）为 2.5 MHz，功耗 110 µW。栅输入 OTA 实测直流增益为 62 dB（带自动增益增强 automatic gain-enhancement），增益带宽积为 10 MHz，功耗 75 µW。

文中还给出了有源 RC（active-RC）滤波器的设计技术，并提出和建模了弱反型 MOS 变容器（weak-inversion MOS varactor）。这些变容器与 0.5 V 栅输入 OTA 一起，被用来设计一个全集成的、135 kHz 的五阶椭圆低通滤波器（fifth-order elliptic low-pass filter）。该原型芯片同样制造于 0.18 µm CMOS 工艺、供电 0.5 V，并在片上集成了用于调谐的锁相环（phase-locked loop, PLL）。该 1 mm² 芯片实测动态范围（dynamic range）为 57 dB，从 0.5 V 电源汲取 2.2 mA 电流。

**索引词（Index Terms）**：有源滤波器（active filters）、模拟集成电路（analog integrated circuits）、体偏置（body bias）、低电压（low voltage）、运算放大器（operational amplifiers）、变容器（varactors）。

---

## 一、引言与动机（Introduction and Motivation）

工艺特征尺寸（feature size）的持续缩小要求供电电压按比例下调，以维持器件可靠性（reliability）。与此同时，又必须保持相对较高的阈值电压（threshold voltage, $V_{TH}$），以限制晶体管的关断电流（OFF current）[1]。这两个因素给未来标准数字 CMOS 工艺中的模拟电路设计带来了重大挑战。

有时会用电荷泵（charge pump）技术将片上电压抬升到电源之上，但这在某些情况下会带来可靠性问题。然而，存在若干技术，可在不进行电压抬升的前提下实现真正的低电压工作，包括：在可行处使用弱反型（weakly inverted）器件、把体节点（body node）用作第四个控制或信号端，以及采用避免晶体管堆叠（stacked transistors）的电路拓扑。

为研究这些技术的协同使用，作者设计了两个 OTA 和一个自动调谐的五阶低通滤波器，并将其工作电压一直压到与标准器件阈值电压相等的水平——在本工作中即 0.5 V [2]、[3]。第二节讨论两种使用标准 CMOS 工艺的真正低电压 OTA 设计技术：先用 MOS 器件的体节点施加信号、而通过栅极完成偏置；再反过来用栅节点施加信号、而通过体完成偏置。作者用 0.18 µm CMOS 工艺上的原型并配以实测结果来对比这两种技术。第三节提出并分析了一种用于有源变容器-R（varactor-R）滤波器的低频弱反型 0.5 V MOS 变容器。第四节展示了以栅输入 OTA 和变容器为构建模块的滤波器设计，并给出对不同芯片、不同供电电压、不同温度的实验结果。

需要指出：未来的 CMOS 工艺将提供速度更快、且相对于 0.18 µm 器件 $V_{TH}$ 更低的晶体管。因此，本文所演示的技术在这些工艺可用后有望获得更高的性能 [1]。

---

## 二、0.5 V 全差分运算跨导放大器（Fully Differential OTAs）

对于任何需要高带宽或高采样率的高性能应用，MOS 器件都被偏置在强反型（strong inversion），即 $V_{GS}-V_{TH}>0$。只要 $V_{DS}\ge V_{DS,\text{sat}}$，器件即可充当跨导器或电流源。在强反型边缘以及弱、中反型区，$V_{DS,\text{sat}}$ 的一个良好估计约为 0.15 V [4]。因此，无论器件 $V_{TH}$ 为何，在任何工作区都需要维持至少 0.15 V 的 $V_{DS}$。由此，共源（common-source）结构具备在 0.5 V 供电下工作的潜力。

体源结（body-source junction）的正向偏置（forward biasing）此前已用于低电压数字电路 [5]–[8]，本文将其用于降低晶体管的 $V_{TH}$。作者通常施加约 250 mV 的正向偏置，可使 $V_{TH}$ 降低约 50 mV。在 0.5 V 工作的语境下，正向偏置结的风险很小：因为即便把整个电源都用作正向偏置，寄生双极器件（parasitic bipolar devices）也不会被激活——前提是电源瞬态过压（transient overvoltage）被充分控制住。

### A. 体输入 OTA（Body-Input OTA）

<aside>
🖼

**图 1（占位）**：带局部共模反馈（local common-mode feedback）的全差分体输入增益级。请从原文 PDF 拷贝此图。

</aside>

具有单端输出的体输入运算放大器此前已被研究，用于低至 0.7 V 的低电压应用 [9]–[13]。为使 MOS 晶体管工作在中反型附近，可在栅极施加较大的偏置电压，而把信号施加到器件的体（body）上。图 1 给出了一个极低电压的基本增益级：两个输入接到 pMOS 管 $M_1$、$M_2$ 的体上，其体跨导（body transconductance）$g_{mb}$ 提供输入跨导。当输入共模电压为 $V_{DD}/2$（0.25 V）时，由此产生的小幅体源正向偏置会降低 $V_{TH}$、进一步提高反型程度。为获得相对较大的体跨导 $g_{mb}$，更倾向于让器件工作在弱-中反型边界附近。

$M_1$、$M_2$ 由作为电流源的 nMOS 管负载。$M_1$、$M_2$ 的体输入构成一个交叉耦合对（cross-coupled pair），向输出引入负电阻（negative resistance），从而提升差分直流增益。电阻检测输出共模电压，并反馈到 pMOS 器件的栅极以实现共模抑制（common-mode rejection）。通过一个小电流在电阻上产生压降，在 0.25 V 的输出共模电压与 0.1 V 的栅偏置之间建立起直流电平移位（DC level shift）。

下文中，$g_{mb}$ 为输入管的体跨导，$g_m$ 为栅跨导，$g_{ds}$ 为漏源电导。差分直流增益为：

<aside>
📐

**公式 (1)**（原文为图片式公式，OCR 未能提取，需对照原文 PDF 核对）：体输入增益级的差分直流增益，主要正比于输入管体跨导 $g_{mb}$，并因交叉耦合对引入的负电阻而被提升。

</aside>

共模直流增益为：

<aside>
📐

**公式 (2)**（OCR 未能提取，需核对）：共模直流增益。由于 $g_m$ 大于 $g_{mb}$，且该增益本身小于 1，共模信号被强烈抑制。

</aside>

在作者的设计中，每级差分增益约 24 dB，每级共模增益约 −14 dB。输入折合白噪声谱密度（input-referred white noise spectral density，单位 $\text{V}^2/\text{Hz}$）为：

<aside>
📐

**公式 (3)**（OCR 未能提取，需核对）：输入折合白噪声谱密度。由于体跨导 $g_{mb}$ 相比栅跨导 $g_m$ 较小，该噪声在本质上较大。

</aside>

<aside>
🖼

**图 2（占位）**：带米勒补偿（Miller compensation）的两级全差分体输入 OTA。请从原文 PDF 拷贝此图。

</aside>

通过级联两个相同的增益块，得到如图 2 所示的两级 OTA。放大器通过加入带串联电阻的米勒补偿电容来稳定：串联电阻把右半平面零点（right half-plane zero）移到左半平面。其频率响应的增益带宽积约为 $\text{GBW}\approx g_{mb1}/C_C$，其中 $g_{mb1}$ 为第一级输入管的体跨导；第二极点频率约在 $g_{mb2}/C_L$，其中 $g_{mb2}$ 为第二级输入管的体跨导，$C_L$ 为负载电容。在含多级 OTA 的应用中，输入 pMOS 的 n 阱到 p 衬底寄生电容会对前一级 OTA 形成负载；不过该电容会成为总补偿电容的一部分，而总补偿电容由补偿电容本身主导。

在每个输出端 20 pF 负载、40 µA 偏置电流下，器件被设计为 2.5 MHz 的增益带宽积。整个设计采用 0.5 µm 沟道长度，以限制反向短沟道效应（reverse short-channel effect）的影响。相关器件被保守地设计为带来 9 dB 的增益改善。该设计标称功耗为 100 µW。晶体管的宽、长以及无源元件取值见表 I。如图 2 所示，40 µA 与 4 µA 的电流分别从“biasn”和“biasi”节点注入，电流镜将其反射到相应支路。

<aside>
📊

**表 I（占位）**：体输入 OTA 的晶体管尺寸与元件取值。具体数值请从原文 PDF 拷贝。

</aside>

一块总面积 0.13 mm × 0.2 mm 的原型在 0.18 µm 混合信号 CMOS 工艺上制造，使用了高电阻率多晶硅电阻（high-resistivity poly resistors）和 MIM 电容。芯片显微照片见图 3(a)（整块电路上做了大面积金属填充 metal fill），对应版图见图 3(b)。

<aside>
🖼

**图 3（占位）**：(a) 体输入 OTA 芯片显微照片；(b) 体输入 OTA 版图。请从原文 PDF 拷贝。

</aside>

### B. 栅输入 OTA（Gate-Input OTA）

本工作开发的第二种 OTA 采用栅输入，而用体端进行偏置。为访问 nMOS 器件的体端，大量使用了三阱（triple-well）器件。

<aside>
🖼

**图 4（占位）**：栅输入 OTA 共模电压的设定。请从原文 PDF 拷贝。

</aside>

图 4 给出了一种围绕 OTA 的电阻反馈（resistive feedback）放大器结构。为获得最大的输出信号摆幅，输出共模电平 $V_{o,cm}$ 通常设为 $V_{DD}/2$；由于每一级的输入都由相似的一级驱动，输入共模电平 $V_{i,cm}$ 也为 $V_{DD}/2$。为开启 OTA 的输入器件（假定为 nMOS 输入），需要把虚地共模电平 $V_x$ 设得尽可能高。如文献 [14]、[15] 所述，只要满足关于开环直流增益 $A_0$ 的条件，这样的共模电平可由图 4 中的电阻 $R_{CM}$ 维持，而不影响电路的整体增益。对于 0.5 V 的 $V_{DD}$、0.25 V 的 $V_{DD}/2$，若要把 $V_x$ 推到 0.4 V，需要相应取值的 $R_{CM}$。这样便能使用栅输入低电压 OTA：信号共模电压保持在 $V_{DD}/2$，同时让 OTA 输入器件处于中反型。

也可用电流源代替电阻 [16]。把直流电流注入虚地节点，会在反馈电阻 $R_F$ 与输入电阻 $R_I$ 上维持直流压降；调节该电流即可把 $V_x$ 设到期望的直流值。但这种方法会把电流源的 $1/f$ 噪声（以及白噪声）直接注入 OTA 的输入端。

<aside>
🖼

**图 5（占位）**：栅输入 OTA 的电路演进。(a) 基本结构；(b) 栅输入 OTA 单级原理图。请从原文 PDF 拷贝。

</aside>

在图 5(a) 的基本差分放大器中，输入差分对 $M_1$、$M_2$ 与有源负载 $M_3$、$M_4$ 一起放大差分输入电压。电阻通过有源负载提供共模反馈。一个电平移位电流在相关电阻上产生 0.15 V 压降，把某节点电压维持在 0.1 V 左右，从而使 $M_3$、$M_4$ 工作在中反型。$M_3$、$M_4$ 的体接到各自的栅以进一步降低其 $V_{TH}$。

$M_1$、$M_2$ 的跨导与总跨导之比决定了共模增益。在所用工艺中，pMOS 跨导相对于 nMOS 跨导不够大，无法获得足够低的共模增益。因此，如图 5 所示，通过相应器件加入了一条共模前馈对消通路（common-mode feed-forward cancellation path）[17]、[18]。在这些器件中，栅与体相连以在体源结上形成正向偏置，把它们推向中反型。整体直流小信号差分增益为：

<aside>
📐

**公式 (4)**（OCR 未能提取，需核对）：栅输入 OTA 的整体直流小信号差分增益。

</aside>

共模小信号增益为：

<aside>
📐

**公式 (5)**（OCR 未能提取，需核对）：共模小信号增益。

</aside>

在本设计中，作者通过取值使共模前馈通路提供约 −6 dB 的抑制。每级差分增益约 25 dB，每级共模增益约 −10 dB。输入折合白噪声谱密度（单位 $\text{V}^2/\text{Hz}$）为：

<aside>
📐

**公式 (6)**（OCR 未能提取，需核对）：输入折合白噪声谱密度。与体输入 OTA 的 (3) 式相比，由于分母中出现 $g_m$，此处噪声明显更小。

</aside>

<aside>
🖼

**图 6（占位）**：带米勒补偿的两级全差分栅输入 OTA。请从原文 PDF 拷贝。

</aside>

为获得大于 50 dB 的直流增益，级联两级增益级构成图 6 所示的两级 OTA。把第一级相关电阻上的直流压降增大到 0.3 V，使第一级输出共模电压约为 0.4 V，从而保证第二级输入器件的正确偏置；第二级 0.15 V 的直流压降把输出共模电平设为 0.25 V。第一级中加入一个交叉耦合对作为负电导（negative conductance），进一步增强差分增益、降低输出电导；作为额外好处，共模增益也被进一步降低。该交叉耦合对的体由片上自动控制的偏置电压 $V_{neg}$ 设定。第二级中加入一个类似的对：其体端可在输出端的低共模电压下工作，用其体跨导提供负电导，而其栅跨导与输入跨导并联。

OTA 通过跨接第二级的米勒电容稳定。增益带宽积约为 $\text{GBW}\approx g_m/C_C$，放大器第二极点约在 $g_m/C_L$，其中 $C_L$ 为单端负载电容。串联电阻把补偿电容引入的零点从右半平面移到左半平面。该 OTA 在 0.18 µm 三阱 CMOS 工艺中设计，采用 0.36 µm 沟道长度以限制反向短沟道效应的影响。晶体管的长、宽及其他元件取值见表 II。

<aside>
📊

**表 II（占位）**：栅输入 OTA（图 6）的元件尺寸与取值。具体数值请从原文 PDF 拷贝。

</aside>

### C. 栅输入 OTA 的片上偏置电路（On-Chip Biasing）

本节讨论如何在工艺、供电电压和温度变化下可靠地偏置栅输入 OTA（类似技术也可用于偏置体输入 OTA）。为正确工作，图 6 中的栅输入 OTA 需要三个偏置电压：$V_{body}$，用于偏置若干器件对的体；$V_{ls}$，用于偏置电平移位电流源、并在相关电阻上维持一个与工艺-温度无关的压降；以及 $V_{neg}$，用于偏置第一级交叉耦合对的体并设定直流增益。

**1) 误差放大器（Error Amplifier）**：在偏置环路中，作者大量使用复制电路（replica circuits）配合有源反馈环路，这需要一个误差放大器。作者使用一个精心定尺寸的反相器作为反相式误差放大器：该反相器把输入电压与其自身的开关阈值电压（switching threshold voltage）相比较并放大其差值。在 0.5 V 供电下，放大器的器件工作在弱反型，由此带来的较慢频率响应在直流偏置环路中可以容忍。

<aside>
🖼

**图 7（占位）**：(a) 将开关阈值电压固定到 $V_{DD}/2$ 的误差放大器偏置环路；(b) 已偏置误差放大器的直流输入-输出特性。请从原文 PDF 拷贝。

</aside>

文献 [7]、[8]、[19]、[20] 曾用自适应体偏置（adaptive body bias）技术优化数字电路关键路径的延迟。本工作中，如图 7(a) 所示，通过含三个相同级的负反馈结构，控制 nMOS 器件的体来调整误差放大器的开关电压。其原理如下：若 “ErrorAmpA” 的开关阈值电压小于 $V_{DD}/2$，则 “ErrorAmpB” 的输入电压下降、“ErrorAmpC” 的输出电压下降、nMOS 器件的体偏置被减小，从而开关阈值电压升高；反之，当开关阈值电压大于 $V_{DD}/2$ 时，反馈作出反应、降低开关阈值电压。该反馈环路把开关阈值电压精确设到标称的 0.25 V。环路稳定性通过一个带零点消除串联电阻的补偿电容建立。此后，每个由该电路偏置的复制误差放大器都成为一个把自身输入与 0.25 V 比较的反相式误差放大器。其直流输入-输出特性见图 7(b)：在 1 pF 负载下，放大器增益带宽为 20 kHz，电流消耗为 2 µA。

**2) 生成固定电平移位（Generating a Fixed Level Shift）**：图 5(b) 中的偏置电流在相关电阻上产生电平移位。该电平移位对于把 pMOS 器件维持在中反型、并维持 OTA 第一级输出的正确共模电平至关重要。作者用单个 nMOS 器件设计电流源；为提高该器件的反型程度，偏置电压同时通过栅与体施加。

<aside>
🖼

**图 8（占位）**：电平移位电流源的偏置。请从原文 PDF 拷贝。

</aside>

如图 8 所示，在偏置电路中使用该电流源的一个复制。器件汲取一个电流，在电阻上产生压降；生成 $V_{ls}$ 使得某节点电压为 0.25 V，对应 100 kΩ 电阻上的压降为 0.15 V。这个良好定义的 IR 压降被按比例缩放并转移到图 6 的电平移位器中。一个补偿电容用于稳定该反馈环路。

**3) 设定 OTA 输出直流共模电压（Setting the OTA Output DC Common-Mode Voltage）**：图 5(b) 中的偏置电压调整 nMOS 器件相对于 pMOS 器件的偏置电平，从而控制 OTA 的直流输出共模电压。该偏置电压越大，放大器的直流输出共模电压越低。

<aside>
🖼

**图 9（占位）**：偏置输入 nMOS 器件的体以设定 OTA 输出共模电压。请从原文 PDF 拷贝。

</aside>

为生成该偏置电压，作者用图 9 的电路在输入共模电压为 0.4 V 时检测放大器复制的输出共模。在本设计中，标称工作时该 0.4 V 由外部提供。输出共模电压与 0.25 V 比较，差值经放大后通过负反馈进行控制。一个补偿电容用于稳定该反馈环路。注意：这个低带宽偏置电路仅设定输出共模电压的直流值，并针对工艺、温度和供电电压的变化进行调整；共模信号的抑制是在每个 OTA 的每一级内局部完成的。

**4) 增益增强（Gain Enhancement）**：图 6 中的交叉耦合器件对为放大器第一级提供负电阻负载，从而增强其增益。负电阻的大小可通过改变这两个器件的 $V_{TH}$（即体跨导）经其体来控制。若负电导恰好抵消输出电导，OTA 增益理论上为无穷大；负电阻较小时增益为正，较大时增益看似为负。

<aside>
🖼

**图 10（占位）**：随该控制偏置变化的 OTA 直流转移特性；当负电导过大时出现滞回（hysteresis），以箭头标出。请从原文 PDF 拷贝。

</aside>

更深入的分析得到图 10 的 OTA 直流转移特性：随着该控制偏置增大，增益先增大、直至变为无穷大，随后放大器出现滞回。带滞回的 OTA 表现得像一个施密特触发器（Schmitt trigger）。

<aside>
🖼

**图 11（占位）**：增益增强；用于改善 OTA 增益的通用施密特触发器振荡器及偏置技术。请从原文 PDF 拷贝。

</aside>

为感知这一行为的临界点，作者设计了一个基于 OTA 的施密特触发器振荡器（图 11），其振荡频率由相应公式给出（其中涉及上升沿/下降沿触发电压之差以及高低输出之差）。

<aside>
📐

**公式（施密特触发器振荡频率）**（OCR 未能提取，需核对）：振荡频率与触发电压差、输出高低电平差有关。

</aside>

当存在振荡时，XNOR 门的输出下降；振荡幅度大时，相应偏置被减小；当振荡停止时，相应偏置被增大。实际中，确定下来的负电阻仍会在施密特触发器振荡器中引起振荡，但该振荡快到 XNOR 门来不及响应。该环路保证负电阻余量很小，因而振荡幅度很小、振荡频率很高。由此得到的电压经一个略小于但接近 1 的增益转换后施加到所有 OTA，从而保证每个 OTA 级的小信号增益为正。

**5) 启动（Start-Up）**：上电时，误差放大器偏置电路自行启动。一旦其稳定，电平移位电流源偏置电路随之稳定并提供其输出。该偏置需要外部电压（0.4 V）和误差放大器偏置。施密特触发器振荡器在此之后启动，使用已稳定的两个电压。偏置中只存在前向依赖关系，保证了平滑启动。

### D. 体输入与栅输入 OTA 的测量（Measurements）

<aside>
🖼

**图 12（占位）**：体输入与栅输入 OTA 的通用测量装置。开环测量时开关闭合，闭环测量时开关断开。虚线电阻仅在测试栅输入 OTA 时需要。探头输入电容 $C_p$ 为差分 10 pF。请从原文 PDF 拷贝。

</aside>

通用测量装置见图 12。使用变压器把单端信号转换为共模电压 0.25 V 的差分信号，作为输入施加到电路。使用 Tektronix P6046 差分探头检测 OTA 输出，使用 HP3585A 频谱分析仪进行大量测量。各参数的实测值与仿真值列于表 III 的相应列中。

<aside>
📊

**表 III（占位）**：体输入与栅输入 OTA 的关键参数。具体数值请从原文 PDF 拷贝。

</aside>

<aside>
🖼

**图 13（占位）**：体输入 OTA 开环的仿真与实测。请从原文 PDF 拷贝。

</aside>

**1) 体输入 OTA**：体输入 OTA 的开环频率响应在 0.5 V 供电下测量，并与仿真结果对比（图 13）。当电压高于 0.5 V 时，偏置电流不变，把输入共模电压调整为比电源低 0.25 V。如预期，OTA 在增益带宽不变、电流消耗略高的情况下工作。供电从 0.5 V 增大到 1 V 时，电流消耗从 220 µA 增大到 245 µA，说明放大器很鲁棒、在很宽的供电范围内维持性能。当供电降到 0.5 V 以下时，作者调整偏置电流以获得最大速度：在 0.4 V 时，增益带宽积为 840 kHz，电流消耗 66 µA，最大输出摆幅 320 mV 差分峰峰值。在这些低供电电压下，增益带宽主要受限于在如此低栅源电压下偏置电流源所能提供的有限电流。

<aside>
🖼

**图 14（占位）**：栅输入 OTA 在不同 $V_{neg}$ 下的实测开环频率传递函数。请从原文 PDF 拷贝。

</aside>

**2) 栅输入 OTA**：栅输入 OTA 及其配套偏置电路被用于第四节讨论的滤波器中。同一芯片上还包含一个独立的栅输入 OTA 作为测试结构。仿真中，该设计的直流小信号增益为 55 dB，标称单位增益带宽为 15 MHz，相位裕度为 60°。OTA 的实测开环频率响应见图 14（对应不同的 $V_{neg}$）。负电阻偏置电路自动设定 $V_{neg}$，使 OTA 直流增益为 62 dB。这些测量在 50 kΩ 负载电阻下完成；负载更小时，直流增益预期会高得多。

### E. 两种 OTA 设计技术的讨论与比较（Discussion and Comparisons）

<aside>
📊

**表 IV（占位）**：与其他低电压 OTA 设计的比较。具体数值请从原文 PDF 拷贝。

</aside>

作者在表 IV 中把两种 OTA 与其他低电压 CMOS OTA 设计的实测结果进行比较。在增益带宽和功耗方面，栅输入 OTA 优于体输入 OTA，这主要是因为它利用了输入器件的 $g_m$ 而非 $g_{mb}$。栅输入 OTA 的输入折合噪声也更小。然而，当栅输入 OTA 用于反馈时，需要额外的电阻来设定输入共模电压（如图 4），这会把电阻白噪声直接加到 OTA 输入端，并在反馈配置中显著贡献电路的总噪声。体输入 OTA 具有很大的输入共模范围，对从 0 到 0.5 V 的所有输入共模电压都有功能；其共模增益在低输入共模电压下会增大。而栅输入 OTA 则被设计为仅在输入共模电压大于 0.4 V 时具有大增益。

---

## 三、用于可调谐积分器的弱反型 MOS 变容器（Weak-Inversion MOS Varactors）

可调谐积分器（tunable integrator）是可调谐滤波器的基本构建模块。传统上，可调谐性可通过以下方式实现：用 MOS 器件代替电阻的 MOSFET-C 结构；用开关及电阻、电容组进行离散调谐；跨导-C（transconductance-C）技术；或用变容器代替电容的变容器-R（varactor-R）结构。MOSFET-C 结构通常要求 MOS 器件处于强反型，这在超低供电要求下可能不可行；在信号通路中使用开关则需要电压抬升来开启开关，带来可靠性顾虑；而在极低供电电压下设计高线性度的可调谐跨导器非常困难。因此，作者转而研究变容器-R 技术。可变电容与电阻、低电压 OTA 一起，使我们能够在 0.5 V 下构建有源 RC 电路。

<aside>
🖼

**图 15（占位）**：(a) 由体（B）调谐的栅（G）到源/漏（S）电容；(b) 不同 $V_{GS}$ 下归一化电容随 $V_{body}$ 的变化（$N=3.5\times10^{17}\ \text{cm}^{-3}$，$V_{DS}=1\ \text{V}$，数值需核对）。请从原文 PDF 拷贝。

</aside>

为此，作者提出使用弱反型 MOS 电容作为三端变容器：电容位于栅与漏源组合之间（记为 $C_{gs}$），调谐电压施加于体 [23]，如图 15(a) 所示。在强反型和积累区，该电容为氧化层电容 $C_{ox}$；在耗尽区，由于不存在反型层，本征电容为零；从弱反型经中反型到强反型，本征电容从零变化到 $C_{ox}$。改变体电压会改变器件阈值电压、也改变器件反型程度，从而改变电容，器件由此表现为一个三端变容器。

作者在 0.18 µm 三阱 CMOS 工艺上制造了一个长沟道 nMOS 器件（15 指，每指宽 100 µm、长 20 µm），体通过 p 阱访问、漏与源短接。该器件在不同偏置电压下的归一化栅源电容见图 15(b)：在所用工艺中，当 $V_{GS}$ 工作点为 0.15 V 时，电容在 $V_{body}$ 从 0.1 到 0.4 V 范围内于 0 到 0.1$C_{ox}$ 之间变化。

<aside>
🖼

**图 16（占位）**：0.5 V 可调谐阻尼积分器（tunable damped integrator）；各节点直流共模电压以粗体标出。请从原文 PDF 拷贝。

</aside>

图 16 给出用所提变容器和一个栅输入 OTA 实现的低电压可调谐阻尼积分器。栅输入 OTA 输入端的电阻在积分器输入、输出共模电压为 0.25 V 时，把虚地处的共模电平维持在 0.4 V，且不影响积分器的传递函数。在所用工艺中，当 $V_{GS}$ 为 0.15 V 时，所提变容器的电容密度较低，约 0.3 fF/µm²。这一局限通过在可变电容旁并联一个固定电容来弥补：可使用任何可用的固定电容类型；为满足原型的面积限制，作者使用原生（native，零 $V_{TH}$）器件作为固定电容——在上述偏置条件下它们工作在强反型，密度为 8 fF/µm²。这使得整体电容密度达到 1.3 fF/µm²。一个额外好处是复合电容具有更好的品质因数（quality factor）。若固定电容是线性的，复合变容器的线性度会得到改善。在本实现中，固定电容取所需标称电容的 80%，变容器在其范围中心处取标称电容的 20%，由此实现 20% 的调谐范围。

<aside>
🖼

**图 17（占位）**：(a) 电容的集总模型，显示有效电容 $C_{eff}$ 与串联电阻 $R_s$；(b) 实测有效电容 $C_{eff}$、(c) 串联电阻 $R_s$ 随 $V_{body}$（不同 $V_{GS}$）的变化，测量频率 1 MHz；(d) 电容的概念模型，含单位长度的栅-源/漏电容、体-源/漏电容、漏-源电阻。请从原文 PDF 拷贝。

</aside>

变容器用 Agilent 4284A LCR 仪测量与表征。有效电容与串联电阻的定义见图 17(a)。有效电容作为 $V_{body}$（不同 $V_{GS}$）的函数见图 17(b)。由于缺乏强反型层，电容存在显著的串联电阻——可归因于分布效应（distributed effects），它降低了电容的品质因数。1 MHz 下与电容串联的电阻见图 17(c)，用于预测该电阻的分布模型见图 17(d)。准确预测串联电阻的技术见文献 [24]，更多变容器建模细节见附录。

---

## 四、0.5 V 五阶有源 RC 滤波器（Fifth-Order Active-RC Filter）

### A. 滤波器拓扑（Filter Topology）

为演示所提超低电压设计技术的能力与协同性，作者设计了一个截止频率 135 kHz 的五阶低通椭圆滤波器。为满足最小灵敏度要求，采用蛙跳（leap-frog）拓扑。把文献 [25] 的设计频率缩放到 135 kHz，使所有 OTA 输出端的信号幅度最大值处于同一水平。各栅输入 OTA 的尺寸根据负载需求缩放——通过并联多个单元、并把各 OTA 的所有内部节点相互连接来实现，从而让全部五级具有可比的相位与失真性能。为获得准确的传递特性，OTA 在直到 280 kHz（滤波器第二个零点）处都应具有可观的开环增益；所提放大器在 280 kHz 处的最坏情况增益为 20 dB，已足够。滤波器电阻和电容经过缩放，使 OTA 与电阻在通带内积分的总噪声贡献相等。

<aside>
🖼

**图 18（占位）**：低电压五阶低通椭圆滤波器。请从原文 PDF 拷贝。

</aside>

<aside>
📊

**表 V（占位）**：滤波器电阻与电容取值。具体数值请从原文 PDF 拷贝。

</aside>

原理图见图 18，元件取值见表 V。仿真得到的整体动态范围——即产生 1% 总谐波失真（total harmonic distortion, THD）时的输入有效值与输入折合噪声之比——为 57 dB。变容器对电路失真贡献显著：若用理想线性电容代替变容器，仿真动态范围为 69 dB；而若用理想 OTA 代替低电压 OTA，仿真动态范围仅为 58 dB。

### B. 基于 PLL 的片上自动频率调谐环路（PLL-Based Automatic Frequency Tuning）

<aside>
🖼

**图 19（占位）**：三级低电压振荡器原理图。请从原文 PDF 拷贝。

</aside>

如图 19 所示，作者用可调谐积分器构建了一个超低电压压控振荡器（voltage-controlled oscillator, VCO），其电阻与电容与滤波器中的相匹配。振荡频率 $f_{osc}$ 选在接近滤波器第二个零点（280 kHz）处。相比双积分器振荡器，作者选用三级振荡器：OTA 具有足够的增益带宽，可在 $f_{osc}$ 处提供每级 60° 的相位滞后以及大于 1 的所需增益，从而可靠维持振荡。振荡器的标称振荡频率由相应公式给出，且仅在满足某条件时才可能起振。

<aside>
📐

**公式（VCO 振荡频率）**（OCR 未能提取，需核对）：振荡频率由匹配的电阻与电容决定，仅在满足相应增益条件时起振。

</aside>

在图 19 中，相关电阻分别为 427 kΩ、207 kΩ、93 kΩ，电容为 2.3 pF。围绕该 VCO 用一个 XOR 门作为鉴相器构建锁相环（PLL）。在 0.5 V 供电下，XOR 门中的器件处于弱反型，但对 280 kHz 仍足够快。该鉴相器把 VCO 频率与外部参考时钟比较，并控制 VCO 中电容的体电压，直至 PLL 锁定。相同的电容体电压也施加到滤波器上。由于滤波器的变容器与电阻和 VCO 中的相匹配，滤波器的转折频率由此被调谐。为便于表征，用分立元件在片外构建了一阶 PLL 环路滤波器。

### C. 原型芯片（Prototype Chip）

<aside>
🖼

**图 20（占位）**：(a) 全芯片框图；(b) 芯片照片（die photograph）。请从原文 PDF 拷贝。

</aside>

如图 20(a) 所示，包含五阶滤波器、VCO、偏置电路和鉴相器的全芯片在 0.18 µm CMOS 工艺上制造，利用了三阱 nMOS 器件、高电阻率电阻和 MIM 电容。总有源芯片面积为 1 mm × 1 mm，芯片照片见图 20(b)。偏置仅使用一个 0.4 V 的外部电压参考，并使用一个 280 kHz 的外部频率参考来调谐滤波器。

### D. 表征结果（Characterization Results）

**1) 测试装置**：用中心抽头变压器把单端信号转换为共模电压 0.25 V 的差分信号。用 Tektronix P6046 差分探头测量滤波器差分输出，用 HP3585A 频谱分析仪进行大量测量。结果见图 21–23 和表 VI。

<aside>
🖼

**图 21（占位）**：滤波器频率响应。(a) 仿真与实测；(b) 有/无增益增强。请从原文 PDF 拷贝。

</aside>

**2) 频率响应**：实测滤波器频率响应与仿真高度吻合（图 21(a)），通带纹波为 1 dB。图 21(b) 给出有/无自动增益增强时的频率响应：有增益增强时，通带响应改善 1 dB。在 10 kHz 共模音下，实测共模抑制比（CMRR）为 65 dB；在电源上加 10 kHz 音时，实测电源抑制比（PSRR）为 43 dB。若为所有偏置电阻使用单独的稳压电源（如图 4），可获得更好的 PSRR。

<aside>
🖼

**图 22（占位）**：0.5 V 供电下滤波器噪声的仿真与实测（输出噪声）。请从原文 PDF 拷贝。

</aside>

**3) 噪声**：实测输出噪声与仿真对比见图 22。在低频处，输出噪声由 $1/f$ 噪声主导；通带内的小段平坦区是白噪声的结果；约 135 kHz 处的峰起源于滤波器拓扑与传递函数。观测到的噪声转折点约为 40 kHz。OTA 在较低频率贡献噪声，而滤波器电阻在噪声转折频率之外占主导。

**4) 失真与调谐范围内的表征**：对 20 kHz 的带内音（其谐波也在带内）和 100 kHz 的带内音（其谐波在带外）测量谐波失真。最坏情况下，在输入差分有效值 50 mV 处观测到 1% 的 THD。互调（intermodulation）测量针对两组：带内输入音 40 与 60 kHz（其互调产物在 20 与 80 kHz），以及阻带两个峰处的带外输入音（标称 180 与 460 kHz）。相应的输入差分有效值 IIP3 分别为 3 与 5 dBV。本文将动态范围定义为：产生 1%（最坏情况）THD 时的输入差分有效电压，与 1 到 150 kHz 输入积分噪声 [25] 之比。观测到的动态范围为 56.6 dB，与仿真的 57 dB 高度一致。

<aside>
🖼

**图 23（占位）**：(a) 不同供电电压下实测滤波器频率响应（PLL 启用）；(b) 不同调谐电压下实测频率响应（PLL 关闭）；(c) 20 片芯片的实测频率响应（PLL 启用）。请从原文 PDF 拷贝。

</aside>

为观察变容器对滤波器失真的贡献，关闭 PLL，并对施加于变容器体的不同调谐电压测量失真。当调谐电压为 0.0 V 时，电容非线性被消除（此时电容仅由固定的强反型晶体管与变容器的交叠电容并联构成），观测到的动态范围为 61 dB；当调谐电压为 0.5 V 时，动态范围降至 55 dB。PLL 关闭时不同调谐电压下的滤波器频率响应见图 23(b)。图 23(b) 中滤波器陷波（notch）的深度取决于 OTA 增益和电容的品质因数。对五阶滤波器，为得到串联电阻的影响，作者通过在器件上串联一个计算所得的电阻（见附录）以及使用分段模型（segmented model，见附录）来仿真变容器。两种情况下，第一个陷波在体调谐电压 0.0 和 0.5 V 处的仿真深度分别为 53 和 46 dB，而实测为 50 和 43 dB。两种情况下 3 dB 的差异可归因于 OTA 增益带宽积的差异。

<aside>
📊

**表 VI（占位）**：启用 PLL 时不同供电电压下的性能。具体数值请从原文 PDF 拷贝。

</aside>

**5) 不同供电电压下的性能**：在不同供电电压下评估了滤波器性能，结果汇总于表 VI。在这些不同供电电压下，PLL 在标称条件下均锁定；除滤波器调谐范围外，所有测量都在 PLL 启用下进行。不同供电电压下的滤波器传递函数见图 23(a)。

**6) 不同芯片间的性能**：对一批 20 片芯片评估标称性能。滤波器 3 dB 截止频率的标准差为 1.3%。不同芯片的实测特性见图 23(c)，彼此高度吻合。0.5 V 下的平均电流消耗为 2.2 mA，标准差为 0.1 mA。

<aside>
📊

**表 VII（占位）**：不同温度下的实测总电流消耗。具体数值请从原文 PDF 拷贝。

</aside>

**7) 温度范围内的性能**：用 Delta Design 3900 温箱在不同温度下表征滤波器。芯片在 −5 ℃ 到 85 ℃ 之间完全可用，未观察到漏电问题。3 dB 截止频率在该范围内变化 8%。这一偏差略大，经诊断是 VCO 特性随温度的系统性漂移所致——该漂移由相对幅度变化引起，而在所用的 0.5 V 供电下这种变化很显著；可通过附加的振荡器幅度稳定电路来修正。不同温度下的标称电流消耗见表 VII。器件阈值电压随温度升高而下降，而偏置电路建立与温度无关的固定直流偏置电压，因此电流消耗随温度升高而增大。

---

## 五、结论（Conclusion）

本文提出了用于真正超低电压模拟 CMOS 电路的设计技术。设计中不使用电压抬升，电路中所有节点都处于电源轨之内。设计标称针对 0.5 V 供电——即等于单个器件的阈值电压。文中给出了两种 OTA 设计技术，以及在工艺、电压和温度变化下控制共模电压电平、最大化信号摆幅的自动偏置技术。文中还分析并建模了一种弱反型 MOS 变容器；这种变容器与栅输入 OTA 一起，被用于一个全集成的可调谐五阶有源 RC 低通滤波器和一个匹配的 VCO。该 VCO 嵌入在用于调谐滤波器频率响应的 PLL 中。所用的 OTA 与变容器都是通用的模拟电路构建模块，可用于类似供电电压水平下的多种其他设计。

---

## 附录：变容器串联电阻的建模技术（Appendix）

作者定义栅源跨导纳（transadmittance）$Y$ 为 $Y=i_g/v_{sd}$，其中 $v_{sd}$ 是施加到源/漏端的电压相量，$i_g$ 是离开栅端的电流相量，且其他端电压保持恒定。如文献 [24] 所讨论的分布分析技术表明：从图 17(d) 的模型出发、令分段数趋于无穷，所得有效跨导纳 $Y_{eff}$（不含交叠电容）由下式给出：

<aside>
📐

**公式 (7)**（OCR 未能提取，需核对）：有效跨导纳 $Y_{eff}$，由单位长度的栅-源/漏电容、体-源/漏电容和源漏电阻经分布模型导出（含双曲函数式的分布解）。

</aside>

其中相关参数分别为集总的栅-源/漏电容、体-源/漏电容和源-漏电阻。本征跨导纳现在可与交叠电容合并，再变换以提取有效串联电阻与有效串联电容。有效栅源电容由下式给出：

<aside>
📐

**公式 (8)**（OCR 未能提取，需核对）：有效栅源电容。

</aside>

有效串联电阻由下式给出：

<aside>
📐

**公式 (9)**（OCR 未能提取，需核对）：有效串联电阻。

</aside>

若在给定 MOS 模型下求得任一偏置点处的上述各量，便可用 (7)、(8)、(9) 式在所关心频率处提取有效串联电阻与电容。该分布效应也可用标准电路仿真器、通过沟道分段（channel segmentation）[4] 快速仿真，如图 17(d) 所示：长沟道 MOS 器件可视为若干短本征 MOS 器件的串联连接，相邻器件的漏与源相互连接。若把长度为 $L$ 的器件分成若干段，每段沟道长度为 $L/n$；分段数越大，越接近真正的分布模型。可以证明，对于分段模型，段数应使得在所关心频率处满足相应条件。注意：当频率使该量为纯虚数时，所预测的串联电阻为零。

---

## 致谢（Acknowledgment）

作者感谢哥伦比亚大学的 T. Musah 和 A. Balankutty 协助测量，感谢 R. Melville 提供差分探头，感谢哥伦比亚大学的 Shepard 教授提供 Agilent 4284A LCR 仪。

---

## 参考文献（References）

[1] The International Technology Roadmap for Semiconductors (2004 Edition) [Online]. Available: [http://public.itrs.net](http://public.itrs.net)

[2] S. Chatterjee, Y. Tsividis, and P. Kinget, “A 0.5-V bulk-input fully differential operational transconductance amplifier,” Proc. 30th ESSCIRC, pp. 147–150, Sep. 2004.

[3] ——, “A 0.5 V filter with PLL-based tuning in 0.18-µm CMOS technology,” in IEEE Int. Solid-State Circuits Conf. Dig. Tech. Papers, 2005, pp. 506–507.

[4] Y. Tsividis, Operation and Modeling of the MOS Transistor, 2nd ed. New York: Oxford Univ. Press, 1999.

[5] M.-J. Chen, J.-S. Ho, T.-H. Huang, C.-H. Yang, Y.-N. Jou, and T. Wu, “Back-gate forward bias method for low-voltage CMOS digital circuits,” IEEE Trans. Electron Devices, vol. 43, pp. 904–910, 1996.

[6] S. Narendra, et al., “Ultra-low voltage circuits and processor in 180 nm to 90 nm technologies with a swapped-body biasing technique,” in IEEE Int. Solid-State Circuits Conf. Dig. Tech. Papers, Feb. 2004, pp. 156–157.

[7] J. W. Tschanz, et al., “Adaptive body bias for reducing impacts of die-to-die and within-die parameter variations on microprocessor frequency and leakage,” IEEE J. Solid-State Circuits, vol. 37, no. 11, pp. 1396–1402, Nov. 2002.

[8] V. R. Kaenel, M. D. Pardoen, E. Dijkstra, and E. A. Vittoz, “Automatic adjustment of threshold and supply voltages for minimum power consumption in CMOS digital circuits,” in Proc. IEEE Symp. Low Power Electronics, 1994, pp. 78–79.

[9] A. Guzinski, M. Bialko, and J. Matheau, “Body driven differential amplifier for application in continuous-time active-C filter,” in Proc. ECCD, 1987, pp. 315–319.

[10] B. Blalock, P. Allen, and G. Rincon-Mora, “Designing 1-V op-amps using standard digital CMOS technology,” IEEE Trans. Circuits Syst. II, vol. 45, no. 7, pp. 769–780, Jul. 1998.

[11] K. Lasanen, E. Raisanen-Ruotsalainen, and J. Kostamovaara, “A 1-V 5 µW CMOS-opamp with bulk-driven input transistors,” in 43rd IEEE Midwest Symp. Circuits and Systems, 2000, pp. 1038–1041.

[12] T. Lehmann and M. Cassia, “1-V power supply CMOS cascode amplifier,” IEEE J. Solid-State Circuits, vol. 36, no. 7, pp. 1082–1086, Jul. 2001.

[13] T. Stockstad and H. Yoshizawa, “A 0.9-V 0.5-µA rail-to-rail CMOS operational amplifier,” IEEE J. Solid-State Circuits, vol. 37, pp. 286–292, 2002.

[14] S. Karthikeyan, S. Mortezapour, A. Tammineedi, and E. Lee, “Low voltage analog circuit design based on biased inverting opamp configuration,” IEEE Trans. Circuits Syst. II, vol. 47, no. 3, pp. 176–184, Mar. 2000.

[15] K. Bult, “Analog design in deep sub-micron CMOS,” in Proc. ESSCIRC, Sep. 2000, pp. 11–17.

[16] H. Huang and E. K. F. Lee, “Design of low-voltage CMOS continuous-time filter with on-chip automatic tuning,” IEEE J. Solid-State Circuits, vol. 36, no. 8, pp. 1168–1177, Aug. 2001.

[17] A. Baschirotto, F. Rezzi, and R. Castello, “Low-voltage balanced transconductor with high input common-mode rejection,” Electron. Lett., vol. 30, no. 20, pp. 1669–1671, Sep. 1994.

[18] F. Rezzi, A. Baschirotto, and R. Castello, “A 3 V 12–55 MHz BiCMOS pseudo-differential continuous-time filter,” IEEE Trans. Circuits Syst. I, vol. 42, no. 11, pp. 896–903, Nov. 1995.

[19] T. Kobayashi and T. Sakurai, “Self-adjusting threshold-voltage scheme (SATS) for low-voltage high-speed operation,” in Proc. IEEE Custom Integrated Circuits Conf., May 1994, pp. 271–274.

[20] J. T. Kao, M. Miyazaki, and A. P. Chandrakasan, “A 175-mV multiply-accumulate unit using an adaptive supply voltage and body bias architecture,” IEEE J. Solid-State Circuits, vol. 37, no. 11, pp. 1545–1554, Nov. 2002.

[21] G. Ferri and W. Sansen, “A 1.3 V op/amp in standard 0.7 µm CMOS with constant $g_m$ and rail-to-rail input and output stages,” in IEEE Int. Solid-State Circuits Conf. Dig. Tech. Papers, vol. 478, 1996, pp. 382–383.

[22] V. Peluso, P. Vancorenland, A. M. Marques, M. Steyaert, and W. Sansen, “A 900-mV low-power A/D converter with 77-dB dynamic range,” IEEE J. Solid-State Circuits, vol. 33, no. 12, pp. 1887–1897, Dec. 1998.

[23] S. Chatterjee, T. Musah, Y. Tsividis, and P. Kinget, “Weak inversion MOS varactors for 0.5 V analog integrated filters,” in Proc. Symp. VLSI Circuits, Jun. 2005, pp. 272–275.

[24] M. S. Ghausi and J. J. Kelly, Introduction to Distributed-Parameter Networks. New York: Holt, Rinehart and Winston, 1968.

[25] M. Banu and Y. Tsividis, “An elliptic continuous-time CMOS filter with on-chip automatic tuning,” IEEE J. Solid-State Circuits, vol. SC-20, no. 12, pp. 1114–1121, Dec. 1985.

---

## 作者简介（Author Biographies）

**Shouri Chatterjee**（S'01）2000 年获印度理工学院马德拉斯分校电气工程学士学位，2002 年获哥伦比亚大学电气工程硕士学位，本文发表时正在哥伦比亚大学攻读博士学位。研究兴趣为低电压、低功耗电路与 delta-sigma 调制器。曾获 2002 年哥伦比亚大学电气工程最佳毕业硕士生 Armstrong Memorial 奖。

**Yannis Tsividis**（Fellow, IEEE）分别于 1972、1973、1976 年获明尼苏达大学学士学位以及加州大学伯克利分校硕士、博士学位。本文发表时为哥伦比亚大学 Charles Batchelor 电气工程讲座教授。曾任职于 Motorola Semiconductor 和 AT&T Bell Laboratories，并曾在伯克利、MIT、雅典国立技术大学任教。著有《Operation and Modeling of the MOS Transistor》（第 2 版，Oxford University Press, 2003）。曾多次获 IEEE 最佳论文奖及教学奖。

**Peter Kinget**（Senior Member, IEEE）1990、1996 年分别获比利时鲁汶大学电气与机械工程学位及电气工程博士学位。曾获比利时国家科学基金会奖学金，并先后任职于 Bell Laboratories（Lucent Technologies）以及 Broadcom、CeLight、MultiLink 等公司。2002 年夏加入哥伦比亚大学电气工程系。研究兴趣为模拟与射频集成电路及信号处理。自 2003 年起任 JSSC 副编辑。