# Ultra-low-power op-amp project 速通阅读包

<aside>
⚡

**目标：24h 速通，不做完整文献苦读。** 这页只服务于快速把 report 变可信：只需要审查核心 4–5 篇，其余论文整理为压缩写作素材。

</aside>

## 0. 速通策略

**最终报告主线不要变**：围绕 ultra-low-voltage / ultra-low-power OTA 的三个核心矛盾写：

1. **电源电压太低** → input headroom 不够 → bulk-driven / floating-gate / rail-to-rail input。
2. **电流太小** → $g_m$、GBW、noise 变差 → weak inversion + $g_m/I_D$ methodology。
3. **性能不够** → current-reuse / inverter-based / recycling folded cascode / class-AB output 补效率。

**重点阅读论文保留 4 篇：**

- Chatterjee 2005：0.5 V analog/OTA design，支撑 low-voltage 主线。
    
    [Chatterjee 2005：0.5 V analog/OTA design](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Chatterjee%202005%EF%BC%9A0%205%20V%20analog%20OTA%20design.md)
    
- Ferreira 2007：ultra-low-voltage ultra-low-power Miller OTA，适合进 comparison table。
    
    [Ferreira 2007：600 mV / 550 nW rail-to-rail Miller OTA](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Ferreira%202007%EF%BC%9A600%20mV%20550%20nW%20rail-to-rail%20Miller%20OT.md)
    
- Assaad 2009：recycling folded cascode，支撑 proposed idea。
    
    [Assaad 2009：Recycling Folded Cascode（RFC）](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Assaad%202009%EF%BC%9ARecycling%20Folded%20Cascode%EF%BC%88RFC%EF%BC%89.md)
    
- Kulej & Khateb 2020：0.3 V class-AB bulk-driven OTA，支撑“新思路”方向。
    
    [Kulej & Khateb 2020：0.3 V Class-AB Bulk-Driven OTA](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Kulej%20&%20Khateb%202020%EF%BC%9A0%203%20V%20Class-AB%20Bulk-Driven%20OTA.md)
    

其余论文由我代读，用作背景 citation 或一句话概括。

## 1. 必读论文 4 篇

