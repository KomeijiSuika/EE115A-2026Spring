# EE115 Lecture1 — Introduction（课程导论）

<aside>
📘

**主题：** 课程导论（Introduction）—— 课程组织与考核、教材、为什么需要模拟电子学、模拟为何比数字更难、为什么走集成与 CMOS。

**教材：** Sedra/Smith《Microelectronic Circuits》、Razavi《Design of Analog CMOS Integrated Circuits》；补充 Gray/Hurst/Lewis/Meyer。

**核心脉络：** 课程信息（时间/考核/作业规则）→ 模拟电子学的必要性（自然信号处理）→ 模拟 vs 数字的设计难点 → 集成化与 CMOS 的优势（摩尔定律、低功耗）→ CMOS 电路与课程体系。

</aside>

## 1️⃣ 课程组织与考核

- **授课时间：** 周四 10:15–11:55（1~14 周），周二（2~12 周的双周）；**地点：** 1D-104。
- **任课：** Dr. Hao Ren 任豪（SIST Bldg [3-328，renhao@shanghaitech.edu.cn](mailto:3-328，renhao@shanghaitech.edu.cn)）；所有更新发布在 **Blackboard**。
- **作业规则：** Blackboard 发布与提交，通常发布后一周截止；**迟交每天扣 20%**；可与同学/TA/老师讨论，但提交必须是自己的工作。
- **成绩构成（各 20 分）：**

| 项目 | 分值 | 形式 |
| --- | --- | --- |
| Homework 作业 | 20 | 个人 |
| Midterm 期中 | 20 | 个人 |
| Final 期末 | 20 | 个人 |
| Research Project 课题 | 20 | 个人 |
| Experiments 实验 | 20 | 刘闯老师负责 |

<aside>
⚠️

**作弊直接判 Fail！** 提交的工作必须是自己的。

</aside>

## 2️⃣ 为什么需要模拟电子学

自 1980 年代起，随着 DSP 与 IC 工艺进步，许多功能从模拟域迁移到数字域 —— 但模拟依然不可替代：

- **自然信号本质是模拟的**：麦克风、光电池、地震传感器等都需先经模拟前端处理。
- 典型应用：神经信号记录（neural recording）、数字通信前端、硬盘驱动电子、无线接收机、光接收机。

<aside>
🎯

**口诀：** 真实世界是模拟的 —— 任何数字系统都要靠模拟前端「接入」现实信号。

</aside>

## 3️⃣ 模拟为什么比数字更难

- **多重权衡：** 数字主要权衡「速度 vs 功耗」；模拟要同时权衡 **速度、功耗、增益、精度** 等多个指标。
- **对干扰更敏感：** 模拟对噪声、串扰、各种干扰远比数字敏感。
- **二阶效应影响大：** 器件的二阶效应对模拟影响显著。
- **几乎无自动化：** 模拟几乎没有自动化，每个器件都要「手工打磨」。

<aside>
🎯

**口诀：** 数字只比「快与省」，模拟却要在速度/功耗/增益/精度间走钢丝，还得手工设计、扛噪声。

</aside>

## 4️⃣ 为什么集成、为什么 CMOS

- **晶体管持续微缩：** 栅长从 1960 年的 25 μm 缩到 2026 年的 2 nm。
- **CMOS 历史：** MOSFET 1930 年代即获专利（早于双极型晶体管），1960 年代初才实用；最初只有 n 型，CMOS 在 1960 年代中期才出现。
- **CMOS 优势：** 只在**开关瞬间**耗功、所需器件数远少于双极型或 GaAs。
- **代价：** MOSFET 比 BJT 更慢、更吵（$g_m$ of MOSFET ≪ $g_m$ of BJT）；但凭借**可缩放性与速度**，CMOS 主导了过去 30 年。

![Slide 16 — 摩尔定律 Moore's Law](EE115%20Lecture1%20%E2%80%94%20Introduction%EF%BC%88%E8%AF%BE%E7%A8%8B%E5%AF%BC%E8%AE%BA%EF%BC%89/slide_16.png)

Slide 16 — 摩尔定律 Moore's Law

![Slide 18 — CMOS 电路：反相器 / 版图 / 工艺流程](EE115%20Lecture1%20%E2%80%94%20Introduction%EF%BC%88%E8%AF%BE%E7%A8%8B%E5%AF%BC%E8%AE%BA%EF%BC%89/slide_18.png)

Slide 18 — CMOS 电路：反相器 / 版图 / 工艺流程

<aside>
🎯

**口诀：** CMOS 赢在「省电 + 可缩放」—— 虽然单管比 BJT 慢、噪声大，但靠摩尔定律的微缩规模化称霸数十年。

</aside>

## 5️⃣ ShanghaiTech 电路课程体系

- Circuits → Analog Circuits / Digital Circuits → Analog IC I / Digital IC I → Advanced Analog IC / Advanced Digital IC（研究生）。
- 本课 **EE115 Analog Circuits（模拟电路）** 处在「电路基础 → 集成电路」的承上启下位置。

## 🧩 本节总结

<aside>
🧠

**导论三句话：**

1. **考核：** 作业/期中/期末/课题/实验各 20 分；迟交每天扣 20%，作弊直接 Fail。
2. **为何学模拟：** 真实世界信号是模拟的，数字系统必须靠模拟前端接入现实。
3. **难点与方向：** 模拟要多重权衡、抗干扰、手工设计；产业走向集成 + CMOS，靠摩尔定律微缩实现低功耗与高规模。
</aside>

## 📎 原始 Slides

[EE115 Lecture1 Introduction.pdf](EE115%20Lecture1%20%E2%80%94%20Introduction%EF%BC%88%E8%AF%BE%E7%A8%8B%E5%AF%BC%E8%AE%BA%EF%BC%89/EE115_Lecture1_Introduction.pdf)