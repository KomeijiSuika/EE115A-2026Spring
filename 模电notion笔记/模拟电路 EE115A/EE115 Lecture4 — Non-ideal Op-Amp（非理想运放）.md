# EE115 Lecture4 — Non-ideal Op-Amp（非理想运放）

<aside>
📘

**主题：** 非理想运放（Non-ideal Op-Amp）—— 真实运放偏离理想模型的八大非理想性，及其对增益、带宽、大信号摆幅的限制。

**教材：** Sedra/Smith — *Microelectronic Circuits*，pp.96–100、95–115（Ch.2）。

**核心脉络：** 理想运放（虚断 / 虚短 / 无限增益·带宽·CMRR）→ 真实运放八大缺陷（有限增益 / 输入输出阻抗 / 偏移电压 / 偏置电流）→ 频率特性（单极点、GBW 不变）→ 大信号限制（输出饱和、压摆率 SR、全功率带宽）。区分「线性带宽限」与「非线性 SR 限」是本节最重要的颗。

</aside>

## 1️⃣ 理想 vs 真实运放

### 理想运放复习

- **无限输入阻抗** → 输入端不抽电流，**虚断（virtual open）**。
- **零输出阻抗**。
- **无限开环增益** $A$ → 两输入端间 **虚短（virtual short）**。
- **无限带宽**、**无限共模抑制比（CMRR）**。

### 真实运放的八大缺陷（Imperfections）

1. 有限开环增益 $A_o<\infty$
2. 有限输入电阻 $R_i<\infty$
3. 非零输出电阻 $R_o>0$
4. 有限带宽
5. 偏移电压（offset voltage，$V_{OS}$）
6. 输入偏置电流与偏移电流（bias & offset currents）
7. 输出饱和（saturation）
8. 压摆率（slew rate）限制

原厂手册里的电气 / 开关特性参数表（直接看老师原 slide）：

![Slide 4 — 真实运放的电气特性（Electrical Characteristics 数据表）](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_4.png)

Slide 4 — 真实运放的电气特性（Electrical Characteristics 数据表）

![Slide 5 — 真实运放的开关特性（Switching Characteristics 数据表）](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_5.png)

Slide 5 — 真实运放的开关特性（Switching Characteristics 数据表）

<aside>
🎯

**口诀：** 理想运放记住「虚断 + 虚短」；真实运放就是把每个「无限 / 零」换成有限值 —— 增益有限、阻抗有限、还多出偏移 / 偏置 / 饱和 / 压摆率。

</aside>

## 2️⃣ 偏移电压 Offset Voltage（$V_{OS}$）

- 理想上输入为 0 时输出为 0；真实运放因内部失配，输入 0 时输出仍有直流 → 等效为输入端串一个 **偏移电压** $V_{OS}$（典型几 mV）。
- 闭环下输出直流偏移：$V_{O} = V_{OS}\left(1+\dfrac{R_2}{R_1}\right)$（被直流增益放大）。
- **调零（Trimming）：** 用电位器接到双 offset-nulling 端子，动端接负电源，把输出直流偏移调到 0。

![Slide 7 — 偏移电压电路模型（Fig 2.28）与 VOS=5mV 转移特性（Fig E2.21）](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_7.png)

Slide 7 — 偏移电压电路模型（Fig 2.28）与 VOS=5mV 转移特性（Fig E2.21）

![Slide 8 — 用电位器调零 offset（Fig 2.29 闭环偏移 / Fig 2.30 调零电路）](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_8.png)

Slide 8 — 用电位器调零 offset（Fig 2.29 闭环偏移 / Fig 2.30 调零电路）

<aside>
🎯

**口诀：** $V_{OS}$ 被「直流闭环增益 $1+R_2/R_1$」放大后出现在输出 —— 高增益级尤其要防偏移。

</aside>

## 3️⃣ 开环频率响应（Open-Loop Frequency Response）

内部补偿的通用运放是 **单极点（主极点 dominant pole** $\omega_b$**）** 响应：

$$
A(s) = \frac{A_0}{1 + s/\omega_b}
$$

- 高频段（$\omega \gg \omega_b$）：$|A(j\omega)| \approx \dfrac{A_0\,\omega_b}{\omega}$，增益以 $-20\ \text{dB/dec}$ 滚降。
- **单位增益带宽（unity-gain bandwidth）**：令 $|A|=1$ 得 $\omega_t = A_0\,\omega_b$，即增益-带宽积 GBW。

![Slide 9 — 内部补偿运放的开环增益伯德图（单主极点），Fig 2.39](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_9.png)

Slide 9 — 内部补偿运放的开环增益伯德图（单主极点），Fig 2.39

<aside>
🎯

**口诀：** 单极点 → $A_0\omega_b=\omega_t$ 是常数（GBW）。增益越高可用带宽越窄，乘积锁死。

</aside>

## 4️⃣ 闭环频率响应（Closed-Loop）

把带宽受限的 $A(s)$ 代入闭环增益表达式，得到一个新的三分贝转角频率：

### 反相放大器（Inverting）

$$
\frac{V_o}{V_i} = \frac{-R_2/R_1}{1 + s/\omega_{3dB}},\qquad \omega_{3dB} = \frac{\omega_t}{1 + R_2/R_1}
$$

