# Assaad 2009：Recycling Folded Cascode（RFC）

<aside>
📌

**必读 #3 · proposed idea 的核心引擎。** 把 folded cascode 里“闲置”的管子拉进信号路 → 等效 $G_m$、GBW ≈ 2×，SR > 2×，而功耗与面积基本不变。

</aside>

## 一句话核心

**RFC（recycling folded cascode）** = 在传统 folded cascode 上，把原本只当电流源、对信号无贡献的管子**拆分复用**成第二对输入管；两路信号电流在输出叠加，等效跨导和 GBW 约翻倍、SR 翻倍多，而 bias 电流和面积几乎不变。

## 它解决什么问题

- 传统 folded cascode 的电流源管“只导流、不出力”——白耗电却不贡献跨导。RFC 把这部分电流**回收**进信号路。

## 核心机制

- 把输入对按比例（如 3:1）拆开，并把折叠支里的电流镜管做成 **active 的第二输入对**；两条路径的信号电流在输出节点汇合，跨导相加。
- 结果：$G_{m,\text{RFC}}\approx 2\,g_{m1}$，GBW 与 SR 同步提升，功耗 / 面积持平。

```
传统 FC：输入对 → 折叠点 → 电流源管（闲置，只导流）
RFC   ：输入对拆 3:1 → 电流镜管“复活”成第二输入对 → 两路跨导叠加
```

## 关键结果

- 相比同功耗 / 同面积的 conventional folded cascode：≈ 2× transconductance、≈ 2× GBW、> 2× slew rate（理论 + 实测确认）。

## 在报告里怎么用

- **角色**：performance-boosting 主线 + proposed idea 的“增益 / 速度增强”手段。

<aside>
🎯

**关键定位（别写错）**：RFC 本身**不是** sub-0.5 V bulk-driven，它解决的是电流效率 / 动态性能。proposed idea 要写成“把 RFC 的电流复用思想**迁移到** bulk-driven 低压 OTA，用来补 $g_{mb}$ 偏低的短板”，而不是说 Assaad 已经做了低压 bulk-driven RFC。

</aside>

<aside>
✍️

Assaad and Silva-Martinez's recycling folded cascode reuses otherwise-idle devices in the signal path to roughly double the transconductance, gain-bandwidth, and slew rate at the same power and area as the conventional folded cascode.

</aside>

## 阅读重点

- conventional FC 图 vs RFC 图；输入对拆分 / 电流镜复用那张；measured GBW & SR 对比表。

## 链接

- IEEE JSSC 2009, vol. 44, pp. 2535–2542：[Semantic Scholar](https://www.semanticscholar.org/paper/The-Recycling-Folded-Cascode%3A-A-General-Enhancement-Assaad-Silva-Mart%C3%ADnez/dfc8aee088fc6e8f06314452a3a970b6eecb8cf6)