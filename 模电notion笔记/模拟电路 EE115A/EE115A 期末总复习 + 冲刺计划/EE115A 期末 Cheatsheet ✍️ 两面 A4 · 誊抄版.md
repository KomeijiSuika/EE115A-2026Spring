# EE115A 期末 Cheatsheet ✍️ 两面 A4 · 誊抄版

<aside>
✍️

**用法：** 这是按「两面 A4」排版的**誊抄底稿** —— 左右两大块对应 **面 1 / 面 2**。每节都标了 🔴必背 / 🟠重点，并直达对应课件。誊抄时**先抄公式清单 + 电路骨架图**，判断题考点用一行短句压在边角。

⚠️ **我无法直接生成手写体图片**；下面用 **ASCII 电路骨架图**（可照着画）＋ **公式** ＋ **课件直达链接**代替。需要原图请点开对应 Lecture 页截图誊抄。

**考试题型：** 仅 **判断正误 ＋ 计算**；范围重心 = 本征增益 → Cascode → 差分对 → 反馈/运放。

</aside>

---

# 📄 面 1 / Side A

## ① 小信号「原子」 $g_m,\ r_o,\ A_0$ 🔴 (L7–L8)

<aside>
🔑

**所有计算题的起点。** 课件：[EE115A 期末复习 — 电流镜 & 本征增益（L11）](EE115A%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%20%E2%80%94%20%E7%94%B5%E6%B5%81%E9%95%9C%20&%20%E6%9C%AC%E5%BE%81%E5%A2%9E%E7%9B%8A%EF%BC%88L11%EF%BC%89.md)

</aside>

