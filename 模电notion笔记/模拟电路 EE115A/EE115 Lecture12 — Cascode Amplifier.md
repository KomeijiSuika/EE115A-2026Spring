# EE115 Lecture12 — Cascode Amplifier

<aside>
📌

**本节主题：** Building Blocks of IC Amplifier — **Cascode Amplifier**

**参考书：** Sedra/Smith — pp. 525–556（Chapter 8 §8.3：Cascode）

**核心脉络：** 复习 current mirror + intrinsic gain → active-load CS 的转移曲线（理解为什么增益 = $A_0$ 拿不到） → “加一个 K 黑盒赋能负载」思路 → 两个提高输出阻的金场竖：**源极退化 (CS+Rs)** 与 **共栅 (CG)** → “上看下 $\div A_0$、下看上 $\times A_0$” → **MOS Cascode** 拿到 $A_0^2$ → 上负载 cascode化 → 两边都 cascode 拿 $\tfrac{1}{2}A_0^2$。

</aside>

## 📅 课程行政信息

- Homework 本节为占位（“Homework x — due April xx”），听老师另行发布。

---

## 1️⃣ 快速复习（slides 3–7）

详见 [EE115 Lecture11 — IC Building Blocks: Current Mirror & Intrinsic Gain](EE115%20Lecture11%20%E2%80%94%20IC%20Building%20Blocks%20Current%20Mirro.md)。三个必记公式：

$$
g_m = \frac{2I_D}{V_{OV}},\quad r_o = \frac{V_A'L}{I_D},\quad A_0 = g_m r_o = \frac{2V_A'L}{V_{OV}}
$$

现实 active-load CS 仅能拿 $A_v = -g_{m1}(r_{o1}\|r_{o2})\approx -\tfrac{1}{2}A_0$。

---

## 2️⃣ active-load CS 的转移曲线（slide 8）

**拓扑**：NMOS 主管 $Q_1$（低，输入 $v_I$）+ PMOS 镜负载 $Q_2$（高）+ 上面一个 $Q_3$ diode-connected 取 $I_{\text{REF}}$。

