# Ferreira 2007：600 mV / 550 nW rail-to-rail Miller OTA

<aside>
📌

**必读 #2 · comparison table 的“低压低功耗代表样本”。** 600 mV 供电、550 nW 功耗、近 rail-to-rail 摆幅。

</aside>

## 一句话核心

一个两级 **Miller OTA**，全部管子工作在**弱反型**；在低于 $V_{TH}$ 的 **600 mV** 电源下做到近乎 rail-to-rail 的输入 / 输出摆幅，整机只耗 **550 nW**。

## 它解决什么问题

- 证明“超低压 ≠ 没摆幅 / 不可用”：在 sub-threshold 供电下仍能给出像样的 swing 与增益，是 ULV / ULP 可行性的标志性样本。

## 核心技术

- **弱反型偏置**：最大化 $g_m/I_D$，用极小电流换够用的 $g_m$。
- **两级 Miller 补偿**：第一级出跨导、第二级出增益与摆幅；Miller 电容做主极点补偿，保证相位裕度。
- **近 rail-to-rail I/O**：输入共模与输出摆幅几乎贴满 0–600 mV。

## 关键结果

| 指标 | 值 |
| --- | --- |
| 最低电源 $V_{DD}$ | 600 mV（低于 $V_{TH}$） |
| 功耗 | 550 nW |
| 输入 / 输出摆幅 | 近 rail-to-rail |

<aside>
⚠️

DC 增益 / GBW / SR / $C_L$ / 相位裕度这些要**回原文表格逐个核对**再填进 comparison table，别抄 AI 初稿里的数。

</aside>

## 在报告里怎么用

- **角色**：comparison table 的关键低压样本 + “ULV 也能有可用摆幅”的论据。

<aside>
✍️

Ferreira et al. reported a 600-mV, 550-nW two-stage Miller OTA operating in weak inversion with almost rail-to-rail input/output swing, serving as a representative ultra-low-voltage, nanopower baseline.

</aside>

## 阅读重点

- topology 图 + performance table；确认 **600 mV / 550 nW / 近 rail-to-rail** 三个标志数字。它是 TCAS-II express brief（短文），重点在结构 + 实测，没有长理论。

## 链接

- IEEE TCAS-II 2007, vol. 54, pp. 843–847：[Semantic Scholar](https://www.semanticscholar.org/paper/An-Ultra-Low-Voltage-Ultra-Low-Power-CMOS-Miller-Ferreira-Pimenta/c90e153aee9a1022fbbad3049177f47e63b26250)