- $g_m=\dfrac{2I_D}{V_{OV}}=\sqrt{2\mu C_{ox}\tfrac{W}{L}I_D}$
- $r_o=\dfrac{|V_A|}{I_D}=\dfrac{1}{\lambda I_D}$
- $A_0=g_m r_o=\dfrac{2|V_A|}{V_{OV}}=\dfrac{2V_A' L}{V_{OV}}$ → **与** $I_D$ **一阶无关**
- 提 $A_0$：↑$L$（$V_A\propto L$）、↓$V_{OV}$（代价：↓$g_m$、↓速度、↓摆幅）
- dB：$A_0=100\Rightarrow 40\,\text{dB}$；$A_0^2=10^4\Rightarrow 80\,\text{dB}$

## ⓪ 增益符号定义 Gain Definitions 🔴 (L9)

<aside>
📐

一个放大器从信号源到输出要过**两次分压**：$v_{sig}$ →【输入分压 $\tfrac{R_{in}}{R_{in}+R_{sig}}$】→ $A_{vo}$ →【输出分压 $\tfrac{R_L}{R_L+R_o}$】→ $v_o$。符号区别全在「从哪算到哪 + 带不带负载」。

</aside>

- 阻抗符号：$R_{in}$ 输入电阻、$R_o$ 输出电阻、$R_{sig}$ 源内阻、$R_L$ 负载

| Symbol | Name | Definition |
| --- | --- | --- |
| $A_{vo}$ | Open-circuit voltage gain | $v_o/v_i$ at $R_L\to\infty$（空载） |
| $A_v$ | Loaded voltage gain | $\dfrac{v_o}{v_i}=A_{vo}\dfrac{R_L}{R_L+R_o}$ |
| $G_v$ | Overall voltage gain | $\dfrac{v_o}{v_{sig}}=\dfrac{R_{in}}{R_{in}+R_{sig}}A_v$ |
| $G_{vo}$ | Overall open-circuit gain | $\dfrac{R_{in}}{R_{in}+R_{sig}}A_{vo}$ |
| $G_m$ | Short-circuit transconductance | $i_{o,sc}/v_i=A_{vo}/R_o$ |
| $A_{is}$ | Short-circuit current gain | $i_o/i_i$（输出短路） |
| $A_0$ | Intrinsic gain | $g_m r_o$（单管·理想负载上限） |
- 关系链：$G_v=\dfrac{R_{in}}{R_{in}+R_{sig}}\,A_{vo}\,\dfrac{R_L}{R_L+R_o}$（输入分压 × 空载增益 × 输出分压）
- $A_{vo}$ = $A_v$ 在 $R_L\to\infty$ 的特例；$A_0=g_m r_o$ 是单管理想情况下的 $A_{vo}$ 上限
- ⚠️ 考试易混：$A_v$ 只从放大器**输入端**算；$G_v$ 要**再乘输入分压**（含 $R_{sig}$）。源内阻越大，$G_v$ 比 $A_v$ 越小。

## ①′ 完整小信号模型 + 电容 + $f_T$ 🔴 (L8)

<aside>
🔬

饱和区线性化：把每个管换成「输入开路 + 三个元件」。

</aside>

- 三元件：受控源 $g_m v_{gs}$ ‖ 输出电阻 $r_o$ ‖ 体效应源 $g_{mb}v_{bs}$
- 体效应：$g_{mb}=\eta g_m$，$\eta=\dfrac{\gamma}{2\sqrt{2\phi_F+V_{SB}}}\approx 0.1\!\sim\!0.3$；源、体短接时 $g_{mb}=0$
- 电容：$C_{gs},C_{gd},C_{db}$；特征频率 $f_T=\dfrac{g_m}{2\pi(C_{gs}+C_{gd})}$
- Miller 效应：反相增益 $A$ 下 $C_{gd}$ 等效到输入 $C_{gd}(1+|A|)$ → 拉低主极点（cascode 可抑制）

## ①″ MOSFET 三区 + I-V 🟠 (L7)

| Region | Condition | $I_D$ |
| --- | --- | --- |
| Cutoff | $V_{GS}<V_t$ | $\approx 0$ |
| Triode | $V_{GS}>V_t,\ V_{DS}<V_{OV}$ | $\mu C_{ox}\tfrac{W}{L}\!\left[V_{OV}V_{DS}-\tfrac{V_{DS}^2}{2}\right]$ |
| Saturation | $V_{GS}>V_t,\ V_{DS}\ge V_{OV}$ | $\tfrac12\mu C_{ox}\tfrac{W}{L}V_{OV}^2(1+\lambda V_{DS})$ |
- $V_{OV}=V_{GS}-V_t$；放大必须在**饱和区**
- 深三极管（小 $V_{DS}$）等效电阻：$r_{DS}=1/[\mu C_{ox}(W/L)V_{OV}]$（做开关 / 压控电阻）

## ①‴ 单级三拓扑 CS / CG / CD 🔴 (L9)

| Topology | Gain | $R_{in}$ | $R_{out}$ | Role |
| --- | --- | --- | --- | --- |
| Common-Source (CS) | $-g_m(R_D\|r_o)$ | $\infty$ | $R_D\|r_o$ | Voltage gain stage |
| Common-Gate (CG) | $+g_m(R_D\|r_o)$ | $\approx 1/g_m$ | High (cascode) | Current buffer |
| Common-Drain (CD) | $\dfrac{g_m R_L}{1+g_m R_L}\lesssim 1$ | $\infty$ | $\approx 1/g_m$ | Voltage buffer |
- 阻抗变换（cascode 之本）：源极串 $R_S$ 从漏看进去 → $R_{out}\approx r_o(1+g_m R_S)$
- 含体效应：把 $g_m\to g_m+g_{mb}$

## ② 电流镜 Current Mirror 🟠 (L11，期中已考·基础)

```
   IREF↓            IO↓
    │                │
    ├───────┬────────┤  (栅极相连, 同 VGS)
   ┌┴┐ VG  │       ┌┴┐
   │M1├────┘       │M2│
   └┬┘ diode接      └┬┘  输出管须在饱和区
   GND (VDS=VGS)    GND
IO/IREF = (W/L)2 / (W/L)1
```

- 镜像比：$\dfrac{I_O}{I_{REF}}=\dfrac{(W/L)_2}{(W/L)_1}$（忽略沟长调制）
- 含沟长调制：$I_O=I_{REF}\dfrac{(W/L)_2}{(W/L)_1}\dfrac{1+\lambda V_{DS2}}{1+\lambda V_{DS1}}$
- 输出阻抗：$R_{out}=r_{o2}=1/(\lambda I_O)$；**cascode 镜** $R_{out}\approx g_m r_o^2$
- 误差主因：$V_{DS}$ 失配（系统）+ 器件失配（随机）

## ③ 本征增益 & 有源负载 🔴 (L11，期中未考·新增重点)

- 理想电流源负载（$R_L\to\infty$）：单级 $A_v=-A_0$
- 有源负载 CS（PMOS 镜负载，$r_{o1}=r_{o2}=r_o$）：$A_v=-g_m(r_{o1}\|r_{o2})=-\dfrac{A_0}{2}$
- ⚠️ 别把 $-g_m r_o$ 当有源负载实际增益（实际 $-A_0/2$）

## ④ Cascode 放大器 🔴 (L12)

<aside>
🏗️

课件：[EE115A 期末复习 — Cascode 放大器（L12）](EE115A%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%20%E2%80%94%20Cascode%20%E6%94%BE%E5%A4%A7%E5%99%A8%EF%BC%88L12%EF%BC%89.md) · [EE115 Lecture12 — Cascode Amplifier](../EE115%20Lecture12%20%E2%80%94%20Cascode%20Amplifier.md)

</aside>

```
       VDD
        │
    [cascode 负载]   ← 上下都要 cascode!
        │──→ vout
       ┌┴┐
 VB ──┤M2│  共栅 (抬阻抗 A0 倍)
       └┬┘
        │   ← M1漏端摆动小 → 抑 Miller → 带宽好
       ┌┴┐
vin ──┤M1│  共源
       └┬┘
       GND
 Rout ≈ gm·ro² = A0·ro ;  Av = -A0²
```

- 共栅抬阻抗：$R_{out}=r_{o2}+r_{o1}+g_{m2}r_{o2}r_{o1}\approx g_m r_o^2=A_0 r_o$
- 增益：$A_v=-g_{m1}R_{out}$ → 理想负载 $-A_0^2$；对称 cascode 负载 $-A_0^2/2$
- ⚠️ **必须上下都 cascode**，否则被另一侧 $r_o$ 拉回 $-A_0$
- 代价 = **摆幅**（多一个 $V_{OV}$）；红利 = 高增益 + 高带宽
- **Telescopic**：增益/功耗优、摆幅小；**Folded**：摆幅 & 输入共模大（不是为提增益）
- 共栅看进源极：$R_{in,CG}=\dfrac{r_o+R_L}{1+g_m r_o}\approx 1/g_m$（$R_L$ 小时）
- 输出摆幅预算：telescopic 输出摆幅 $\approx V_{DD}-(V_{OV1}+V_{OV2}+V_{OV3}+V_{OV4})$（4 管各扣一个 $V_{OV}$）

### Cascode 电流镜（高精度偏置）🟠

- 标准 cascode 镜 $R_{out}\approx g_m r_o^2$，钳住 $V_{DS}$ → 镜像更准
- 代价：多一个 $V_{DS,sat}$ 余量 → 改进 = **wide-swing（低压）cascode 镜**

---

# 📄 面 2 / Side B

## ⑤ 差分对 Differential Pair 🔴 (L13–L14)

<aside>
⚖️

课件：[EE115A 期末复习 — 差分对（L13–L14）](EE115A%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%20%E2%80%94%20%E5%B7%AE%E5%88%86%E5%AF%B9%EF%BC%88L13%E2%80%93L14%EF%BC%89.md) · [EE115 Lecture13 — Cascode & Differential Amplifiers](../EE115%20Lecture13%20%E2%80%94%20Cascode%20&%20Differential%20Amplifier.md) · [EE115 Lecture14 — Differential Amplifiers 2](../EE115%20Lecture14%20%E2%80%94%20Differential%20Amplifiers%202.md)

</aside>

```
        VDD
    ┌────┴────┐
  [镜负载]  [镜负载]   ← 电流镜负载: 双端转单端
    │         │
    │        ●─→ vout (单端)
   ┌┴┐       ┌┴┐
v1┤M1│       │M2├v2
   └┬┘       └┬┘
    └────┬────┘   ← 公共源点: 差模 = 虚地
       [ISS]      尾电流源 (输出阻抗 RSS)
         │
        VSS
```

- 差模（半电路）：$A_d=-g_m(R_D\|r_o)$；有源负载 $A_d=g_m(r_{on}\|r_{op})$
- 共模（单端）：$A_{cm}\approx -\dfrac{R_D}{2R_{SS}}$
- $\text{CMRR}=|A_d/A_{cm}|\propto g_m R_{SS}$ → 尾源 cascode 抬 $R_{SS}$ 提 CMRR
- **电流镜负载**：双端转单端 + $G_m=g_{m1,2}$（**满** $g_m$，不是 $g_m/2$）
- 线性满摆：$|v_{id}|\ge\sqrt{2}\,V_{OV}$
- 失调：$V_{OS}\approx\Delta V_t+\dfrac{V_{OV}}{2}\!\left[\dfrac{\Delta(W/L)}{W/L}+\dfrac{\Delta R_D}{R_D}\right]$
- 大信号：差模电压把 $I_{SS}$ 在两管间分配；小信号线性 $i_d=g_m v_{id}/2$，当 $|v_{id}|\ge\sqrt2\,V_{OV}$ 时电流全偏一侧（limiting）
- 跨导：电阻负载单端 $G_m=g_m/2$；**电流镜负载** $G_m=g_m$（满）
- ICMR：$V_{ICM,\max}\approx V_{DD}-|V_{OV,load}|-V_{OV}+V_{tn}$；$V_{ICM,\min}\approx V_{SS}+V_{CS,sat}+V_{OV}+V_{tn}$
- 提 CMRR / 抗电源噪声：尾电流源用 **cascode** 抬 $R_{SS}$

## ⑥ 负反馈 Feedback 🔴 (L15–L16)

<aside>
🔁

课件：[EE115A 期末复习 — 反馈与运放（L15–L16）](EE115A%20%E6%9C%9F%E6%9C%AB%E5%A4%8D%E4%B9%A0%20%E2%80%94%20%E5%8F%8D%E9%A6%88%E4%B8%8E%E8%BF%90%E6%94%BE%EF%BC%88L15%E2%80%93L16%EF%BC%89.md) · [EE115 Lecture15 — Feedback 基础与负反馈四大优势](../EE115%20Lecture15%20%E2%80%94%20Feedback%20%E5%9F%BA%E7%A1%80%E4%B8%8E%E8%B4%9F%E5%8F%8D%E9%A6%88%E5%9B%9B%E5%A4%A7%E4%BC%98%E5%8A%BF.md) · [EE115 Lecture16 — Feedback 基础与 Series-Shunt 拓扑](../EE115%20Lecture16%20%E2%80%94%20Feedback%20%E5%9F%BA%E7%A1%80%E4%B8%8E%20Series-Shunt%20%E6%8B%93%E6%89%91.md)

</aside>

- 核心：$A_f=\dfrac{A}{1+A\beta}$；环路增益 $T=A\beta$；去敏因子 $(1+A\beta)$
- 深反馈（$A\beta\gg1$）：$A_f\approx 1/\beta$
- 展带宽：$\omega_f=\omega_0(1+A\beta)$；**GBW 守恒** $A_0\omega_0=A_f\omega_f$
- 四大好处（全靠去敏因子）：稳增益 / 展带宽 / 降失真 / 改 $R_{in},R_{out}$；代价：↓增益、可能振荡
- **口诀**：输入**串联**↑$R_{in}$，**并联**↓$R_{in}$；采样**电压(shunt)**↓$R_{out}$，采样**电流(series)**↑$R_{out}$

| Topology | Amplifier | $R_{in}$ | $R_{out}$ |
| --- | --- | --- | --- |
| Series-Shunt | Voltage $A_v$ | ↑ | ↓ |
| Shunt-Series | Current $A_i$ | ↓ | ↑ |
| Series-Series | Transconductance $G_m$ | ↑ | ↑ |
| Shunt-Shunt | Transresistance $R_m$ | ↓ | ↓ |
- ❓ Q：Series-Shunt 就是「串联-并联」吗？
    
    **字面对：** Series = 串联，Shunt = 并联。但这两个词不是随便并排，而是 **[输入端混合方式]-[输出端采样方式]**：
    
    - **第一个词 = 输入比较**：Series = 反馈信号**串联**进输入回路 → 比较的是**电压** → ↑$R_{in}$
    - **第二个词 = 输出采样**：Shunt = 反馈网络**并联**在输出 → 采样的是**电压** → ↓$R_{out}$
    - 所以 **Series-Shunt = 输入串联(比电压) + 输出并联(采电压) = 电压放大器**（又叫 voltage-voltage / 电压-电压反馈）。
    
    **「串联 / 并联」到底指电路怎么接（不是元件，是接法）：**
    
    - **串联(series)** = 反馈元件与目标支路**首尾相接、共用同一电流**（KVL 回路，电压相加减）。在**输入端**→反馈电压与源电压在一个回路里相减 → **比电压**；在**输出端**→反馈网络与负载串成一路、被同一电流流过 → **采电流**。
    - **并联(shunt)** = 反馈元件与目标节点**接在同一节点、共用同一电压**（KCL 节点，电流相加减）。在**输入端**→反馈电流与源电流在节点处相减 → **比电流**；在**输出端**→反馈网络与输出节点并接 → **采电压**。
    - 一句话：**串联看回路(共电流)、并联看节点(共电压)**。
    
    **最直观例子（Series-Shunt）：** 同相运放 + 电阻分压反馈 —— 分压网络**并联**在 $v_{out}$ 上取样输出电压(shunt)，反馈电压送回反相输入端、与 $v_{in}$ 在差分回路里**串联**相减(series)。
    
    **口诀验证：** 输入串联 → ↑$R_{in}$；采样电压(shunt) → ↓$R_{out}$，正好对上表里 Series-Shunt 那一行。
    
    ⚠️ 别只记「串联/并联」字面，要记清**前缀=输入、后缀=输出**，否则四种拓扑会张冠李戴。
    

### 反馈分析三步法 🔴

1. **判拓扑**：看输出采样（并联 = 采电压 / 串联 = 采电流）+ 输入比较（串联 = 比电压 / 并联 = 比电流）
2. **求** $\beta$：反馈网络的反馈系数（量纲与 $A$ 互逆）
3. **算** $A$（含反馈网络加载）→ $A_f=\dfrac{A}{1+A\beta}$；$R_{in},R_{out}$ 按口诀 ×/÷ $(1+A\beta)$

### 稳定性 & 相位裕度 🟠

- 环路增益 $T(j\omega)=A\beta$；穿越频率 $\omega_c$ 处 $|T|=1$
- 相位裕度 $PM=180°+\angle T(\omega_c)$；经验 $PM\ge 60°$ 稳、过冲小；$PM\to0$ 振荡
- 多极点 → **Miller 补偿做极点分裂** 拉开主 / 次极点 → 提 PM

## ⑦ 两级 CMOS 运放 + Miller 补偿 🔴 (L16)

```
vin± →[差分对+电流镜负载]→[CS 第二级]→ vout
            A1                A2
                    ╠═Cc═╣  ← Miller补偿: 极点分裂
  A = A1·A2 ; 闭环 Af = A/(1+Aβ)
```

- 一级 = 差分对 + 电流镜负载（高增益 + 双转单）；二级 = 共源增益级；$A=A_1A_2$
- **Miller 补偿**：跨接 $C_c$ → 主极点压低、次极点推高 → ↑相位裕度、保稳定（**不是降增益**）
- **单位增益带宽 GBW**：$\omega_t=\dfrac{g_{m1}}{C_c}$（$g_{m1}$ = 输入级跨导）
- **压摆率 Slew Rate**：$SR=\dfrac{I_{SS}}{C_c}$（大信号受尾电流限）
- 极点：主极点 $\omega_{p1}\approx\dfrac{1}{g_{m2}R_1 R_2 C_c}$（Miller 压低）；次极点 $\omega_{p2}\approx\dfrac{g_{m2}}{C_L}$（推高）
- 右半平面零点 $\omega_z=\dfrac{g_{m2}}{C_c}$ → 串调零电阻 $R_z$ 消除
- 其它非理想：$V_{OS}$、有限 CMRR / PSRR、有限 $A_0$ 与带宽

## ⑧ 解题套路 & 速查p

<aside>
🧰

**小信号计算流程：** ①确认所有管在**饱和区** → ②算 $g_m=2I_D/V_{OV}$、$r_o=V_A/I_D$ → ③画小信号等效 → ④KCL/KVL 或半电路 → ⑤组合 $R_{out}$、$G_m$ 得 $A_v=-G_m R_{out}$。

</aside>

- 关键数：$g_m=2I_D/V_{OV}$、$r_o=V_A/I_D$、$A_0=2V_A/V_{OV}$、$I_O/I_{REF}=(W/L)_2/(W/L)_1$、$A_f=A/(1+A\beta)$
- 增益量级：单级 $A_0$ → 有源负载 $A_0/2$ → cascode $A_0^2$ → 两级运放 $A_0^2\!\sim\!A_0^3$
- 判断题高频陷阱（✅ 为正确表述）：
    1. cascode **必须上下都堆**才得 $A_0^2$；只堆一侧退回 $A_0$
    2. 电流镜负载差分对 $G_m=g_m$（满），**不是** $g_m/2$
    3. 负反馈是**降增益换带宽 / 稳定**，GBW 守恒
    4. Miller 补偿是**移极点保相位**，不降增益
    5. $A_0$ 与 $I_D$ **一阶无关**（加大 $I_D$ 不提 $A_0$）
    6. 差模时公共源点是**交流虚地**，非真接地
    7. 镜像比由 $W/L$ **比**定，与 $I_{REF}$ 无关
    8. 提 $A_0$ 靠 ↑$L$ / ↓$V_{OV}$
    9. 公式只在**饱和区**成立
    10. folded cascode 为**摆幅 / 共模**，非提增益

### 📐 公式总表（考前最后一扫）

| Quantity | Formula |
| --- | --- |
| Transconductance $g_m$ | $2I_D/V_{OV}=\sqrt{2\mu C_{ox}(W/L)I_D}$ |
| Output resistance $r_o$ | $V_A/I_D=1/(\lambda I_D)$ |
| Intrinsic gain $A_0$ | $g_m r_o=2V_A/V_{OV}$ |
| Active-load CS | $-g_m(r_{on}\|r_{op})=-A_0/2$ |
| Cascode | $R_{out}=g_m r_o^2$, $A_v=-A_0^2$ |
| Current mirror | $I_O/I_{REF}=(W/L)_2/(W/L)_1$ |
| Diff / Common mode | $A_d=g_m(r_{on}\|r_{op})$, $A_{cm}=-R_D/2R_{SS}$ |
| CMRR | $\propto g_m R_{SS}$ |
| Feedback | $A_f=A/(1+A\beta)$, $\omega_f=\omega_0(1+A\beta)$ |
| Op-amp GBW / SR | $\omega_t=g_{m1}/C_c$, $SR=I_{SS}/C_c$ |

### 🔢 单位 & 量级速查

- $1\,\text{mA/V}=1\,\text{mS}$；$g_m r_o$ 无量纲
- $\text{dB}=20\log_{10}|A|$：×10→20 dB，×100→40 dB，×$10^4$→80 dB
- 典型量级：$g_m\sim$ mA/V，$r_o\sim 10\!-\!100\,\text{k}\Omega$，$A_0\sim 20\!-\!100$，cascode $A_0^2\sim 10^3\!-\!10^4$

[9m= 10.pdf](EE115A%20%E6%9C%9F%E6%9C%AB%20Cheatsheet%20%E2%9C%8D%EF%B8%8F%20%E4%B8%A4%E9%9D%A2%20A4%20%C2%B7%20%E8%AA%8A%E6%8A%84%E7%89%88/9m_10.pdf)