[Slide 8 — Graphical Solution of Voltage Transfer Curve](https://app.notion.com)

Slide 8 — Graphical Solution of Voltage Transfer Curve

### 四个区间（slide 8 右下图）

| 区间 | $v_I$ 范围 | $Q_1$ | $Q_2$ | $v_O$ 状态 |
| --- | --- | --- | --- | --- |
| **I** | $v_I < V_{tn}$ | cutoff | triode | $v_O = V_{DD}$ |
| **II** | $V_{IA}$ 附近 | saturation | triode | $v_O$ 上拐点 $V_{OA} = V_{DD}-|V_{OV2}|$ |
| **III** | $V_{IA}\le v_I\le V_{IB}$ | saturation | saturation | **高增益陕区**，斜率 $-A_v$ |
| **IV** | $v_I > V_{IB}$ | triode | saturation | $v_O$ 下拐点 $V_{OB}$ |

### 各关键点位置

- $V_{OA} = V_{DD} - |V_{OV2}|$：$Q_2$ 刚别离 triode。
- $V_{OB}$：$Q_1$ 刚别走进 triode，$V_{IB} = V_{OB}+V_{tn}$。
- 区间 III 是唯一能拿到高增益的区域。

<aside>
⚠️

**踩坑**：要在实际电路中拿到高增益，必须把工作点调到区间 III。设计偏置时要使 $V_{DS1} > V_{OV1}$ 且 $V_{SD2} > |V_{OV2}|$。原 slide 右上图（$i\!-\!v_{SG}$ 曲线）里 $Q_2$ 从 triode 转到 saturation 的拐点坐标在 $v_{SG}-|V_{tp}|$。

</aside>

---

## 3️⃣ 如何提高电压增益？引入“K 黑盒”思想（slide 9）

[Slide 9 — How To Increase Voltage Gain?](https://app.notion.com)

Slide 9 — How To Increase Voltage Gain?

原始的 CS：从 $Q_1$ 漏看进去，小信号等效 = $g_{m1}v_i$ 与 $r_{o1}$ 并联 → 增益 = $-g_{m1}r_{o1} = -A_0$。

加上一个「黑盒 K」：让输出看到的阻抗从 $r_{o1}$ 变为 $K\cdot r_{o1}$ → 增益跟着变为 $-g_{m1}(Kr_{o1})$。

<aside>
🔑

**思想原型**：提高 $R_{\text{out}}$ 是提高增益的主路。“黑盒 K” 最自然的实现 → **再叠一个 MOSFET 作共栅 (cascode)**。

</aside>

- Q：Slide 9 这个等效到底怎么来的？
    
    **一句话：** Slide 9 不是在做一个严格的新电路变换，而是在做“小信号输出阻抗抽象”：把晶体管漏端看到的本征输出电阻 $r_{o1}$，替换成一个更大的等效输出电阻 $K r_{o1}$，然后用同一个 CS 增益公式重算。
    
    原始 CS 小信号模型可看成：
    
    - $Q_1$ 的受控电流源：$g_{m1}v_i$
    - 从 drain 到 AC ground 的输出电阻：$r_{o1}$
    
    所以如果输出端没有别的有限负载：
    
    $$
    v_o = -g_{m1}v_i r_{o1}
    $$
    
    $$
    A_v=\frac{v_o}{v_i}=-g_{m1}r_{o1}=-A_0
    $$
    
    Slide 9 的黑盒 $K$ 做的事情是：**不改变输入侧的跨导** $g_{m1}$**，只把输出端等效电阻放大**：
    
    $$
    r_{o1}\quad\Longrightarrow\quad R_{\text{out,eq}}=K r_{o1}
    $$
    
    于是同一个公式直接变成：
    
    $$
    v_o = -g_{m1}v_i(Kr_{o1})
    $$
    
    $$
    A_v=-g_{m1}Kr_{o1}=-K A_0
    $$
    
    这里负号来自 common-source：输入变大 → 漏电流变大 → 输出节点被拉低。
    
    **为什么可以只换电阻？** 因为中频小信号下，CS 的电压增益本质就是：
    
    $$
    A_v\approx -G_m R_{\text{out}}
    $$
    
    而这个结构里 $G_m$ 主要由下面的输入管 $Q_1$ 决定，slide 9 想强调的是：如果能用某个结构把 drain 端看到的 $R_{\text{out}}$ 乘上 $K$，增益就同步乘上 $K$。
    
    **后面 cascode 的意义：** 共栅管正是这个黑盒 $K$ 的物理实现。对 MOS 来说，$K$ 大约就是本征增益：
    
    $$
    K\approx g_m r_o=A_0
    $$
    
    所以一层 cascode 会把：
    
    $$
    -A_0\quad\Longrightarrow\quad -A_0^2
    $$
    
- Q：为什么等效电阻和受控电流源是并联？
    
    **读者的理解方向是对的：** 求某个端口的等效电阻时，通常就是在这个端口加测试电压 $v_x$，把独立源置零，然后算流入电流 $i_x$，最后：
    
    $$
    R_{\text{eq}}=\frac{v_x}{i_x}
    $$
    
    但这里要分清两件事：
    
    1. **求输出电阻** $R_{\text{out}}$ **时**：把输入信号置零，也就是 gate 小信号接地，算 drain 端口看到的电阻。
    2. **画放大器小信号等效模型时**：输入 $v_i$ 没有置零，所以受控电流源 $g_m v_i$ 还在。
    
    ---
    
    ### 1. 为什么 $r_o$ 本来就在 drain-source 之间？
    
    MOS 在饱和区时，电流不是完全不随 $v_{DS}$ 变，而是有一点 channel-length modulation：
    
    $$
    i_D \approx I_D(1+\lambda v_{DS})
    $$
    
    小信号化以后：
    
    $$
    i_d = g_m v_{gs}+g_{ds}v_{ds}
    $$
    
    其中：
    
    $$
    g_{ds}=\frac{1}{r_o}
    $$
    
    这句话等价于说：drain 和 source 之间有一个电导 $g_{ds}$，也就是一个电阻 $r_o$。
    
    所以 MOS 小信号模型天然就是：
    
    - 一个从 drain 到 source 的受控电流源 $g_m v_{gs}$；
    - 一个从 drain 到 source 的电阻 $r_o$。
    
    它们接在**同一对节点** drain 和 source 之间，所以拓扑上就是并联。
    
    ---
    
    ### 2. “并联”的判断不是因为计算方法，而是因为它们接在同两个节点上
    
    两个元件并联的定义是：它们两端接在同样的两个节点上。
    
    在 CS 里 source 是 AC ground，所以：
    
    - 受控电流源 $g_m v_i$：从 drain 指向 source，也就是从 output 到 ground；
    - $r_o$：也从 drain 到 source，也就是从 output 到 ground。
    
    因此它们都跨在：
    
    $$
    \text{output node}\quad \leftrightarrow \quad \text{AC ground}
    $$
    
    所以它们并联。
    
    ---
    
    ### 3. 求 $R_{\text{out}}$ 时为什么要把输入置零？
    
    因为输出电阻问的是：
    
    > 输出端口自己有多“硬”？如果我从外面推一下输出节点，它会产生多少反作用电流？
    > 
    
    这时要排除输入信号造成的电流，所以把独立输入 $v_i$ 置零。
    
    CS 中：
    
    $$
    v_i=0 \Rightarrow v_{gs}=0
    $$
    
    于是：
    
    $$
    g_m v_{gs}=0
    $$
    
    受控电流源关掉，输出端口只看到 $r_o$：
    
    $$
    R_{\text{out}}=r_o
    $$
    
    但注意：这只是**求输出电阻的过程**。等你画完整放大器模型、计算增益时，输入 $v_i$ 又打开了，于是受控电流源 $g_m v_i$ 又出现。
    
    ---
    
    ### 4. 为什么计算增益时是 $v_o=-(g_m v_i)r_o$？
    
    因为输出节点同时连着：
    
    - 往下拉的受控电流源 $g_m v_i$；
    - 到地的电阻 $r_o$。
    
    受控电流源把电流拉走，这个电流必须经过 $r_o$ 形成电压变化，所以：
    
    $$
    v_o=-(g_m v_i)r_o
    $$
    
    负号表示输出节点电压下降。
    
    ---
    
    ### 5. Slide 9 的 $K r_o$ 是同一个位置的电阻被放大
    
    Slide 9 不是说多了一个和受控源串联的东西，而是说：
    
    原来输出节点到 AC ground 的等效电阻是：
    
    $$
    R_{\text{out}}=r_o
    $$
    
    加了某种黑盒结构以后，从同一个 output node 看进去：
    
    $$
    R_{\text{out}}=K r_o
    $$
    
    所以完整小信号模型仍然是：
    
    - 受控电流源 $g_m v_i$；
    - 输出等效电阻 $K r_o$；
    
    二者仍然接在 output 和 AC ground 之间，因此仍然并联。
    
- Q：能不能先变成受控电压源串联内阻？为什么会推出 $g_m v_i/K$？
    
    可以换成 Thevenin / Norton 的思路看，但要注意：**变换前后必须保持同一个端口的开路电压和输出电阻同时一致**。
    
    Norton 形式：
    
    - 电流源：$I_N=g_m v_i$
    - 并联电阻：$R_N=K r_o$
    
    等效成 Thevenin 形式时：
    
    $$
    V_{TH}=I_N R_N=(g_m v_i)(K r_o)
    $$
    
    所以正确的受控电压源应该是：
    
    $$
    V_{TH}=g_m v_i K r_o
    $$
    
    串联电阻是：
    
    $$
    R_{TH}=K r_o
    $$
    
    再从 Thevenin 变回 Norton：
    
    $$
    					I_N=\frac{V_{TH}}{R_{TH}}
    =\frac{g_m v_i K r_o}{K r_o}
    =g_m v_i
    $$
    
    所以电流源不会变成 $g_m v_i/K$。
    
    ---
    
    如果推成 $g_m v_i/K$，通常是因为做了这一步：
    
    $$
    V_{TH}=g_m v_i r_o
    $$
    
    但同时又把串联电阻换成：
    
    $$
    R_{TH}=K r_o
    $$
    
    于是：
    
    $$
    I_N=\frac{g_m v_i r_o}{K r_o}=\frac{g_m v_i}{K}
    $$
    
    这在数学上没错，但它已经不是 slide 9 想表达的那个放大器了。它表示的是：
    
    - 开路输出电压仍然只有 $g_m v_i r_o$；
    - 输出电阻却变成 $K r_o$；
    - 因此等效 transconductance 反而被压小到 $g_m/K$。
    
    也就是说，这个模型不是“提高输出电阻同时保持输入管跨导不变”，而是“提高电阻但把 Norton 电流源同步削小”。那增益不会提高。
    
    ---
    
    Slide 9 的假设是：
    
    $$
    g_m v_i\quad \text{不变}
    $$
    
    只把输出端看到的电阻改成：
    
    $$
    r_o\rightarrow K r_o
    $$
    
    因此开路输出电压必须跟着变大：
    
    $$
    V_{oc}=-(g_m v_i)(K r_o)
    $$
    
    这才对应：
    
    $$
    A_v=-g_m K r_o=-K A_0
    $$
    
    **结论：** 如果用受控电压源串联内阻看，电压源的大小也要一起变成 $g_m v_i K r_o$。不能保留原来的 $g_m v_i r_o$，再单独把内阻改成 $K r_o$。
    
- Q：在这种等效里，大信号输入也可以等效成地吗？
    
    **短答：不能把“大信号输入本身”当成地；能当成 AC ground 的是 DC bias / 大信号工作点对应的固定电压源。**
    
    小信号分析会先把总量拆成：
    
    $$
    V_G(t)=V_{GQ}+v_g(t)
    $$
    
    其中：
    
    - $V_{GQ}$：大信号 / DC bias，用来决定工作点；
    - $v_g(t)$：小信号扰动，用来算增益、输入阻抗、输出阻抗。
    
    ---
    
    ### 1. 求小信号等效时，为什么 DC 电源可以当地？
    
    理想 DC 电压源的小信号变化量是 0：
    
    $$
    v_{DD}=0
    $$
    
    所以在小信号图里，$V_{DD}$ 接 AC ground。
    
    同理，如果 gate 是由一个理想固定偏置 $V_G$ 提供的，那么它的小信号变化量也是 0，所以 gate 在小信号图里接 AC ground。
    
    这不是说 DC 电压真的等于 0，而是说它的**小信号增量**等于 0。
    
    ---
    
    ### 2. 如果输入本身就是信号源，就不能直接接地
    
    如果 gate 接的是输入信号：
    
    $$
    V_G(t)=V_{GQ}+v_i(t)
    $$
    
    那么小信号图里：
    
    $$
    v_g(t)=v_i(t)
    $$
    
    此时 gate 不能接地，因为 $v_i$ 正是产生 $g_m v_i$ 的原因。
    
    如果把它接地，就等于令：
    
    $$
    v_i=0
    $$
    
    于是：
    
    $$
    g_m v_i=0
    $$
    
    放大器就没有受控电流源输出了。
    
    ---
    
    ### 3. 什么时候输入可以接地？
    
    只有在求输出电阻 $R_{\text{out}}$ 时，为了测输出端口本身的阻抗，要把独立输入信号置零：
    
    $$
    v_i=0
    $$
    
    如果输入是理想电压源，置零就是短路到 AC ground。
    
    所以：
    
    - **算增益时**：输入不能接地，保留 $v_i$；
    - **算输出电阻时**：输入置零，gate 接 AC ground；
    - **做 DC 工作点时**：不用小信号接地规则，而是用原来的大信号偏置电压。
    
    ---
    
    ### 4. 这和 slide 9 的关系
    
    Slide 9 里的 $g_m v_i$ 是小信号受控电流源。它来自输入扰动 $v_i$，所以在算增益时必须保留。
    
    但当我们单独求输出电阻 $R_o$ 时，要令：
    
    $$
    v_i=0
    $$
    
    于是受控源关掉，只测输出端看到的电阻：
    
    $$
    R_o=r_o\quad \text{或}\quad R_o=K r_o
    $$
    
    **一句话记忆：** 大信号 bias 决定工作点；小信号输入决定受控源；求 $R_o$ 时才把小信号输入置零。
    
- Q：这里算的 $R_{\text{out}}$ 都是小信号输出电阻吗？
    
    对，这里所有的 $R_{\text{out}}$ / $R_o$ 都是**小信号输出电阻**，也就是在某个 DC 工作点附近，从输出端口看进去的增量电阻。
    
    定义是：
    
    $$
    R_{\text{out}}=\frac{v_x}{i_x}
    $$
    
    这里的 $v_x$ 和 $i_x$ 都是**小信号测试量**，不是大信号 DC 电压 / 电流。
    
    更严格地说，它对应的是输出端口的微分电阻：
    
    $$
    r_{\text{out}}=\left.\frac{\partial v_o}{\partial i_o}\right|_{\text{bias point}}
    $$
    
    所以它依赖于当前 bias point，因为：
    
    $$
    g_m,\quad r_o,\quad A_0
    $$
    
    都由大信号工作点决定。
    
    ---
    
    ### 和大信号输出电阻有什么区别？
    
    大信号里，如果看 $I_D$-$V_{DS}$ 曲线，某个工作点附近的斜率决定小信号 $r_o$：
    
    $$
    r_o=\left(\frac{\partial I_D}{\partial V_{DS}}\right)^{-1}
    $$
    
    但我们不会直接用：
    
    $$
    \frac{V_{DS}}{I_D}
    $$
    
    来当这里的 $r_o$。那是 DC 比值，不是小信号增量电阻。
    
    ---
    
    ### 为什么这里一定是小信号？
    
    因为 slide 9 / 10 / 11 / cascode 推导里用到的：
    
    - $g_m v_i$
    - $r_o$
    - $K r_o$
    - $A_v=-g_mR_{\text{out}}$
    
    全部都是小信号模型里的量。也就是说，这些公式只描述“在工作点附近小幅扰动时，输出怎么变”。
    
    **一句话：** 大信号负责定工作点，小信号 $R_{\text{out}}$ 负责描述这个工作点附近输出端有多硬。
    
- Q：Slide 10 / 11 的 $R_{\text{out}}$、$R_{\text{in}}$ 公式对 PMOS / NMOS 都一样吗？
    
    **短答：一样，算出来的阻抗大小公式一样。** 只要是在同一个小信号拓扑下，用正的 $g_m$、正的 $r_o$ 表示器件参数，NMOS 和 PMOS 的阻抗变换公式是同形的。
    
    ---
    
    ### 1. Slide 10：source degeneration 看 $R_{\text{out}}$
    
    公式：
    
    $$
    R_{\text{out}}=r_o+R_s+g_m r_o R_s
    $$
    
    也可以写成：
    
    $$
    R_{\text{out}}=r_o+(1+g_m r_o)R_s
    $$
    
    当 $g_m r_o\gg 1$ 时：
    
    $$
    R_{\text{out}}\simeq g_m r_o R_s=A_0R_s
    $$
    
    这个公式对 NMOS / PMOS 都一样。因为它本质是在说：
    
    > 从 drain 看进去时，source 端的电阻会被本征增益 $A_0$ 放大后反映到 drain 端。
    > 
    
    PMOS 只是器件方向上下翻转、电流参考方向可能反过来；但小信号阻抗是正的，公式不变。
    
    ---
    
    ### 2. Slide 11：common-gate 看 $R_{\text{in}}$
    
    公式：
    
    $$
    R_{\text{in}}=\frac{r_o+R_L}{1+g_m r_o}
    $$
    
    当 $g_m r_o\gg 1$ 时：
    
    $$
    R_{\text{in}}\simeq \frac{1}{g_m}+\frac{R_L}{g_m r_o}
    $$
    
    也就是：
    
    $$
    R_{\text{in}}\simeq \frac{1}{g_m}+\frac{R_L}{A_0}
    $$
    
    这个对 NMOS common-gate 和 PMOS common-gate 也一样。它本质是在说：
    
    > 从 source 看进去时，drain 端负载 $R_L$ 会被 $A_0$ 除小后反映到 source 端。
    > 
    
    ---
    
    ### 3. 为什么 PMOS / NMOS 会同形？
    
    因为小信号模型里，NMOS 和 PMOS 都可以抽象成：
    
    - drain-source 之间有 $r_o$；
    - controlled current 大小由 $g_m v_{gs}$ 或等价的 $g_m v_{sg}$ 决定；
    - gate 对小信号近似开路；
    - gate 若接固定 bias，则是 AC ground。
    
    只要统一采用正的 $g_m$ 和 $r_o$，最后求出来的阻抗大小就是一样的。
    
    区别主要在：
    
    - 电流方向符号；
    - 电压极性；
    - PMOS 的 source 通常在上面，NMOS 的 source 通常在下面；
    - 算电压增益时 PMOS / NMOS 对输出电压的正负贡献要小心。
    
    但对于 $R_{\text{in}}$ / $R_{\text{out}}$ 这种“端口阻抗大小”，公式同形。
    
    ---
    
    ### 4. 什么时候公式会变？
    
    如果加入 body effect，源极小信号变化会引入 $g_{mb}$，这时可以粗略把：
    
    $$
    g_m\rightarrow g_m+g_{mb}
    $$
    
    对应阻抗会变成含 $g_m+g_{mb}$ 的形式。
    
    如果 gate bias 不是理想 AC ground，或者 drain / source 外接网络不同，也不能直接套 slide 10 / 11 的简化公式。
    
    **一句话记忆：** PMOS / NMOS 小信号阻抗公式同形；符号方向会变，阻抗大小不变。
    
- Q：$g_m$ 和 $r_o$ 的数量级一般是多少？
    
    先记单位：
    
    - $g_m$ 是跨导，单位是 S，也常写成 mS；
    - $r_o$ 是小信号输出电阻，单位是 $\Omega$，常见是 k$\Omega$ 到 M$\Omega$。
    
    ---
    
    ### 1. $g_m$ 的数量级
    
    长沟道饱和区常用：
    
    $$
    g_m=\frac{2I_D}{V_{OV}}
    $$
    
    如果模拟 IC 里：
    
    $$
    I_D\sim 10\,\mu\text{A}\text{ 到 }1\,\text{mA}
    $$
    
    $$
    V_{OV}\sim 0.1\text{ 到 }0.3\,\text{V}
    $$
    
    那么：
    
    $$
    g_m\sim 0.1\,\text{mS}\text{ 到 }20\,\text{mS}
    $$
    
    手算时最常见的量级可以先抓：
    
    $$
    g_m\sim 1\,\text{mS}
    $$
    
    也就是：
    
    $$
    \frac{1}{g_m}\sim 1\,\text{k}\Omega
    $$
    
    ---
    
    ### 2. $r_o$ 的数量级
    
    常用：
    
    $$
    r_o=\frac{V_A}{I_D}
    $$
    
    其中 $V_A$ 是 Early voltage / MOS 里的等效 $V_A'L$。
    
    如果：
    
    $$
    V_A\sim 5\text{ 到 }50\,\text{V}
    $$
    
    $$
    I_D\sim 10\,\mu\text{A}\text{ 到 }1\,\text{mA}
    $$
    
    那么：
    
    $$
    r_o\sim 10\,\text{k}\Omega\text{ 到 several M}\Omega
    $$
    
    手算时常用：
    
    $$
    r_o\sim 50\,\text{k}\Omega\text{ 到 }500\,\text{k}\Omega
    $$
    
    ---
    
    ### 3. 本征增益 $A_0=g_m r_o$ 的数量级
    
    因为：
    
    $$
    A_0=g_m r_o
    $$
    
    常见模拟电路里：
    
    $$
    A_0\sim 10\text{ 到 }100
    $$
    
    老工艺 / 长沟道 / 小电流时可以更高；短沟道先进工艺里 $r_o$ 会变小，$A_0$ 常常只有几十。
    
    一个典型手算例子：
    
    $$
    I_D=100\,\mu\text{A},\quad V_{OV}=0.2\,\text{V}
    $$
    
    $$
    g_m=\frac{2I_D}{V_{OV}}=1\,\text{mS}
    $$
    
    若：
    
    $$
    V_A=10\,\text{V}
    $$
    
    则：
    
    $$
    r_o=\frac{10\,\text{V}}{100\,\mu\text{A}}=100\,\text{k}\Omega
    $$
    
    所以：
    
    $$
    A_0=g_m r_o=1\,\text{mS}\cdot100\,\text{k}\Omega=100
    $$
    
    ---
    
    ### 4. 对这几页最有用的直觉
    
    可以先用这组数量级做 sanity check：
    
    $$
    g_m\sim 1\,\text{mS},\quad r_o\sim 100\,\text{k}\Omega,\quad A_0\sim 100
    $$
    
    于是：
    
    $$
    \frac{1}{g_m}\sim 1\,\text{k}\Omega
    $$
    
    而 cascode 后：
    
    $$
    A_0r_o\sim 100\times100\,\text{k}\Omega=10\,\text{M}\Omega
    $$
    
    所以 slide 10 / 11 里说“乘 $A_0$ / 除 $A_0$”确实是很大的阻抗变换。
    

---

## 4️⃣ 进阶阶梯 1：源极退化看 $R_{\text{out}}$（slide 10）

[Slide 10 — Source Degeneration (CS+Rs)，看 R_out](https://app.notion.com)

Slide 10 — Source Degeneration (CS+Rs)，看 R_out

**拓扑**：MOS 源极串 $R_s$，栅接地，从漏点注入测试电压 $v_x$ 看电流 $i_x$。

$$
v_{gs} = -i_x R_s
$$

$$
v_x = (i_x - g_m v_{gs})r_o + i_x R_s = (i_x + g_m i_x R_s)r_o + i_x R_s
$$

$$
\boxed{\;R_{\text{out}} = r_o + R_s + g_m r_o R_s = r_o + (1+g_m r_o)R_s \simeq (g_m r_o)R_s = A_0 R_s\;}
$$

<aside>
💡

**口诀（从源看漏方向）：源极电阻被本征增益** $A_0$ **「乘」后出现在漏端。**

</aside>

---

## 5️⃣ 进阶阶梯 2：共栅看 $R_{\text{in}}$（slide 11）

[Slide 11 — Impedance Transformation of Common Gate Amplifier](https://app.notion.com)

Slide 11 — Impedance Transformation of Common Gate Amplifier

**拓扑**：MOS 共栅 (CG)，栅接偏置 $V_G$，漏接负载 $R_L$，源接信号。从源点注入 $v_x$ 看 $i_x$。

$$
v_{gs} = -v_x,\quad i_x + g_m v_{gs} = i_x - g_m v_x\Rightarrow v_x = (i_x + g_m v_{gs})r_o + i_x R_L
$$

整理后：

$$
\boxed{\;R_{\text{in}} = \frac{r_o + R_L}{1 + g_m r_o} \simeq \frac{1}{g_m} + \frac{R_L}{g_m r_o} = \frac{1}{g_m} + \frac{R_L}{A_0}\;}
$$

<aside>
💡

**口诀（从漏看源方向）：输出负载被本征增益** $A_0$ **「除」后出现在源端。**

</aside>

---

## 6️⃣ 两条口诀合一—cascode 的灵魂（slide 12）

<aside>
🎯

**Cascode 三句话**：

1. **源看漏**：$R_{\text{out}} = r_o + (1+g_m r_o)R_s \simeq A_0 R_s$ —— 源极阻抗被 $A_0$ 乘起。
2. **漏看源**：$R_{\text{in}} \simeq 1/g_m + R_L/A_0$ —— 漏载被 $A_0$ 除小。
3. **上看下除、下看上乘** —— 后面任何 cascode 的计算都只是这两条口诀的重复应用。
</aside>

---

## 7️⃣ MOS Cascode 放大器（slide 13）

[Slide 13 — MOS Cascode Amplifier](https://app.notion.com)

Slide 13 — MOS Cascode Amplifier

**拓扑**：$Q_1$ 共源（提供 $g_{m1}$）在下，$Q_2$ 共栅（偏置 $V_{G2}$）在上，负载为理想电流源 $I$。

### 输出阻抗

从 $Q_2$ 漏看下：$Q_2$ 是共栅，源极看到 $r_{o1}$ 作 $R_s$。应用“源看漏”口诀：

$$
R_o = r_{o2} + (1+g_{m2}r_{o2})r_{o1}\simeq (g_{m2}r_{o2})r_{o1} = A_0\, r_o
$$

### 增益

$$
A_{vo} = -g_{m1}R_o = -(g_{m1}r_{o1})(g_{m2}r_{o2}) = \boxed{\;-A_0^2\;}
$$

<aside>
🔑

**狘点**：一层 cascode 把本征增益从 $A_0$ 抬到 $A_0^2$。典型数据：$A_0=50$ → $A_0^2=2500$，增益高达 **68 dB**。

</aside>

---

## 8️⃣ Cascode + 简单 Active Load（slide 14）

[Slide 14 — Cascode Amplifier with Simple Active Load](https://app.notion.com)

Slide 14 — Cascode Amplifier with Simple Active Load

**拓扑**：$Q_1, Q_2$ 仍是下面的 cascode、负载是单管 PMOS 电流源 $Q_3$（单层，阻抗 $r_{o3}$）。

$$
R_{\text{out}} = R_o\|R_L = (g_{m2}r_{o2}r_{o1})\|r_{o3}
$$

但 $g_{m2}r_{o2}r_{o1}\gg r_{o3}$，并联后 $\simeq r_{o3}$。

$$
A_v = -g_{m1}(R_o\|R_L)\simeq -g_{m1}r_{o3}\sim -A_0
$$

<aside>
⚠️

**踩坑警报**：输入側 cascode 只额够在**肩被下拉**下，反而被输出侧的 $r_{o3}$ 拖垃。**Cascode 要拿** $A_0^2$**，上下都必须 cascode**。

</aside>

---

## 9️⃣ Cascode + Cascode Current-Source Load（slide 15）

[Slide 15 — Cascode Amplifier with Cascode Current-Source Load](https://app.notion.com)

Slide 15 — Cascode Amplifier with Cascode Current-Source Load

**拓扑**：下两管 $Q_1, Q_2$ NMOS cascode；上两管 $Q_3, Q_4$ PMOS cascode。

### 两侧输出阻抗

- $R_{on} = (g_{m2}r_{o2})r_{o1}\simeq A_0\, r_o$
- $R_{op} = (g_{m3}r_{o3})r_{o4}\simeq A_0\, r_o$

### 总增益

$$
A_v = -g_{m1}(R_{on}\|R_{op})
$$

如果 NMOS/PMOS 对称（同 $g_m, r_o$），$R_{on}=R_{op}=A_0 r_o$：

$$
A_v = -g_{m}\cdot\frac{A_0 r_o}{2} = -\tfrac{1}{2}(g_m r_o)^2 = \boxed{\;-\tfrac{1}{2}A_0^2\;}
$$

<aside>
🎯

**这是单级 (single-stage) 拓扑能拿到的最高电压增益**。现代后续 op-amp 设计 / OTA 都以这个结构为原型（只是把 $Q_1$ 换为差分对，看 Lecture 13）。

</aside>

### 设计代价

- 纵向堆 4 个管子 + 2 个 $|V_{OV}|$ 预留→ 需 $V_{DD}\ge 4|V_{OV}|+|V_t|$，在 0.18 µm / 1.8 V 下勉强可行，在 65 nm / 1.0 V 已推不动 → 帕望转向 **folded cascode**（Lecture 13）。
- 4 个 $r_o$ 同时在作用 → **频响在输出节点处变慢**（$f_p=1/(2\pi R_{\text{out}}C_L)$，$R_{\text{out}}$ 变大 = pole 变低）。

---

## 📝 本节总结（本节记三条）

<aside>
🎯

1. **Cascode 的本质**：“上看下 ÷ $A_0$、下看上 × $A_0$”。靠这两句口诀就可以推出任何 cascode 结构的 $R_{\text{out}},\ A_v$。
2. **增益抬代价**：MOS Cascode 拿到 $-A_0^2$；**但记得上下两边都 cascode**，否则输出被单管负载拉回 $A_0$。完全对称时 $A_v = -\tfrac{1}{2}A_0^2$。
3. **代价**：堆高 = 压差提升 + 输出摆幅变小 + pole 变低。低压进程里 folded cascode 是首选结构。
</aside>

---

## 🔗 与上下文衔接

- 后续与 [EE115 Lecture13 — Cascode & Differential Amplifiers](EE115%20Lecture13%20%E2%80%94%20Cascode%20&%20Differential%20Amplifier.md) 重叠。Lecture 13 的 slides 3–8 是本节 slides 9–15 的换言复读，随后进入 folded cascode 与差分对。复习时可以两页同看。

## 📚 Homework（占位）

- [ ]  SEDRA/SMITH Book chapter x problem xx — due April xx 23:00

[EE115 Lecture12.pdf](EE115%20Lecture12%20%E2%80%94%20Cascode%20Amplifier/EE115_Lecture12.pdf)