| 优先级 | 论文 | 链接 | 读者要看什么 | 用于报告哪一部分 |
| --- | --- | --- | --- | --- |
| 1 | Chatterjee, Tsividis, Kinget, 2005 — 0.5-V analog circuit techniques and OTA/filter design | IEEE: [https://ieeexplore.ieee.org/document/1546214/](https://ieeexplore.ieee.org/document/1546214/) | 看 abstract、OTA architecture、measured results table。重点理解 0.5 V 下 body-input / gate-input OTA 怎么维持 common-mode 和 swing。 | Section II/III：low-voltage design challenge + bulk/body input technique。 |
| 2 | Ferreira, Pimenta, Moreno, 2007 — Ultra-low-voltage ultra-low-power CMOS Miller OTA | Semantic Scholar: [https://www.semanticscholar.org/paper/An-Ultra-Low-Voltage-Ultra-Low-Power-CMOS-Miller-Ferreira-Pimenta/c90e153aee9a1022fbbad3049177f47e63b26250](https://www.semanticscholar.org/paper/An-Ultra-Low-Voltage-Ultra-Low-Power-CMOS-Miller-Ferreira-Pimenta/c90e153aee9a1022fbbad3049177f47e63b26250) | 看 topology 图、performance table。重点抓 600 mV、nW-level power、rail-to-rail input/output。 | Comparison table 的关键样本；支撑“低压 + 低功耗确实可实现”。 |
| 3 | Assaad & Silva-Martinez, 2009 — The Recycling Folded Cascode | Semantic Scholar: [https://www.semanticscholar.org/paper/The-Recycling-Folded-Cascode%3A-A-General-Enhancement-Assaad-Silva-Mart%C3%ADnez/dfc8aee088fc6e8f06314452a3a970b6eecb8cf6](https://www.semanticscholar.org/paper/The-Recycling-Folded-Cascode%3A-A-General-Enhancement-Assaad-Silva-Mart%C3%ADnez/dfc8aee088fc6e8f06314452a3a970b6eecb8cf6) | 看 conventional folded cascode vs recycling folded cascode 的对比图。重点理解：把原本“闲置”的 current/device 放进 signal path，提高 $G_m$、GBW、SR。 | Proposed idea 的核心依据：用 recycling 思想补偿 bulk-driven 的低 $g_{mb}$。 |
| 4 | Kulej & Khateb, 2020 — A Compact 0.3-V Class AB Bulk-Driven OTA | IEEE: [https://ieeexplore.ieee.org/document/8836116/；IEEE](https://ieeexplore.ieee.org/document/8836116/；IEEE) Computer Society: [https://www.computer.org/csdl/journal/si/2020/01/08836116/1dia6t8HoYg](https://www.computer.org/csdl/journal/si/2020/01/08836116/1dia6t8HoYg) | 看 abstract、class-AB bulk-driven structure、performance table。重点抓 0.3 V、12.6 nW、64.7 dB、2.96 kHz、4.15 V/ms。 | 新思路页面的直接参考：bulk-driven + class-AB 是已经有人验证过的方向。 |

<aside>
📄

**每篇必读论文的详细笔记页**：[Chatterjee 2005：0.5 V analog/OTA design](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Chatterjee%202005%EF%BC%9A0%205%20V%20analog%20OTA%20design.md) · [Ferreira 2007：600 mV / 550 nW rail-to-rail Miller OTA](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Ferreira%202007%EF%BC%9A600%20mV%20550%20nW%20rail-to-rail%20Miller%20OT.md) · [Assaad 2009：Recycling Folded Cascode（RFC）](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Assaad%202009%EF%BC%9ARecycling%20Folded%20Cascode%EF%BC%88RFC%EF%BC%89.md) · [Kulej & Khateb 2020：0.3 V Class-AB Bulk-Driven OTA](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Kulej%20&%20Khateb%202020%EF%BC%9A0%203%20V%20Class-AB%20Bulk-Driven%20OTA.md)

</aside>

## 2. 次要论文：速读整理，读者不用细看

| 论文 | 链接 | 核心概述（速览） | 报告中怎么用 |
| --- | --- | --- | --- |
| Vittoz & Fellrath, 1977 — weak inversion CMOS analog circuits | Semantic Scholar: [https://www.semanticscholar.org/paper/CMOS-analog-integrated-circuits-based-on-weak-Devereaux-Fellrath/6ea4680aaf2cb70edef183cd1e3a3a74b8804742](https://www.semanticscholar.org/paper/CMOS-analog-integrated-circuits-based-on-weak-Devereaux-Fellrath/6ea4680aaf2cb70edef183cd1e3a3a74b8804742) | 弱反型 / 亚阈值 CMOS 模拟电路的奠基作。核心：MOSFET 在弱反型下 $g_m/I_D$ 达理论上限（≈ $1/(nV_T)$，约 25–30 V⁻¹），即“每安培电流换到的跨导最大”——这正是一切 nanopower analog 把管子偏置到弱反型的根本理由。报告用法：Background 引 1–2 句，作为“低功耗为何选弱反型”的理论原点。 | Background 里引用 1–2 句即可，不进重点表。 |
| Silveira, Flandre, Jespers, 1996 — $g_m/I_D$ methodology | DOI: [https://doi.org/10.1109/4.535416](https://doi.org/10.1109/4.535416) | 把 $g_m/I_D$ 从一个比值升级成完整设计方法学：以 $g_m/I_D$ 为自变量，用一条跨弱 / 中 / 强反型的统一曲线选工作点与管子尺寸，在效率（大 $g_m/I_D$）与速度（大 $f_T$）间定量取舍。报告用法：Section II 理论 backbone，是“弱反型高效率”落到实际 sizing 的桥梁。 | Section II 的理论 backbone。 |
| Blalock, Allen, Rincon-Mora, 1998 — 1-V op amps in standard CMOS | IEEE: [https://ieeexplore.ieee.org/document/700924/](https://ieeexplore.ieee.org/document/700924/) | 用标准数字 CMOS 做 1-V bulk-driven op-amp 的早期经典。核心：信号从 body 注入绕开 $V_{TH}$，首次系统说明 body-driven 是低压模拟的可行路线，也点出 $g_{mb}$ 偏低、增益受限的代价。报告用法：bulk-driven 的历史起点，可进 comparison table，不必精读。 | 作为 bulk-driven 历史起点；可放 comparison table，但不必精读。 |
| Ramirez-Angulo et al., 2004 — quasi-floating-gate analog signal processing | IEEE PDF: [https://ieeexplore.ieee.org/iel5/4/28416/01269919.pdf](https://ieeexplore.ieee.org/iel5/4/28416/01269919.pdf) | 准浮栅（QFG）/ 浮栅信号处理的代表作。核心机制：用大电阻 + 耦合电容把 DC 偏置与 AC 信号解耦，使输入共模可自由设定、绕开低压 $V_{TH}$ 限制；代价是面积、栅漏电（leakage）与初始化 / 漂移。报告用法：topology review 中 floating-gate / QFG 一段的主引用。 | Topology review 中简短一段即可。 |
| Carvajal et al., 2005 — Flipped Voltage Follower | IEEE: [https://ieeexplore.ieee.org/document/1487657/](https://ieeexplore.ieee.org/document/1487657/) | Flipped Voltage Follower（FVF）。核心：一个低压、超低输出阻抗的基础 cell，能在很小静态电流下吸 / 灌大电流，是 class-AB 输出级与 buffer 的常用积木。报告用法：支撑 class-AB / output stage 一段，点到即可。 | 支撑 class-AB output stage，不需要展开。 |
| Enz & Temes, 1996 — autozero/chopper stabilization | IEEE: [https://ieeexplore.ieee.org/document/542410/](https://ieeexplore.ieee.org/document/542410/) | auto-zero 与 chopper 稳定技术的经典综述。核心：用调制把 offset 和 $1/f$（闪烁）噪声搬到高频再滤掉，是低频 biomedical 前端（EEG / ECG）压低噪声的标准武器。报告用法：noise subsection 简短引用。 | Noise subsection 简短提及即可。 |
| Rodovalho et al., 2021 — self-biased inverter-based OTA | MDPI: [https://www.mdpi.com/2079-9292/10/8/935](https://www.mdpi.com/2079-9292/10/8/935) | 自偏置 inverter-based OTA 的现代范例。核心：用 CMOS 反相器当跨导级，NMOS / PMOS 共享同一支电流（current-reuse），使等效 $G_m = g_{mn} + g_{mp}$，同电流下跨导翻倍。报告用法：说明 current-reuse / inverter-based 是近年低功耗主流方向。 | 说明 current-reuse 是近年主流方向。 |
| Bai, Li, Xu, 2022 — wearable low-power high-gain op-amp | MDPI: [https://www.mdpi.com/2079-9292/11/1/74](https://www.mdpi.com/2079-9292/11/1/74) | 面向 wearable 的低功耗高增益 op-amp（0.8 V、11.2 µW、74.1 dB）。核心偏工程应用：展示在中低压下如何兼顾高增益与低功耗，给出贴近真实穿戴场景的设计点。报告用法：recent application example，不作理论核心。 | 可作为 recent application example；不作为理论核心。 |

## 3. arXiv / open-access 可用材料

<aside>
🔎

很多经典 analog IC 论文没有 arXiv 版本，通常在 IEEE / JSSC / TCAS。下面这些是能直接打开的 arXiv/open-access 补充材料；它们不一定最权威，但适合速通理解。

</aside>

| 材料 | arXiv / open link | 用途 | 是否进正式 references |
| --- | --- | --- | --- |
| Low Power Low Voltage Bulk Driven Balanced OTA | [https://arxiv.org/abs/1201.1970；PDF](https://arxiv.org/abs/1201.1970；PDF): [https://arxiv.org/pdf/1201.1970](https://arxiv.org/pdf/1201.1970) | 快速理解 bulk-driven OTA 的基本原理和 0.9 V simulation example。 | 可选。若 references 太少，可以放；否则只当学习材料。 |
| Two-Stage Ultra-Low-Power Subthreshold Op-Amp comparison | PDF: [https://arxiv.org/pdf/2012.12088](https://arxiv.org/pdf/2012.12088) | 快速理解 subthreshold two-stage op-amp 和 180/90/45 nm 对比。 | 可选，不建议作为核心论文。 |
| 0.6-V, µW-Power 4-Stage OTA with Minimal Components | PDF: [https://arxiv.org/pdf/2508.05499](https://arxiv.org/pdf/2508.05499) | 了解较新的 multi-stage OTA compensation 思路；和本文主线关系较弱。 | 通常不进主 references，除非你想补“future direction”。 |

## 4. 最终报告怎么写最快

### 4.1 章节直接定稿

1. **Introduction**：IoT / biomedical / wearable → op-amp 是 analog front-end 核心 → low voltage + low power 难。
2. **Background**：$g_m/I_D$、headroom、noise-power、PVT。
3. **Topology Review**：
    - weak inversion / $g_m/I_D$
    - bulk-driven / QFG
    - current-reuse / inverter-based
    - recycling folded cascode
    - class-AB / FVF
    - chopper/auto-zero
4. **Comparison & Discussion**：只放 8–10 篇，不要追求全。
5. **Proposed Direction**：bulk-driven input + recycling folded-cascode load + optional class-AB output。
6. **Conclusion**：按 trade-off 总结。

### 4.2 Comparison table 只做“够用版”

建议只放这些行：

- Blalock 1998
- Chatterjee 2005 body-input OTA
- Chatterjee 2005 gate-input OTA
- Ferreira 2007
- Assaad 2009 RFC
- Harrison 2003 neural amp
- Rodovalho 2021 inverter-based OTA
- Kulej & Khateb 2020
- Bai 2022

不需要把所有 FoM 都填满。缺失就写 **N/A / not reported**，比乱填更可信。

### 4.3 一天内的最小工作流

- **2 h**：审查这页 + 读 4 篇必读论文的 abstract / figures / tables。
- **2 h**：把 comparison table 的数值从原文/摘要中补齐，缺失写 N/A。
- **1 h**：确认 proposed idea 子页里的逻辑是否能接受。
- **剩下时间**：进入正式写作，不再继续扩论文。

## 5. 当前初稿最该改的地方

- 把 references [14]、[15] 的 “Author(s) to confirm” 补成真实作者。
- 删掉 table 里的 “very high / high” 这种模糊 FoM，改成 N/A 或重新计算。
- 把 topology review 从“八类并列”改成“低压输入 / 电流效率 / 性能增强 / 低频噪声”四大类。
- Proposed idea 保留，但写得保守：不是声称已验证的新电路，而是 “candidate direction”。

[Proposed idea — Bulk-driven + RFC + Class-AB OTA](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Proposed%20idea%20%E2%80%94%20Bulk-driven%20+%20RFC%20+%20Class-AB%20OTA.md)

[Core papers — 24h 速通代读笔记](Ultra-low-power%20op-amp%20project%20%E9%80%9F%E9%80%9A%E9%98%85%E8%AF%BB%E5%8C%85/Core%20papers%20%E2%80%94%2024h%20%E9%80%9F%E9%80%9A%E4%BB%A3%E8%AF%BB%E7%AC%94%E8%AE%B0.md)