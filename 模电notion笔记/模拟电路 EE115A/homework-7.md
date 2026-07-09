# homework-7

## P1:

[](https://app.notion.com)

## P1 解答

### (a) 当 $v_{G1}=v_{G2}=0\,\text{V}$

由于电路完全对称，尾电流 $0.5\,\text{mA}$ 平分：

- $I_{D1}=I_{D2}=0.25\,\text{mA}$

PMOS 饱和区电流公式：

$$
I_D=\frac{1}{2}k_p'(W/L)(V_{SG}-|V_{tp}|)^2
$$

其中 $k_p'(W/L)=4\,\text{mA}/\text{V}^2$，所以：

$$
0.25=\frac{1}{2}\cdot4\cdot |V_{OV}|^2
$$

$$
|V_{OV}|=\sqrt{0.125}=0.354\,\text{V}
$$

因此：

$$
V_{SG}=|V_{tp}|+|V_{OV}|=0.8+0.354=1.154\,\text{V}
$$

因为 gate 接地：

$$
V_S=V_G+V_{SG}=0+1.154=1.154\,\text{V}
$$

每个 $4\,\text{k}\Omega$ 电阻电流为 $0.25\,\text{mA}$，压降为：

$$
I_DR=0.25\,\text{mA}\times4\,\text{k}\Omega=1.0\,\text{V}
$$

电阻下端接 $-2.5\,\text{V}$，所以：

$$
V_{D1}=V_{D2}=-2.5+1.0=-1.5\,\text{V}
$$

最终：

$$
\boxed{|V_{OV}|=0.354\,\text{V}}
$$

$$
\boxed{V_{SG}=1.154\,\text{V}}
$$

$$
\boxed{V_S=1.154\,\text{V}}
$$

$$
\boxed{V_{D1}=V_{D2}=-1.5\,\text{V}}
$$

### (b) 输入共模范围

令：

$$
v_{G1}=v_{G2}=V_{CM}
$$

对称时电流仍然平分，所以 $|V_{OV}|$、$V_{SG}$、$V_D$ 与 (a) 相同：

$$
V_{SG}=1.154\,\text{V},\qquad V_D=-1.5\,\text{V}
$$

公共源极节点为：

$$
V_S=V_{CM}+V_{SG}=V_{CM}+1.154
$$

#### 上限：电流源需要至少 $0.4\,\text{V}$ 压降

电流源接在 $+2.5\,\text{V}$ 和 $V_S$ 之间，因此：

$$
2.5-V_S\ge0.4
$$

$$
V_S\le2.1\,\text{V}
$$

$$
V_{CM}+1.154\le2.1
$$

$$
V_{CM}\le0.946\,\text{V}
$$

#### 下限：PMOS 必须保持饱和

PMOS 饱和条件：

$$
V_{SD}\ge |V_{OV}|
$$

也就是：

$$
V_S-V_D\ge0.354
$$

代入 $V_D=-1.5\,\text{V}$：

$$
V_S+1.5\ge0.354
$$

$$
V_S\ge-1.146\,\text{V}
$$

又因为 $V_S=V_{CM}+1.154$：

$$
V_{CM}+1.154\ge-1.146
$$

$$
V_{CM}\ge-2.30\,\text{V}
$$

所以输入共模范围为：

$$
\boxed{-2.30\,\text{V}\le V_{CM}\le0.946\,\text{V}}
$$

## 疑问记录 / Q&A

- **Q：P1(b) 的 common-mode 是 MOSFET 饱和电压吗？**
    - 不是。这里的 **input common-mode voltage** 指两个输入端同时加上的相同电压：$v_{G1}=v_{G2}=V_{CM}$。
    - 当两个输入相同，尾电流 $0.5\,\text{mA}$ 平分，所以每支 PMOS 电流为 $0.25\,\text{mA}$，从而 $|V_{OV}|=\sqrt{0.25/(0.5\times4)}\approx0.354\,\text{V}$，$V_{SG}=|V_{tp}|+|V_{OV}|\approx1.154\,\text{V}$。
    - 因此公共源节点 $V_S=V_{CM}+V_{SG}$。common-mode range 是让电流源仍有足够压降、且 PMOS 仍处于饱和区的输入电压范围。
    - 上限由电流源 compliance 决定：$2.5-V_S\ge0.4\Rightarrow V_{CM}\le0.946\,\text{V}$。
    - 下限由 PMOS 饱和条件决定：$V_{SD}\ge V_{OV}$，且 $V_D=-1.5\,\text{V}$，所以 $V_{CM}\ge-2.30\,\text{V}$。
    - 所以本题的输入共模范围约为 $-2.30\,\text{V}\le V_{CM}\le0.946\,\text{V}$。

## P2:

[](https://app.notion.com)

## P2 解答

题目要求在 $v_{G1}=v_{G2}=0\,\text{V}$ 时，两个 drain 的 dc 电压都是：

$$
V_{D1}=V_{D2}=+0.1\,\text{V}
$$

并且所有 MOSFET 都工作在：

$$
V_{OV}=0.15\,\text{V}
$$

已知：

$$
V_{tn}=0.4\,\text{V},\qquad \mu_n C_{ox}=400\,\mu\text{A}/\text{V}^2
$$

因此所有 NMOS 的：

$$
V_{GS}=V_{tn}+V_{OV}=0.4+0.15=0.55\,\text{V}
$$

### 1. 电流分配

图中尾电流为：

$$
I_{tail}=0.4\,\text{mA}
$$

在差分输入为零时，电路左右对称，所以：

$$
I_{D1}=I_{D2}=\frac{I_{tail}}{2}=0.2\,\text{mA}
$$

参考支路电流为：

$$
I_{D4}=0.1\,\text{mA}
$$

电流镜输出管：

$$
I_{D3}=0.4\,\text{mA}
$$

### 2. 求 $R_D$

每个 $R_D$ 上端接 $+0.9\,\text{V}$，下端 drain 要设计为 $+0.1\,\text{V}$，因此电阻压降为：

$$
0.9-0.1=0.8\,\text{V}
$$

电流为 $0.2\,\text{mA}$，所以：

$$
R_D=\frac{0.8}{0.2\,\text{mA}}=4\,\text{k}\Omega
$$

因此：

$$
\boxed{R_D=4\,\text{k}\Omega}
$$

### 3. 求电流镜参考电阻 $R$

$Q_4$ 是 diode-connected NMOS，source 接 $-0.9\,\text{V}$，且：

$$
V_{GS4}=0.55\,\text{V}
$$

所以 $Q_4$ 的 gate/drain 节点电压为：

$$
V_{G4}=V_{D4}=-0.9+0.55=-0.35\,\text{V}
$$

电阻 $R$ 上端接 $+0.9\,\text{V}$，下端为 $-0.35\,\text{V}$，压降：

$$
0.9-(-0.35)=1.25\,\text{V}
$$

参考电流为 $0.1\,\text{mA}$，因此：

$$
R=\frac{1.25}{0.1\,\text{mA}}=12.5\,\text{k}\Omega
$$

所以：

$$
\boxed{R=12.5\,\text{k}\Omega}
$$

### 4. 求各管 $W/L$

忽略 channel-length modulation，NMOS 饱和区电流为：

$$
I_D=\frac{1}{2}\mu_n C_{ox}\frac{W}{L}V_{OV}^2
$$

先计算单位 $W/L$ 对应的电流：

$$
\frac{1}{2}\mu_n C_{ox}V_{OV}^2
=\frac{1}{2}(400)(0.15)^2\,\mu\text{A}
=4.5\,\mu\text{A}
$$

因此：

$$
\frac{W}{L}=\frac{I_D}{4.5\,\mu\text{A}}
$$

对 $Q_1,Q_2$：

$$
\left(\frac{W}{L}\right)_1=\left(\frac{W}{L}\right)_2
=\frac{200\,\mu\text{A}}{4.5\,\mu\text{A}}
=44.4
$$

对 $Q_3$：

$$
\left(\frac{W}{L}\right)_3
=\frac{400\,\mu\text{A}}{4.5\,\mu\text{A}}
=88.9
$$

对 $Q_4$：

$$
\left(\frac{W}{L}\right)_4
=\frac{100\,\mu\text{A}}{4.5\,\mu\text{A}}
=22.2
$$

所以：

$$
\boxed{\left(\frac{W}{L}\right)_1=\left(\frac{W}{L}\right)_2=44.4}
$$

$$
\boxed{\left(\frac{W}{L}\right)_3=88.9}
$$

$$
\boxed{\left(\frac{W}{L}\right)_4=22.2}
$$

### 5. 输入 common-mode range

令：

$$
v_{G1}=v_{G2}=V_{CM}
$$

对 $Q_1,Q_2$，因为它们保持 $V_{OV}=0.15\,\text{V}$：

$$
V_{GS1}=V_{GS2}=0.55\,\text{V}
$$

所以公共源极节点：

$$
V_S=V_{CM}-0.55
$$

#### 下限：$Q_3$ 必须保持饱和

$Q_3$ 的 source 接 $-0.9\,\text{V}$，drain 是公共源极节点 $V_S$。饱和条件：

$$
V_{DS3}\ge V_{OV3}
$$

即：

$$
V_S-(-0.9)\ge0.15
$$

$$
V_S\ge-0.75\,\text{V}
$$

代入 $V_S=V_{CM}-0.55$：

$$
V_{CM}-0.55\ge-0.75
$$

$$
V_{CM}\ge-0.20\,\text{V}
$$

#### 上限：$Q_1,Q_2$ 必须保持饱和

对输入差分对 NMOS：

$$
V_{DS1}\ge V_{OV1}
$$

即：

$$
V_D-V_S\ge0.15
$$

其中 drain 电压设计为 $V_D=+0.1\,\text{V}$，所以：

$$
0.1-V_S\ge0.15
$$

$$
V_S\le-0.05\,\text{V}
$$

代入 $V_S=V_{CM}-0.55$：

$$
V_{CM}-0.55\le-0.05
$$

$$
V_{CM}\le0.50\,\text{V}
$$

因此输入共模范围为：

$$
\boxed{-0.20\,\text{V}\le V_{CM}\le0.50\,\text{V}}
$$

### 最终答案汇总

$$
\boxed{R=12.5\,\text{k}\Omega}
$$

$$
\boxed{R_D=4\,\text{k}\Omega}
$$

$$
\boxed{\left(\frac{W}{L}\right)_1=\left(\frac{W}{L}\right)_2=44.4}
$$

$$
\boxed{\left(\frac{W}{L}\right)_3=88.9,\qquad \left(\frac{W}{L}\right)_4=22.2}
$$

$$
\boxed{-0.20\,\text{V}\le V_{CM}\le0.50\,\text{V}}
$$

## P3:

[](https://app.notion.com)

## P3 解答

这题的核心是：diode-connected PMOS load 在 small-signal 下等效成一个到 AC ground 的电阻。由于是纯差模输入，公共源节点为 virtual ground，因此可以用 differential half-circuit。

### (a) Differential half-circuit 与 $A_d$

差模激励为：

$$
v_{g1}=+\frac{v_{id}}{2},\qquad v_{g2}=-\frac{v_{id}}{2}
$$

理想尾电流源在 small-signal 下为开路；对纯 differential mode，公共源节点电压不变，所以 source 节点是：

$$
\text{AC ground}
$$

取右半边 half-circuit：$Q_2$ 是 NMOS common-source 管，gate 输入为 $-v_{id}/2$，source 接 AC ground，drain 为输出节点；$Q_4$ 是 diode-connected PMOS，source 接 $V_{DD}$，small-signal 下也是 AC ground，gate 和 drain 短接在输出节点。

输出节点到 AC ground 的小信号通路有三项：

$$
g_{o1,2}=\frac{1}{r_{o1,2}},\qquad
g_{o3,4}=\frac{1}{r_{o3,4}},\qquad
g_{m3,4}
$$

所以等效输出电阻为：

$$
R_{out}
=\frac{1}{g_{m3,4}+g_{o1,2}+g_{o3,4}}
=r_{o1,2}\parallel r_{o3,4}\parallel \frac{1}{g_{m3,4}}
$$

对右输出节点写 KCL：

$$
\left(g_{m3,4}+g_{o1,2}+g_{o3,4}\right)v_{o2}
+g_{m1,2}v_{gs2}=0
$$

而：

$$
v_{gs2}=-\frac{v_{id}}{2}
$$

因此：

$$
v_{o2}
=\frac{g_{m1,2}}{2\left(g_{m3,4}+g_{o1,2}+g_{o3,4}\right)}v_{id}
$$

左边同理：

$$
v_{o1}
=-\frac{g_{m1,2}}{2\left(g_{m3,4}+g_{o1,2}+g_{o3,4}\right)}v_{id}
$$

如果定义 differential output：

$$
v_{od}=v_{o2}-v_{o1}
$$

则：

$$
A_d=\frac{v_{od}}{v_{id}}
=\frac{g_{m1,2}}{g_{m3,4}+g_{o1,2}+g_{o3,4}}
$$

也可以写成：

$$
\boxed{
A_d
=g_{m1,2}\left(r_{o1,2}\parallel r_{o3,4}\parallel \frac{1}{g_{m3,4}}\right)
}
$$

如果题目采用 single-ended output，则右边输出增益为：

$$
\boxed{
\frac{v_{o2}}{v_{id}}
=\frac{g_{m1,2}}{2\left(g_{m3,4}+g_{o1,2}+g_{o3,4}\right)}
}
$$

### (b) 忽略 $r_o$ 时的 $A_d$

忽略 output resistance 等价于：

$$
g_{o1,2}=g_{o3,4}=0
$$

于是 differential gain 变为：

$$
A_d=\frac{g_{m1,2}}{g_{m3,4}}
$$

因为 $Q_1,Q_2$ 与 $Q_3,Q_4$ 在静态时每支电流相同，均为 $I/2$。MOS 管：

$$
g_m=\sqrt{2\mu C_{ox}\frac{W}{L}I_D}
$$

所以：

$$
\frac{g_{m1,2}}{g_{m3,4}}
=
\sqrt{
\frac{\mu_n C_{ox}(W/L)_{1,2}(I/2)}
{\mu_p C_{ox}(W/L)_{3,4}(I/2)}
}
$$

约去相同的 $C_{ox}$ 和 $I/2$：

$$
\boxed{
A_d
=
\sqrt{
\frac{\mu_n (W/L)_{1,2}}
{\mu_p (W/L)_{3,4}}
}
}
$$

如果采用 single-ended definition，则结果要再乘 $1/2$：

$$
\boxed{
A_{d,\text{single}}
=
\frac{1}{2}
\sqrt{
\frac{\mu_n (W/L)_{1,2}}
{\mu_p (W/L)_{3,4}}
}
}
$$

### (c) 已知 $\mu_n=4\mu_p$，求 $W_{1,2}/W_{3,4}$

题目给：

$$
\mu_n=4\mu_p
$$

且四个晶体管 channel length 相同，所以：

$$
\frac{(W/L)_{1,2}}{(W/L)_{3,4}}
=\frac{W_{1,2}}{W_{3,4}}
$$

使用 differential output gain：

$$
A_d
=
\sqrt{
\frac{4\mu_p W_{1,2}}
{\mu_p W_{3,4}}
}
=
2\sqrt{\frac{W_{1,2}}{W_{3,4}}}
$$

令 $A_d=10$：

$$
10=2\sqrt{\frac{W_{1,2}}{W_{3,4}}}
$$

$$
\sqrt{\frac{W_{1,2}}{W_{3,4}}}=5
$$

$$
\boxed{
\frac{W_{1,2}}{W_{3,4}}=25
}
$$

如果采用 single-ended gain definition，则：

$$
10=\frac{1}{2}\cdot2\sqrt{\frac{W_{1,2}}{W_{3,4}}}
=\sqrt{\frac{W_{1,2}}{W_{3,4}}}
$$

所以 single-ended 情况会得到：

$$
\boxed{
\frac{W_{1,2}}{W_{3,4}}=100
}
$$

一般这题的 $A_d$ 若未特别说明 single-ended，按 differential output gain 处理，因此答案取：

$$
\boxed{
\frac{W_{1,2}}{W_{3,4}}=25
}
$$

### P3 疑问记录 / Q&A：$A_d$、$v_o$ 与 small-signal ratio

- **Q：这题里** $A_d$ **的定义是什么？**
    - $A_d$ 是 **differential-mode voltage gain**，也就是差模输入 $v_{id}$ 引起的输出小信号电压变化之比。
    - 如果题目 / 解答采用 **single-ended output**，通常定义为：

$$
A_d=\frac{v_o}{v_{id}}
$$

- 如果采用 **differential output**，则定义为：

$$
A_d=\frac{v_{od}}{v_{id}},\qquad v_{od}=v_{o2}-v_{o1}
$$

- 本题用 differential half-circuit 时，常见做法是先算单边输出；若 $v_o$ 取右边输出节点，即 $Q_2$ 与 $Q_4$ drain 的公共节点，则它就是该节点的小信号 drain voltage：$v_o=v_{d4}=v_{d2}$。注意这里是 **小信号量**，不是 DC 电压 $V_{D4}$。
- **Q：为什么** $A_d$ **是小信号的比值，而不是 DC 电压比值？**
    - 因为 amplifier gain 描述的是在某个 bias point 附近，输入发生一个很小变化时，输出跟着变化多少：

$$
A_d=\frac{\Delta v_o}{\Delta v_{id}}\approx\frac{\partial V_o}{\partial V_{id}}\bigg|_{\text{bias point}}
$$

- MOSFET 是非线性器件，DC 电压只负责把管子偏置到合适工作区；真正的“增益”是该工作点附近的局部斜率，所以用 lowercase small-signal：$v_o/v_{id}$。
- 对这题的半电路，若取右边输出：

$$
v_{g2}=-\frac{v_{id}}{2}
$$

输出节点电阻近似为：

$$
R_{out}=r_{o1,2}\parallel r_{o3,4}\parallel \frac{1}{g_{m3,4}}
$$

因此单端差模增益大小为：

$$
|A_d|=\frac{g_{m1,2}}{2}\left(r_{o1,2}\parallel r_{o3,4}\parallel \frac{1}{g_{m3,4}}\right)
$$

- 若使用双端差分输出 $v_{od}=v_{o2}-v_{o1}$，则没有 $1/2$：

$$
|A_d|=g_{m1,2}\left(r_{o1,2}\parallel r_{o3,4}\parallel \frac{1}{g_{m3,4}}\right)
$$

- **Q：我的等效图里 output current 为什么不是** $2i+v_x/r_{o12}$**，还要不要加** $r_{o34}$**？**
    - 该等效图思路大体是对的：对 differential half-circuit 来说，输出节点要看到三条到 AC ground 的小信号通路：

$$
r_{o12},\qquad r_{o34},\qquad \frac{1}{g_{m34}}
$$

- 所以若令右半边输出为 $v_x$，KCL 应写成：

$$
\left(g_{m34}+g_{o12}+g_{o34}\right)v_x+g_{m12}v_{gs2}=0
$$

其中 $g_o=1/r_o$。

- 右边输入为 $v_{gs2}=-v_{id}/2$，因此：

$$
v_x=\frac{g_{m12}}{2\left(g_{m34}+g_{o12}+g_{o34}\right)}v_{id}
$$

- 不是 $2i$ 的原因：$2i$ 是把左右两边的差动变化合在一起看 differential output 时才会出现的“差分效果”；单个输出节点只由本半边的受控电流源驱动，电流大小是 $g_{m12}(v_{id}/2)$，不是 $2g_{m12}(v_{id}/2)$。
- $r_{o34}$ 需要加；因为 diode-connected PMOS 的小信号等效不是只有 $1/g_m$，而是：

$$
r_{o34}\parallel \frac{1}{g_{m34}}
$$

只是如果题目要求忽略 $r_o$，才把 $r_{o12},r_{o34}$ 都拿掉，得到：

$$
A_d\approx \frac{g_{m12}}{2g_{m34}}
$$

- **Q：为什么我 P3 的推导和上述结果有一点小差别？**
    - 差别主要来自 **half-circuit 的定义、输出定义、以及 diode-connected PMOS 的小信号等效**。在纯 differential excitation 下，公共源节点是 AC ground，所以右半边可单独看成：$Q_2$ 的 source 接小信号地，gate 为 $-v_{id}/2$，drain 输出为 $v_o$。
    - 右半边输出节点到 AC ground 有三条 conductance：

$$
g_{o12},\qquad g_{o34},\qquad g_{m34}
$$

- 因此 KCL 应写成：

$$
(g_{m34}+g_{o12}+g_{o34})v_o+g_{m12}v_{gs2}=0
$$

其中：

$$
v_{gs2}=-\frac{v_{id}}{2}
$$

所以：

$$
\frac{v_o}{v_{id}}=\frac{g_{m12}}{2(g_{m34}+g_{o12}+g_{o34})}
$$

- 该推导容易多出系数，是因为把左右两边的差动电流一起算进了单端输出节点；但 **single-ended output** 只看一边，所以输入小信号是 $v_{id}/2$，不是整个 $v_{id}$，也不是两边电流叠加后的 $2i$。
- 如果题目定义的是 differential output：$v_{od}=v_{o2}-v_{o1}$，则左右两边相减时才会把 $1/2$ 抵消：

$$
\frac{v_{od}}{v_{id}}=\frac{g_{m12}}{g_{m34}+g_{o12}+g_{o34}}
$$

- 如果忽略所有 $r_o$，就有：

$$
A_d\approx \frac{g_{m12}}{2g_{m34}}\quad\text{single-ended}
$$

或：

$$
A_d\approx \frac{g_{m12}}{g_{m34}}\quad\text{differential output}
$$

- **Q：为什么将写成负的** $g_{m12}$ **并联电阻，而且把** $g_{m12}$ **和** $g_{m34}$ **反了？**
    - 这里最容易混淆的是：$g_m$ **不是普通电阻，不能随便拿来“并联”**。只有 diode-connected MOS 的 $g_m$ 才表现得像一个到 AC ground 的 conductance。
    - 对输入管 $Q_1,Q_2$ 来说，$g_{m12}v_{gs}$ 是由 gate 输入控制的 **dependent current source**，它负责“注入 / 抽走输出节点电流”，所以它应该在 KCL 的右边当 driving current，而不是作为输出节点到地的并联电阻。
    - 对 diode-connected PMOS $Q_3,Q_4$ 来说，gate 和 drain 短接到输出节点，source 接 $V_{DD}$，小信号下 source 是 AC ground。因此：

$$
v_{sg34}=-v_o
$$

若只看电流方向符号，可能会得到一个负号；但这个负号只是因为你定义的电流方向和 PMOS 的参考方向相反。把 KCL 统一成“离开输出节点的电流为正”后，diode-connected PMOS 对输出节点表现为正 conductance：

$$
i_o=(g_{m34}+g_{o34})v_o
$$

所以它等效为：

$$
R_{load}=\frac{1}{g_{m34}+g_{o34}}\approx\frac{1}{g_{m34}}
$$

- 因此 **分母里的 load conductance 是** $g_{m34}$，不是 $g_{m12}$；**分子里的 transconductance 是输入管** $g_{m12}$，因为输出电流由输入管把 $v_{id}$ 转成电流。
- 直觉记法：

$$
\text{gain}=\frac{\text{input pair converts voltage to current}}{\text{load converts output voltage to current}}
\approx \frac{g_{m12}}{g_{m34}}
$$

- 如果是 single-ended 输出，还要乘上输入一半的因子：

$$
A_d\approx\frac{g_{m12}}{2g_{m34}}
$$

- 所以你反过来写成本质上是把 **输入管的 controlled current source** 当成了 **diode-connected load 的等效电阻**；实际应该是 $Q_{3,4}$ 提供输出电阻，$Q_{1,2}$ 提供驱动电流。

## P4:

[](https://app.notion.com)

### P4 疑问记录 / Q&A：CMRR 与这张图

- **Q：P4 的 CMRR 是什么？对应的是这张图吗？**
    - 是的，P4 的 **CMRR** 对应的就是这张 **NMOS differential pair + PMOS current-mirror active load** 电路。
    - CMRR 全称是 **common-mode rejection ratio**，中文叫 **共模抑制比**。它衡量差分放大器“放大差模信号、抑制共模信号”的能力：

$$
\mathrm{CMRR}=\left|\frac{A_d}{A_{cm}}\right|
$$

其中：

$$
A_d=\frac{v_o}{v_{id}}
$$

是差模增益，表示输入两端一正一负变化时，输出 $v_o$ 对差模输入 $v_{id}$ 的响应；而：

$$
A_{cm}=\frac{v_o}{v_{icm}}
$$

是共模增益，表示两个输入端同时加同一个小信号：

$$
v_{G1}=v_{G2}=v_{icm}
$$

时，输出 $v_o$ 的响应。

- 对这张图来说，$v_o$ 是右侧 $Q_2$ 与 $Q_4$ drain 相连的单端输出节点。
- 理想情况下，两个输入一起升高或降低只会让公共源节点跟着移动，左右两支电流仍然相等，current mirror load 也会同步变化，因此输出不应该对 common-mode 有反应：

$$
A_{cm}\approx0
$$

所以理想 CMRR 会趋近于：

$$
\mathrm{CMRR}\to\infty
$$

- 实际电路中，由于尾电流源输出电阻有限、MOS 的 $r_o$ 有限、电流镜不完全理想、器件 mismatch 等因素，$A_{cm}$ 不为零，所以 CMRR 是一个有限值。
- 如果用 dB 表示：

$$
\mathrm{CMRR}_{dB}=20\log_{10}\left|\frac{A_d}{A_{cm}}\right|
$$

- 直觉上可以记成：

$$
\text{CMRR 越大，说明电路越像真正的差分放大器；越能拒绝两个输入共同带来的噪声。}
$$

- **Q：P4 题目里 “2% difference between the W/L ratios of the two transistors” 是什么意思？**
    - 这句话的意思是：本来应该完全匹配的两个 MOS 管，它们的几何尺寸比 $W/L$ 实际上相差了 $2\%$。
    - 理想匹配时：

$$
\left(\frac WL\right)_1=\left(\frac WL\right)_2
$$

- 但题目说存在 $2\%$ mismatch，通常可以理解为：

$$
\frac{(W/L)_1-(W/L)_2}{(W/L)}\approx 2\%=0.02
$$

或者近似写成：

$$
\left(\frac WL\right)_2\approx1.02\left(\frac WL\right)_1
$$

- 它不是说输入信号差 $2\%$，也不是说 CMRR 差 $2\%$，而是说晶体管制造出来后，两个 nominally matched transistors 的实际尺寸比例有 $2\%$ 偏差。
- 这个 mismatch 会让两个支路的小信号参数不完全一样，例如 $g_m$、电流镜比例或负载等效电阻不完全相等。于是 common-mode input 本来应该被完全抵消，但因为左右两边不对称，会有一小部分被转换成输出，这就是限制 CMRR 的原因之一。
- 在这题里，计算时通常把这个 mismatch 写成：

$$
\epsilon=0.02
$$

然后用它去估计 common-mode leakage，例如：

$$
A_{cm,\text{effective}}\approx \epsilon A_{cm}
$$

所以 CMRR 会被这个 $2\%$ mismatch 降低。

- **Q：**$R_o$ **公式是什么？**
    - 这里的 $R_o$ 一般指 **current-source transistor 的小信号输出电阻**，也就是 MOS 管的 $r_o$。
    - MOS 管饱和区考虑 channel-length modulation 时：

$$
r_o=\frac{1}{\lambda I_D}
$$

- 又因为 Early voltage：

$$
V_A=\frac{1}{\lambda}
$$

所以：

$$
r_o=\frac{V_A}{I_D}
$$

- 题目给的是工艺参数：

$$
V_A'=5\,\text{V}/\mu\text{m}
$$

因此对沟道长度为 $L$ 的 current-source transistor：

$$
V_A=V_A'L
$$

所以最终公式是：

$$
\boxed{
R_o=r_o=\frac{V_A'L}{I}
}
$$

- 在 P4 里尾电流源电流是：

$$
I=100\,\mu\text{A}
$$

因此：

$$
R_o=\frac{5L}{100\,\mu\text{A}}
$$

其中 $L$ 用 $\mu\text{m}$ 作单位，$R_o$ 的单位就是 $\Omega$。

- **Q：P4 不知道** $R_{SS}$**、**$R_D$**，怎么求** $r_o$ **然后得到** $L$**？**
    - 这题不需要先知道 $R_D$，因为在 CMRR 公式里 $R_D$ 或 active-load 的输出电阻会被约掉。
    - 对这类 **current-mirror active-load differential amplifier**，若唯一 mismatch 是 current mirror 两管的 $W/L$ 有相对误差：

$$
\epsilon=2\%=0.02
$$

则常用近似为：

$$
\mathrm{CMRR}\approx \frac{2g_m R_{SS}}{\epsilon}
$$

其中 $R_{SS}$ 就是尾电流源的小信号输出电阻：

$$
R_{SS}=r_o
$$

- 题目要求：

$$
\mathrm{CMRR}_{dB}=80\,\text{dB}
$$

所以线性值为：

$$
\mathrm{CMRR}=10^{80/20}=10^4
$$

- 总尾电流为：

$$
I=100\,\mu\text{A}
$$

差分对每边电流为：

$$
I_D=\frac{I}{2}=50\,\mu\text{A}
$$

又因为：

$$
V_{OV}=0.2\,\text{V}
$$

所以输入管跨导：

$$
g_m=\frac{2I_D}{V_{OV}}
=\frac{2(50\,\mu\text{A})}{0.2\,\text{V}}
=0.5\,\text{mS}
$$

- 由 CMRR 反推：

$$
R_{SS}
=\frac{\mathrm{CMRR}\cdot\epsilon}{2g_m}
=\frac{10^4\cdot0.02}{2(0.5\,\text{mS})}
$$

$$
R_{SS}=200\,\text{k}\Omega
$$

因此尾电流源晶体管需要：

$$
r_o=200\,\text{k}\Omega
$$

- 再用：

$$
r_o=\frac{V_A'L}{I}
$$

代入：

$$
V_A'=5\,\text{V}/\mu\text{m},\qquad I=100\,\mu\text{A}
$$

得到：

$$
L=\frac{r_o I}{V_A'}
=\frac{(200\,\text{k}\Omega)(100\,\mu\text{A})}{5\,\text{V}/\mu\text{m}}
$$

$$
\boxed{L=4\,\mu\text{m}}
$$

- 所以思路是：

$$
\boxed{
\mathrm{CMRR}\Rightarrow R_{SS}=r_o\Rightarrow L
}
$$

而不是先求 $R_D$。

- **Q：为什么课件/书上的 CMRR 公式和前面简化式不一样？为什么** $R_{SS}$ **就等于 current-source transistor 的** $r_o$**？**
    - 你图里这个公式：

$$
\mathrm{CMRR}
=\frac{g_m}{\Delta g_m}\cdot g_m r_o\cdot
\frac{1}{1+\dfrac{r_o+R_D}{2R_{SS}}}
$$

是 **更完整的公式**，它把输入管有限 $r_o$ 也考虑进去了。

- Lecture14 Slide 8 里常用的公式：

$$
\mathrm{CMRR}\approx
\frac{2g_mR_{SS}}{\Delta g_m/g_m}
$$

是上面完整公式在：

$$
r_o\to\infty
$$

时的近似。也就是说，Slide 8 默认输入管输出电阻很大，忽略 channel-length modulation。

- 可以直接从完整公式看极限。若 $r_o\to\infty$，则：

$$
1+\frac{r_o+R_D}{2R_{SS}}
\approx
\frac{r_o}{2R_{SS}}
$$

所以：

$$
\mathrm{CMRR}
\approx
\frac{g_m}{\Delta g_m}\cdot g_m r_o\cdot
\frac{2R_{SS}}{r_o}
=
\frac{2g_mR_{SS}}{\Delta g_m/g_m}
$$

这就回到了 Lecture14 Slide 8 的结果。

- 为什么 P4 可以用简化式？因为题目没有给 $R_D$，也没有给输入管的 $r_o$ 或输入管的 $L$。如果要求你用完整公式，这些量必须给出来。因此这题的意图就是使用 Slide 8 的近似公式，用 CMRR 反推尾电流源所需的 $R_{SS}$。
- 这里要区分两个不同的电阻：
    - $r_o$：完整公式里的 $r_o$ 通常指 **输入差分对晶体管** 的输出电阻。
    - $R_{SS}$：尾电流源的小信号输出电阻。
- 如果尾电流源由一个 MOS current-source transistor 实现，那么从公共源节点往下看进去，看到的就是这个电流源管的 drain 小信号输出电阻：

$$
R_{SS}=r_{o,\text{tail}}
$$

- 所以 P4 问 “current-source transistor 的 $L$” 时，本质上就是：

$$
\mathrm{CMRR}\Rightarrow R_{SS}
\Rightarrow r_{o,\text{tail}}
\Rightarrow L_{\text{tail}}
$$

- 这不是说所有 $R_{SS}$ 都天然等于所有 MOS 的 $r_o$；而是说：**当尾电流源就是一个 MOS 管实现时，**$R_{SS}$ **等于这只尾电流源 MOS 管的输出电阻** $r_o$**。**
- 因此本题的简化计算逻辑是：

$$
10^{80/20}
=\frac{2g_mR_{SS}}{\Delta g_m/g_m}
$$

再由：

$$
R_{SS}=r_{o,\text{tail}}=\frac{V_A'L}{I}
$$

求出 current-source transistor 的沟道长度 $L$。

- **Q：为什么** $r_{o,\text{tail}}$ **就是** $R_{SS}$**？它和公式里的** $r_o$ **不是完全没关系吗？**
    - 对，这个疑问是对的：$r_{o,\text{tail}}$ **和完整 CMRR 公式里的** $r_o$ **不是同一个东西。**
    - 完整公式里的：

$$
r_o
$$

通常指 **输入差分对晶体管** 的输出电阻，例如 $Q_1,Q_2$ 的 $r_o$。

- 而：

$$
R_{SS}
$$

指的是 **tail current source 从公共源节点看进去的小信号输出电阻**。

- 如果 tail current source 是用一只 MOS 管实现的，那么这个 MOS 管的小信号模型就是：

$$
\text{ideal current source}\parallel r_{o,\text{tail}}
$$

- 在 small-signal 里，理想 DC 电流源等效为 open circuit，所以公共源节点往下看，只剩：

$$
R_{SS}=r_{o,\text{tail}}
$$

- 所以这里不是说 “所有 $r_o$ 都等于 $R_{SS}$”，而是说：

$$
\boxed{
R_{SS}=\text{tail current source 的输出电阻}
}
$$

如果这个 tail current source 由一只 MOS 管实现，则：

$$
\boxed{
R_{SS}=r_{o,\text{tail}}
}
$$

- 若 tail current source 是 cascode current source，则：

$$
R_{SS}\neq r_o
$$

而会变成大约：

$$
R_{SS}\sim g_m r_o^2
$$

- 因此 P4 里要找 current-source transistor 的 $L$，就是因为题目默认 tail current source 由一只 MOS 管提供，故：

$$
R_{SS}=r_{o,\text{tail}}=\frac{V_A'L}{I}
$$

- 关键区分：
    - **输入管** $r_o$：影响完整 CMRR 公式里的 intrinsic gain 限制。
    - **尾电流源** $r_{o,\text{tail}}$：就是 $R_{SS}$，决定 common-mode source degeneration 强不强。

- **Q：为什么这里的** $V_A'$ **用来算** $r_{o,\text{tail}}$**，而不是先考虑输入 MOSFET 的** $r_o$**？**
    - $V_A'$ 不是某一只管子专属的参数，而是工艺给出的 Early-voltage-per-length 参数。对同一类 MOS 管，任意一只管子都可以用：

$$
V_A=V_A'L,\qquad r_o=\frac{V_A}{I_D}=\frac{V_A'L}{I_D}
$$

- 所以输入管也有：

$$
r_{o,\text{in}}=\frac{V_A'L_{\text{in}}}{I_D}
$$

尾电流源管也有：

$$
r_{o,\text{tail}}=\frac{V_A'L_{\text{tail}}}{I}
$$

- 但 P4 最后问的是 **current-source transistor 的** $L$，也就是尾电流源管的 $L_{\text{tail}}$。因此我们把 $V_A'$ 用在：

$$
r_{o,\text{tail}}=\frac{V_A'L_{\text{tail}}}{I}
$$

- 完整 CMRR 公式里的 $r_o$ 通常是输入差分对管子的 $r_{o,\text{in}}$。如果题目要你用完整公式，就必须给 $R_D$ 和 $L_{\text{in}}$ 或 $r_{o,\text{in}}$。但题目没有给这些量，所以不能从完整公式唯一求解。
- 因此这题的解题意图是：忽略输入管 $r_o$ 限制，用 Slide 8 的近似公式先求需要的 $R_{SS}$，再令：

$$
R_{SS}=r_{o,\text{tail}}
$$

最后由 $V_A'L_{\text{tail}}/I$ 求尾电流源管长度。

- 一句话：$V_A'$ **可以用于任何 MOS 的** $r_o$**，但本题要求设计的是 tail current-source transistor，所以用它来算** $r_{o,\text{tail}}$**。输入 MOSFET 的** $r_o$ **属于更完整模型，但题目没有足够信息让我们考虑它。**

- **Q：P4 最终到底应该怎么处理** $2\%$ **mismatch 和** $L$**？**
    - 严格按题目文字，$2\%$ 是 $W/L$ **mismatch**：

$$
\frac{\Delta(W/L)}{W/L}=0.02
$$

- 但 Slide 8 的 CMRR 公式用的是 $g_m$ **mismatch**：

$$
\mathrm{CMRR}\approx \frac{2g_mR_{SS}}{\Delta g_m/g_m}
$$

- 因为在饱和区：

$$
g_m\propto \sqrt{W/L}
$$

所以小变化下：

$$
\frac{\Delta g_m}{g_m}
=\frac{1}{2}\frac{\Delta(W/L)}{W/L}
=0.01
$$

- 因此更严谨地代入：

$$
\mathrm{CMRR}=10^{80/20}=10^4
$$

$$
I_D=\frac{I}{2}=50\,\mu\text{A}
$$

$$
g_m=\frac{2I_D}{V_{OV}}
=\frac{2(50\,\mu\text{A})}{0.2\,\text{V}}
=0.5\,\text{mS}
$$

- 反推：

$$
R_{SS}
=\frac{\mathrm{CMRR}(\Delta g_m/g_m)}{2g_m}
=\frac{10^4(0.01)}{2(0.5\,\text{mS})}
=100\,\text{k}\Omega
$$

- 由于 $R_{SS}=r_{o,\text{tail}}$，且：

$$
r_{o,\text{tail}}=\frac{V_A'L}{I}
$$

所以：

$$
L=\frac{r_{o,\text{tail}}I}{V_A'}
=\frac{(100\,\text{k}\Omega)(100\,\mu\text{A})}{5\,\text{V}/\mu\text{m}}
=2\,\mu\text{m}
$$

- 因此严格答案应为：

$$
\boxed{L=2\,\mu\text{m}}
$$

- 如果把题目的 $2\%$ 直接当成 $\Delta g_m/g_m$，则会得到 $L=4\,\mu\text{m}$；但从 “$W/L$ differs by $2\%$” 的文字看，更严谨是先折半成 $1\%$ 的 $g_m$ mismatch。

## 通用小信号模型 / Q&A

- **Q：PMOS 的 T 等效模型是怎么样的？**
    - 拓扑上与 NMOS T-model 完全一样，只是把 source 翻到顶部，习惯上用 $v_{sg}$ 代替 $v_{gs}$ 避免负号。
    - **NMOS T-model 结构**：
        - 受控电流源 $g_m v_{gs}$ 在 D–S 之间，箭头 D→S。
        - $r_o$ 与电流源并联在 D–S 之间。
        - $1/g_m$ 串联在 source 支路，gate 从电阻上端引出，外部 $i_G=0$。
    - **PMOS T-model**：source 翻到顶部，电流源箭头从内部节点 X → D，$1/g_m$ 在 S 与 X(=G) 之间。
    - **三种等价写法**：
        - 用 $v_{sg}$：$i_{sd}=g_m v_{sg}+v_{sd}/r_o$，所有量为正。
        - 用 $v_{gs}$：$i_d=g_m v_{gs}+v_{ds}/r_o$，PMOS 下 $v_{gs}<0$、$i_d<0$，公式仍成立。
        - 翻转大法：小信号下把 $V_{DD}$ 看作 AC ground，PMOS 就变成一个 source 在下、drain 在上的 NMOS。
    - **在本作业里的用法**：
        - P3 的 $Q_3,Q_4$（diode-connected PMOS load）：source 接 AC ground，gate与drain 短接，从输出节点看进去的等效电阻为 $\dfrac{1}{g_{m3,4}}\parallel r_{o3,4}\approx \dfrac{1}{g_{m3,4}}$。
        - P1 的 common-source PMOS：$v_{sg}=-v_g$，$i_{sd}=-g_m v_g$，跨阻关系跟 NMOS 一致，只是符号约定不同。
    - **T-model vs. 混合 π**：T-model 在以下场景更好用：source degeneration（直接看 $1/g_m$ 与 $R_S$ 串联）、diode-connected（秒出等效电阻 $1/g_m$）、cascode 共栅级输入电阻（$R_{in}=1/g_m$）。有 body effect 时加上 $g_{mb} v_{bs}$ 受控源，与 $g_m v_{gs}$ 并联。
    - **一句话记忆**：PMOS T-model = NMOS T-model 上下翻转，用 $v_{sg}$ 替 $v_{gs}$，所有小信号参数取正；diode-connected PMOS 等效电阻 $\dfrac{1}{g_m}\parallel r_o$。
    

## P5:

[](https://app.notion.com)

## P5 解答

题目给：simple current source 时差分放大器的 CMRR 为：

$$
\mathrm{CMRR}_1=60\,\text{dB}
$$

希望通过给 current source 加 cascode transistor，把 CMRR 提高到：

$$
\mathrm{CMRR}_2=100\,\text{dB}
$$

### 1. 把 dB 转成线性倍率

CMRR 的 dB 定义是：

$$
\mathrm{CMRR}_{dB}=20\log_{10}(\mathrm{CMRR})
$$

所以：

$$
\mathrm{CMRR}_1=10^{60/20}=10^3
$$

$$
\mathrm{CMRR}_2=10^{100/20}=10^5
$$

因此 CMRR 需要提高的倍数为：

$$
\frac{\mathrm{CMRR}_2}{\mathrm{CMRR}_1}
=\frac{10^5}{10^3}
=100
$$

### 2. CMRR 与尾电流源输出电阻的关系

对差分放大器，CMRR 近似正比于尾电流源的小信号输出电阻：

$$
\mathrm{CMRR}\propto R_{SS}
$$

simple current source 时：

$$
R_{SS,\text{simple}}\approx r_o
$$

加 cascode 后：

$$
R_{SS,\text{cascode}}\approx A_0r_o
$$

因此：

$$
\frac{\mathrm{CMRR}_2}{\mathrm{CMRR}_1}
\approx
\frac{R_{SS,\text{cascode}}}{R_{SS,\text{simple}}}
\approx
\frac{A_0r_o}{r_o}
=A_0
$$

所以所需的 cascode transistor intrinsic gain 为：

$$
\boxed{A_0=100}
$$

### 3. 由 $A_0$ 求 $V_A$

MOS 的 intrinsic gain：

$$
A_0=g_mr_o
$$

又有：

$$
g_m=\frac{2I_D}{V_{OV}}
$$

$$
r_o=\frac{V_A}{I_D}
$$

相乘得到：

$$
A_0
=\frac{2I_D}{V_{OV}}\cdot\frac{V_A}{I_D}
=\frac{2V_A}{V_{OV}}
$$

因此：

$$
V_A=\frac{A_0V_{OV}}{2}
$$

题目给 cascode transistor：

$$
V_{OV}=0.2\,\text{V}
$$

所以：

$$
V_A=\frac{100(0.2)}{2}=10\,\text{V}
$$

因此：

$$
\boxed{V_A=10\,\text{V}}
$$

### 4. 由 $V_A'$ 求 channel length $L$

题目给：

$$
V_A'=5\,\text{V}/\mu\text{m}
$$

并且：

$$
V_A=V_A'L
$$

所以：

$$
L=\frac{V_A}{V_A'}
=\frac{10\,\text{V}}{5\,\text{V}/\mu\text{m}}
=2\,\mu\text{m}
$$

最终：

$$
\boxed{L=2\,\mu\text{m}}
$$

### P5 最终答案

$$
\boxed{A_0=100}
$$

$$
\boxed{V_A=10\,\text{V}}
$$

$$
\boxed{L=2\,\mu\text{m}}
$$

## P5 疑问记录 / Q&A

- **Q：P5 的 cascode transistor 是什么？是 P3 那样吗？**
    - 不是 P3 那种。P3 里的 $Q_3,Q_4$ 是 **diode-connected PMOS load / current-mirror load**，它们在差分对上方，作用是作为 active load，把电流变化转换成输出电压。
    - P5 说的 **cascode transistor** 是加在 **tail current source** 上方的一只共栅 MOS 管，用来提高尾电流源的小信号输出电阻 $R_{SS}$。
    - 原来的 simple current source 可以理解为：

```
公共源节点
   |
current-source transistor
   |
-VSS
```

- 加 cascode 后变成：

```
公共源节点
   |
cascode transistor   ← P5 问的这只
   |
current-source transistor
   |
-VSS
```

- 这只 cascode transistor 的 gate 接固定偏置电压，source 接下面的 current-source transistor，drain 接差分对公共源节点。它的作用是让下面 current-source transistor 的 $V_{DS}$ 更稳定，从而减小 channel-length modulation。
- simple current source 时：

$$
R_{SS}\approx r_o
$$

- 加 cascode 后：

$$
R_{SS,\text{cascode}}\approx A_0r_o
$$

其中：

$$
A_0=g_mr_o
$$

是 cascode transistor 的 intrinsic gain。

- 因为：

$$
\mathrm{CMRR}\propto R_{SS}
$$

所以 P5 本质是在问：为了把 CMRR 从 $60\,\text{dB}$ 提高到 $100\,\text{dB}$，cascode transistor 的 intrinsic gain $A_0$ 要有多大。

- 关键区别：
    - P3：上方 active load / diode-connected PMOS，不是 tail cascode。
    - P5：下方 tail current source 加 cascode，用来提高 $R_{SS}$。

- **Q：为什么加 cascode 后有** $R_{SS,\text{cascode}}\approx A_0r_o$**？**
    - 这是从 **cascode current source 的输出电阻** 推出来的。设上面那只 cascode transistor 为 $M_2$，下面原来的 current-source transistor 为 $M_1$。
    - 从差分对公共源节点往下看进去，等效输出电阻为：

$$
R_{out}=r_{o1}+r_{o2}+g_{m2}r_{o1}r_{o2}
$$

其中：

- $r_{o1}$ 是下面 current-source transistor 的输出电阻；
- $r_{o2}$ 是上面 cascode transistor 的输出电阻；
- $g_{m2}$ 是 cascode transistor 的跨导。
- 因为通常：

$$
g_{m2}r_{o2}\gg 1
$$

所以主导项是：

$$
R_{out}\approx g_{m2}r_{o1}r_{o2}
$$

- 如果两只 MOS 的小信号输出电阻数量级相同：

$$
r_{o1}\approx r_{o2}\approx r_o
$$

那么：

$$
R_{SS,\text{cascode}}
\approx g_mr_o^2
$$

- 又因为 cascode transistor 的 intrinsic gain 定义为：

$$
A_0=g_mr_o
$$

于是：

$$
R_{SS,\text{cascode}}
\approx (g_mr_o)r_o
=A_0r_o
$$

- 直觉上说，cascode 管用自己的 intrinsic gain $A_0$ 把原来 simple current source 的输出电阻 $r_o$ 放大了约 $A_0$ 倍。
- 所以：

$$
\boxed{
R_{SS,\text{cascode}}\approx A_0r_o
}
$$

- 这里的 $r_o$ 指原来 simple current source transistor 的输出电阻；而 $A_0$ 是新增 cascode transistor 的 intrinsic gain。

## P6：

[](https://app.notion.com)

## P6 解答

题目是一个 **NMOS differential pair + 两个 drain resistor** 的差分放大器。尾电流为 $I$，所以在零差分输入时：

$$
I_{D1}=I_{D2}=\frac{I}{2}
$$

两个 drain 电阻存在 mismatch：

$$
R_{D1}=R_D+\frac{\Delta R_D}{2},\qquad
R_{D2}=R_D-\frac{\Delta R_D}{2}
$$

因此两边输出 DC 电压会不同，产生 input-referred offset voltage $V_{OS}$。

### (a) 求 $A_d$、$V_{OS}$，并建立二者关系

#### 1. 差模增益 $A_d$

每个输入管的跨导为：

$$
g_m=\sqrt{2k_nI_D}
$$

因为：

$$
I_D=\frac{I}{2}
$$

所以：

$$
g_m=\sqrt{2k_n\frac{I}{2}}
=\sqrt{k_nI}
$$

若采用 differential output：

$$
v_{od}=v_{o2}-v_{o1}
$$

则差模增益大小为：

$$
A_d=g_mR_D
$$

因此：

$$
\boxed{
A_d=R_D\sqrt{k_nI}
}
$$

如果题目采用 single-ended output，则增益为上述结果的一半：

$$
A_{d,\text{single}}=\frac{1}{2}R_D\sqrt{k_nI}
$$

本解答后续按 differential output gain 处理。

#### 2. 由 drain-resistor mismatch 产生的输出 offset

在 $v_{id}=0$ 时，两边电流仍约为 $I/2$，因此两个输出 DC 电压分别为：

$$
V_{O1}=V_{DD}-\frac{I}{2}R_{D1}
$$

$$
V_{O2}=V_{DD}-\frac{I}{2}R_{D2}
$$

所以输出 offset 的大小为：

$$
|V_{OS,\text{out}}|
=|V_{O2}-V_{O1}|
=\frac{I}{2}|R_{D1}-R_{D2}|
$$

而：

$$
R_{D1}-R_{D2}=\Delta R_D
$$

所以：

$$
|V_{OS,\text{out}}|
=\frac{I}{2}\Delta R_D
$$

#### 3. input-referred offset $V_{OS}$

input-referred offset voltage 的定义是：需要加在输入端、用来抵消这个输出 offset 的等效输入电压。所以：

$$
V_{OS}=\frac{|V_{OS,\text{out}}|}{A_d}
$$

代入：

$$
V_{OS}
=\frac{\frac{I}{2}\Delta R_D}{g_mR_D}
$$

写成相对 mismatch：

$$
\delta=\frac{\Delta R_D}{R_D}
$$

得到：

$$
V_{OS}
=\frac{I}{2g_m}\frac{\Delta R_D}{R_D}
$$

又因为：

$$
g_m=\sqrt{k_nI}
$$

所以：

$$
\boxed{
V_{OS}
=\frac{1}{2}\sqrt{\frac{I}{k_n}}\frac{\Delta R_D}{R_D}
}
$$

#### 4. $V_{OS}$ 与 $A_d$ 的关系

由：

$$
A_d=R_D\sqrt{k_nI}
$$

可得：

$$
\sqrt{\frac{I}{k_n}}=\frac{A_d}{k_nR_D}
$$

代入 $V_{OS}$：

$$
V_{OS}
=\frac{1}{2}\frac{A_d}{k_nR_D}\frac{\Delta R_D}{R_D}
$$

因此：

$$
\boxed{
V_{OS}
=
\frac{A_d}{2k_nR_D}
\frac{\Delta R_D}{R_D}
}
$$

也可以反过来写成：

$$
\boxed{
A_d
=
\frac{2k_nR_D}{\Delta R_D/R_D}V_{OS}
}
$$

这说明在 $R_D$ mismatch 固定时，增益越大，input offset 也越大。

### (b) 数值计算

题目给：

$$
k_n=4\,\text{mA}/\text{V}^2=0.004\,\text{A}/\text{V}^2
$$

$$
R_D=10\,\text{k}\Omega
$$

$$
\frac{\Delta R_D}{R_D}=0.02
$$

由上面的关系：

$$
A_d
=
\frac{2k_nR_D}{\Delta R_D/R_D}V_{OS}
$$

先算系数：

$$
2k_nR_D
=2(0.004)(10^4)
=80\,\text{V}^{-1}
$$

所以：

$$
A_d=\frac{80}{0.02}V_{OS}
=4000V_{OS}
$$

其中 $V_{OS}$ 用 V。

偏置电流由：

$$
A_d=R_D\sqrt{k_nI}
$$

得到：

$$
I=\frac{1}{k_n}\left(\frac{A_d}{R_D}\right)^2
$$

### 计算表

| $V_{OS,\max}$ | $A_{d,\max}$ | Required $I$ |
| --- | --- | --- |
| $1\,\text{mV}$ | $4$ | $40\,\mu\text{A}$ |
| $2\,\text{mV}$ | $8$ | $160\,\mu\text{A}$ |
| $3\,\text{mV}$ | $12$ | $360\,\mu\text{A}$ |
| $4\,\text{mV}$ | $16$ | $640\,\mu\text{A}$ |
| $5\,\text{mV}$ | $20$ | $1.0\,\text{mA}$ |

### P6 最终答案

$$
\boxed{
A_d=R_D\sqrt{k_nI}
}
$$

$$
\boxed{
V_{OS}
=\frac{1}{2}\sqrt{\frac{I}{k_n}}\frac{\Delta R_D}{R_D}
}
$$

$$
\boxed{
V_{OS}
=
\frac{A_d}{2k_nR_D}
\frac{\Delta R_D}{R_D}
}
$$

数值结果：

$$
\boxed{
(V_{OS},A_d,I)
=
(1\,\text{mV},4,40\,\mu\text{A})
}
$$

$$
\boxed{
(2\,\text{mV},8,160\,\mu\text{A})
}
$$

$$
\boxed{
(3\,\text{mV},12,360\,\mu\text{A})
}
$$

$$
\boxed{
(4\,\text{mV},16,640\,\mu\text{A})
}
$$

$$
\boxed{
(5\,\text{mV},20,1.0\,\text{mA})
}
$$

可以看到：允许的 $V_{OS}$ 越大，可以实现的最大 gain 越大，但需要的 bias current 也会快速增大。

## Exam1:

[](https://app.notion.com)

[](https://app.notion.com)

## Exam1 解答

<aside>
📌

**题目：** Sedra–Smith P7.118，NMOS **CS amplifier**，$V_t=0.7\,\text{V}$，$V_A=50\,\text{V}$。

**约定：** 仅 (a) 求 bias 时按题意 **neglect Early effect**；(b)–(d) 的小信号计算都保留 $r_o$。所有数值尽量保留多位小数。

</aside>

### 0. 电路 DC 参数

栅极偏置分压（$R_{G1}=300\,\text{k}\Omega$ 接 $V_{DD}$，$R_{G2}=200\,\text{k}\Omega$ 接地）：

$$
V_G=V_{DD}\frac{R_{G2}}{R_{G1}+R_{G2}}=5\times\frac{200}{300+200}=2.0\,\text{V}
$$

其余元件：$R_D=5\,\text{k}\Omega$，$R_S=2\,\text{k}\Omega$，$R_L=5\,\text{k}\Omega$，$R_{sig}=120\,\text{k}\Omega$。$C_{C1},C_{C2},C_S$ 在信号频率下视为短路（$R_S$ 被 $C_S$ bypass）。

### (a) 验证 saturation，求 $k_n$ 与 $V_D$

source 端电压：

$$
V_S=I_D R_S=0.5\,\text{mA}\times2\,\text{k}\Omega=1.0\,\text{V}
$$

$$
V_{GS}=V_G-V_S=2.0-1.0=1.0\,\text{V}
$$

$$
V_{OV}=V_{GS}-V_t=1.0-0.7=0.3\,\text{V}\quad\checkmark
$$

由 $I_D=\tfrac12 k_n V_{OV}^2$：

$$
k_n=\frac{2I_D}{V_{OV}^2}=\frac{2\times0.5\,\text{mA}}{(0.3)^2}=\frac{1}{0.09}=11.1111\,\text{mA/V}^2
$$

drain 直流电压：

$$
V_D=V_{DD}-I_D R_D=5-0.5\,\text{mA}\times5\,\text{k}\Omega=2.5\,\text{V}
$$

saturation 判据 $V_{DS}\ge V_{OV}$：$V_{DS}=V_D-V_S=2.5-1.0=1.5\,\text{V}\ge0.3\,\text{V}$ ✓

$$
\boxed{k_n=11.1111\,\text{mA/V}^2,\qquad V_D=2.5\,\text{V}\ (\text{saturation})}
$$

### (b) 求 $R_{in}$ 与 $G_v$

输入电阻（gate 不取电流，只剩偏置分压网络）：

$$
R_{in}=R_{G1}\parallel R_{G2}=300\parallel200=\frac{300\times200}{500}=120\,\text{k}\Omega
$$

小信号参数：

$$
g_m=\frac{2I_D}{V_{OV}}=\frac{2\times0.5}{0.3}=3.33333\,\text{mA/V}
$$

$$
r_o=\frac{V_A}{I_D}=\frac{50}{0.5\,\text{mA}}=100\,\text{k}\Omega
$$

drain 端总负载：

$$
R_{out}=r_o\parallel R_D\parallel R_L=100\parallel2.5=\frac{100\times2.5}{102.5}=2.43902\,\text{k}\Omega
$$

gate→输出增益：

$$
A_v=-g_m R_{out}=-3.33333\times2.43902=-8.13008\,\text{V/V}
$$

含输入分压 $\dfrac{R_{in}}{R_{in}+R_{sig}}=\dfrac{120}{120+120}=0.5$ 的总增益：

$$
G_v=-\frac{R_{in}}{R_{in}+R_{sig}}\,g_m R_{out}=-0.5\times8.13008
$$

$$
\boxed{R_{in}=120\,\text{k}\Omega,\qquad G_v=-4.06504\,\text{V/V}}
$$

### (c) 最大 $\hat v_{sig}$（保持 saturation）及对应 $\hat v_o$

saturation 瞬时条件 $v_D\ge v_G-V_t$，可用的 drain 下摆裕度为：

$$
V_D-(V_G-V_t)=2.5-(2.0-0.7)=1.2\,\text{V}
$$

令 gate 信号为 $v_g$（source 被 bypass，$v_{gs}=v_g$），drain 信号 $v_d=A_v v_g$，则 $v_g-v_d=v_g(1+|A_v|)$。正向摆动最先逼近 triode：

$$
\hat v_g(1+|A_v|)\le1.2\ \Rightarrow\ \hat v_g=\frac{1.2}{1+8.13008}=\frac{1.2}{9.13008}=0.131433\,\text{V}
$$

折算到信号源（输入分压 0.5）：

$$
\hat v_{sig}=\frac{\hat v_g}{0.5}=0.262866\,\text{V}
$$

对应输出幅度：

$$
\hat v_o=|A_v|\,\hat v_g=8.13008\times0.131433=1.068566\,\text{V}
$$

$$
\boxed{\hat v_{sig,\max}=0.262866\,\text{V}\ (\approx262.9\,\text{mV}),\qquad \hat v_o=1.068566\,\text{V}}
$$

### (d) 与 $C_S$ 串联的 $R_s$（使 $\hat v_{sig}$ 翻倍）及新的输出

在 $C_S$ 串联 $R_s$ 后，信号频率下 $C_S$ 短路，**串联的** $R_s$ **与原来的** $R_S=2\,\text{k}\Omega$ **源电阻并联**，所以 source 的 AC degeneration 电阻是 $R_{deg}=R_S\parallel R_s$（不是 $R_s$ 本身）。saturation 裕度 $v_D\ge v_G-V_t$ 只含 $v_D,v_G$，约束仍是 $v_g(1+|A_v'|)\le1.2$；又因为 $R_{in}$ 不变、输入分压仍是 0.5，所以「$\hat v_{sig}$ 翻倍」$\Leftrightarrow$「$\hat v_g$ 翻倍」$\Leftrightarrow$$(1+|A_v'|)$ 减半：

$$
1+|A_v'|=\frac{1+|A_v|}{2}=\frac{9.13008}{2}=4.56504\ \Rightarrow\ |A_v'|=3.56504
$$

含 $r_o$ 的 source-degenerated CS 增益（$R_L'=R_D\parallel R_L=2.5\,\text{k}\Omega$，这里的 degeneration 电阻用总值 $R_{deg}$）：

$$
|A_v'|=\frac{g_m R_L'}{1+g_m R_{deg}+\dfrac{R_L'+R_{deg}}{r_o}}
$$

代入 $g_m R_L'=3.33333\times2.5=8.33333$，**先解出所需的总 degeneration 电阻** $R_{deg}$：

$$
\frac{8.33333}{3.56504}=2.33751=1+3.33333\,R_{deg}+\frac{2.5+R_{deg}}{100}=1.025+3.34333\,R_{deg}
$$

$$
R_{deg}=\frac{2.33751-1.025}{3.34333}=0.392576\,\text{k}\Omega
$$

再由「串联 $R_s$ 与 $R_S=2\,\text{k}\Omega$ 并联」反解出物理电阻 $R_s$：

$$
R_{deg}=R_S\parallel R_s=\frac{2R_s}{2+R_s}=0.392576
$$

$$
R_s=\frac{1}{\dfrac{1}{R_{deg}}-\dfrac{1}{R_S}}=\frac{1}{\dfrac{1}{0.392576}-\dfrac{1}{2}}=\frac{1}{2.547270-0.5}=0.488453\,\text{k}\Omega
$$

$$
\boxed{R_s\approx0.48845\,\text{k}\Omega=488.45\,\Omega}
$$

新的输出幅度（$\hat v_g$ 翻倍为 $2\times0.131433=0.262866\,\text{V}$）：

$$
\hat v_o'=|A_v'|\,\hat v_g'=3.56504\times0.262866=0.937129\,\text{V}
$$

$$
\boxed{\hat v_{sig}'=0.525732\,\text{V},\qquad \hat v_o'=0.937129\,\text{V}}
$$

<aside>
💡

**口诀：** 加 source degeneration 后，输入摆幅靠 $(1+|A_v|)$ 减半翻倍上去；但增益降了一半多，所以输出反而从 $1.069\,\text{V}$ 掉到 $0.937\,\text{V}$ —— **线性度换来的是更小的输出摆幅**。

</aside>