### 同相放大器（Noninverting）

$$
G = 1 + \frac{R_2}{R_1},\qquad \omega_{3dB} = \frac{\omega_t}{G}\ \Rightarrow\ G\cdot\omega_{3dB} = \omega_t
$$

<aside>
🎯

**口诀：** 闭环「括增益与带宽」互换 —— 增益 × 带宽 = $\omega_t$ **GBW 不变**。调闭环增益就是在这条恒 GBW 线上滑动。

</aside>

## 5️⃣ 输出饱和 Output Saturation

输出摆幅受两个因素限制：

1. **饱和电压**（通常比电源电压低 1–2 V）。
2. **最大输出电流**（小负载电阻时受限）。

任一条件触发时，输出波形被「削顶（clipped）」。

![Slide 12 — 名义增益 10 V/V 运放在 ±13V 饱和 / ±20mA 限流下输出被削顶，Fig 2.42](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_12.png)

Slide 12 — 名义增益 10 V/V 运放在 ±13V 饱和 / ±20mA 限流下输出被削顶，Fig 2.42

<aside>
🎯

**口诀：** 饱和 = 「天花板」，输出撞上供电或限流就被拍平成平顶波。

</aside>

## 6️⃣ 压摆率 Slew Rate（SR）

输出变化速率被「压摆率」封顶：

$$
SR = \left.\frac{dv_o}{dt}\right|_{max}\quad [\text{V/}\mu\text{s}]
$$

- **SR 限制 ≠ 带宽限制**：

| 维度 | 带宽限制（Bandwidth） | 压摆率限制（Slew Rate） |
| --- | --- | --- |
| **本质** | 线性（单极点低通） | 非线性（输出爬升速度封顶） |
| **对正弦** | 只衰减 / 移相，不改变波形 | 大信号被扭成三角波 → 非线性失真 |
| **阶跃响应斜率** | $\omega_t V e^{-\omega_t t}$（指数上升，始斜率 $<SR$） | 恒定 $= SR$（直线上升） |

![Slide 13 — 单位增益跟随器的阶跃响应：压摆限制下的线性上升 vs 未受限的指数上升，Fig 2.43](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_13.png)

Slide 13 — 单位增益跟随器的阶跃响应：压摆限制下的线性上升 vs 未受限的指数上升，Fig 2.43

<aside>
🎯

**口诀：** 带宽限 = 线性（不改形），压摆限 = 非线性（变三角波）。阶跃下一个指数爬、一个直线爬。

</aside>

## 7️⃣ 全功率带宽 Full-Power Bandwidth（$f_M$）

对单位增益跟随器输入正弦 $v_I = V_i\sin\omega t$，变化率峰值不能超 SR：

$$
\frac{dv_I}{dt} = V_i\,\omega\cos\omega t \le SR
$$

当输出为最大额定摆幅 $V_{omax}$ 时，开始出现 SR 失真的频率即全功率带宽：

$$
\omega_M V_{omax} = SR \quad\Rightarrow\quad f_M = \frac{SR}{2\pi V_{omax}}
$$

![Slide 15 — 压摆限制对输出正弦波的影响（峰值越大 / 频率越高越容易失真），Fig 2.44](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/slide_15.png)

Slide 15 — 压摆限制对输出正弦波的影响（峰值越大 / 频率越高越容易失真），Fig 2.44

<aside>
🎯

**口诀：** $f_M=\dfrac{SR}{2\pi V_{omax}}$ —— 要输出越大摆幅×越高频率，就越早撞上压摆天花板。注意它与小信号带宽（GBW）是两回事。

</aside>

## 📝 作业 / Deadline

<aside>
ℹ️

本节 slides 末页的作业页是占位模板（“Homework x — due March xx”、“chapter xx problem xxx”），**并未给出具体题号与截止日期**，所以没有可同步的 DDL。等老师公布具体作业后告诉我，我再补进课程页和日历。

</aside>

## 🧩 本节总结

<aside>
🧠

**非理想运放三句话：**

1. **直流 / 低频缺陷：** 有限增益 $A_o$、输入输出阻抗、偏移电压 $V_{OS}$、偏置电流、CMRR —— $V_{OS}$ 会被 $1+R_2/R_1$ 放大进输出。
2. **频率限制（线性）：** 单极点 $A(s)=\dfrac{A_0}{1+s/\omega_b}$，$\omega_t=A_0\omega_b$；闭环下 **增益×带宽 = GBW 恒定**。
3. **大信号限制（非线性）：** 输出饱和削顶、压摆率 $SR=dv_o/dt|_{max}$ 把大信号扭成三角波，全功率带宽 $f_M=\dfrac{SR}{2\pi V_{omax}}$。**SR 限≠带宽限**。
</aside>

## 📎 原始 Slides

[EE115 Lecture4.pdf](EE115%20Lecture4%20%E2%80%94%20Non-ideal%20Op-Amp%EF%BC%88%E9%9D%9E%E7%90%86%E6%83%B3%E8%BF%90%E6%94%BE%EF%BC%89/EE115_Lecture4.pdf)