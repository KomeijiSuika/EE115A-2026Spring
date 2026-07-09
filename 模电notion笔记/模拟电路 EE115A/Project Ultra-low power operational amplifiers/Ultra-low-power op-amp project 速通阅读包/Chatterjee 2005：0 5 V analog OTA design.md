# Chatterjee 2005：0.5 V analog/OTA design

<aside>
📌

**必读 #1 · low-voltage 主线的“开山级”证据。** 证明在标准 0.18 μm、标准 $V_{TH}$ 工艺下，0.5 V 供电也能做出可用 OTA，并给出系统的低压设计方法。

</aside>

## 一句话核心

在 $V_{DD}=0.5\text{ V}$（已 ≤ 一个 $V_{TH}$）下做出两种 OTA——**body-input（bulk-driven）** 和 **gate-input**——并系统给出“低压下怎么稳住共模电平、把摆幅做满”的偏置方法。

## 它解决什么问题

- 0.5 V 时电源已经不够一个 $V_{TH}$，传统 gate-driven 堆叠结构没有 headroom。
- 真正的难点不止“放大”，而是三件事：① 信号怎么进输入级（body vs gate）；② PVT 漂移下怎么稳住 common-mode；③ 增益不够怎么补。

## 核心技术

1. **Body-input OTA**：信号从 body 进、绕开 $V_{TH}$ → 天然低压 / 近 rail-to-rail，但 $g_{mb}$ 小、增益偏低。
2. **Gate-input OTA + 自动增益增强**：信号仍走 gate（$g_m$ 高效、增益大、速度快），靠偏置 / level-shift 把工作点塞进 0.5 V，再用自动增益增强补 DC 增益。
3. **共模与偏置策略**：在 PVT 下维持各管饱和、最大化 swing。
4. （滤波器部分）提出并建模 **弱反型 MOS varactor**，服务 active-RC / $G_m$-C 滤波 tuning。

## 关键结果（实测，0.18 μm，0.5 V）

| 版本 | DC 增益 | GBW | 功耗 |
| --- | --- | --- | --- |
| Body-input OTA | 52 dB | 2.5 MHz | 110 µW |
| Gate-input OTA（带自动增益增强） | 62 dB | 10 MHz | 75 µW |

<aside>
💡

**读法**：gate-input 增益更高、更快、还更省——因为 $g_m$ 比 $g_{mb}$ 高效；代价是要额外偏置 / 增益增强电路。body-input 胜在结构天生耐低压。

</aside>

## 在你报告里怎么用

- **角色**：Introduction / Background 里“supply scaling 是第一道坎”的硬证据；也是 body-input vs gate-input 取舍的经典对照。

<aside>
✍️

Chatterjee et al. demonstrated 0.5-V CMOS OTAs in a standard-$V_T$ 0.18-µm process, comparing body-input and gate-input topologies and proposing common-mode biasing strategies to maximize signal swing over process, voltage, and temperature.

</aside>

## 阅读重点（≈ 10 min）

- Abstract → body-input vs gate-input 两张架构图 → measured results 表 → common-mode biasing 那段。

<aside>
⚠️

**别误用**：这篇主战场是 0.5 V 的“可行性 + 方法”，功耗其实是 **µW 级**，不是 nW 级超低功耗。把它当 **low-voltage 方法论样本**，不要当 nanopower 样本。

</aside>

## 链接

- JSSC 2005, vol. 40, pp. 2373–2387：[IEEE](https://ieeexplore.ieee.org/document/1546214/)
- [Semantic Scholar](https://www.semanticscholar.org/paper/0.5-V-analog-circuit-techniques-and-their-in-OTA-Chatterjee-Tsividis/3a4e6b7381376536a21a5e91abd45d386be82f9c)

[Chatterjee 2005 — 0.5 V Analog/OTA Design（全文中文翻译）](Chatterjee%202005%EF%BC%9A0%205%20V%20analog%20OTA%20design/Chatterjee%202005%20%E2%80%94%200%205%20V%20Analog%20OTA%20Design%EF%BC%88%E5%85%A8%E6%96%87%E4%B8%AD%E6%96%87%E7%BF%BB%E8%AF%91%EF%BC%89.md)