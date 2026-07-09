# Core papers — 24h 速通代读笔记

<aside>
📚

**用途：** 这是读者 24h 速通用的核心论文代读页。这里不追求完整 literature review，只抓写 report 必须用的观点、数字和可疑点。

</aside>

## 0. arXiv 情况说明

<aside>
⚠️

Analog IC / JSSC / TCAS 经典论文很多**没有 arXiv 版本**。所以这页对每篇都标出：

- **arXiv**：如果找得到就放。
- **Official / open link**：没有 arXiv 时，用 IEEE / DOI / Semantic Scholar / MDPI / open PDF 替代。
</aside>

## 1. Chatterjee, Tsividis & Kinget 2005 — 0.5 V analog / OTA

- **Title**：0.5-V analog circuit techniques and their application in OTA and filter design
- **arXiv**：未找到可靠 arXiv 版本
- **Official**：[https://ieeexplore.ieee.org/document/1546214/](https://ieeexplore.ieee.org/document/1546214/)
- **Semantic Scholar**：[https://www.semanticscholar.org/paper/0.5-V-analog-circuit-techniques-and-their-in-OTA-Chatterjee-Tsividis/3a4e6b7381376536a21a5e91abd45d386be82f9c](https://www.semanticscholar.org/paper/0.5-V-analog-circuit-techniques-and-their-in-OTA-Chatterjee-Tsividis/3a4e6b7381376536a21a5e91abd45d386be82f9c)

### 速读结论

这篇是 **low-voltage analog design** 的关键论文。它不是只给一个 OTA，而是展示：当 $V_{DD}=0.5\text{ V}$ 时，传统 gate-driven stacked analog topology 会遇到严重 headroom 问题，需要重新设计 common-mode bias、input stage 和 gain enhancement。

### 报告中用法

- 用来支撑：**supply scaling is the first obstacle**。
- 可写成：
    
    > Chatterjee et al. demonstrated that 0.5-V CMOS analog operation is possible, but only with careful common-mode control, body-input/gate-input OTA structures, and gain-enhancement techniques.
    > 

### 需要检查

- Abstract
- OTA architecture figure
- measured results table
- body-input OTA vs gate-input OTA 的差别

### 注意

这篇适合放在 Introduction / Background / bulk-driven section，不一定作为 proposed idea 的直接来源。

## 2. Ferreira, Pimenta & Moreno 2007 — ULV ULP Miller OTA

- **Title**：An Ultra-Low-Voltage Ultra-Low-Power CMOS Miller OTA With Rail-to-Rail Input/Output Swing
- **arXiv**：未找到可靠 arXiv 版本
- **Semantic Scholar**：[https://www.semanticscholar.org/paper/An-Ultra-Low-Voltage-Ultra-Low-Power-CMOS-Miller-Ferreira-Pimenta/c90e153aee9a1022fbbad3049177f47e63b26250](https://www.semanticscholar.org/paper/An-Ultra-Low-Voltage-Ultra-Low-Power-CMOS-Miller-Ferreira-Pimenta/c90e153aee9a1022fbbad3049177f47e63b26250)
- **IEEE PDF entry**：[https://ieeexplore.ieee.org/iel5/8920/4349216/04349224.pdf](https://ieeexplore.ieee.org/iel5/8920/4349216/04349224.pdf)

### 速读结论

这篇适合拿来做 **comparison table** 的“低压低功耗代表样本”。它的重点是：在低于常规 threshold/headroom 的 supply 下，用 Miller OTA 架构实现 rail-to-rail input/output swing，并把功耗压到 nW/µW 级。

### 报告中用法

- 用来支撑：**ultra-low-voltage does not necessarily mean unusable swing**。
- 可写成：
    
    > Ferreira et al. showed that a Miller OTA can achieve rail-to-rail input/output swing under ultra-low-voltage operation, making it a representative baseline for nanopower OTA comparison.
    > 

### 需要检查

- supply voltage
- power/current
- DC gain
- GBW
- SR
- load capacitance
- phase margin

### 注意

这篇的 table 数字要从原文表格确认。不要只抄 AI 初稿里的数值。

## 3. Assaad & Silva-Martinez 2009 — Recycling Folded Cascode

- **Title**：The Recycling Folded Cascode: A General Enhancement of the Folded Cascode Amplifier
- **arXiv**：未找到可靠 arXiv 版本
- **Semantic Scholar**：[https://www.semanticscholar.org/paper/The-Recycling-Folded-Cascode%3A-A-General-Enhancement-Assaad-Silva-Mart%C3%ADnez/dfc8aee088fc6e8f06314452a3a970b6eecb8cf6](https://www.semanticscholar.org/paper/The-Recycling-Folded-Cascode%3A-A-General-Enhancement-Assaad-Silva-Mart%C3%ADnez/dfc8aee088fc6e8f06314452a3a970b6eecb8cf6)
- **Related IET precursor**：[https://digital-library.theiet.org/doi/10.1049/el%3A20072031](https://digital-library.theiet.org/doi/10.1049/el%3A20072031)

### 速读结论

这篇是 proposed idea 的核心。传统 folded cascode 中有些 device/current 对小信号 transconductance 的贡献不充分；RFC 把这些“闲置”的路径重新放入 signal path，提高 effective $G_m$，从而提升 GBW 和 slew rate。

### 报告中用法

- 用来支撑：**current recycling is a transconductance-boosting technique without proportional power increase**。
- 可写成：
    
    > The recycling folded cascode improves the folded-cascode OTA by reusing current paths that are weakly exploited in the conventional topology, thereby increasing effective transconductance, gain-bandwidth, and slew rate at comparable bias current.
    > 

### 需要检查

- conventional folded cascode 图
- recycling folded cascode 图
- measured comparison：GBW / SR 是否提升

### 注意

这篇**本身不是 ultra-low-voltage bulk-driven**，它解决的是 current efficiency / dynamic performance。所以 proposed idea 应该写成“把 RFC 思想迁移到 bulk-driven / low-voltage OTA”，而不是说 Assaad 已经做了 sub-0.5 V bulk-driven RFC。

## 4. Kulej & Khateb 2020 — 0.3 V Class-AB Bulk-Driven OTA

- **Title**：A Compact 0.3-V Class AB Bulk-Driven OTA
- **arXiv**：未找到可靠 arXiv 版本
- **IEEE**：[https://ieeexplore.ieee.org/document/8836116/](https://ieeexplore.ieee.org/document/8836116/)
- **IEEE Computer Society**：[https://www.computer.org/csdl/journal/si/2020/01/08836116/1dia6t8HoYg](https://www.computer.org/csdl/journal/si/2020/01/08836116/1dia6t8HoYg)
- **Semantic Scholar**：[https://www.semanticscholar.org/paper/A-Compact-0.3-V-Class-AB-Bulk-Driven-OTA-Kulej-Khateb/80cb9d7b977da59d80daf8c9da4f30c0ad93a9d3](https://www.semanticscholar.org/paper/A-Compact-0.3-V-Class-AB-Bulk-Driven-OTA-Kulej-Khateb/80cb9d7b977da59d80daf8c9da4f30c0ad93a9d3)

### 速读结论

这篇是“新思路”的直接参考：**bulk-driven + class-AB** 已经被验证可以做到 0.3 V、nW-level power。它说明低压输入和动态增强可以结合，但代价是 bandwidth 很低，更适合低频 biomedical / wearable sensing。

### 可抓数字

- $V_{DD}=0.3\text{ V}$
- power ≈ 12.6 nW
- gain ≈ 64.7 dB
- GBW ≈ 2.96 kHz
- average SR ≈ 4.15 V/ms

### 报告中用法

- 用来支撑：**bulk-driven enables sub-0.5 V operation; class-AB improves large-signal behavior under tiny quiescent current**。
- 可写成：
    
    > Kulej and Khateb demonstrated a 0.3-V class-AB bulk-driven OTA with nanowatt power consumption, confirming the feasibility of combining body input with dynamic current enhancement for low-frequency ultra-low-power applications.
    > 

### 需要检查

- 0.3 V / 12.6 nW 是否准确
- SR 单位是 **V/ms**，不要误写成 V/µs
- application 是否主要偏低频

## 5. 其他论文速读整理：写作时只用一句话

### Vittoz & Fellrath 1977

- **arXiv**：未找到
- **Semantic Scholar**：[https://www.semanticscholar.org/paper/CMOS-analog-integrated-circuits-based-on-weak-Devereaux-Fellrath/6ea4680aaf2cb70edef183cd1e3a3a74b8804742](https://www.semanticscholar.org/paper/CMOS-analog-integrated-circuits-based-on-weak-Devereaux-Fellrath/6ea4680aaf2cb70edef183cd1e3a3a74b8804742)
- **一句话**：weak inversion 是 nanopower analog 的理论起点，因为 MOSFET 在弱反型下有最高 $g_m/I_D$。

### Silveira, Flandre & Jespers 1996

- **arXiv**：未找到
- **DOI**：[https://doi.org/10.1109/4.535416](https://doi.org/10.1109/4.535416)
- **一句话**：$g_m/I_D$ methodology 让 weak/moderate/strong inversion 可以放在一个统一 design framework 下比较。

### Blalock, Allen & Rincon-Mora 1998

- **arXiv**：未找到
- **IEEE**：[https://ieeexplore.ieee.org/document/700924/](https://ieeexplore.ieee.org/document/700924/)
- **一句话**：早期 1-V bulk-driven op-amp 代表，用来说明 body input 是低压 analog 的经典解决方案。

### Ramirez-Angulo et al. 2004

- **arXiv**：未找到
- **IEEE PDF**：[https://ieeexplore.ieee.org/iel5/4/28416/01269919.pdf](https://ieeexplore.ieee.org/iel5/4/28416/01269919.pdf)
- **一句话**：QFG/floating-gate 可以解耦 signal input 和 DC bias，但面积、leakage、initialization 是代价。

### Carvajal et al. 2005

- **arXiv**：未找到
- **IEEE**：[https://ieeexplore.ieee.org/document/1487657/](https://ieeexplore.ieee.org/document/1487657/)
- **一句话**：FVF 是 low-voltage class-AB / buffer / output stage 的基础 cell。

### Enz & Temes 1996

- **arXiv**：未找到
- **IEEE**：[https://ieeexplore.ieee.org/document/542410/](https://ieeexplore.ieee.org/document/542410/)
- **一句话**：chopper / auto-zero 是低频低噪声场景处理 offset 和 $1/f$ noise 的经典方法。

## 6. 可用 arXiv 速通材料

这些不是最核心的经典论文，但方便打开学习。

### Low Power Low Voltage Bulk Driven Balanced OTA

- **arXiv**：[https://arxiv.org/abs/1201.1970](https://arxiv.org/abs/1201.1970)
- **PDF**：[https://arxiv.org/pdf/1201.1970](https://arxiv.org/pdf/1201.1970)
- **用途**：快速理解 bulk-driven OTA 基本机制。

### Two-Stage Ultra-Low-Power Subthreshold Op-Amp

- **PDF**：[https://arxiv.org/pdf/2012.12088](https://arxiv.org/pdf/2012.12088)
- **用途**：快速理解 subthreshold op-amp 的 simulation comparison。

### 0.6-V, µW-Power 4-Stage OTA with Minimal Components

- **PDF**：[https://arxiv.org/pdf/2508.05499](https://arxiv.org/pdf/2508.05499)
- **用途**：了解较新的 multi-stage OTA compensation 思路；不建议作为主线核心。