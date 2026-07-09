# EE115 Lecture2 — Amplifier Basics（放大器基础）

<aside>
📘

**主题：** 放大器基础（Amplifier Basics）—— 从增益、功率、线性/饱和，到二端口模型、级联、放大器类型，再到频率响应、STC 滤波器与波特图。

**教材：** Sedra/Smith — *Microelectronic Circuits*，pp.15–43（Ch.1）。

**核心脉络：** 增益（电压/电流/功率 + dB）→ 功率与效率 → 线性区 vs 饱和失真 → 二端口电压放大器模型（$A_{vo},R_i,R_o$）→ 级联 → 四类放大器 → 频率响应（传递函数 $T(\omega)$、波特图、3dB 带宽）→ 低通/高通 STC 网络。

</aside>

## 1️⃣ 放大器增益 Gain

$$
A_v = \frac{v_O}{v_I},\qquad A_i = \frac{i_O}{i_I},\qquad A_p = \frac{p_L}{p_I} = A_v A_i
$$

- $A_v,A_i$ 可正、可负、甚至复数；**负增益 = 输出与输入反相 180°**；但功率增益 $A_p$ 恒为正。
- 分贝（dB）表示：

$$
|A_v|_{dB}=20\log|A_v|,\quad |A_i|_{dB}=20\log|A_i|,\quad A_p|_{dB}=10\log A_p
$$

<aside>
🎯

**口诀：** 电压/电流增益用 $20\log$，功率增益用 $10\log$；负号只代表相位反相，不代表功率为负。

</aside>

## 2️⃣ 功率供给与效率

- 放大器需 dc 电源供电，典型记为 $V_{CC}$ 与 $-V_{EE}$。
- 直流功耗：$P_{dc}=V_{CC}I_{CC}+V_{EE}I_{EE}$。
- 功率平衡：$P_{dc}+P_I = P_L + P_{diss}$（电源功率 + 信号源功率 = 负载功率 + 内部耗散）。
- 功率效率：$\eta = \dfrac{P_L}{P_{dc}}$ —— 对功放（音响、发射机）尤其重要。

## 3️⃣ 线性区 vs 饱和区

- **线性区：** 输出与输入成正比（不失真）。
- **饱和区：** 输出波形被削顶饱和 → 产生**失真（distortion）**，音响中即丢失保真度。

![Slide 6 — 线性但带输出饱和的放大器转移特性（Fig 1.14）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_6.png)

Slide 6 — 线性但带输出饱和的放大器转移特性（Fig 1.14）

<aside>
🎯

**口诀：** 转移特性中间直线段是线性放大区，两端拐平就是饱和 → 想不失真，信号摆幅必须留在线性区内。

</aside>

### 符号约定 Symbol Notation

| 记法 | 含义 |
| --- | --- |
| $i_C(t)$（小写下标小写） | 瞬时总量 = 直流 + 交流 |
| $I_C$（大写大写） | 直流分量（dc） |
| $i_c(t)$（小写小写） | 小信号交流分量 |
| $I_c$（大写小写） | 交流分量的幅值（amplitude） |

总量关系：$i_C(t) = I_C + i_c(t)$，其中 $i_c = I_c\sin(\omega t)$。

## 4️⃣ 电压放大器二端口模型

- $A_{vo}$：开路电压增益；$R_i$：输入电阻；$R_o$：输出电阻；$R_s$：源内阻；$R_L$：负载。
- 接上信号源与负载后的总增益（两次分压衰减）：

$$
\frac{v_o}{v_s} = A_{vo}\cdot\underbrace{\frac{R_i}{R_i+R_s}}_{\text{输入分压}}\cdot\underbrace{\frac{R_L}{R_L+R_o}}_{\text{输出分压}}
$$

![Slide 8 — 电压放大器电路模型及接源/负载（Fig 1.16）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_8.png)

Slide 8 — 电压放大器电路模型及接源/负载（Fig 1.16）

<aside>
🎯

**口诀：** 想少丢增益 → 输入端要 $R_i\gg R_s$、输出端要 $R_o\ll R_L$。

</aside>

## 5️⃣ 级联放大器 Cascaded Amplifier

实际系统多级级联，目的：① 提供足够增益；② 提供合适的输入/输出阻抗。

- **输入级：** 高输入阻抗（不衰减源）。
- **中间级：** 提供主要增益。
- **输出级：** 低输出阻抗（强驱动负载）。
- 总增益 = 各级增益之积（dB 则相加），但需注意级间阻抗匹配带来的分压。

![Slide 9 — 三级级联放大器（Fig 1.17）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_9.png)

