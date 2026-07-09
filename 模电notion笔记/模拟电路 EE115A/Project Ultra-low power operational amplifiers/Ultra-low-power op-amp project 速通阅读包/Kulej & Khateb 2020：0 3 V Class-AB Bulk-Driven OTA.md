# Kulej & Khateb 2020：0.3 V Class-AB Bulk-Driven OTA

<aside>
📌

**必读 #4 · “新思路”的直接背书。** bulk-driven + class-AB 已被实测验证可做到 0.3 V / 12.6 nW。

</aside>

## 一句话核心

一个紧凑的 **0.3 V class-AB bulk-driven OTA**：用 body input 在 0.3 V 工作，用 class-AB 在 nA 级静态电流下提升大信号 SR；整机 **12.6 nW、64.7 dB、GBW 2.96 kHz、SR 4.15 V/ms**。

## 它解决什么问题

同时解决两件事：① 0.3 V 超低压怎么收信号（bulk-driven 绕开 $V_{TH}$）；② 静态电流压到 nA 级后大信号 slew 太慢（class-AB 动态增流）。

## 核心技术

- **Bulk-driven 输入**：绕开 $V_{TH}$，0.3 V 可工作、近 rail-to-rail。
- **Class-AB 动态增强**：大信号时输出瞬时电流远超静态电流，提升 SR 而不增静态功耗。
- 结构紧凑、管子数少。

## 关键数字

| 指标 | 值 |
| --- | --- |
| 电源 $V_{DD}$ | 0.3 V |
| 功耗 | 12.6 nW |
| DC 增益 | 64.7 dB |
| GBW | 2.96 kHz |
| 平均 SR | 4.15 V/ms |

## 在报告里怎么用

- **角色**：proposed direction（bulk-driven + class-AB）的“已有人验证”背书；comparison table 的极端低压样本。

<aside>
✍️

Kulej and Khateb demonstrated a 0.3-V class-AB bulk-driven OTA consuming only 12.6 nW, confirming that body input combined with dynamic class-AB enhancement is feasible for low-frequency ultra-low-power applications.

</aside>

<aside>
⚠️

两个坑：① SR 单位是 **V/ms**，不是 V/µs（差 1000 倍）；② 带宽只有 kHz 级 → 场景是低频 biomedical / wearable sensing，别拿它论证高速应用。

</aside>

## 链接

- IEEE TVLSI 2020：[IEEE](https://ieeexplore.ieee.org/document/8836116/) · [Semantic Scholar](https://www.semanticscholar.org/paper/A-Compact-0.3-V-Class-AB-Bulk-Driven-OTA-Kulej-Khateb/80cb9d7b977da59d80daf8c9da4f30c0ad93a9d3)