Slide 9 — 三级级联放大器（Fig 1.17）

## 6️⃣ 放大器类型

| 类型 | 输入→输出 | 理想输入阻抗 | 理想输出阻抗 |
| --- | --- | --- | --- |
| 电压放大器 Voltage | V → V | ∞ | 0 |
| 电流放大器 Current | I → I | 0 | ∞ |
| 跨导放大器 Transconductance | V → I | ∞ | ∞ |
| 跨阻放大器 Transimpedance | I → V | 0 | 0 |

## 7️⃣ 频率响应 Frequency Response

正弦输入经线性放大器后，输出仍是**同频率**正弦，但幅度与相位改变：

$$
T(\omega) = \frac{V_o(\omega)}{V_i(\omega)},\qquad |T(\omega)|=\frac{V_o}{V_i},\qquad \angle T(\omega)=\phi
$$

- **波特图（Bode Plot）：** 纵轴 $20\log|T(\omega)|$，横轴 $\log\omega$ 的 log–log 图。
- **带宽（Bandwidth）：** 增益相对中频下降 **3 dB** 所覆盖的频率范围。

![Slide 12 — 放大器典型幅度响应与 3dB 带宽（Fig 1.21）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_12.png)

Slide 12 — 放大器典型幅度响应与 3dB 带宽（Fig 1.21）

### 低通 STC 网络 Low-Pass

$$
T(\omega)=\frac{1}{1+j\omega/\omega_0},\qquad \omega_0=\frac{1}{RC}
$$

$$
|T(\omega)|=\frac{1}{\sqrt{1+(\omega/\omega_0)^2}},\quad \angle T(\omega)=-\tan^{-1}\!\frac{\omega}{\omega_0},\quad \omega_{3dB}=\omega_0
$$

![Slide 13 — 低通网络幅度与相位响应（Fig 1.22/1.23）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_13.png)

Slide 13 — 低通网络幅度与相位响应（Fig 1.22/1.23）

### 高通 STC 网络 High-Pass

$$
T(\omega)=\frac{1}{1-j\omega_0/\omega},\qquad \omega_0=\frac{1}{RC}
$$

$$
|T(\omega)|=\frac{1}{\sqrt{1+(\omega_0/\omega)^2}},\quad \angle T(\omega)=\tan^{-1}\!\frac{\omega_0}{\omega},\quad \omega_{3dB}=\omega_0
$$

![Slide 14 — 高通网络幅度与相位响应（Fig 1.24）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_14.png)

Slide 14 — 高通网络幅度与相位响应（Fig 1.24）

<aside>
🎯

**口诀：** STC 网络转折频率都是 $\omega_0=1/RC$，且 $\omega_{3dB}=\omega_0$；低通压高频、高通压低频。

</aside>

## 8️⃣ 典型频率响应

- **(a) 电容耦合放大器：** 低频被耦合电容滚降（low-frequency roll-off）。
- **(b) 直接耦合放大器：** 直流也能放大，无低频滚降。
- **(c) 调谐/带通放大器：** 只在中心频段附近有增益。
- **高频截止：** 来自晶体管内部寄生电容；**低频滚降：** 来自级间耦合电容。

![Slide 18 — 三类典型频响（电容耦合/直接耦合/带通）与耦合电容（Fig 1.26/1.27）](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/slide_18.png)

Slide 18 — 三类典型频响（电容耦合/直接耦合/带通）与耦合电容（Fig 1.26/1.27）

## 🧩 本节总结

<aside>
🧠

**放大器基础四句话：**

1. **增益：** $A_p=A_vA_i$；电压/电流用 $20\log$、功率用 $10\log$；负增益=反相。
2. **模型：** 二端口 $(A_{vo},R_i,R_o)$，接源接载有两次分压衰减 → 电压放大要 $R_i$ 大、$R_o$ 小；级联用输入级抬阻抗、输出级降阻抗、中间级提增益。
3. **四类放大器：** 电压(V→V)/电流(I→I)/跨导(V→I)/跨阻(I→V)，理想输入输出阻抗各异。
4. **频响：** $T(\omega)=V_o/V_i$，波特图看 $20\log|T|$ vs $\log\omega$，带宽=增益跌 3dB 范围；STC 低通/高通转折 $\omega_0=1/RC=\omega_{3dB}$。
</aside>

## 📎 原始 Slides

[EE115 Lecture2.pdf](EE115%20Lecture2%20%E2%80%94%20Amplifier%20Basics%EF%BC%88%E6%94%BE%E5%A4%A7%E5%99%A8%E5%9F%BA%E7%A1%80%EF%BC%89/EE115_Lecture2.pdf)